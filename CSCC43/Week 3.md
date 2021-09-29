---
title: Week 3
weight: 3
---

# Week 3

#### Limitations of Relational Algebra

- RA is set based

- Expensive to eliminate duplicates, order output

- Need a way to apply scalar expressions to values

- Values/Attributes that are *not* there, can be more important than what *is* there

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



#### Outer Join (⟗)

- Extends the Inner Join by padding the relation with NULL ($\bot$) values

- Notation: $T = R ⟗ S$

- Padding Type:

  - LEFT (⟕): Pads tuples in the left relation with NULL if there is no matching tuple in the right relation
  - RIGHT (⟖): Reverse of left join (pads right)
  - FULL (⟗): Pads both

