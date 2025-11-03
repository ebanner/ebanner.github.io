---
layout: default
title:  "shell"
description: Various commands for the shell
---

# Shell commands

List all files in a directory on their own separate line

```
$ ls -l train/images | awk '{ print $NF }'

08e1d82f-IMG_1346.jpeg
2dd1ddf2-IMG_1326.jpeg
2e84ab2a-IMG_1345.jpeg
6139148f-IMG_1338.jpeg
7091701e-IMG_1339.jpeg
79f5d449-IMG_1335.jpeg
86e5d119-IMG_1328.jpeg
8fa782dc-IMG_1333.jpeg
93cddce7-IMG_1336.jpeg
b7788762-IMG_1325.jpeg
f7fbf073-IMG_1340.jpeg
```
