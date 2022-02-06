---
id: Erlg7m7bmrbLhdecY8NFs
title: DBMS Architecture
desc: ''
updated: 1644116222383
created: 1644115727195
---

## Query Execution Flow

> [[books.database.databaseinternals]] pages 8-10

Requests communicate via Transport subsystem, flow through to a query processor, then go to the execution engine.

Transport subsystem effectively coordinates between nodes.

Query processor validates queries.
Query processor creates an optimized execution plan from a query that is then handed off for execution.

Execution can happen either locally or remotely depending on what node needs to be queried and can be reads and writes.

Execution engine collects results from all executing nodes.

## Major Components

> [[books.database.databaseinternals]] page 10

Databases consist of the following major components:

- **Transaction Processor** - ensures that transactions are isolated
- **Lock Manager** - enforces locks around db resources
- **Access Methods / Storage Structures** - Various storage structures on disk such as heap files, b-trees, etc. See [[databases.data_files]]
- **Buffer Manager** - handles caching for optimization
- **Recovery Manager** - manages operation log, retains consistency on crashes