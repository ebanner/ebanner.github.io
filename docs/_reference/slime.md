---
layout: default
title:  "slime"
description: SLIME keystrokes and functions
---

# SLIME

Start common lisp mode

```emacs-lisp
SPC SPC common-lisp-mode
```

Start SLIME repl

```emacs-lisp
SPC m s i ; slime
```

Boilerplate

```emacs-lisp
(defpackage :my-playground
  (:use :cl))

(in-package :my-playground)
```

Eval last sexp

```emacs-lisp
C-x C-e ; slime-eval-last-expression
```

Eval expression in repl

```emacs-lisp
SPC m s e ; slime-eval-last-expression-in-repl
```

Describe

```emacs-lisp
C-c C-d d ; slime-describe-symbol

C-c C-d f ; slime-describe-function
```
