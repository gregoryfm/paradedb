<h1 align="center">
  <img src="../docs/logo/pg_lakehouse.svg" alt="pg_lakehouse" width="500px">
<br>
</h1>

[![Test pg_lakehouse](https://github.com/paradedb/paradedb/actions/workflows/test-pg_lakehouse.yml/badge.svg)](https://github.com/paradedb/paradedb/actions/workflows/test-pg_lakehouse.yml)
[![Benchmark pg_lakehouse](https://github.com/paradedb/paradedb/actions/workflows/benchmark-pg_lakehouse.yml/badge.svg)](https://github.com/paradedb/paradedb/actions/workflows/benchmark-pg_lakehouse.yml)

## Overview

`pg_lakehouse` is a Postgres extension that puts DuckDB inside Postgres. With `pg_lakehouse` installed, Postgres can query foreign object stores like S3 and table formats like Iceberg or Delta Lake. Queries are pushed down to DuckDB, a high performance analytical query engine.

`pg_lakehouse` uses DuckDB v1.0.0 and is supported on Postgres 14, 15, and 16. Support for Postgres 12 and 13 is coming soon.

Check out the `pg_lakehouse` benchmarks [here](https://benchmark.clickhouse.com/#eyJzeXN0ZW0iOnsiQWxsb3lEQiI6dHJ1ZSwiQXRoZW5hIChwYXJ0aXRpb25lZCkiOmZhbHNlLCJBdGhlbmEgKHNpbmdsZSkiOmZhbHNlLCJBdXJvcmEgZm9yIE15U1FMIjpmYWxzZSwiQXVyb3JhIGZvciBQb3N0Z3JlU1FMIjp0cnVlLCJCeUNvbml0eSI6ZmFsc2UsIkJ5dGVIb3VzZSI6ZmFsc2UsImNoREIgKFBhcnF1ZXQsIHBhcnRpdGlvbmVkKSI6ZmFsc2UsImNoREIiOmZhbHNlLCJDaXR1cyI6ZmFsc2UsIkNsaWNrSG91c2UgQ2xvdWQgKGF3cykiOmZhbHNlLCJDbGlja0hvdXNlIENsb3VkIChhd3MpIFBhcmFsbGVsIFJlcGxpY2FzIE9OIjpmYWxzZSwiQ2xpY2tIb3VzZSBDbG91ZCAoQXp1cmUpIjpmYWxzZSwiQ2xpY2tIb3VzZSBDbG91ZCAoQXp1cmUpIFBhcmFsbGVsIFJlcGxpY2EgT04iOmZhbHNlLCJDbGlja0hvdXNlIENsb3VkIChBenVyZSkgUGFyYWxsZWwgUmVwbGljYXMgT04iOmZhbHNlLCJDbGlja0hvdXNlIENsb3VkIChnY3ApIjpmYWxzZSwiQ2xpY2tIb3VzZSBDbG91ZCAoZ2NwKSBQYXJhbGxlbCBSZXBsaWNhcyBPTiI6ZmFsc2UsIkNsaWNrSG91c2UgKGRhdGEgbGFrZSwgcGFydGl0aW9uZWQpIjpmYWxzZSwiQ2xpY2tIb3VzZSAoZGF0YSBsYWtlLCBzaW5nbGUpIjpmYWxzZSwiQ2xpY2tIb3VzZSAoUGFycXVldCwgcGFydGl0aW9uZWQpIjpmYWxzZSwiQ2xpY2tIb3VzZSAoUGFycXVldCwgc2luZ2xlKSI6ZmFsc2UsIkNsaWNrSG91c2UgKHdlYikiOmZhbHNlLCJDbGlja0hvdXNlIjpmYWxzZSwiQ2xpY2tIb3VzZSAodHVuZWQpIjpmYWxzZSwiQ2xpY2tIb3VzZSAodHVuZWQsIG1lbW9yeSkiOmZhbHNlLCJDbG91ZGJlcnJ5IjpmYWxzZSwiQ3JhdGVEQiI6ZmFsc2UsIkRhdGFiZW5kIjpmYWxzZSwiRGF0YUZ1c2lvbiAoUGFycXVldCwgcGFydGl0aW9uZWQpIjpmYWxzZSwiRGF0YUZ1c2lvbiAoUGFycXVldCwgc2luZ2xlKSI6ZmFsc2UsIkFwYWNoZSBEb3JpcyI6ZmFsc2UsIkRydWlkIjpmYWxzZSwiRHVja0RCIChQYXJxdWV0LCBwYXJ0aXRpb25lZCkiOmZhbHNlLCJEdWNrREIiOmZhbHNlLCJFbGFzdGljc2VhcmNoIjp0cnVlLCJFbGFzdGljc2VhcmNoICh0dW5lZCkiOnRydWUsIkdsYXJlREIiOmZhbHNlLCJHcmVlbnBsdW0iOmZhbHNlLCJIZWF2eUFJIjpmYWxzZSwiSHlkcmEiOnRydWUsIkluZm9icmlnaHQiOmZhbHNlLCJLaW5ldGljYSI6ZmFsc2UsIk1hcmlhREIgQ29sdW1uU3RvcmUiOmZhbHNlLCJNYXJpYURCIjpmYWxzZSwiTW9uZXREQiI6ZmFsc2UsIk1vbmdvREIiOmZhbHNlLCJNb3RoZXJkdWNrIjpmYWxzZSwiTXlTUUwgKE15SVNBTSkiOmZhbHNlLCJNeVNRTCI6ZmFsc2UsIk94bGEiOmZhbHNlLCJQYXJhZGVEQiAoUGFycXVldCwgcGFydGl0aW9uZWQpIjp0cnVlLCJQYXJhZGVEQiAoUGFycXVldCwgc2luZ2xlKSI6dHJ1ZSwiUGlub3QiOmZhbHNlLCJQb3N0Z3JlU1FMICh0dW5lZCkiOnRydWUsIlBvc3RncmVTUUwiOnRydWUsIlF1ZXN0REIgKHBhcnRpdGlvbmVkKSI6ZmFsc2UsIlF1ZXN0REIiOmZhbHNlLCJSZWRzaGlmdCI6ZmFsc2UsIlNlbGVjdERCIjpmYWxzZSwiU2luZ2xlU3RvcmUiOmZhbHNlLCJTbm93Zmxha2UiOmZhbHNlLCJTUUxpdGUiOmZhbHNlLCJTdGFyUm9ja3MiOmZhbHNlLCJUYWJsZXNwYWNlIjpmYWxzZSwiVGVtYm8gT0xBUCAoY29sdW1uYXIpIjp0cnVlLCJUaW1lc2NhbGVEQiAoY29tcHJlc3Npb24pIjp0cnVlLCJUaW1lc2NhbGVEQiI6dHJ1ZSwiVW1icmEiOmZhbHNlfSwidHlwZSI6eyJDIjp0cnVlLCJjb2x1bW4tb3JpZW50ZWQiOnRydWUsIlBvc3RncmVTUUwgY29tcGF0aWJsZSI6dHJ1ZSwibWFuYWdlZCI6dHJ1ZSwiZ2NwIjp0cnVlLCJzdGF0ZWxlc3MiOnRydWUsIkphdmEiOnRydWUsIkMrKyI6dHJ1ZSwiTXlTUUwgY29tcGF0aWJsZSI6dHJ1ZSwicm93LW9yaWVudGVkIjp0cnVlLCJDbGlja0hvdXNlIGRlcml2YXRpdmUiOnRydWUsImVtYmVkZGVkIjp0cnVlLCJzZXJ2ZXJsZXNzIjp0cnVlLCJhd3MiOnRydWUsInBhcmFsbGVsIHJlcGxpY2FzIjp0cnVlLCJBenVyZSI6dHJ1ZSwiUnVzdCI6dHJ1ZSwic2VhcmNoIjp0cnVlLCJkb2N1bWVudCI6dHJ1ZSwiYW5hbHl0aWNhbCI6dHJ1ZSwic29tZXdoYXQgUG9zdGdyZVNRTCBjb21wYXRpYmxlIjp0cnVlLCJ0aW1lLXNlcmllcyI6dHJ1ZX0sIm1hY2hpbmUiOnsiMTYgdkNQVSAxMjhHQiI6dHJ1ZSwiOCB2Q1BVIDY0R0IiOnRydWUsInNlcnZlcmxlc3MiOnRydWUsIjE2YWN1Ijp0cnVlLCJjNmEuNHhsYXJnZSwgNTAwZ2IgZ3AyIjp0cnVlLCJMIjp0cnVlLCJNIjp0cnVlLCJTIjp0cnVlLCJYUyI6dHJ1ZSwiYzZhLm1ldGFsLCA1MDBnYiBncDIiOnRydWUsIjE5MkdCIjp0cnVlLCIyNEdCIjp0cnVlLCIzNjBHQiI6dHJ1ZSwiNDhHQiI6dHJ1ZSwiNzIwR0IiOnRydWUsIjk2R0IiOnRydWUsIjE0MzBHQiI6dHJ1ZSwiZGV2Ijp0cnVlLCI3MDhHQiI6dHJ1ZSwiYzVuLjR4bGFyZ2UsIDUwMGdiIGdwMiI6dHJ1ZSwiYzUuNHhsYXJnZSwgNTAwZ2IgZ3AyIjp0cnVlLCJjNmEuNHhsYXJnZSwgMTUwMGdiIGdwMiI6dHJ1ZSwiY2xvdWQiOnRydWUsImRjMi44eGxhcmdlIjp0cnVlLCJyYTMuMTZ4bGFyZ2UiOnRydWUsInJhMy40eGxhcmdlIjp0cnVlLCJyYTMueGxwbHVzIjp0cnVlLCJTMiI6dHJ1ZSwiUzI0Ijp0cnVlLCIyWEwiOnRydWUsIjNYTCI6dHJ1ZSwiNFhMIjp0cnVlLCJYTCI6dHJ1ZSwiTDEgLSAxNkNQVSAzMkdCIjp0cnVlLCJjNmEuNHhsYXJnZSwgNTAwZ2IgZ3AzIjp0cnVlfSwiY2x1c3Rlcl9zaXplIjp7IjEiOnRydWUsIjIiOnRydWUsIjQiOnRydWUsIjgiOnRydWUsIjE2Ijp0cnVlLCIzMiI6dHJ1ZSwiNjQiOnRydWUsIjEyOCI6dHJ1ZSwic2VydmVybGVzcyI6dHJ1ZSwiZGVkaWNhdGVkIjp0cnVlfSwibWV0cmljIjoiaG90IiwicXVlcmllcyI6W3RydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWUsdHJ1ZSx0cnVlLHRydWVdfQ==).

### Motivation




Today, a vast amount of non-operational data — events, metrics, historical snapshots, vendor data, etc. — is ingested into data lakes like S3. Querying this data by moving it into a cloud data warehouse or operating a new query engine is expensive and time-consuming. The goal of `pg_lakehouse` is to enable this data to be queried directly from Postgres. This eliminates the need for new infrastructure, loss of data freshness, data movement, and non-Postgres dialects of other query engines.

`pg_lakehouse` uses the foreign data wrapper (FDW) API to connect to any object store or table format and the executor hook API to push queries to DuckDB. While other FDWs like `aws_s3` have existed in the Postgres extension ecosystem, these FDWs suffer from two limitations:

1. Lack of support for most object stores and table formats
2. Too slow over large datasets to be a viable analytical engine

`pg_lakehouse` differentiates itself by supporting a wide breadth of object stores and formats and by being very fast (thanks to DuckDB).

### Roadmap

- [x] Read support for `pg_lakehouse`
- [ ] Write support for `pg_lakehouse`
- [ ] `EXPLAIN` support
- [x] Automatic schema detection
- [ ] Integration with the catalog providers
- [ ] Geospatial data support
- [ ] Table format metadata caching

#### Object Stores

- [x] AWS S3
- [x] S3-compatible stores (MinIO, R2)
- [x] Azure Blob Storage
- [x] Azure Data Lake Storage Gen2
- [x] Google Cloud Storage
- [x] HTTP server
- [x] Local file system

#### Table Formats

- [x] Parquet
- [x] CSV
- [ ] Avro
- [ ] JSON
- [x] Apache Iceberg
- [x] Delta Lake

## Installation

### From ParadeDB

The easiest way to use the extension is to run the ParadeDB Dockerfile:

```bash
docker run \
  --name paradedb \
  -e POSTGRESQL_USERNAME=<user> \
  -e POSTGRESQL_PASSWORD=<password> \
  -e POSTGRESQL_DATABASE=<dbname> \
  -e POSTGRESQL_POSTGRES_PASSWORD=<superuser_password> \
  -v paradedb_data:/bitnami/postgresql \
  -p 5432:5432 \
  -d \
  paradedb/paradedb:latest
```

This will spin up a Postgres instance with `pg_lakehouse` preinstalled.

### From Self-Hosted PostgreSQL

If you are self-hosting Postgres and would like to use the extension within your existing Postgres, follow the steps below.

It's **very important** to make the following change to your `postgresql.conf` configuration file. `pg_lakehouse` must be in the list of `shared_preload_libraries`:

```c
shared_preload_libraries = 'pg_lakehouse'
```

This ensures the best query performance from the extension .

#### Debian/Ubuntu

We provide prebuilt binaries for Debian-based Linux for Postgres 16, 15 and 14. You can download the latest version for your architecture from the [releases page](https://github.com/paradedb/paradedb/releases).

ParadeDB collects anonymous telemetry to help us understand how many people are using the project. You can opt out of telemetry by setting `export PARADEDB_TELEMETRY=false` (or unsetting the variable) in your shell or in your `~/.bashrc` file before running the extension.

#### macOS

We don't suggest running production workloads on macOS. As a result, we don't provide prebuilt binaries for macOS. If you are running Postgres on macOS and want to install `pg_lakehouse`, please follow the [development](#development) instructions, but do `cargo pgrx install --release` instead of `cargo pgrx run`. This will build the extension from source and install it in your Postgres instance.

You can then create the extension in your database by running:

```sql
CREATE EXTENSION pg_lakehouse;
```

Note: If you are using a managed Postgres service like Amazon RDS, you will not be able to install `pg_lakehouse` until the Postgres service explicitly supports it.

#### Windows

Windows is not supported. This restriction is [inherited from pgrx not supporting Windows](https://github.com/pgcentralfoundation/pgrx?tab=readme-ov-file#caveats--known-issues).

## Usage

The following example uses `pg_lakehouse` to query an example dataset of 3 million NYC taxi trips from January 2024, hosted in a public `us-east-1` S3 bucket provided by ParadeDB.

```sql
CREATE EXTENSION pg_lakehouse;
CREATE FOREIGN DATA WRAPPER parquet_wrapper HANDLER parquet_fdw_handler VALIDATOR parquet_fdw_validator;

-- Provide S3 credentials
CREATE SERVER parquet_server FOREIGN DATA WRAPPER parquet_wrapper;

-- Create foreign table with auto schema creation
CREATE FOREIGN TABLE trips ()
SERVER parquet_server
OPTIONS (files 's3://paradedb-benchmarks/yellow_tripdata_2024-01.parquet');

-- Success! Now you can query the remote Parquet file like a regular Postgres table
SELECT COUNT(*) FROM trips;
  count
---------
 2964624
(1 row)
```

To query your own data, please refer to the [documentation](https://docs.paradedb.com/analytics/object_stores).

## Shared Preload Libraries

Because this extension uses Postgres hooks to intercept and push queries down to DuckDB, it is **very important** that it is added to `shared_preload_libraries` inside `postgresql.conf`.

```bash
# Inside postgresql.conf
shared_preload_libraries = 'pg_lakehouse'
```

## Development

### Install Rust

To develop the extension, first install Rust via `rustup`.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup install <version>

rustup default <version>
```

Note: While it is possible to install Rust via your package manager, we recommend using `rustup` as we've observed inconsistencies with Homebrew's Rust installation on macOS.

Then, install the PostgreSQL version of your choice using your system package manager. Here we provide the commands for the default PostgreSQL version used by this project:

### Install Other Dependencies

Before compiling the extension, you'll need to have the following dependencies installed.

```bash
# macOS
brew install make gcc pkg-config openssl

# Ubuntu
sudo apt-get install -y make gcc pkg-config libssl-dev
```

### Install Postgres

```bash
# macOS
brew install postgresql@16

# Ubuntu
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
sudo apt-get update && sudo apt-get install -y postgresql-16 postgresql-server-dev-16
```

If you are using Postgres.app to manage your macOS PostgreSQL, you'll need to add the `pg_config` binary to your path before continuing:

```bash
export PATH="$PATH:/Applications/Postgres.app/Contents/Versions/latest/bin"
```

### Install pgrx

Then, install and initialize `pgrx`:

```bash
# Note: Replace --pg16 with your version of Postgres, if different (i.e. --pg15, --pg14, etc.)
cargo install --locked cargo-pgrx --version 0.11.3

# macOS arm64
cargo pgrx init --pg16=/opt/homebrew/opt/postgresql@16/bin/pg_config

# macOS amd64
cargo pgrx init --pg16=/usr/local/opt/postgresql@16/bin/pg_config

# Ubuntu
cargo pgrx init --pg16=/usr/lib/postgresql/16/bin/pg_config
```

If you prefer to use a different version of Postgres, update the `--pg` flag accordingly.

Note: While it is possible to develop using pgrx's own Postgres installation(s), via `cargo pgrx init` without specifying a `pg_config` path, we recommend using your system package manager's Postgres as we've observed inconsistent behaviours when using pgrx's.

### Running Tests

We use `cargo test` as our runner for `pg_lakehouse` tests. Tests are conducted using [testcontainers](https://github.com/testcontainers/testcontainers-rs) to manage testing containers like [LocalStack](https://hub.docker.com/r/localstack/localstack). `testcontainers` will pull any Docker images that it requires to perform the test.

You also need a running Postgres instance to run the tests. The test suite will look for a connection string on the `DATABASE_URL` environment variable. You can set this variable manually, or use `.env` file with contents like this:

```text
DATABASE_URL=postgres://<username>@<host>:<port>/<database>
```

## License

`pg_lakehouse` is licensed under the [GNU Affero General Public License v3.0](../LICENSE) and as commercial software. For commercial licensing, please contact us at [sales@paradedb.com](mailto:sales@paradedb.com).
