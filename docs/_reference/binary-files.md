---
layout: default
title:  "Binary files"
description: Commands for inspecting binary files
---

# Binary files

View how big each section is in an object file

```
% objdump -h boot_sector_asm.o

boot_sector_asm.o:	file format elf32-i386

Sections:
Idx Name          Size     VMA      Type
  0               00000000 00000000
  1 .text         0000004b 00000000 TEXT
  2 .shstrtab     0000002b 00000000       # Section Header STRing TABle
  3 .symtab       000000b0 00000000
  4 .strtab       00000084 00000000
  5 .rel.text     00000020 00000000
```

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
