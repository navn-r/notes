---
title: Week 9
weight: 9
---



# Week 9

### Infix Operators

> Any curried function that takes 2 parameters can be converted, and vice versa

```haskell
x = 3 + 2 -- infix
x = (+) 3 2 -- function

y = elem 3 [1,2,3]
y = 3 `elem` [1,2,3] -- infix
```



 ### Type Synonyms

> Giving existing types an alias or new name

```haskell
type NewType = OldType
```

- Useful for readability (`String` instead of `[Char]`)



## User Defined Data Types

General Syntax:

```haskell
data NewType = Constructor1 Type1 | ... | ConstructorN TypeN
```

- `|`: represents union of the constructor and types

  ```typescript
  export Type NewType = Type1 | ... | TypeN 
  ```

- `TypeI`: previously defined types

- `ConstructorI`: creates a value of `NewType` type

- `Type` can be omitted if the constructor does not need any argument (constants)

#### Enumerated Types

```haskell
data NumberType = Odd | Even
--- only constants

--- pattern matching on constructors
mod2 :: Num a => NumberType -> a
mod2 Odd = 1
mod2 Even = 2
```

#### Variant/Union Types

```haskell
data Text = Letter Char | Word [Char]
--- either a single Letter or multiple letters (word)

textLen :: Text -> Int
textLen (Letter _) = 1
textLen (Word w) = length w
```



#### Mutually Recursive Types

```haskell
--- parametric polymorphism here
data Tree a = Leaf a | Node a (Tree a) (Tree a)
--- Tree is a type constructor
--- Node is a curried value constructor

countNodes:: Tree a -> Int
countNodes (Leaf _) = 1
countNodes (Node _ left right) = countNodes left + countNodes right + 1
--- the structure of the function is the same as the data type
--- i.e the recursive pattern is the same on both
```

