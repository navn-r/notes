---
title: Week 2
weight: 2
---

# Week 2

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



### Constraints

> Suppose $R$ and $S$ are expressions in Relational Algebra.

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

