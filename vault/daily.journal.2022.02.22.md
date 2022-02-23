---
id: ThR6iHtvR6M8Gc0XHira1
title: '2022-02-22'
desc: ''
updated: 1645581891693
created: 1645576157331
traitIds:
  - journalNote
---

## FU Course

Ask students to help with review portion

Nice note: DDL outside of transactions

N:M instead of N:N


### Formal Definitions

`R(A1,A2,A3)` is the **schema** of a relation

Domain or `dom` is the range of possible acceptable values for an attribute

E.G. `dom(A1) = {0, 1}`, `dom(A2) = {a,b,c}`

**Relation state** is the union of all domains of all attributes

**Entity Integrity Constraint** ensures that keys exist that can be used to identify an entity

**Superkey** a set of attributes that can uniquely identify a row. No duplicated sets of attributes exist for this.

**Key** is a minimal superkey - removes non-critical identifying aspects of superkey. Uniquely identifies the row. Doesn't need to be a PK

Constraints on Relationships:

- Domain (Not allowable value)
- Key (PK already exists)
- Referential Integrity (
- Bad FK)
- Entity (PK is null)

Cascade options (set on relationship creation)
- Restrict - Reject deletion
- Cascade - Propagate the change. Use when you ALWAYS want those rows to be deleted.
- Set Null - Sets FKs to null 

U on EER diagrams indicates "inherits from" (specializes)

Circle nodes:
  - d - Distinct - can only be one
  - o - Overlap - could be multiple ones 
  - U - Similar to distinct?