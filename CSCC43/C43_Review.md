---
slug: "final-review"
title: "Final Review"
---
# CSCC43 - Final Review
#### Fall 2021

> Thank god the SQL to this course is an elective.


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

> Property that must be satisfied by all database instances

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
  

---



## Relational Algebra

> A procedural language, consisting of a set of operations mapping one relation to another 


**Query Format:**

```sql
SELECT tuples FROM a_relation WHERE predicate
```

- Operators are compose-able; output is a relation

### Unary Operators

#### Select ($\sigma$)

- Removes unwanted rows, preserves arity (same schema as input relation)

- Notation: $\sigma_p(R)$

  - $R$: Relation
  - $p$: Selection predicate, a boolean formula of terms and connectives

- Example: $\sigma_{\textrm{age} \ge 21}(\textrm{people})$ 

  ```sql
  SELECT * FROM people WHERE age >= 21
  ```

  

#### Project ($\pi$)

- Removes unwanted columns, preserves cardinality (same number of rows as input relation)

- Notation: $\pi_Y(R)$

  - Outputs relation $Y \subset X$, where $X$ is the set of attributes of $R$

- Example: $\pi_{\textrm{name, age}}(\textrm{people})$

  ```sql
  SELECT name, age FROM people
  ```

  

#### Rename ($\rho$)

- Renames attributes or the entire relation

- Useful for when comparing a relation with itself (comparing one person’s age with everyone else)

- Notations

  - $\rho_{S(A, B)}(R)$: Rename the attributes of $R$ to $A, B$, then rename the relation as $S$
  - $\rho_S(R)$: Rename the entire relation as $S$ (attributes do not change)
  - $\rho_{A = X, B = Y}(R)$: Rename attributes $A$ and $B$ only (relation does not change)



### Binary Operators

#### Cartesian Product ($\cross$)

- Combines information from any two relations (Cross Product)

- Renames may be required when two relations share common attributes

- Notation: $r \cross s$ 

  - Outputs the cross product of $r$ and $s$

#### Natural Join ($\bowtie$)

- Also combines two relations into a single relation, accounts for **attribute names**
- Tuples are joined if the attribute is shared between both relations
- Notation: $T = R \bowtie S$

  - $\textrm{schema}(T) = \textrm{schema}(R) \cup \textrm{schema}(S)$
  
  - $|T| \le |R| * |S|$, usually around $\textrm{max}(|R|, |S|)$
  
  - Commutative, Associative, and no ambiguous N-Arity joins ($A \bowtie B \bowtie C$ is unambiguous)
  
- Equivalent to $\pi(\sigma_\theta(R \cross S))$
  - Projects away attributes
  
- Special schema cases:

  - If there is **no** overlap:  $\bowtie$ = $\cross$ and $|T| = |R| * |S|$ 
  - If there is **complete** overlap: $\bowtie$ = $\cap$

#### Theta Join ($\theta$)

- Returns the pairwise combo of rows that satisfy $\theta = p$, where $p$ is some predicate
- Notation: $T = R \bowtie_\theta S$
  - $|T| \le |R| * |S|$

- Doesn’t have to be an equality predicate, *any works*

- Equivalent to $\sigma_\theta(R \cross S)$
  - **No overlap** in schemas
  - Doesn’t project away attributes, doesn’t discard dangling tuples

#### Equijoin

- Special case of Theta Join
- Notation: $T = R \bowtie_{A = X, B = Y \dots} S$
  - Attributes names in $R$ and $S$ can differ
- Like Natural Join, but for arbitrary attributes
- Equivalent to $R \bowtie \rho(S)$


#### Outer Join (⟗)

- Extends the Inner Join by padding the relation with NULL ($\bot$) values

- Notation: $T = R ⟗ S$

- Padding Type:

  - LEFT (⟕): Pads tuples in the left relation with NULL if there is no matching tuple in the right relation
  - RIGHT (⟖): Reverse of left join (pads right)
  - FULL (⟗): Pads both


 #### Additive Operators ($\cup$, $\cap$, $-$)

> Operates on tuples *within* the relations, and not on the schema

Prerequisite for relations $r$ and $s$:

- Must have the same Arity

- Attribute domains/types of both must be compatible

**Union ($\cup$)**

- Combines two relations with into one

- Tuples are not repeated if they are in common, only one tuple will result

- Renaming attributes might be require if attribute names differ on both relations

- Notation: $r \cup s$

**Intersection ($\cap$)** 

- Outputs tuples that are present only in both relations

- Notation: $r \cap s$

**Difference ($-$)**

- Finds tuples that are present in one relation *but not* in the other

- Notation: $r - s$



#### Division ($/$)

> Just think of integer division, but for relations

- Prerequisite for relations $r$ and $s$: The attributes of $s$ are found in $r$ 

- Notation: $T = r / s$ 

  - $T$ is the largest possible set such that $(s \cross T) \subseteq R$
  - If tuple $x \in T$ and tuple $y \in S$, then the tuple $x || y \in R$ (concatenation)


### Non-Set Extensions

#### Bag Semantics

- Relations are bags (multi-sets)

**Set semantics for Bags**

- Union: Unordered concatenation

- Intersection: Minimum count of each value in left bag and right bag

- Difference: Takes the difference between the occurrences in the right bag and the left

- Union and intersection no longer distribute

#### Duplicate Elimination ($\delta$)

- Turns a bag into a set

- Notation: $T = \delta(R)$

#### Aggregation

- `SUM, AVG, MIN, MAX, COUNT`

- Includes the duplicates

#### Grouping ($\gamma$) and Aggregating

- Group tuples based on a similar/related key

- **Grouping key**: subset of attributes to test equality

- Notation: $\gamma_{A, B, C, f(X), g(Y), h(Z)}(R)$
  - Grouping keys: $A, B, C$
  
  - Attributes: $X, Y, Z$
  
  - Commutative aggregate Functions: $f, g, h$

#### Extended Projection ($\pi_L$)

- Notation: $\pi_L(R)$

- $L$ contains any of the following:

  - One attribute of $R$

  - Rename of attributes expression $x \rightarrow y$

  - Rename expression $E \rightarrow z$

    - $E$: expression involving attributes of $R$, constants, arithmetic & string operators
    - $z$: new name of the resulting attribute (return value of $E$)

    
#### Sorting ($\tau$)

- Sorts tuples in a relation based on its attributes

- Notation: $\tau_L(R)$:

	- $L$: List of tuples, sort is based on the first tuple, fall-back to the next if tie
	- Default is ascending order, $-$ is descending


### Constraints

> Suppose $R$ and $S$ are expressions in Relational Algebra

Notation:

- **Empty Equals**: $R = \empty$ 
	- Equivalently, $R \subseteq \empty$ 
	
- **Subset**: $R \subseteq S$
	- Equivalently, $R - S = \empty$

**Example**:

Schema:

```sql
-- Primary Key: certificateID
MovieExecutive (
	name: string,
    address: string,
    certificateID: int,
    netWorth: int
)

-- Primary Key: name
-- Foreign Key: presidentCertificateID
Studio (
	name: string,
    address: string,
    presidentCertificateID: int
)
```

Problem:

```
To be a president of a studio, you must have a net worth of at least $10 000 000
```

Constraint:

- $\sigma_{\text{netWorth} \lt 10,000,000}(\text{Studio}\bowtie_{\text{presidentCertificateID } = \text{ certificateID}} \textrm{MovieExecutive}) = \empty$

  - The relation of selecting all tuples from the **theta-joined** relation where `netWorth` is strictly less than 10 Million must be empty

- Alternatively, $\pi_{\textrm{presidentCertificateID}}(\textrm{Studio}) \subseteq \pi_{\text{certificateID}}(\sigma_{\text{netWorth} \ge 10,000,000}(\textrm{MovieExecutive}))$

  - All of the `presidentCertificateID`s found in `Studio`, must be found in all of the `certificateID`s of all `MovieExecutive`s that have a `netWorth` of greater than or equal to 10 Million

#### Limitations of Relational Algebra

- RA is set based

- Expensive to eliminate duplicates, order output

- Need a way to apply scalar expressions to values

- Values/Attributes that are *not* there, can be more important than what *is* there

---

## Structured Query Language (SQL)
> Also known as, "The easiest part of this course"

- Non procedural; One query always returns one table
- **Not** a Turing machine equivalent language
- Usually embedded in other languages (Java, JS, Ruby)



### Data Manipulation Language (DML)

> Query Language (creating, reading, updating, deleting data)

#### General Query Syntax

```sql
SELECT <DISTINCT?> <attribute> AS <renamed_attribute>
FROM <tables | subquery>
WHERE <predicate on tuples>
GROUP BY <attributes> 
HAVING <predicate on groups>
ORDER BY <attributes> <ASC | DESC>;

-- Set Operations
(<query_1>)
<UNION | INTERSECT | EXCEPT>
(<query_2>)
```


#### Example syntax for common queries

- Joining two tables
```sql
SELECT *
FROM table_1 
<NATURAL | CROSS | INNER | OUTER | LEFT | RIGHT> JOIN table_2
ON table_1.attr1 = table_2.attr2;

-- Cartesian Product
SELECT * FROM table_1, table_2;
```

-  Getting the count of an attribute
```sql
SELECT attr, COUNT(attr) AS attr_count
FROM my_table
GROUP BY attr; -- GROUP BY must be included
```

- Inserting into another table
```sql
INSERT INTO my_table (
-- Query goes here
);
```

- Updating one tuple
```sql
UPDATE my_table 
SET <attribute> = <value>
WHERE <predicate on tuples>;
```

- Deleting
```sql
DELETE FROM my_table (
-- Query goes here
);
```

- Creating virtual views
```sql
CREATE VIEW my_view (
-- Query goes here
);

DROP VIEW my_view; -- Important!
```



#### Transactions

> Collection of one (or more) **atomic** and **serializable** operation(s)

- Each SQL statement is a transaction itself

- Can group several statements together into a single transaction


##### Isolation Levels

- **Dirty Read**: reads values written by another transaction that hasn’t committed yet

- **Non Repeatable Read**: reads the same object twice, but finds a different value on the second time even though the transaction didn't change it

- **Phantom Read**: transaction re-executes a query, finds that the set of rows returned has been changed based on another committed transaction



##### General Syntax

```SQL
SET TRANSACTION ISOLATION LEVEL <isolation_level>;
```

| `<isolation_level>` | Dirty Reads  | Non Repeatable Reads | Phantom Reads |
| ------------------- | ------------ | -------------------- | ------------- |
| `READ UNCOMMITTED`  | Possible     | Possible             | Possible      |
| `READ COMMITTED `   | Not Possible | Possible             | Possible      |
| `REPEATED READ`     | Not Possible | Not Possible         | Possible      |
| `SERIALIZABLE`      | Not Possible | Not Possible         | Not Possible  |




### Data Definition Language (DDL)

> Notation for defining a database schema



#### Schema Syntax

> Basically namespaces.

```sql
CREATE SCHEMA University;

-- delete schema
DROP SCHEMA IF EXISTS University CASCADE;

-- do this if you only use one schema
SET search_path to University

-- namespaced tables
CREATE TABLE University.professors;
```



#### Built-in Types

```sql
CHAR(n)     -- length = n
VARCHAR(n)	-- length <= n
INT			
SMALLINT	-- Smaller subset of Integer
FLOAT		
BOOLEAN		-- TRUE / FALSE
DATE		-- 'YYYY-MM-DD'
TIME		-- 'HH:MM:SS'
TIMESTAMP	-- DATE & TIME
```



#### Table Syntax

```SQL
CREATE TABLE my_table (
	<attribute_1>	<type>,
	<attribute_2>	<type>,
    -- ...
    primary key (attr),
    foreign key (other_table_key) references other_table
);

-- delete table
DROP TABLE my_table <CASCADE?>; 
-- cascade only if other tables reference it

-- update table
ALTER TABLE my_table
	ADD COLUMN <attribute> <type>;
	
ALTER TABLE my_table
	DROP COLUMN <attribute>;
```



---



## Database Design Theory

> Domain Knowledge also influences theory



### Definitions and Concepts


- **Normalization**: Eliminating redundancies in a given database schema by breaking down the relation

- **Functional Dependency**: If two tuples have the same values for attributes $A_1, \dots, A_n$ then they must also have the same values for attributes $B_1, \dots, B_m$ 
  - Syntax: $X \to Y$ ($X$ functionally depends on $Y$)

- **Multivalued Dependency**: If you fix the values for one set of attributes $A$, then the values in other attributes $B$ are independent from the values of all other attributes in the relation
	- Syntax: $A \to\to B$ 
	
- **Key**: Candidate Key ($K$)
	- FD Definition: $K \to R$, where $R$ is the entire relation 
	- Also known as the *Minimal* Super Key
	- One candidate key becomes the Primary Key

- **Prime Attributes**: Attributes that form a Candidate Key



### Functional Dependency Rules

#### Trivial FDs

> Practically meaningless and do not impose anything on the relation

```haskell
-- identity
x -> x 

-- same attribute on both sides
abc -> a 
```



#### Splitting Rule

> Attributes on the RHS are **independent** of each other 

```haskell
-- original
abc -> def

-- refactored (singleton)
abc -> d
abc -> e
abc -> f
```



#### Combining Rule

> Useful to combine the RHS and identify candidate keys

````haskell
-- original
xy -> ab
yz -> cd

-- refactored
xyz -> abcd
````



#### Transitive Rule

> Can identify hidden FDs, useful for finding cycles

```haskell
-- given
x -> y
y -> z

-- implied by rule
x -> z
```



### Multivalued Dependency Rules

- Splitting/combining not allowed

#### Trivial MVDs

> Practically meaningless and do not impose anything on the relation

```haskell
a1 a2 a3 ->-> b1 b2 b3

-- trivial if {b1, b2, ...} subset {a1, a2, ...}
```

#### Transitive Rule

> Similar to FD rule

```haskell
-- given
x ->-> y
y ->-> z

-- implied by rule
x ->-> z
```

#### FD Promotion Rule

> All FDs are MVDs

```haskell
-- given
x -> y

-- implied by rule
x ->-> y
```

#### Compliment Rule

> $X \to\to A$ implies $X \to\to (R - A)$

```haskell
-- given
R = (a, b, c)

a ->-> b

-- implied by rule
a ->-> c
```



#### Suprising Rule

> For a given MVD, and **_any_** FD whose RHS is a subset of the RHS of the MVD

```haskell
-- given 
R = (a, b, c, d)

a ->-> bc
d -> c  -- c subset of bc

-- implied by rule
a -> c
```





### Closures 

> Approach to find all trivial and non-trivial FDs for a given relation ($R$) and FD set ($S$).

#### Construction Algorithm

1. Split all FDs in $S$ into singletons, and find any implied FDs using transitivity

2. List out all combinations of attributes in $R$ 
   - Example:  If $R = (A, B, C)$ then $X = \{A, B, C, AB, AC, BC, ABC\}$)

3. For each combination, list out all other attributes you can reach using the given FDs
   - Example: If $S = (AB \to C)$ then $\{AB\}^+ = \{A, B, C\}$

4. Remove all rows that are trivial (LHS = RHS)



#### Example Table

- $R = (A, B, C, D, E)$
- $S = (AB \to C, A \to D, D \to E, AC \to B)$

| $X$  | $X_s^+$             |
| ---- | ------------------- |
| $A$  | $\{A, D, E\}$       |
| $AB$ | $\{A, B, C, D, E\}$ |
| $AC$ | $\{A, B, C, D, E\}$ |
| $D$  | $\{D, E\}$          |



### Minimal Basis

> The complete opposite of a closure

- **Definition**: FD set $S' \subseteq S$ is a minimal basis if $\forall X \in S, S'$ entails (can reach) $X$ 
- *Not* necessarily unique for a given FD set ($S$)



#### Construction Algorithm

1. Split all FDs in $S$ into singletons, and find any implied FDs using transitivity ($S'$).

2. $\forall$ Functional Dependency $X \in S'$, manually test if $(S' - X)^+ \equiv S^+$
	- i.e. Remove all redundant FDs that are either trivial or implied

3. $\forall X \in S'$, for attributes $i \in \textrm{LHS}(X)$, let  $\textrm{LHS}(X') = \textrm{LHS}(X') - i$  
   - Manually test if $(S' - X + X')^+ \equiv S^+$ 
   - i.e. Remove any redundant attributes in each FD

4. Repeat with $S'$ as the new $S$ until minimal



### FD Projection

> Used to refactor FDs when splitting a relation

- Given a relation $R$ with FD set $S$, decompose $R$ into $R_1$ and $R_2$ with their own FD sets $S_1$ and $S_2$

- Each FD sets must only include attributes from its respective relation

- Many possible projections


#### Projection Algorithm

For $R_1$:

1. Set $S_1 = \empty$
2. Compute the closure of each combination of attributes of $R_1$, and add to $S_1$ all non-trivial FDs $X \to A$ such that the RHS attribute $A \in X^+$ and $A \in R_1$
3. Construct a minimal basis from $S_1$

Repeat for all decomposed relations ($R_2, \dots$)

##### Improvements

- Ignore Trivial dependencies

- Ignore Trivial subsets ($\empty$ or $X$)

- If $X$ is a set of attributes such that $X^+ = R$ ($X$ is a super key), then ignore any other sets that contain $X$




### Chase Test
> Used to infer FDs and MVDs

#### For Functional Dependencies 

> For original relation $R$, decomposed relations $R_1, R_2$, and FD set $S_F$

1. Construct a tableau using the decomposed relations

2. Apply FDs from $S_F$ onto the rows of the tableau
   - i.e. if a row has $a, b_1, c_1, d$ and another row has $a, b_2, c, d_1$ with FD $A \to B$
   - Then $b_1 = b_2$ in this case

3. Repeat until you end up with a row without subscripts
   - i.e. if $R = (A, B, C, D)$, you want a row on the tableau that looks like $a, b, c, d$

**Note**: If you just want to see if a particular FD holds, you can also just compute the closure of the LHS and verify, rather than chasing

#### For Multivalued Dependencies 

> Same relation and FD set, use $S_M$ as the MVD set

1. Construct a tableau

2. Apply FDs the same way

3. For MVDs in $S_M$, create new rows (if necessary) 
	- i.e. if $A \to\to B$ is in $S_M$ and R = (A, B, C)
	- Then must have rows with $a, b_1, ...$ and $a, b, ...$
	- `...` must be the same in both rows (want $b$ to be independant of $a$)

4. Repeat until you end with a row without subscripts




### Normal Forms

> What it means to have a ‘good’ schema

#### 1st Normal Form (1NF)

- No multi valued attributes (no lists, arrays)

- No duplicate rows

- Not expressible in RA

#### 2nd Normal Form (2NF)

- Must satisfy 1NF

- No partial dependencies 

- i.e. For all non-prime attributes $a$, $\exist$ FD $X \to a$ such that $X$ must be a candidate key

#### 3rd Normal Form (3NF)

- Must satisfy 2NF

- No Transitive dependencies (LHS must be a prime attribute or a super key)

##### Decomposition Steps

1. For the FD set $S$, find the minimal basis $M$
2. For all FDs $X \to A$ in $M$, create a new relation $R_i = (X, A)$
3. If there isn't a relation whose attributes $K = \{k_1, ..., k_m\}$ form a super key for $R$, create a new relation $R_k = (k_1, ..., k_m)$
4. Verify each $R_i$ is in 3NF, if not then decompose further

#### Boyce-Codd Normal Form (BCNF)

- Must satisfy 3NF

- No non-trivial FDs

- i.e. if $X \to Y$ is a non-trivial FD, then $X$ must be a super key

##### Decomposition Steps

1. For all FDs $X \to A$ where $R = A \cup B$, set $R_1 = (X, A)$ and $R_2 = (X, B)$
2. Project the remaining FDs onto $R_1$ and $R_2$
3. Verify $R_1$ and $R_2$ are in BCNF, if not then decompose further


#### 4th Normal Form (4NF)

- Must satisfy BCNF

- No non-trivial MVDs

- i.e if $X \to\to Y$ is a non-trivial MVD, then $X$ must be a super key

##### Decomposition Steps
1. For all 4NF violations (non-trivial MVDs) $X \to A$, create two new relations $R_1 = (X, A)$ and $R_2 = (X, R - A)$
2. Find (project) all FDs and MVDs from $R$ onto $R_1$ and $R_2$
3. Verify $R_1$ and $R_2$ are in 4NF, if not then decompose further

---



## eXtensible Markup Language (XML)

> Basically if HTML and JSON had a child

- **Data-only format**: No implied representation

- Heirarchical, Tree like data structure

- Ordering is implied

- Flexible schema

- Syntax similar but *stricter* than HTML
	- Tags must always be closed
	- Tag and Attribute names have no meaning semantically

### Example Syntax (Well-Formed XML)

```xml
<? xml version = "1.0" encoding = "utf-8" standalone = "yes" ?> 
<StarMovieData> 
    <Star starID = "cf" starredIn = "sw"> 
        <Name>Carrie Fisher</Name> 
        <Address> 
            <Street>123 Maple St.</Street> 
            <City>Hollywood</City> 
        </Address> 
        <Address> 
            <Street>5 Locust Ln.</Street> 
            <City>Malibu</City> 
        </Address> 
    </Star> 
    <Star starID = "mh" starredIn = "sw"> 
        <Name>Mark Hamill</Name> 
        <Address>
        	<Street>456 Oak Rd.</Street> 
        	<City>Brentwood</City> 
        </Address>
    </Star> 
    <Movie movieID = "sw" starsOf = "cf", "mh">  
        <Title>Star Wars</Title> 
        <Year>1977</Year> 
    </Movie> 
</StarMovieData>
```

- **Root Element**: `StarMovieData`
	- There must only be one Root for it to be Well-Formed

- **Sub Elements**: `Star, Name, Address, ...`

- **Attributes**: `starID, movieID, starredIn, starsOf, ...`

