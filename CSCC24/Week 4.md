---
title: Week 4
weight: 4
---

# Week 4

## Local Bindings

> Creating variables with a local scope, and bind them to the result of expressions

- **Scope**: Visibility of variables  

### `let`

Usage:

```scheme
(let ([var1 expr1] ... [varn exprn]) body)
```

- `expr1, ..., exprn` are evaluated in an **undefined** order, has the appearance of running in parallel

- The scope of `var1, ..., varn` is `body` 

- **Note**: `vari` is not in the scope of  `[varj, exprj]` when `i != j`, due to the parallel nature

Example:

```scheme
(define double-triple (x) 
  (let ([double (* 2 x)]
        [triple (* 3 x)])
    (list double triple)))
```

### `let*`

- Same usage as `let`

- `expr1, ..., exprn` are evaluated **sequentially** left to right

- The scope of `vari` is everything to the right of `vari` 

Example:

```scheme
(define double-triple (x) 
  (let* ([double (* 2 x)]
        [triple (+ x double)])
    (list double triple)))
```

### `letrec`

- Same usage and order of evaluation as `let`

- The scope of `vari` is *the entire* `letrec`

Example:

```scheme
;; Tail Recursion - allows var to be called in expr
(define len-of-list (lst)
  (letrec ([length-tail (lambda (lst len) (if (empty? lst) 
                              len 
                              (length-tail (rest lst) (+ len 1))))])
    (length-tail lst 0)))

;; another recursive example
(letrec 
  ([my-even? (lambda (x) (if (eq? x 0) #t (my-odd? (- x 1))))]
  [my-odd? (lambda (x) (if (eq? x 0) #f (my-even? (- x 1))))])
  ; allows my-even? to be in scope when called in my-odd?
  ; and vice versa
  (if (my-even? 4) 1 0))
```
