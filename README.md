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
  - [Differences with traditional RDBMS](#differences-with-traditional-rdbms)
  - [Type system](#type-system)
  - [Hive Data Models](#hive-data-models)
  - [Partitioning](#partitioning)
  - [Bucketing/Clustering](#bucketingclustering)
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

## Differences with traditional RDBMS

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
   2. Within HDFS, databases are stored as folders, with a file structure of tables within.
   3. Each table has a copy of its data, that is separately stored from other tables' data. These tables are refered to as **managed tables**.
   4. **External tables** are a special kind of tables that donot have ownership of the data. They refer to some other source, and display its data, in a column format.
   5. Dropping a managed table destroys the data, while dropping an external data preserves the data.
2. Partitions
3. Buckets or clusters

## Partitioning

Partitioning is a way in which certain `where` queries can be sped up, by having to look at lesser data files. It divides a table into smaller, manageable parts.

Consider a table named `transac_recds`. Its data files would be stored in the following way:

```
/usr/hive/warehouse/DB_NAME/transac_recds/file1.txt
/usr/hive/warehouse/DB_NAME/transac_recds/file2.txt
```

Now, if we want to run the following query:

```sql
SELECT * FROM transac_recds WHERE month="JAN";
```

It would require processing of all records in the table. This may lead to unnecessary overhead in case filtering by `month` is a common task.

To reduce query execution time, we may partition our table as follows:

```
/usr/hive/warehouse/DB_NAME/transac_recds/month=JAN  // Only JAN data
/usr/hive/warehouse/DB_NAME/transac_recds/month=FEB  // Only FEB data
/usr/hive/warehouse/DB_NAME/transac_recds/month=MAR  // Only MAR data
...
```

Now the above query will only process the records in the relevant partion (of `JAN`), thus decreasing execution time.

Basically, partitioning creates as many partitions as there are unique values in a column. This column is called the **partition key**.

Partitioning is commonly used to speed up slicing operations.

Note that when a table is partitioned based on some column, that column is no longer stored with the rest of the table data, as its value is implicit in the partitions.

Strict partitioning: If the `where` clause can only refer to the partition key, it is strict partitioning. We can turn it off by:

```
SET hive.exec.dynamic.partition.mode=nonstrict;
```

## Bucketing/Clustering

This is similar to partitioning. Consider a case where there are a very high number of unique values in a column. If a table is partitioned on such a column, there will be a large number of partitions, leading to a high maintainence overhead.

Bucketing allows an alternate strategy. Here, we create a fixed number of buckets, say N. Each value in a column is hashed, and compressed to get an integer in the range 0 to N - 1. Based on this value, the bucket for the column is decided.

We can use bucketing and partitioning together for best performance.

## Common commands

1. `create database <DB name> [comment 'place comment here'];`: used to create a new database with the given name. Can optionally add a comment explaining the purpose of the DB.
2. `describe database [extended] <DB name>;`: Displays the properties of the specified database.
3. `show databases;`: Displays all databases.
4. `use <DB name>;`: Sets the specified database to the currently active database.
5. `create table <table name>[...schema...];`: Creates a table with the desired schema in the active DB.
6. `create external table <table name>[...schema...] LOCATION <path to data file in HDFS>;`: Creates an external table with the desired schema in the active DB, and refers to a specified data file.
7. `show tables;`: Displays all tables in the current database.
8. `LOAD DATA [Data source] INTO TABLE <table name>;`: Loads the data into the specified table.
9. Select-where queries are similar to those in SQL.
10. `drop table <table name>;`: Drops the specified table, and deletes the data if the table was a managed table.
