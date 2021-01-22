---
title: Week 6
weight: 6
---

# Week 6

## Pure Functional Languages

- **Programs**: collections of *functions*
- **Execution**: view as *evaluation*



### Referential Transparency

> The value of applying a function is *independent* of its context

Consider the program

```sh
# Top part of program
f(a, b, c) # well-formed expression
# Bottom part of program
```

A program exhibits **Referential Transparency**, if it behaves the exact same, when the expression `f(a, b, c)` is replaced with its value.

The expression **_does not depend_** on the global state of computation. i.e. only on `f`, `a`, `b`,  and `c`.   



### No Assignment Statements

> Variables retain their acquired value until the end of the evaluation

In an imperative language...

```C
int x = 42;
x = x + 1; 
// goes to the locker that stores x
// increments the value
// and overwrites the locker
```

In a functional language...

```scheme
(define x 42)
(define x (+ x 1))
; just an association
; points to a different locker
```



### No Side Effects

> Functions do not change things outside of its own scope

```python
lst = [1, 2, 3]

# no side effects
def good-sum(lst, x):
    return sum(lst) + x

# value of lst outside of bad-sun's scope is changed
def bad-sum(lst, x):
    lst.append(x) 
    return sum(lst)
# lst is now [1, 2, 3, x]
```



### Closures

> A combination of a function, and the **lexical environment** within which the function was declared

Consider this closure from a functional PL

```js
const addThree = (x, y) => x + y + z
// 1. get values x, y from the lexical scope (parameters)
// 2. get z from the global scope
// 3. return the sum of x, y, z
```

- **Lexical Environment**: reference to its surrounding state

- **Lexical Scope**: uses the location of *where* the variable has been declared in the source code, in order to evaluate it
  - i.e. `x` and `y` are declared when calling `addThree`
  
- Functions have access to variables declared in *their outer scope*
  - i.e. the outer scope of `addThree` is the global scope, which is where it will find `z`

Only upon evaluation time, will the value of `z` be determined, meaning `addThree` basically is a recipe of steps to follow every time it is called.

