---
name: delta-lake-read-shared-table
description: >-
  Read the current or a historical snapshot of a shared Delta table through a Delta Sharing
  server — resolve the version, fetch the schema, then request the pre-signed data file URLs.
api: Delta Sharing Protocol
provider: Delta Lake
spec: openapi/delta-lake-delta-sharing-protocol-openapi.yml
operations:
  - GetTableVersion
  - GetTableMetadata
  - QueryTable
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/delta-lake-delta-sharing-protocol-openapi.yml and
  https://github.com/delta-io/delta-sharing/blob/main/PROTOCOL.md
---

# Read a shared Delta table

Delta Sharing does not stream rows. It hands you **pre-signed URLs to Parquet files** (or, in
directory access mode, temporary cloud credentials) and you read the data yourself. Plan for
two hops: the sharing server, then object storage.

## Steps

1. **Resolve the version** — `GetTableVersion`,
   `GET /shares/{share}/schemas/{schema}/tables/{table}/version`.
   Pass `startingTimestamp` to resolve a point in time instead of the current version. Pin the
   returned version if you intend to make several consistent reads.
2. **Fetch the schema** — `GetTableMetadata`,
   `GET /shares/{share}/schemas/{schema}/tables/{table}/metadata`.
   The response is **newline-delimited JSON**: a `protocol` action then a `metaData` action.
   The OpenAPI types this as a bare `string`; the real shape is in PROTOCOL.md.
   Check `metaData` for the `accessModes` array — it tells you whether the server offers
   URL-based access, directory-based access, or both.
3. **Negotiate capabilities** with the `delta-sharing-capabilities` request header, a
   semicolon-separated list of `key=value1,value2` pairs, case-insensitive. Send
   `responseformat=delta` if you can process advanced reader features (deletion vectors,
   column mapping); omit it or send `responseformat=parquet` otherwise. A server that does not
   recognise the header answers in parquet.
4. **Query the table** — `QueryTable`,
   `POST /shares/{share}/schemas/{schema}/tables/{table}/query` with a JSON body.
   Useful body fields: `jsonPredicateHints` (current filter shape), `limitHint`, `version`,
   `timestamp`, `startingVersion`, `endingVersion`, `includeHistoricalProtocol`.
   Prefer `jsonPredicateHints` over `predicateHints` — PROTOCOL.md states the SQL-expression
   form will be deprecated once clients and servers have migrated.
5. **Read the response** — newline-delimited JSON: `protocol`, `metaData`, then one `file`/`add`
   action per data file, each carrying a pre-signed URL, and optionally a terminating
   `endStreamAction` that may carry a `refreshToken` or a `nextPageToken`.
6. **Fetch the data files** from those URLs directly. They expire — refresh a long-running scan
   using the `refreshToken` from the `endStreamAction` rather than re-planning the whole query.

## Rules

- **This surface is read-only.** The protocol defines no write, update or delete operation, so
  there is nothing here to undo. See the `reversibility` block in
  `conventions/delta-lake-conventions.yml`.
- **Idempotency is narrow.** If you negotiate `asyncquery=true`, include an `idempotencyKey` in
  the `QueryTable` body so a retried submission maps to the same `queryId` instead of starting a
  duplicate query. That is the protocol's *only* replay-protection mechanism; every other
  operation is a safe GET.
- **Asynchronous mode:** with `asyncquery=true` the server returns a `queryId` and you poll the
  Get Query Info API until the query succeeds. That operation is documented in PROTOCOL.md but is
  **not present in the published OpenAPI** — read the prose spec before implementing it.
- **`fileidhash`** is a standalone request header (`parquet` or `delta`) selecting how file `id`
  values are derived. The server must echo it back lowercased; an unsupported value returns
  **400**. Verify the echo before trusting the ids.
- **401 vs 403 vs 404:** 401 means the bearer token is missing, wrong or expired; 403 means you
  are authenticated but not granted; 404 means the object does not exist *or* is invisible to you.
