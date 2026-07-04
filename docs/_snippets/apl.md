---
layout: default
title:  "apl"
description: APL code snippets
---

# APL

Under `⍢`

```apl
_U_ ← {⍵⍵⍣¯1⊢ ⍺⍺ ⍵⍵ ⍵}

_U_ ← {⍺←{⍵ ⋄ ⍺⍺} ⋄ ⍵⍵⍣¯1⊢(⍵⍵ ⍺)⍺⍺(⍵⍵ ⍵)}
```

Use zero-indexing

```apl
⎕IO ← 0
```

Output visualization

```apl
]box on -style=max

]boxing off
```
