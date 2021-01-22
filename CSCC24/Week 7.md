---
title: Week 7
weight: 7
---

# Week 7

## Type Systems

- **Type**: Name of a set of values and operations which can be performed on the set
  - Alternate: Collection of computational entities that share some common property
  - What is or *is not* a type, is language dependent
  
- A PL is **Type Safe** if it won't execute a function if it's not applicable to the arguments

- **Type Checking**: 
  - **Static**: at compile-time
  - **Dynamic**: at run-time

### Motivation

- Catch *all* errors at compile-time (impossible, even in theory)
	- Solution: Guarantee to catch only a certain class of errors
- Type safety without explicit declaration

### Benefits of Type Systems

- Easier to debug

- Static analysis - information obtained at compile-time

- Can compile quicker/more optimized code (save computation, limit memory)

- Can be used to prove correctness

- Self documenting code

### Type Notations (`Haskell`)

- `Int`: integers

- `Integer`: unbounded integers

- `Float, Double`: floating point numbers; single and double precision

- `Char`: characters

- `Bool`: boolean values

- `TypeA -> TypeB`: function that takes in `TypeA` and returns `TypeB`
  - Every function accepts ***exactly*** one argument (can be a tuple)

- `()`: 'unit' - empty tuple

- `(Type1, Type2, ..., TypeN)`: tuples of `Type`s (heterogeneous)

- `[Type]`: list whose elements *are all* `Type`


#### Parametric Polymorphism (Type Variables)

> When the type cannot be inferred

Consider the `Haskell` function

```haskell
prompt> func (x, y, z) = if x then y else z
prompt> :t func
func :: (Bool, p, p) -> p
```

- `p` is a **Type Variable** - any valid type, `func` is a **Polymorphic** function

- $\alpha \to \alpha$: for every valid type $\alpha$
  - i.e. when `y, z` is of type `Int`, $\alpha$ is instantiated to `Int`

Example: 
```haskell
prompt> swap (x, y) = (y, x)
prompt> :t swap
swap :: (t, t1) -> (t1, t)
```

- $(\alpha, \beta) \to (\beta, \alpha)$: for valid types $\alpha, \beta$

<br />

## Type Checking

> The process of verifying + enforcing type constraints


### Dynamic Type Checking
> Performed at run-time

- Slower execution

- More flexible/freedom: easier to re-factor


### Static Type Checking
>  Performed at compile-time

- Faster execution

- **_Arguably_** safer and more elegant/modular programs

- More compiler optimization

- Programmers may end up writing *worse* code to get around static type checkers
  ```ts
  const o: Type = { foo: 42 };
  (o as any).bar = baz;
  ```

#### Explicit Static Typing
> Declaring type annotations *in the code*

```java
// explicitly declaring main to be String[] -> void
public static void main(String[] boostMyMark) {
	int x; // declaring x to be of primitive int
}
```

#### Type Inference
> Deduce types without explicitly declaring them

```scheme
(define (func x) (if (empty? x) 0 (length x)))
; can deduce that x is a pair, and return value is an int
```