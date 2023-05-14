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