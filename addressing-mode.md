# Addressing Mode

ในการใช้คำสั่งนั้น ทุก operand ต้องมี addressing mode เพื่อใช้บอกกับ microprocessor ว่าให้ทำงานกับข้อมูลจากที่ใด โดยแบ่งได้ 7 กลุ่ม

Content
- [Addressing Mode](#addressing-mode)
  - [Register addressing](#register-addressing)
  - [Immediate addressing](#immediate-addressing)
  - [Direct addressing](#direct-addressing)
  - [Register indirect addressing](#register-indirect-addressing)
  - [Base relative addressing](#base-relative-addressing)
  - [Direct indexed addressing](#direct-indexed-addressing)
  - [Base indexed addressing](#base-indexed-addressing)

## Register addressing

format: ชื่อ register ขนาด 8 หรือ 16 บิต

segment register: none

```asm
MOV AX,CX
```

## Immediate addressing

format: ค่าคงที่ขนาด 8 หรือ 16 บิตเป็น source opeand

segment register: none

```asm
MOV CX,59 ;59 is immediate addressing
```

```asm
MOV CL,'STRING' ;'STRING' is immediate addressing
```

## Direct addressing

format: symbolic reference (คล้าย ๆ ชื่อตัวแปร)

segment register: `DS`

เป็น offset โดย microprocessor จะคำนวณ 20-bit address เติม 0H จาก `DS` แล้วบวกกับ offset

```asm
MOV AX,TAB ;TAB is direct addressing
```

## Register indirect addressing

ใช้กับ register ที่กำหนดเท่านั้น ใช้เป็นค่า offset

ทำงานกับข้อมูลใน `DS`: `[BX]`, `[DI]`, and `[SI]`

ทำงานกับข้อมูลใน SS: `[BP]`

```asm
MOV BX,2
MOV AX,[BX] ;offset 2 in AX register
```

## Base relative addressing

| operand format | segment register |
| --- | --- |
| [BX]+displacement | DS |
| [BP]+displacement | SS |

displacement คือ ค่า 16-bit signed displacement value หรือ symbolic reference อาจเขียนได้หลายวิธี เช่น `[BP]+4`, `4[BP]`, `[BP+4]`, `NAME[BP]`

```asm
MOV AX,[BX]+4
```

```asm
MOV AX,TABLE[BX]
```

## Direct indexed addressing

| operand format | segment register |
| --- | --- |
| [DI]+displacement | DS |
| [SI]+displacement | DS |

```asm
MOV AX,WTAB[DI]
```

## Base indexed addressing

| operand format | segment register |
| --- | --- |
| [BX][SI]+displacement? | DS |
| [BX][DI]+displacement? | DS |
| [BP][SI]+displacement? | SS |
| [BP][DI]+displacement? | SS |

offset คือผลบวกของ base register และ index register กับ displacement (ซึ่งมีหรือไม่มีก็ได้)

```asm
MOV AX,[BX][SI]
```

```asm
MOV CX,NEXT[BP][SI]
```