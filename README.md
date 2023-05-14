# Hive Notes

## Contents

- [Hive Notes](#hive-notes)
  - [Contents](#contents)
  - [Background](#background)
  - [Hive Components](#hive-components)
    - [Hive Shell](#hive-shell)
    - [Metastore](#metastore)
    - [Hive driver](#hive-driver)
    - [Other components](#other-components)
  - [Limitations](#limitations)
  - [Abilities](#abilities)
  - [Differneces with traditional RDBMS](#differneces-with-traditional-rdbms)
  - [Type system](#type-system)
  - [Hive Data Models](#hive-data-models)
  - [Common commands](#common-commands)

## Background

Hive was developed to handle _big data_, by using **SQL - like queries** (HiveQL), that were converted to MapReduce jobs.

Hive is used to manage big data, but it is limited to only structured data.

Hive internally uses HDFS to store data.

## Hive Components

### Hive Shell

Used to interact with the Hive ecosystem.

### Metastore

Stores metadata about the Hive deployment, such as table definition, view definition, etc.

There are many possible types of metastore:

1. Embedded metastore: Comes built into the Hive ecosystem. It stores the metadata in a derby database, however, only one user connection, such as only one shell can access it at a time.
2. Local metastore: May use some database such as MySQL. Allows multiple user connections.
3. Remote metastore: Like local metastore, however, accessed via a remote machine.

### Hive driver

Takes high level HiveQL commands and converts them into Hadoop commands.

### Other components

Other components include hive compiler, execution engine, etc.

## Limitations

1. Not designed for OLTP.
2. Does not offer real - time queries and row level updates.
3. Latency for Hive queries is generally very high (some minutes).
4. Provides unoptimal latency for interactive data browsing.

## Abilities 

1. Can filter rows from a table using a `where` clause.
2. Can store the results of a query into another table.
3. Can manage tables and partitions (`create`, `drop` and `alter`)
4. Can store the results of a query in an HDFS directory.
5. Can perform an equi-join between two tables.

## Differneces with traditional RDBMS

1. **Schema-on-read**: This scheme means that the schema of data is validated not when the data is loaded, but when it is queried (i.e., read). This allows for a very fast initial load.
2. No updates, transactions or indexes are supported.

## Type system

The primitive types are:

1. Boolean type:
   - `BOOLEAN`: {`TRUE`, `FALSE`}
2. Integers:
   - `TINYINT`: 1 Byte integer
   - `SMALLINT`: 2 Byte integer
   - `INT`: 4 Byte integer
   - `BIGINT`: 8 Byte integer
3. Floating point numbers:
   - `FLOAT`: Single precision
   - `DOUBLE`: Double precision
4. String type:
   - `STRING`: Sequence of characters

Hive also supports structs, maps, and arrays.

## Hive Data Models

1. Databases: contain tables with unique names, and have their own namespaces. Behave as a normal RDBMS database.
   1. A `default` database is provided by default. Rest can be created.
2. Partitions
3. Buckets or clusters

## Common commands

1. `create database <DB name> [comment 'place comment here'];`: used to create a new database with the given name. Can optionally add a comment explaining the purpose of the DB.
2. `describe database [extended] <DB name>;`: Displays the properties of the specified database.
3. `show databases;`: Displays all databases.
4. `use <DB name>;`: Sets the specified database to the currently active database.
5. `create table <table name>[...schema...];`: Creates a table with the desired schema in the active DB.
6. `show tables;`: Displays all tables in the current database.
7. `LOAD DATA [Data source] INTO TABLE <table name>;`: Loads the data into the specified table.