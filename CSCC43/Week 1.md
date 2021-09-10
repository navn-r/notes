---
title: Week 1
weight: 1
---

# Week 1

## Database Management System (DBMS)

- **Database**: A structured system to persist data over a long period of time, with certain rules imposing it

- **DBMS**: Software to manage databases
  - Manages data from multiple databases
  - Enforces rules on the data



### Types of DMBS

- **Relational** (RDBMS)
  - Examples: PostgreSQL, MySQL, Oracle, SQLite
- Hierarchical
- Networked
- Object-Oriented
- NoSQL



## Key Concepts Overview

### Data Model
> A collection of tools for describing data, data relationships and semantics, and consistency constraints

#### Types of Data Models

- **Relational**
  - Data is stored in tables with rows and columns

- Entity-Relationship 
  - Mainly used for db design

- Object-based 
  - Object-oriented and Object-relational models

- Semi-structured (XML)

- Hierarchical

- Network


### Database Instance and Schema

> Similar to types and variables in programming languages

- **Logical Schema**: Overall logical structure of the db (variable types)

- **Physical Schema**: Overall physical structure of the db

- **Instance**: Snapshot of the db contents, *at a particular point in time* (variable value)



### Database Languages

- **Definition Language (DDL)**: Notation for defining a db schema
	
	```SQL
	CREATE TABLE students (
		ID		char(5),
		name	varchar(20)
	)
	```
	
- **Manipulation Language (DML)**: Query Language (creating, reading, updating, deleting data)
  - Types: Procedural and Declarative DML



### Structured Query Language (SQL)

- Non procedural; One query always returns one table
- **Not** a Turing machine equivalent language
- Usually embedded in other languages (Java, JS, Ruby)

Example
```SQL
SELECT * FROM students WHERE ID IS NOT NULL
```



### Database Engine

> A DBMS is partitioned into modules, each with its own responsibility

Functional Components:

- Storage Manager
- Query Processor 
- Transaction Manager



### Database Users

> Who uses databases, and for what?

- Naive Users (Average consumers)
  - Uses Client-facing app interfaces/GUIs to indirectly communicate with the db

- Application Programmers
  - Writes embedded SQL

- Sophisticated Users (Analysts + Scientists)
  - Uses tools to query data from the db

- Database Admininistrators (DBA)
  - Has central control over the db system
  - General functions:
    - Grants authorization to access data
    - Periodically backs up db
    - Monitors disk space and running jobs
    - Schema definition and modification permissions

---

## Relational Data Model

- 2D Tables logically represent data
- Relational Algebra is used for manipulating relations (tables)
- Set-based: Arbitrary row and column ordering

### Definitions

- $R$: Relation/Table Name
- **Attribute**: Column heading (defined in schema)
- **Tuple**: Row of *atomic* cells
- **Arity**: Number of attributes  $m = |\textrm{schema}(R)|$
- **Cardinality**: Number of tuples $n = |R|$
- **Domain**: Set of allowed values for each attribute
  - `null` is a member of every domain (can use it anywhere), this may cause problems (not null constraints)



#### Notation

Attributes

```js
tuple[attr] = value

// Alternatively
tuple.attr = value
```

Multiple Attributes

```js
tuple[attr1, attr2] = <value1, value2>
        
// Generally
tuple[A1, ..., AN] = <tuple[A1], ..., tuple[AN]>
```



### Integrity Constraints

> Property that must be satisfied by all db instances

- A db is **legal** if it satisfies all integrity constraints

  

#### Tuple + Domain Constraints

> Expresses conditions on values of each tuple, independently of others

**Tuple Constraint**: Multiple Attributes

```js
// Net, Gross, Deductions are attributes
Net = Gross - Deductions
```

**Domain Constraint**: Single attributes

```SQL
STUDENT_ID NOT NULL AND LENGTH(STUDENT_ID) >= 5
```



### Keys

> Set of attributes that **uniquely identifies** tuples in a relation

#### Formal Definition

“A set of attributes $K \subset R$ is a **superkey** if $R$ *does not* contain two distinct tuples $t_1$ and $t_2$ such that $t_1[K] = t_2[K]$”

- $K$ is a **minimal superkey** for $R$ if $K$ is the smallest key it can be, i.e. $\nexists$  $K'$ | $K' \subset K$ (nothing smaller)
  - Also known as a **candidate key**

### Primary Key

- Each relation must have a non-null primary key

- Candidate keys are chosen to be the primary key

- Relations reference each other using them (foreign keys)

