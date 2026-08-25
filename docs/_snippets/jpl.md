---
layout: default
title:  "jpl"
description: JPL code snippets
---

# JPL

Fixed point scan

```apl
{⍵÷2} ⍣ ⍬ ⊢ 55
```

Power scan with condition

```apl
{⍵÷2} ⍣ {⍵≥1} ⍣ ⍬ ⊢ 10
```

Amend

```apl
1 (<1 1) ⊙ 3 3⍴0
```

Stencil

```apl
2 2 < ⌺ ¯3 ⊢ 3 3⍴0
```

Counts

```apl
(↑,≢) ⌸⍨ 1 2 1 2 1
```
