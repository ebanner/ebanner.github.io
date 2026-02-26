---
layout: default
title:  "racket"
description: Racket scaffolding
---

# Racket

Macros

```racket
(define-syntax-rule (≠ x y) (not (= x y)))
```

Utility functions

```racket
(define (in-pairs xs)
  (make-do-sequence
   (λ ()
     (define (pos->vals s)
       (define t (car s))
       (cond
         [(and (pair? t) (pair? (cdr t)) (null? (cddr t)))
          (values (car t) (cadr t))]
         [else
          (error 'in-pairs "expected 2-element list, got ~v" t)]))
     (values pos->vals cdr xs pair? #f #f))))

(define (safe-substring s start [end (string-length s)])
  (define N (string-length s))
  (define safe-end (min end N))
  (substring s start safe-end))
```
