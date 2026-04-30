# Delta Lake (delta-lake)

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
