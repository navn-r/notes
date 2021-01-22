---
title: Week 5
weight: 5
---

# Week 5

## Tail Recursion

Consider the following function

```scheme
(define (len xs) 
  (if (empty? xs) 
      0 
      (+ 1 (len (rest xs)))))
; trace
(len '(1 2))
(+ 1 (len '(2)))
(+ 1 (+ 1 (len '())))
(+ 1 (+ 1 0))
(+ 1 1)
2
```

The space complexity for this is precisely $O(n)$, where $n$ is the length of `xs`.

For a small enough stack space, and large enough list, this function can result in a [**Stack Overflow**](https://navn.me/notes/CSCA48/final-review/#the-memory-model). To fix this issue, we must implement this function using tail recursion.

```scheme
(define (len xs) 
  (local [(define (len* xs l) ; accumulator
            (if (empty? xs) 
                l 
                (len* (rest xs) (+ 1 l))))]
         (len* xs 0)))
; trace
(len '(1 2))
(len* '(1 2) 0)
(len* '(2) 1)
(len* '() 2)
2
```



#### Tail-Call Optimization

- A language can implement Tail-Call Optimization, where no stack is required  

- Some languages (Scheme) require tail-recursion to be implemented, some don't (Python)



### Types of Recursion

- **Linear Recursion**: At most one recursive call occuring in the body of the function  

- **Tail Recursion**: Recusive call is the last evaluated step in the body of the function  

- **Flat Recursion**: Only recurse on the top level of the list  

- **Deep (or Tree) Recursion**: Recurses on all items in the list (sublists)  
  
- **Mutual Recursion**: Pair of functions, that call each other, not themselves



## Continuation-Passing Style (CPS)

> Every *well-formed* subexpression has a **continuation**; what needs to be done to complete the expression



Consider the following expression

```scheme
(* 2 (+ 4 5))
```

- The continuation of `(+ 4 5)` would be `(+ 2 [])` or in Scheme

  ```scheme
  (lambda (v) (+ 2 v))
  ```

- Every *well-formed* sub-expression in the evaluation corresponds to a point **_in the evaluation of the program_**, and v.v



Rewriting the above `len` function using CPS

```scheme
(define (len xs) 
  (local [define (len* xs k) ; k -> continuation function
           (if (empty? xs) 
               (k 0) 
               (len* (rest xs) (lambda (v) (k (+ 1 v)))))]
         (len* xs identity)))
; trace
(len '(1 2))
(len* '(1 2) identity) ; identity = (lambda (v) v)
(len* '(2) (lambda (v1) (identity (+ 1 v1))))
(len* '() (lambda (v2) ((lambda (v1) (identity (+ 1 v1))) (+ 1 v2))))
2

; computed all in one-step
((lambda (v2) ((lambda (v1) (identity (+ 1 v1))) (+ 1 v2))) 0) ; v2 = 0
((lambda (v1) (identity (+ 1 v1))) 1) ; v1 = 1
(identity 2)
2
```

