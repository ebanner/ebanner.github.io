---
layout: default
title:  "extipy"
description: Commands to attach to a jupyter kernel
---

# extipy

Insert a breakpoint

```python
import IPython
IPython.embed_kernel(local_ns={**locals(), **globals()})
```

Attach jupyter lab to the kernel

```
jupyter lab --KernelProvisionerFactory.default_provisioner_name=extipy-provisioner
```
