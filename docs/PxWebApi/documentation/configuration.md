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

### Saved query storage

Saved queries are an essential part of the API. Users can create, view, and run
saved queries. Saved queries can also be used as the default selection when a
user does not specify any selection when downloading data see.
See [Default selection](#default-selection)

You can store saved queries either as files on the file system or in a relational
database. Select the storage method by setting `SavedQuery.Backend`.

#### File

With this option saved queries are stored to the local file system. Each saved
query is represented with two files with the name but with two diffrent suffixes
`.sqa` and `.sqs`. The `sqa` file is the file that contains the saved query. The
`sqs` file contains metadata about the saved query.

The files are stored in a subdirectory to `SavedQuery.FileStorage.Path` the name
of the subdirectory is the same of the first character of the id for the saved query.

You can change the locationwhere saved queries are stored by setting `SavedQuery.FileStorage.Path`.

#### Database

If you select to store saved queries in a relational database you then must set
the connection string to the database having the tables needed for saved queries
by setting `Saved.Query.DatabaseStorage.ConnectionString`. There are two
implementations one for `Microsoft` Sql Server and one for `Oracle`. Set the one
wanted with the setting `SavedQuery.DatabaseStorage.DatabaseVendor`.

The database needs to have two tables `SavedQueryMeta2` and `DefaultSelection`
With the following structure.

```
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

When data is extracted without giving a selection a default selection is used.
Then default selection is calculated by an algorithm that selected diffrent
values from the variables in the tabled depending on the type of variables and
the number of values. If you are not satisfied with the selection that the
algorithms selects, you can replace what is beeing used as default selection with
a saved query. The way to do this is manual and how it is done depends on the
type of storage for your saved queries.

!!! note
    Make sure that the `tableId` is the same as the one that the saved query targets.

### Saved queries as files

When you have saved queries stored as files on the filesystem. You replace the
default selection for a table by taking a copy of the files for the saved queries
that you would like to be used as the default selection. Rename the copy of the
files by replacing the saved query id with the id for the table. Then place the
copy of the files in the right subfolder.

### Saved queries in a database

If you have saved queries stored in a relational database. You replace the
default selection by adding a entry in the `DefaultSelection` table. This
requiers two values the id for the table and the id for the saved query.

!!! note
    Make sure that the `tableId` is the same as the one that the saved query targets.
    TableId is the primary key so you might have to do a update insted of an insert
    if a saved query have already been used before.
