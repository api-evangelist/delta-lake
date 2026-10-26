# Delta Lake (delta-lake)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Delta Lake is a graduated project of the Linux Foundation AI & Data Foundation providing an open source storage framework for building Lakehouse architectures. Originally contributed by Databricks, it adds reliability, quality, and performance to data lakes with ACID transactions, schema enforcement, time travel, and Iceberg/Hudi interoperability via UniForm.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/delta-lake/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party
- **x-type:** opensource

## Tags

- Data, Data Lake, Lakehouse, Linux Foundation, Open Source, Storage, Streaming

## Timestamps

- **Created:** 2026-03-16
- **Modified:** 2026-04-28

## APIs

### Delta Lake Storage Framework

The Delta Lake storage framework defines the on-disk transaction log and protocol that adds ACID transactions, schema enforcement, and time travel to Parquet-based data lakes. Delta Lake exposes Spark, Rust, and Python APIs (delta-spark, delta-rs, deltalake) rather than a network API.

- **Human URL:** https://docs.delta.io/

#### Properties

- [Documentation](https://docs.delta.io/)
- [Protocol](https://github.com/delta-io/delta/blob/master/PROTOCOL.md)
- [SourceCode](https://github.com/delta-io/delta)

### Delta Sharing Protocol

Delta Sharing is an open protocol for secure data sharing across organizations, defined as a REST API specification. Sharing servers expose endpoints for listing shares, schemas, and tables, and for retrieving signed URLs to underlying data files.

- **Human URL:** https://delta.io/sharing/

#### Properties

- [Documentation](https://github.com/delta-io/delta-sharing)
- [Protocol](https://github.com/delta-io/delta-sharing/blob/main/PROTOCOL.md)
- [SourceCode](https://github.com/delta-io/delta-sharing)

### Delta Catalog-Managed Tables

Catalog-managed Delta tables delegate table scan planning to an external catalog using the Iceberg REST Catalog protocol, enabling interoperability with Unity Catalog and other catalog services.

- **Human URL:** https://delta.io/blog/2026-02-02-delta-catalog-managed-tables/

#### Properties

- [Documentation](https://delta.io/blog/2026-02-02-delta-catalog-managed-tables/)
- [Protocol](https://github.com/delta-io/delta/blob/master/PROTOCOL.md)

## Common Properties

- [Website](https://delta.io/)
- [Documentation](https://docs.delta.io/)
- [GitHub Org](https://github.com/delta-io)
- [Linux Foundation](https://lfaidata.foundation/projects/delta-lake/)
- [Vocabulary](vocabulary/delta-lake-vocabulary.yml)

## Maintainers

- **Kin Lane** - kin@apievangelist.com
