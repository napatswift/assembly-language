# Arithmetic Instructions

คำสั่งทางคณิตศาสตร์กับข้อมูลชนิดตัวเลข

## ADD

บวกข้อมูลขนาด 8 หรือ 16 bit และนำผลบวกไปเก็บไว้ใน destination (∼`dest=dest+source`) ไม่สามารถบวกกันระหว่าง memory locations และ flags  ที่จะถูกแปลงค่าหลังจากใช้คำสั่งคือ CF, PF, AF, ZF, SF, OF

 format `ADD destination,source`

| destination | source |
| --- | --- |
| register | register |
| register | memory location |
| register | data segment |
| memory location | register |
| memory location | data segment |

| Flag | 0 | 1 |
| --- | --- | --- |
| CF |  | มีการทดจาก MSB |
| PF |  | มีผลลัพธ์ (Low-order 8 bit) มี but ที่เป็น 1 จำนวนคู่ตัว |
| AF |  |  |
| ZF |  | ถ้าผลลัพธ์เป็น 0 |
| SF |  | ถ้าผลลัพธ์เป็นลบ |
| OF |  | ถ้าเกิด Overflow |

กรณีที่จะไม่เกิด Overflow คือทดทั้งสอง bit (MSB กับ bit ถัดจาก MSB) และไม่ทดทั้งสอง bit

กรณีที่จะเกิด Overflow คือทด 1 bit แต่ไม่ทดอีก bit (MSB กับ bit ถัดจาก MSB)

```wasm
ADD AX,BX
```

```wasm
ADD AX,MEM_WORD
```

```wasm
ADD MEM_WORD,AX
```

```wasm
ADD AL,10
```

```wasm
ADD MEM_BYTE,10
```

การลบต้องนำตัวลบมาทำ 2’s complement แล้วนำมาบวกกับตัวตั้ง

## SUB

การลบ

format `SUB destination,source`

## MUL

คูณข้อมูลที่อาจเป็น byte, word general register, or memory location, but NOT immediate value

format `MUL source`

ในเมื่อมี operand แค่ตัวเดียว สำหรับอีกตัวนั้นต้องเก็บไว้ที่ `AL` สำหรับ byte และ `AX` สำหรับ word และสำหรับผลลัพธ์นั้นจะเก็บไว้ที่ `AX` สำหรับการคูณแบบ byte ส่วนการคูณแบบ word นั้นผลลัพธ์นั้นจะเก็บไว้ที่ `DX,AX`

|  | second operand | result | high-order result | low-order result |
| --- | --- | --- | --- | --- |
| byte | AL | AX | AH | AL |
| word | AX | DX,AL | DX | AX |

การคูณมีผลต่อ `OF` และ `CF` หาก high-order เป็น 0 ทั้งหมด `CF` และ `OF` จะเป็น `0` และหากว่า high-order ไม่เป็น 0 ทั้งหมด `CF` และ `OF` จะเป็น `1`

```wasm
MUL CX ;CX * AX
MUL MBYTE ;MBYTE * AL
```

```wasm
MOV CL,2
MOV AL,3
MUL CL ;CL * AL
```

## IMUL

integer multiply signed

format `IMUL source`

```wasm
IMUL DL ;DL*AL
IMUL MWORD ;MWORD*AX
```

```wasm
MOV AX,19
MOV CX,2
IMUL CX
```

## DIV

หารเลข unsigned โดย source คือตัวหาร และเป็น byte หรือ word register, หรือ memory location แต่ไม่ใช่ immediate value

format `DIV source`

|  | dividend | quotient | remainder |
| --- | --- | --- | --- |
| byte | AX | AL | AH |
| word | DX,AX | AX | DX |

ไม่มีผลกับ status flags แต่หากเกิด overflow จะเกิด type 0 interrupt โดยกรณีที่อาจเกิด interrupt มี

1. หารด้วย 0
2. ตัวหารมีขนาด 1 byte และตัวตั้งมากกว่าตัวหารอย่างน้อย (≥)256 เท่า
3. ตัวหารมีขนาด 1 word และตัวตั้งมากกว่าตัวหารอย่างน้อย (≥)65536 เท่า

```wasm
DIV SI ;(DX,AX)/SI
DIVE AX;AX/MBYTE
```

```wasm
MOV AX,39
MOV CL,4
DIV CL ;39/4
```

## IDIV

integer divide signed

format `IDIV source`

## NEG

หาค่าลบ หรือหา 2’s ของ destination

format `NEG destination`

## CBW

convert byte to word ใช้เพื่อให้ sign bit ของ register `AL` ไปอยู่ที่ทุก bit ของ `AH` ทำให้ข้อมูล 1 byte ใน `AL` เป็น 1 word ใน `AX` และไม่มีผลต่อ status flags

format `CBW`

## CWD

convert word to double-word ใช้เพื่อให้ sign bit ของ `AX` ไปอยู่ที่ทุก bit ของ `DX` ไม่มีผลต่อ status flags

format `CWD`

## DEC

decrement destination value by 1 มีผลกับ OF, SF, ZF, AF, PF

format `DEC destination`

```wasm
MOV CL,5BH
DEC CL ;CL=5AH
```

## INC

increment destination value by 1 มีผลกับ OF, SF, ZF, AF, PF

format `INC destination`

```wasm
MOV CX,010FH
INC CX ;CX=0110H
```

## CMP

ใช้เพื่อ set flags สำหรับการตัดสินใจการเปรียบเทียบค่า และกำหนดว่าค่า destiantion เป็นค่า constant ไม่ได้

 format `CMP destinatioin,source`

Table: unsigned operand

| condition | ZF | CF |
| --- | --- | --- |
| dest>source | 0 | 0 |
| dest=source | 1 | 0 |
| dest<source | 0 | 1 |

Table: signed operand

| condition | OF | SF | ZF |
| --- | --- | --- | --- |
| dest>source | 0/1 | 0 | 0 |
| dest=source | 0 | 0 | 1 |
| dest<source | 0/1 | 1 | 0 |

การทำงานของคำสั่ง `CMP` ในการตั้งค่าธงต่าง ๆ คือการลบ source ออกจาก destiantion แต่ค่าใน destination ไม่เปลี่ยน

```wasm
MOV BL,32
CMP BL,20
```

หลังทำคำสั่งจะได้ว่า OF=0 SF=0 CF=0 ZF=0

```wasm
MOV BX,123
MOV AX,32
CMP AX,BX ;32 CMP to 123
```

หลังทำคำสั่งจะได้ว่า OF=0 SF=1 CF=1 ZF=0