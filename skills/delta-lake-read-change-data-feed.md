---
name: delta-lake-read-change-data-feed
description: >-
  Read the change data feed of a shared Delta table over a version or timestamp range, to sync
  incrementally instead of re-reading a full snapshot.
api: Delta Sharing Protocol
provider: Delta Lake
spec: openapi/delta-lake-delta-sharing-protocol-openapi.yml
operations:
  - GetTableVersion
  - GetTableMetadata
  - GetTableChanges
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/delta-lake-delta-sharing-protocol-openapi.yml and
  https://github.com/delta-io/delta-sharing/blob/main/PROTOCOL.md
---

# Read a shared table's change data feed

Use this instead of `QueryTable` when you already hold a copy of the table and only need what
changed. The change data feed must be enabled on the table by the data provider; if it is not,
expect a **400**.

## Steps

1. **Establish your starting point.** Either keep the version you last processed, or call
   `GetTableVersion`, `GET /shares/{share}/schemas/{schema}/tables/{table}/version`, with
   `startingTimestamp` to resolve a time to a version.
2. **Fetch the schema** — `GetTableMetadata` — so you can interpret the change rows. Do this on
   every sync, not once: the schema can evolve between versions.
3. **Read the changes** — `GetTableChanges`,
   `GET /shares/{share}/schemas/{schema}/tables/{table}/changes`.
   Range parameters, all optional query parameters:
   `startingVersion`, `endingVersion`, `startingTimestamp`, `endingTimestamp`,
   `includeHistoricalMetadata`, `includeHistoricalProtocol`.
   Give a **bounded** range — an open-ended feed on a busy table can return a very large response.
4. **Parse the newline-delimited JSON** — `protocol`, `metaData`, then `add`, `cdf` and `remove`
   actions across the range, each carrying a pre-signed URL to the file holding those rows.
5. **Advance your cursor** to `endingVersion` (or the highest version you actually applied) and
   store it. There is no server-side cursor: resumption is entirely your responsibility.

## Rules

- **Set `includeHistoricalMetadata=true`** when the range may cross a schema change, or you will
  parse later files with an earlier schema.
- **The response format is negotiated,** not fixed: send
  `delta-sharing-capabilities: responseformat=delta` to read tables using deletion vectors or
  column mapping. Without it the server answers in parquet format and a table with advanced
  features may be unreadable.
- **Reads are safe to retry.** `GetTableChanges` is a GET with no side effects, so a network
  failure mid-sync costs only bandwidth — re-request the same range.
- **Errors** use `{"errorCode","message"}` on `application/json`. There is no published
  `errorCode` registry, so branch on status: 400 (CDF not enabled, or a bad range), 401, 403,
  404, 500. See `errors/delta-lake-problem-types.yml`.
