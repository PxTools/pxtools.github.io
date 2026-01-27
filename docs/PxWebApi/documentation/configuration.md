# Configuring PxWebApi 2

The configuration is split into several different files, each with a specific
purpose. The main file you will need to modify is `appsettings.json`.

## appsettings.json

This file contains all functional settings. A recommended configuration after
installation is the Admin protection settings, which control access to the admin
API. The settings in `appsettings.json` can also be overridden by environment
variables. For example, to set the DataSourceType, define an environment variable
called `DataSource__DataSourceType`.

### General settings

#### Language

Specify which languages should be available by editing the
`PxApiConfiguration.Languages` section. Set the default language by assigning
`PxApiConfiguration.DefaultLanguage` to the `id` of one of the languages listed
under `PxApiConfiguration.Languages`.

#### Downloading data

You can limit the size of data that can be downloaded using the
`PxApiConfiguration.MaxDataCells` setting.
You can also control which formats are available for downloading data by setting
`PxApiConfiguration.OutputFormats`.
The following formats can be enabled or disabled:

- Excel (`xlsx`)
- HTML table (`html`)
- Legacy PxJson format (`json-px`)
- Parquet (`parquet`)

The following formats are always available and cannot be disabled:

- PX (`px`)
- JSON-stat v2 (`json-stat2`)
- Comma separated (`csv`)

!!! note
    JSON-stat v1 has been deprecated and is no longer supported as an output format.

#### Attribution

You can specify how you would like others to reference your data by setting
`PxApiConfiguration.SourceReference`. Make sure to provide a section for each
language specified under `PxApiConfiguration.Languages`.

You can also specify the license for the data under `PxApiConfiguration.License`.

#### Caching

You can control how long a response should be cached by setting
`PxApiConfiguration.CacheTime` (in seconds).

You also have the option to clear the cache at a specific time by setting `PxApiConfiguration.CacheClearTime`.

### Datasource

PxWebApi 2.0 supports two different data sources: PX file-based or CNMM. You can
only configure one at a time. Set `DataSource.DataSourceType` to either `PX` or
`CNMM` to specify which you want to use.

#### PX datasource

For the PX file datasource, you can set `DataSource.DataSourceType.PX.StrictAggregations`
to either `true` or `false` to determine if aggregations can be overlapping or not.

#### CNMM datasource

You should only have one database specified, and you must set `DataSource.DataSourceType.CNMM.DatabaseId`
to match the `id` of the database in `SqlDb.config`.

When you configure the CNMM datasource, a health check is automatically set up
to verify the database connection. It connects to the default database in `SqlDb.config`
and executes a statement. The default statement is `select 1`, but you can change
this by setting `DataSource.DataSourceType.CNMM.HealthCheckQuery`. Avoid using
queries that take a long time to process.

##### Change the root node

A `CNMM` database must have a root node in `MenuSelection` called `START`.
However, there are cases where you have a single physical `CNMM` database that
contains data for several logical statistical databases. In this scenario, the
second level represents the root node for each logical statistical database. You
can configure the API to only expose tables located under a specific node in
`MenuSelection` by setting the `DataSource.DataSourceType.CNMM.RootNode`
configuration option.

For example, if your database structure looks like this:

```sh
START
├── AA
│   └── TABLE_01
└── BB
    ├── TABLE_02
    ├── TABLE_03
    ├── TABLE_04
    └── TABLE_05
```

If you set your configuration in `appsettings.json` like this:

```json
{
    "DataSource": {
        "DataSourceType": "CNMM",
        "CNMM": {
            "DatabaseID": "My",
            "HealthCheckQuery": "select 1",
            "RootNode": "AA"
        }
    }
}
```

Then only `TABLE_01` would be accessible from the API.

!!! note
    Since the API can only expose the contents of one database at a time, you
    would need a separate instance of the API if you also want to expose the
    contents under the node `BB` in the example above.

### Saved query storage

Saved queries can be viewed as templates for repeatable data extractions. Users
can create, list, and execute them. A saved query can also serve as the default
selection for a table when a request omits an explicit selection (see
[Default selection](#default-selection)).

Supported storage backends:

- File system
- Relational database (SQL Server or Oracle)

Choose backend with `SavedQuery.Backend` (e.g. `"File"` or `"Database"`).

#### File

Each saved query is represented by two sibling files sharing the same base name
but having diffrent suffixes:

- `.sqa` – the serialized saved query (definition)
- `.sqs` – metadata (creation date, last run, etc.)

Directory layout: Saved queries are grouped in subfolders under
`SavedQuery.FileStorage.Path`. The subfolder name is the first character of the
saved query id. This prevents extremely large single directories.

Change the root storage path with `SavedQuery.FileStorage.Path`.

#### Database

Provide:

- Connection string: `SavedQuery.DatabaseStorage.ConnectionString`
- Vendor: `SavedQuery.DatabaseStorage.DatabaseVendor` (`SqlServer` or `Oracle`)

The database requires the tables: `SavedQueryMeta2` and `DefaultSelection`.

Schema (conceptual):

```sql
SavedQueryMeta2 (
    QueryId int NOT NULL,
    DataSourceType varchar(10) NOT NULL,
    DatabaseId varchar(500) NULL,
    DataSourceId varchar(500) NOT NULL,
    Status char(1) NOT NULL,
    StatusUse char(1) NOT NULL,
    StatusChange char(1) NOT NULL,
    OwnerId varchar(80) NOT NULL,
    MyDescription varchar(250) NOT NULL,
    Tags varchar(250) NULL,
    CreatedDate smalldatetime NOT NULL,
    ChangedDate smalldatetime NULL,
    ChangedBy varchar(80) NULL,
    UsedDate smalldatetime NULL,
    DataSourceUpdateDate smalldatetime NULL,
    SavedQueryFormat varchar(10) NOT NULL,
    SavedQueryStorage char(1) NOT NULL,
    QueryText varchar(max) NOT NULL,
    Runs int NOT NULL,
    Fails int NOT NULL,
)

DefaultSelection (
    TableId varchar(20) NOT NULL,
    SavedQueryId int NOT NULL,
)
```

Notes:

- `QueryText` stores the actual saved query (e.g. JSON) used for execution.
- `Runs` / `Fails` enable simple usage statistics.
- `DefaultSelection` maps a table id to the saved query used as its default.

### Rate limiting

API rate limiting settings are found under `IpRateLimiting`. You can read more
about the options [here](https://github.com/stefanprodan/aspnetcoreratelimit/wiki/ipratelimitmiddleware).
The most important settings are `IpRateLimiting.GeneralRules.Period` and
`IpRateLimiting.GeneralRules.Limit`, which control the time window and the number
of calls allowed during that period.

If you do not run behind a proxy or load balancer, you should remove the
`IpRateLimiting.RealIpHeader` setting.

### Admin protection

The admin API is protected by both an IP filter and a token that must be specified
in the request. You can set these options using `AdminProtection.IpWhitelist` and
`AdminProtection.AdminKey`. You can specify more than one IP in
`AdminProtection.IpWhitelist`, including ranges of IPs in CIDR notation.

## app.config

This is for legacy configuration. It contains paths for:

- language files
- aggregation and valueset files
- the name of the CNMM configuration file

In most cases, the default settings are sufficient and you do not need to modify
this file.

## web.config

The `web.config` file is used to configure PxWebApi 2.0 when running on Microsoft
Internet Information Services (IIS). In most cases, the default settings are
sufficient and you do not need to modify this file. Only advanced scenarios or
specific server requirements may require changes.

## SqlDb.config

Currently, this file is the same as in PxWeb 2023, with one exception: you should
only have one database specified.

## log4net.config

The `log4net.config` file manages how PxWebApi 2.0 creates and stores logs. By
default, logs are saved in the `/logs` directory. PxWebApi 2.0 relies on log4net
for logging, which allows you to customize log levels, formats, and destinations.
For more information on how to configure log4net, please refer to the official
documentation [here](https://logging.apache.org/log4net/manual/configuration.html).

## Default selection

When a data request is submitted without any explicit selection, PxWebApi builds
one automatically. The built-in algorithm picks a small, representative set of
variable values based on variable types and cardinalities.

If that automatic selection is not suitable, you can override it with a saved
query. The override process depends on the saved query backend.

!!! note
    The saved query must target the same table (`tableId` match) or the override
    will fail or have unexpected behavior.

    Also note that this is an manual process to replace the default selection.

### File backend override

Steps:

1. Identify (or create) the saved query you want to use (its `.sqa` + `.sqs`).
2. Copy both files.
3. Rename the copies so the base name equals the target table id (keep suffixes).
4. Place them in the correct subfolder under `SavedQuery.FileStorage.Path`.
    (Subfolder name = first character of the table id.)
5. Clear cache if needed; subsequent no-selection requests now use the saved query.

### Database backend override

Add or update the mapping in `DefaultSelection`:

```sql
-- Pseudocode mapping description
TableId      = <table id>
SavedQueryId = <QueryId from SavedQueryMeta2>
```

Procedure:

1. Confirm the saved query exists (`SavedQueryMeta2.QueryId`).
2. UPDATE if present, INSERT if absent.
3. Clear caches if immediate effect is required.

!!! note
    `TableId` should acts as primary key; use UPDATE if a row already exists.
