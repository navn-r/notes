---
title: Week 3
weight: 3
---

# Week 3

## Higher-Order Procedures

> Procedures as returned values.

All modern functional PLs manipulates functions as values.

```scheme
; f, g are functions and (f (g)) is well-defined
(define (compose f g) (lambda x (f (g x))))
; returns the composed function
```

### `map`

Usage:

```scheme
(map proc l1 l2 ... ln)
```

- `proc`: n-ary procedure (expects n arguments)

- `l1, ..., ln`: lists of length `m`

- Returns `(e1 ... em)` where `ei = proc(l1[i], l2[i], ... ln[i])` 

Example:

```scheme
(map (lambda (x y) (+ x y)) '(1 2 3) '(6 5 4))
; 	==> '(7 7 7)
```



### `fold`

Usage:

```scheme
(foldr op id lst)
(foldl op id lst)
```

- `op`: binary procedure

- `lst`: list of arguments

- `id`: identity element; always used

- Returns result of applying `op` to elements in `lst`
  - `foldr`: right-associatively 
  - `foldl`: left-associatively

Examples:

```scheme
(foldr op id '(e1 ... en))
;	==> (op e1 (op e2 (op ... (op en id))))
; where (op en id) = id

(foldr + 0 '(1 2 3))
(+ 1 (foldr + 0 '(2 3)))
(+ 1 (+ 2 (foldr + 0 '(3))))
(+ 1 (+ 2 (+ 3 (foldr + 0 '()))))
(+ 1 (+ 2 (+ 3 0)))
(+ 1 (+ 2 3))
(+ 1 5)
6

; (append xs ys)
(define (fappend xs ys)
  (foldr cons ys xs))

; (map f xs)
(define (fmap f xs)
  (foldr (lambda (x r) (cons (f x) r)) '() xs))
```

