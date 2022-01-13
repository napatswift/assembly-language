# Register

### Data registers

|  | 7 ← 0 | 7 ← 0 |
| --- | --- | --- |
| AX | AH | AL |
| BX | BH | BL |
| CX | CH | CL |
| DX | DH | DL |

`[ABCD]X` 16 bits and separate in to `[ABCD][HL]` 8 bits (H for high, and L for low)

general use in a program, or specific use

### Pointer and index registers

16 bits registers and cannot be separated like data registers

- `SP` *stack pointer*
- `BP` *base pointer*
- `SI` *source index*
- `DI` *destination index*

stores **offset** for computing **actual address**

8088 has 20-bit address โดยการนำ **segment number * 16 + offset**

actual address ของคำสั่งได้จาก **(CS)*16 + IP** (*instruction pointer)* **(offset)**

20-bit address อ้างอิงข้อมูลได้ 1024 Kb = 1 Mb

### Segment registers

16 bits and cannot be separated

- `CS` *code segment (stores code)*
- `DS` *data segment (stores data)*
- `SS` *stack segment (stores data and temporary address e.g. return address)*
- `ES` extra segment (stores data that be used with String code)

stores beginning address of any segment

each segment has 64Kbytes

every program MUST have code segment

### Segment Number and Offset

| segment | Segment number | Offset |
| --- | --- | --- |
| Data segment | DS | BX, SI, DI |
| Stack segment | SS | SP, BP |
| Extra segment | ES | DI |