---
layout: default
title:  "Binary files"
description: Commands for inspecting binary files
---

# Binary files

Disassemble a binary

```
ndisasm -b 16 boot_sect.bin
```

Read ELF file headers

```
readelf -h os.elf
```

View the assembly in your object file

```
objdump -d boot_sector.o
```

Viewing a binary file

```
hexdump -C file.bin
```

Viewing the contents of a binary file

```
xxd file.bin
```

Viewing how many bytes are in the file

```
wc -c file.bin
```

Viewing how many bytes are in the file

```
ls -lh file.bin
```
