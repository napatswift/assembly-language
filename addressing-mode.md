# Addressing Mode

ในการใช้คำสั่งนั้น ทุก operand ต้องมี addressing mode เพื่อใช้บอกกับ microprocessor ว่าให้ทำงานกับข้อมูลจากที่ใด โดยแบ่งได้ 7 กลุ่ม

1. register addressing
2. immediate addressing
3. direct addressing
4. register indirect addressing
5. base relative addressing
6. direct indexed addressing
7. base indexed addressing

### Register addressing

format: ชื่อ register ขนาด 8 หรือ 16 บิต

segment register: none

```wasm
MOV AX,CX
```

### Immediate addressing

format: ค่าคงที่ขนาด 8 หรือ 16 บิตเป็น source opeand

segment register: none

```wasm
MOV CX,59 ;59 is immediate addressing
```

```wasm
MOV CL,'STRING' ;'STRING' is immediate addressing
```

### Direct addressing

format: symbolic reference (คล้าย ๆ ชื่อตัวแปร)

segment register: `DS`

เป็น offset โดย microprocessor จะคำนวณ 20-bit address เติม 0H จาก `DS` แล้วบวกกับ offset

```wasm
MOV AX,TAB ;TAB is direct addressing
```

### Register indirect addressing

ใช้กับ register ที่กำหนดเท่านั้น ใช้เป็นค่า offset

ทำงานกับข้อมูลใน `DS`: `[BX]`, `[DI]`, and `[SI]`

ทำงานกับข้อมูลใน SS: `[BP]`

```wasm
MOV BX,2
MOV AX,[BX] ;offset 2 in AX register
```

### Base relative addressing

| operand format | segment register |
| --- | --- |
| [BX]+displacement | DS |
| [BP]+displacement | SS |

displacement คือ ค่า 16-bit signed displacement value หรือ symbolic reference อาจเขียนได้หลายวิธี เช่น `[BP]+4`, `4[BP]`, `[BP+4]`, `NAME[BP]`

```wasm
MOV AX,[BX]+4
```

```wasm
MOV AX,TABLE[BX]
```

### Direct indexed addressing

| operand format | segment register |
| --- | --- |
| [DI]+displacement | DS |
| [SI]+displacement | DS |

```wasm
MOV AX,WTAB[DI]
```

### Base indexed addressing

| operand format | segment register |
| --- | --- |
| [BX][SI]+displacement? | DS |
| [BX][DI]+displacement? | DS |
| [BP][SI]+displacement? | SS |
| [BP][DI]+displacement? | SS |

offset คือผลบวกของ base register และ index register กับ displacement (ซึ่งมีหรือไม่มีก็ได้)

```wasm
MOV AX,[BX][SI]
```

```wasm
MOV CX,NEXT[BP][SI]
```