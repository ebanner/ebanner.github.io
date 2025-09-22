---
layout: default
title:  "jupyter"
description: Commonly used jupyter commands and file locations
---

## [Kernelspecs](https://jupyter-client.readthedocs.io/en/stable/kernels.html)

```
~/Library/Jupyter/kernels
```

## [Add a kernel](https://queirozf.com/entries/jupyter-kernels-how-to-add-change-remove)

```
source venv/bin/activate
pip install ipykernel
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

