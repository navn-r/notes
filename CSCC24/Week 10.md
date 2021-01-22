---
title: Week 10
weight: 10
---

# Week 10

### Short Circuiting

> Defined in most if not all programing languages

Consider the expression

```python
if x > 0 and 69 / x < 1:
    print('hoe')
```

- If `x` happened to be 0, evaluating `69 / x` only would result in a division by 0 error
- The second comparison would only be evaluated *if the first was not false*
  - i.e. if `x` was non-positive, then the rest wouldn't be evaluated, acts as a 'guard'



## Laziness

> Only evaluate things when you need to

Haskell is a **lazy** language, where everything is evaluated 'at the very last second'.

Consider this Java code

```java
public void m(T x, T y) {
 // ...
}
// ...
public static void main(String[] boostMyMark) {
    System.out.println(m(functionThatReturnsX(), functionThatReturnsY()));
}
```

When `m` is called, both functions that are the arguments are evaluated, this is **Eager Evaluation**.

Now consider this Haskell code

```haskell
and' :: Bool -> Bool -> Bool
and' False _ = False
and _ x = x

and' False (45 / 0 == -1)
```

Due to the pattern matching in the definition of `and'`, the second param will not be evaluated *whatsoever*, as in that pattern, its a do not care value.

##### Recursive Values with Lazy Evaluation

```haskell
nats = 0 : map (+1) nats
--- [1, 2, 3, 4, ...]

GHCI> length nats
--- This will never be evaluated, since this is infinite

GHCI> take 10 nats
[0,1,2,3,4,5,6,7,8,9]
--- Works! Since 10 is finite

--- fibonacci numbers with list comprehension
--- infinite definition, linear complexity
fib = 0 : 1 : [x + y | (x, y) <- zip fib (tail fib)]
```

