---
name: delta-lake-discover-shared-data
description: >-
  Walk a Delta Sharing server's three-level namespace — shares, schemas, tables — to find out
  what a recipient has been granted access to, without reading any table data.
api: Delta Sharing Protocol
provider: Delta Lake
spec: openapi/delta-lake-delta-sharing-protocol-openapi.yml
operations:
  - ListShares
  - GetShare
  - ListSchemas
  - ListTables
  - ListALLTables
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/delta-lake-delta-sharing-protocol-openapi.yml and
  https://github.com/delta-io/delta-sharing/blob/main/PROTOCOL.md
---

# Discover what a Delta Sharing server has shared with you

Every Delta Sharing operation is scoped by a recipient bearer token. Discovery tells you what
that token can see. It is entirely read-only.

## Before you start

Read the recipient **profile file** you were given. It is JSON with `shareCredentialsVersion`,
`endpoint`, `bearerToken` and an optional `expirationTime`. The `endpoint` is your base URL —
Delta Sharing is self-hosted, so there is no single global host. Send
`Authorization: Bearer {bearerToken}` on every request.

If `expirationTime` is in the past, stop and ask the data provider for a new profile file.
An expired token returns **401**, which is indistinguishable from a wrong token.

## Steps

1. **List the shares** — `ListShares`, `GET /shares`.
   Returns `{items: [{name, id}], nextPageToken}`. Pass `maxResults` and `pageToken` to page.
2. **Page until the token is gone.** `nextPageToken` may be an **empty string** *or* **absent**
   when there are no more results. Handle both — treating only "absent" as the end is the
   single most common client bug against this protocol.
3. **Optionally confirm one share** — `GetShare`, `GET /shares/{share}`.
4. **List the schemas in a share** — `ListSchemas`, `GET /shares/{share}/schemas`.
   Each `Schema` carries `name` and a `share` back-reference.
5. **List the tables in a schema** — `ListTables`,
   `GET /shares/{share}/schemas/{schema}/tables`. Each `Table` carries `name`, `schema`, `share`.
6. **Or flatten the whole share in one call** — `ListALLTables`,
   `GET /shares/{share}/all-tables`. Prefer this when you want an inventory rather than a
   guided walk; it saves one request per schema.

## Rules

- **Names, not ids.** Share, schema and table names are the addressing keys. They are
  case-insensitive, at most 255 characters, and may not contain a space, a forward slash, an
  ASCII control character or DELETE; schema and table names additionally may not contain a
  period. Never construct a name — always list the parent collection first.
- **Do not guess a path.** A name that does not exist returns **404**, which reads identically
  to a name you are not permitted to see.
- **Error envelope** is `{"errorCode": "string", "message": "string"}` as `application/json` —
  *not* RFC 9457 problem+json. `errorCode` has no published registry, so branch on the HTTP
  status, not on the code string. See `errors/delta-lake-problem-types.yml`.
- **No rate-limit signal exists.** The protocol defines no 429, no `Retry-After` and no
  `RateLimit-*` headers. Whatever throttling a given server applies is undocumented, so pace
  yourself and back off on 5xx.
