---
layout: default
title:  "vim"
description: Useful ex commands in vim
---

# vim

Match whitespace at end of lines

```
/\v\s+$
```

Delete all lines matching a patten

```
:g//d
```

Delete all lines NOT matching a patten

```
:g!//d
```

Replace white space with newlines

```
:s//\r/g
```

Non-whitespace character

```
\S
```

