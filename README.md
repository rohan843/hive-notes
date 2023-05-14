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

