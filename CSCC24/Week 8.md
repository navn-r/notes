---
title: Week 8
weight: 8
---

# Week 8

## Pattern Matching

> Input parameters for a function are matched from top down

```haskell
len :: [a] -> Integer
len [] = 0  -- if the input is null, returns 0
len (_ : xs) = 1 + len xs
```

The `_` represents a 'do not care' value, as its not bound to a variable

```ts
this.handle((_, event) => ...);
```

#### Value Matching in Haskell

```haskell
reverse xs = rev xs [] where
	rev [] rs = rs
	rev (y : ys) rs = rev ys (y : rs)
	
abs x = if x >= 0 then x else =x
abs x 
	| x >= 0 = x
	| otherwise = -x
```



## Type Classes

> Offer a controlled approach to overloading

- Type Classes basically have lots of types, where each type has implementations of those defined in the class

- Predifined type classes in Haskell are `Eq`, `Ord`, `Show`, etc.

Consider the type for the number `69`

```haskell
69 :: Num a -> a
--- can be Int, Integer, Rational ...
```

This means:

- `Num` is the type class

- `Num a` is a type class constraint

- `69` is of type `a`, where `a` is a type that belongs to the `Num` type class

#### Type Classes in a function

```haskell
member :: Eq t => (t, [t]) -> Bool
--- type t belongs to the Eq class
--- instances of type t have the (==) function
member (y, (x:xs)) = y == x || member (y, xs)
```



#### Defining Type Classes

```haskell
class TypeClass a where
	classFunction :: a -> a --- could be any function
	--- optional: provide a default definition
	
--- Example
class Eq a where
	(==), (/=) :: a -> a -> Bool
	x == y = not x /= y
	x /= y = not x == y
```

- If possible, default definitions should be **circular**
  - When making an instance of that class, only one needs to be implemented and the other is free



## Currying

> Converting a multi-argument function into a sequence of evaluated functions, each with a single argument

Alternative meaning: The action of cooking a traditional spicy dish of Indian and Oriental origin

- In Haskell, each function takes **_exactly one argument_**

Consider multiple ways of writing this function

```haskell
sum :: Num a => (a, a) -> a
sum (x, y) = x + y 

-- curried function
sum' :: Num a => a -> a -> a
--- Num a => a -> (a -> a)
--- -> is right associative
sum' x y = x + y
sum' x = \y -> x + y
sum' = \x -> \y -> x + y

:t (sum' x)
(sum' x) :: Num a => a -> a
--- this is just a function
```

