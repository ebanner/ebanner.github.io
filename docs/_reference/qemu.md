---
layout: default
title:  "qemu"
description: qemu debug commands
---

# qemu

Show 256 "giga" (8 byte) words starting at 0x10000 in hex

```
(qemu) xp /256gx 0x10000
0000000000010000: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010010: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010020: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010030: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010040: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010050: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010060: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010070: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010080: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
0000000000010090: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
00000000000100a0: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
00000000000100b0: 0xccdd8ebbaaaaeeff 0xccdd8ebbaaaaeeff
...
```
