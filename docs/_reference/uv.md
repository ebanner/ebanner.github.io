---
layout: default
title:  "uv"
description: uv commands
---

# uv

## Start a new project

```
uv init
```

## Install a package (this creates a `.venv`)

```
uv add pandas
```

## Add one-off dependency

```
uv add --dev pandas
```

## Run one off with a library

```
uv run --with pandas main.py
```
