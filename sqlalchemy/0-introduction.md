# SQLAlchemy - Introduction

## Introduction
- is a SQL toolkit and Object Relation Mapper (ORM).
- give the power and fexibility of SQL.
- provide a full suite of well known enterprise-level persistence patterns.
- designed for efficient and high-performing database access.

### SQLAlchemy's philosophy
- consider SQL databases to be a relational algebla engine.
- row can be selected not only from table also from joins and other statements.
- these units can be composed into a larger structure.

## Key features
- consist of two distinct components: the `Core` and the `ORM` (optional).
    - Core:
        - a fully featured SQL abstraction toolkit.
        - provide a smooth layer of abstraction over a wide variety of DBAPI implementations and behaviors.
    - ORM:
        - is an optional package which builds upon the Core.
- mature, high performance architecture, DBA approved.
- non-optionated: never "generates" schemas or relies on naming conventions.
- [unit of work](https://martinfowler.com/eaaCatalog/unitOfWork.html): organizes pending insert/update/delete operations into queues and flushes them all in one batch.
- function-based query construction: allows SQL clauses to be built via Python functions and expressions.
- modular and extensible: different parts of SQLAlchemy can be used independently of the rest.
- separate class design and mapping.
- cache collections and references between objects once loaded.
- eager loading feature allows entire graphs of objects linked by collections and references to be loaded with few or just one query.
- inheritance mapping.
- primary and foreign keys are represented as sets of columns.
- raw SQL statement mapping: map result of raw SQL queries to objects.
- pre/post data processing at the bind parameter and the result set level (getter, setter...)
- supported platforms: >= Python2.5, Python3.x, Jython and Pypy.
- supported databases: SQLite, Postgresql, MySQL, Oracle, MS-SQL, Firebird, Sybase and others
