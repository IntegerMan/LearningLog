---
id: 2opEPip83yz8sLrQUR5Tr
title: Data Files
desc: ''
updated: 1644116431745
created: 1644116194766
---

## Types of Files
> [[books.database.databaseinternals]] page 18

Data files are how databases store their information. Notable formats include:

- **Index-oriented Files (IOF)** - stores data inside the actual index file
- **Heap-organized Tables (Heap Files)** - typically stored in write order. No strong rules. Needs a dedicated index to store a _file offset_.
- **Hash-organized Tables (Hash Tables)** - similar to a map/dictionary. Hash files can be stored sequentially or in write-order.