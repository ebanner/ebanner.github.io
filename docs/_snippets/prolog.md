---
layout: default
title:  "prolog"
description: Prolog code snippets
---

# Prolog

Total element equality

```prolog
?- maplist(=(E), [a, a, a]).
E = a.
```

Unnamed variable

```prolog
?- maplist(=(_), [a, a, a]).
true.
```

With strings

```prolog
?- maplist(=(_), ["a", "a", "a"]).
true.
```

