# ASM

Assembly Language


Table of contents

- [ASM](#asm)
  - [MASM](#masm)
  - [Source Satement](#source-satement)
  - [Constants](#constants)
- [Register](#register)
    - [Data registers](#data-registers)
    - [Pointer and index registers](#pointer-and-index-registers)
    - [Segment registers](#segment-registers)
    - [Segment Number and Offset](#segment-number-and-offset)
  - [Internal Units and Bus Unit](#internal-units-and-bus-unit)
    - [internal units](#internal-units)
    - [bus unit](#bus-unit)
    - [instruction unit](#instruction-unit)
    - [execution unit (EU)](#execution-unit-eu)
    - [instruction pointer (IP)](#instruction-pointer-ip)
  - [Status Flags (Status Register)](#status-flags-status-register)
- [Instruction](#instruction)
    - [Label](#label)
    - [Mnemonic](#mnemonic)
    - [Operand](#operand)
    - [Comment](#comment)
- [Instruction Set](#instruction-set)
  - [MOV](#mov)
  - [PUSH](#push)
  - [POP](#pop)
  - [XCHG](#xchg)
- [Arithmetic Instructions](#arithmetic-instructions)
  - [ADD](#add)
  - [SUB](#sub)
  - [MUL](#mul)
  - [IMUL](#imul)
  - [DIV](#div)
  - [IDIV](#idiv)
  - [NEG](#neg)
  - [CBW](#cbw)
  - [CWD](#cwd)
  - [DEC](#dec)
  - [INC](#inc)
  - [CMP](#cmp)
- [Control Transfer Instructions](#control-transfer-instructions)
  - [Conditional Transfer](#conditional-transfer)
  - [Conditional transfer กับ `CMP`](#conditional-transfer-กับ-cmp)
  - [LOOP](#loop)
- [Logical Instruction](#logical-instruction)
- [Load Effective Address](#load-effective-address)
- [Operator pointer](#operator-pointer)
- [Interrupt Intruction](#interrupt-intruction)
  - [Interrupt Return](#interrupt-return)
  - [Interrupt Type](#interrupt-type)
    - [INT 21H](#int-21h)
  - [Binary to ASCII](#binary-to-ascii)
- [Procedure](#procedure)
  - [CALL](#call)
  - [RET](#ret)
  - [การส่งต่อข้อมูล](#การส่งต่อข้อมูล)
    - [ผ่านทาง registers](#ผ่านทาง-registers)
    - [ผ่านทาง memory location](#ผ่านทาง-memory-location)
    - [ผ่านทาง stack](#ผ่านทาง-stack)
- [Directive](#directive)
- [Assembler directive](#assembler-directive)
  - [Directive `DB` `DW` `DD`](#directive-db-dw-dd)
  - [Directive `SEGMENT`, `ENDS`](#directive-segment-ends)
  - [Directive `ASSUME`](#directive-assume)
  - [Directive `PROC`, `ENDP`](#directive-proc-endp)
    - [Distance attribute](#distance-attribute)
  - [Directive `END`](#directive-end)
- [Listing directive](#listing-directive)
  - [Directive `PAGE`](#directive-page)
  - [Directive `TITLE`](#directive-title)
  - [Directive `SUBTTL`](#directive-subttl)
- [Addressing Mode](#addressing-mode)
  - [Register addressing](#register-addressing)
  - [Immediate addressing](#immediate-addressing)
  - [Direct addressing](#direct-addressing)
  - [Register indirect addressing](#register-indirect-addressing)
  - [Base relative addressing](#base-relative-addressing)
  - [Direct indexed addressing](#direct-indexed-addressing)
  - [Base indexed addressing](#base-indexed-addressing)
- [SYMDEB](#symdeb)
  - [G](#g)
  - [T และ P](#t-และ-p)
  - [Q](#q)
  - [D](#d)
  - [R](#r)
  - [เริ่มกันเลย](#เริ่มกันเลย)

## MASM

MASM ย่อมาจาก Macro Assembler Language จาก Microsoft

assemble → *object code* → link → *executable program*

programmer *-assembly-lang-program→* assembler *-object-prog→* liker *-run-module→* execute

## Source Satement

คำสั่งในภาษา Assembly 

- **Assembly language** เป็นคำสั่งของ processor ที่จะทำตอน execute
- **Assembler directive** หรือ pseudo-operation เป็นคำสั่งของ assembler ที่ทำตอน assemble

## Constants

ค่าคงที่ในภาษา

1. binary constant: sequence of `01` followed by B e.g. 1010B
2. decimal constant: sequence of `0-9`  optionally followed by D e.g. 12D or 12
3. hexadecimal constant: sequence of `0-9A-F` followed by H and MUST start with `0-9` e.g. 12H, 0A4H 
4. character and string: sequence of letters, numbers, or special characters that surrounded by `'` or `"` e.g. ‘a’, “drop”

ค่าคงที่จำนวนลบ

1. decimal: add - in front of a number
2. binary and hexadecimal: in [2’s complement](https://en.wikipedia.org/wiki/Two%27s_complement)

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

## Internal Units and Bus Unit

### internal units

8086/8088 ทำงามตามคำสั่งโดยมี bus unit, instruction unit และ execution unit ซึ่งเป็น special-purpose units ในชิปเป็นตัวช่วย

### bus unit

อ่านคำสั่งจาก memory

ส่งผ่านข้อมูลระหว่าง processor กับภายนอก

นำคำสั่งไปเก็บใน code queue เพี่อให้ instruction unit นำไปใช้ 

### instruction unit

นำคำสั่งจาก bus unit code queue แปลคำสั่ง แล้วนำคำสั่งไปเก็บใน instruction queue หรือ pipeline

### execution unit (EU)

ทำงานตามคำสั่ง (execute) ที่อยู่ใน instruction queue

### instruction pointer (IP)

is register

stores offset of instruction that `EU` will execute next

## Status Flags (Status Register)

ใช้บอก status conditions หลังทำคำสั่งบางคำสั่ง เช่นคำสั่งคำนวน เปรียบเทียบ ฯลฯ จะมีผลกับ status register

ใช้ 9 bits

(bit) (flagname) (code)

0 Carry Flag (`CF`)

2 Parity Flag (`PF`)

4 Auxiliary carry Flag (`AF`)

6 Zero Flag (`ZF`)

7 Sign Flag (`SF`)

8 Trap Flag (`TF`)

9 Interrupt enable Flag (`IF`)

10 Direction Flag (`DF`)

11 Overflow Flag (`OF`)

# Instruction

format

```
[label:]? mnemonic [operand]? [;comment]?
```

- อยู่ที่ column ไหนก็ได้
- แยกกันอย่างน้อย 1 วรรค
- ไม่ยาวเกิน 132 columns

### Label

- ใช้เวลามีการอ้างอิงคำสั่ง
- ยาวได้มากสุด 31 characters
- ลงท้ายด้วย `:`
- `A-Z 0-9 ? . @ _ $`
- ห้ามขึ้นต้นด้วยตัวเลข
- หากมี `.` ต้องให้เป็นตัวแรก
- ห้ามซ้ำกับ registers

### Mnemonic

- 2-7 characters
- บางคำสั่งต้องมี operand ตามหลัง

### Operand

- บอกกับ microprocessor ว่าต้องทำงานกับข้อมูลจากใหน ไปเก็บไว้ที่ไหน
- อาจมี 0 1 หรือ 2 operands
- สำหรับ 2 operands ตัวแรกเป็น destination และตัวที่สองเป็น source

### Comment

- start with `;`
- a line that start with `;` is a comment line

# Instruction Set

## MOV

ใช้คัดลอกข้อมูลจาก source operand ไปที่ destination operand ทั้งสอง operand ต้องมีขนาดเท่ากัน และทุก mode ที่ใช้เพื่อทำงานกับข้อมูลใน data segment ใช้ `DS` เป็น segment register

format `MOV destination,source`

## PUSH

ใช้กับ stack เพื่อเก็บข้อมูลชั่วคราว จะเก็บ content ของ 16-bit register or memory operand at the top of stack

format `PUSH source`

```asm
PUSH SI ;register addr
```

เอาข้อมูลที่อยู่ใน `SI`  ไปไว้ใน stack โดยที่ข้อมูลใน `SI` ยังคงเหมือนเดิม

```asm
PUSH DS ;register addr
```

```asm
PUSH WTAB[BX] ;base relative addr
```

`PUSH` จะเก็บข้อมูลใว้ที่ top of stack หรือที่ location ที่ `SP` ชี้ไป มี `SP` เป็น offset ของ stack segment เริ่มจากมากไปน้อย โดย `SP` จะชี้ไปที่ word ที่เก็บใน stack ล่าสุด เวลาที่ PUSH มันจะ -2 จาก `SP` แล้วจึงจะเก็บข้อมูล

เช่นเมื่อ `PUSH BX` จากที่ SP pointing to `01FE`,  มันก็เลื่อนไปที่ `01FC` แล้วจึงเก็บข้อมูล

ส่วน `SS` จะเก็บ address เริ่มต้นของ stack

ตัวอย่างการจอง stack segment ขนาด 128 words ว่าง ๆ เพื่อเก็บข้อมูล

```asm
STACK SEGMENT STACK 'STACK'
      DW 128 DUP (?)
STACK ENDS
```

## POP

คัดลอกข้อมูลจาก the top of stack ไปไว่ที่ memory location หรือ register และ `SP` จะ +2

format `POP destination`

```asm
PUSH AX
    ...
POP AX
```

```asm
POP [SI] ;เก็บที่ DS ตำแหน่งที่ SI
```

## XCHG

ใช้สลับข้อมูลใน destination กับ source ระหว่าง registers ที่ไม่ใช่  segment registers หรือระหว่าง register กับ memory location สำหรับคำสั่งนี้ไม่มีผลต่อ status flags

format `XCHG destination,source`

```asm
XCHG AX,BX ;word registers
XCHG AH,BL ;byte registers
XCHG AX,MWORD ;register and memory location
```

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

```asm
ADD AX,BX
```

```asm
ADD AX,MEM_WORD
```

```asm
ADD MEM_WORD,AX
```

```asm
ADD AL,10
```

```asm
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

```asm
MUL CX ;CX * AX
MUL MBYTE ;MBYTE * AL
```

```asm
MOV CL,2
MOV AL,3
MUL CL ;CL * AL
```

## IMUL

integer multiply signed

format `IMUL source`

```asm
IMUL DL ;DL*AL
IMUL MWORD ;MWORD*AX
```

```asm
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

```asm
DIV SI ;(DX,AX)/SI
DIVE AX;AX/MBYTE
```

```asm
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

```asm
MOV CL,5BH
DEC CL ;CL=5AH
```

## INC

increment destination value by 1 มีผลกับ OF, SF, ZF, AF, PF

format `INC destination`

```asm
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

```asm
MOV BL,32
CMP BL,20
```

หลังทำคำสั่งจะได้ว่า OF=0 SF=0 CF=0 ZF=0

```asm
MOV BX,123
MOV AX,32
CMP AX,BX ;32 CMP to 123
```

หลังทำคำสั่งจะได้ว่า OF=0 SF=1 CF=1 ZF=0

# Control Transfer Instructions

คำสั่งที่ใช้ในการควบคุมการไหลของโปรแกรม แบ่งได้เป็น condtitional transfer และ unconditional transfer

1. JMP

เป็นแบบไม่มีเงื่อนไข (unconditional transfer) และไม่มีผลต่อ flags

format `JMP label`

```asm
JMP NEXT
    ...
NEXT: ...
```

```asm
BACK: ...
      ...
      JMP BACK
```

## Conditional Transfer

เป็นแบบมีเงื่อนไข (conditional transfer) และไม่มีผลต่อ flags

format `J_ label`

1. `JA` “jump if above” มากกว่า
2. `JNBE` “jump if not below nor equal” ไม่น้อยกว่าเท่ากับ
3. `JAE` “jump if above or equal” มากกว่าหรือเท่ากับ
4. `JNB` “jump if not below” ไม่น้อยกว่า
5. `JB` “jump if below” มากกว่า
6. `JNAE` “jump if not above or equal” ไม่มากกว่าเท่ากับ
7. `JC` “jump if carry” ถ้า CF=1
8. `JNC` “jump if not carry” ถ้า CF=0
9. `JBE` “jump if below or equal” ถ้า C=1 or ZF=1
10. `JNA` “jump if not above” ถ้า C=1 or ZF=1
11. `JCXZ` “jump if CX is zero” ถ้า CX=0
12. `JE` “jump if equal” ถ้า ZF=1
13. `JZ` “jump if zero” ถ้า ZF=1
14. `JNE` “jump if not equal” ถ้า ZF=0
15. `JNZ` “jump if not zero” ถ้า ZF=0
16. `JNO` “jump if no overflow” ถ้า OF=0
17. `JO` “jump if overflow” ถ้า OF=1
18. `JNF` “jump if no parity (odd)” ถ้า PF=0
19. `JPO` “jump if parity odd” ถ้า PF=0
20. `JNS` “jump if no sign (positive)” ถ้า SF=0
21. `JG` “jump if greater”
22. `JNLE` “jump if not less nor equal”
23. `JNG` “jump if not greater”
24. `JGE` “jump if greater or equal”
25. `JNL` “jump if not less”
26. `JL` “jump if less”
27. `JNGE` “jump if not greater nor equal”
28. `JNE` “jump if not equal”
29. `JNZ` “jump if not zero”
30. `JNO` “jump if no overflow”
31. `JNP` “jump if no parity (odd)”
32. `JPO` “jump if parity odd”
33. `JNS` “jump if no sign (positive)”
34. `JP` ”jump on parity (even)”
35. `JPE` ”jump if parity even”
36. `JS` ”jump on sign (negative)”

ตัวอย่าง

```asm
ADD AL,CL
JC BIG
```

```asm
ADD BX,AX
JO OVER
```

```asm
SUB AL,BL
JZ NULL
```

```asm
DEC COUNT
JNZ MORE
```

```asm
CMP AL,DL
JE EQL
```

## Conditional transfer กับ `CMP`

|  | unsigned | signed |
| --- | --- | --- |
| dest>src | JA,JNBE | JG,JNLE |
| dest=src | JE | JE |
| dest≠src | JNE | JNE |
| dest<src | JB | JL |
| dest≤src | JBE | JLE |
| dest≥src | JAE | JGE |

```asm
CMP BX,CX ;unsigned
JA BX_MORE
```

```asm
CMP AL,BL ;signed
JG ...
```

```asm
CMP CX,123 ;signed
JLE ..
```

```asm
	CMP BL,10
	JAE L1 :if BL is above or equal to 10
	...
	JMP NEXT

L1: JA L2 ;if BL is above 10
	...
	JMP NEXT

L2: ...
	...

NEXT:
```

## LOOP

ทำชุดคำสั่งซ้ำ ๆ กันโดยใช้ register CX เป็น index ของ loop โดยในแต่ละ loop ค่าใน register CX ลง 1 แล้วจะวนต่อ ดังนั้นให้ใช้เป็นคำส่ังสุดท้าย คำสั่ง LOOP ไม่มีผลต่อ flags

format `LOOP label`

ตัวอย่างการวนรอบ 10 ครั้ง

```asm
	MOV CX,10
NEXT: ...
	...
	LOOP NEXT ;
```

จะจบการวนรอบ ถ้าใน `CX` เป็น 0

# Logical Instruction

ทำงานกับ bits ของ operands

1. AND
2. OR
3. XOR

format `AND destination,source`

มีผลต่อ flags `SF`, `ZF`, `PF` โดย `OF`=0 `CF`=0 `AF`=undifined


4. NOT

format `NOT destination`

5. TEST ทำงานเหมือน AND แต่จะไม่เปลี่ยน destination operand และใช้เพื่อ set flags

# Load Effective Address

ใช้โหลดค่าออฟเซ็ตไว้ใน register ขนาด 16 บิต

 format `LEA register,memory-location`

```asm
MOV BX,8
LEA DI,TAB[BX]
```

```asm
MOV BX,10
MOV DI,4
LEA SI,[BX][DI]+7
```

หลังจบคำสั่ง `SI` จะมีค่า offset คือ 10 + 4 + 7 = 21

# Operator pointer

ใช้ในกรณีที่ operand เป็น memory location และไม่สามารถบอกขนาดของ operand ได้ จึงใช้ `PTR` เป็นตัวกำหนดขนาดของ operand ว่ามีขนาดเป็น byte หรือ word

format `(BYTE|WORD) PTR expression`

เช่น

```asm
NEG [SI]
CMP [BX],37H
```

แต่ไม่รู้ขนาดว่าเป็น byte หรือ word จึงใช้ `PTR`

```asm
NEG BYTE PTR [SI]
CMP WORD PTR [BX],37H
CMP BYTE PTR [BX],37H
```

# Interrupt Intruction

ใช้เรียก interrupt routine โดยที่ใช้ interrupt type เป็นค่า 0-255 ในฐาน 10 หรือ 0-FFH มาคำนวณหาที่อยู่ของ interrupt vector ซึ่งเป็นที่เก็บที่อยู่ของ interrupt routine มีการทำงานคือ (1) Push flag register ไปใน stack ดังนั้นต้องมี stack segment ในโปรแกรม ต่อมา (2) clear `TF` และ `IF` เพื่อป้องกันการทำงานแบบ single-step และไม่ให้เกิด interrupt อื่น ๆ (3) push `CS` ใน stack (4) คำนวณหา address ของ interrupt vector โดยคำนวณจาก interrupt-type (5) โหลด word ที่ 2 ของ intrrupt vector ใน `CS` จากนั้น (6) push `IP` ลงใน stack และ (7) โหลด word แรกของ interrupt vector ใน `IP`

หลังจากทำงานใน interrupt routine จบหรือในอีกแง่คือ execute ไปถึงคำสั่ง `IRET` (interrupt return) จะ pop return address จาก stack แล้วทำงานต่อ

format `INT interrupt-type`

interupt-type คือ 0-255 หรือ 0-FFH

- [Interrupt Intruction](#interrupt-intruction)
  - [Interrupt Return](#interrupt-return)
  - [Interrupt Type](#interrupt-type)
    - [INT 21H](#int-21h)
  - [Binary to ASCII](#binary-to-ascii)

## Interrupt Return

คำสั่ง `IRET` อยู่ใน interrupt routine ใช้เพื่อ return จาก interrupt โดยการ pop 3 words จาก stack แล้วไปเก็บไว้ใน IP, CS, และ flags register

## Interrupt Type
1. Type 0: divide error เมื่อผลหารเกิด overflow หรือ หารด้วย 0
2. Type 1: single step จะเกิดกรณีเป็น sigle-step debugging mode
3. Type 2: nonmaskable interrupts เป็น interrupt ที่ไม่สามารถ locked out ได้ เช่น ไฟตก
3. Type 3: breakpoint จะเกิดการ execute โปรแกรมจนกว่าจะถึง breakpoint (stop address) ใช้ debug โปรแกรม
4. ...
5. 21H: ใช้แสดงผล

### INT 21H

ใช้แสดงผลข้อมูล โดยจะใช้ค่าใน AH ร่วมด้วย

format `INT 21H`

ตารางแสดงค่าใน AH
| AH | Operation | Input value | result |
| --- | --- | --- | --- |
| 2 | display a character | DL = Character | cursor follows character |
| 9 | display a string | DS:DX = string โดยที่ string ต้อง terminate ด้วย `$` | cursor follows string |

ตัวอย่างการแสดงอักษร A

```asm
MOV AH,2
MOV DL,'A'
INT 21H
```

หรือ

```asm
MOV AH,2
MOV DL,65
INT 21H
```

ตัวอย่างการขึ้นบรรทัดใหม่

```asm
MOV AH,2
MOV DL,13 ;carriage return
MOV 21H
```

ตัวอย่างการแสดง string

```asm
MES DB 'Hello world!'13,10,'$' ;13=carriage return, 10=line feed

  ...

  MOV AH,9
  LEA DX,MES ;นำ offset ไปเก็บใน DX
  INT 21H
```

สำหรับการแสดงตัวเลข ต้องคำนวณหา[รหัส ASCII](https://en.wikipedia.org/wiki/ASCII)

## Binary to ASCII

เปลี่ยนเลข binary ให้เป็น ASCII code ของตัวเลขเพื่อแสดงผล

หารเลข binary ด้วย 10 แล้วให้นำเศษที่ได้เปลี่ยนเป็น ASCII code แล้วเก็็บใว้ จากนั้นนำเอาผลลัพธ์ที่เป็นจำนวนเต็มมาเป็นตัวตั้ง และทำแบบนี้ไปเรื่อย ๆ จนตัวตั้งเป็น 0 แล้วจึงแสดงผล

ตัวอย่างการแสดง 789

```
789/10 = 78 เศษ 9
 78/10 = 7  เศษ 8
  7/10 = 0  เศษ 7
```

เปลี่ยน 9 ไปเป็น ASCII ด้วยการบวกกับ 30H + 9


ตัวอย่างการแปลงและแสดงเลขฐาน 10

```asm
SSEG SEGMENT STACK
  DW 64 DUP (?)
SSEG ENDS

DSEG SEGMENT
  LINE DB 6 DUP (?),13,10,'$'
DSEG ENDS

CSEG SEGMNET
  ASSUME CS:CSEG,DS:DSEG,SS:SSEG
  BTOA PROC
      MOV AX,DSEG
      MOV DS,AX ;load data segment
      LEA BX,LINE ;offset of LINE
      MOV CX,6 ;loop index
    FILL: MOV BYTE PTR[BX],' ' ;fill LINE with blanks
      INC BX
      LOOP FILL

      MOV AX,-123 ; THE NUMBER
      PUSH AX
      MOV DI,10
      CMP AX,0 ; compare with 0
      JGE NEXT ; if the number >= 0 (positive num)
      NEG AX ; change to positive num

    NEXT: CWD ; convert word in AX to double word in DX,AX
      DIV DI ; AX div with 10
      ADD DX,'0' ; convert to ASCII
      DEC BX
      MOV [BX],DL ; store character in string
      CMP AX,0 ; compare with 0
      JNE NEXT ; if it's not 0 then continue loop

      POP AX ;get the number
      CMP AX,0
      JGE DONE ; if the number is positive

      ;store - at the beginning
      DEC BX
      MOV BYTE PTR[BX],'-' ;constant - can be byte or word so we use PTR

    DONE: MOV AH,9
      LEA DX,LINE ;offset of string
      INT 21H ; display string
      MOV AH,4CH ; to terminate programe 
      INT 21H
  BTOA ENDP
CSEG ENDS
  END BTOA
```

# Procedure

เป็นกลุ่มของคำสั่งเพื่อทำงานบางอย่าง (คล้าย function) และคำสั่งเกี่ยวกับ procdure มีดังนี้

- [Procedure](#procedure)
  - [CALL](#call)
  - [RET](#ret)
  - [การส่งต่อข้อมูล](#การส่งต่อข้อมูล)
    - [ผ่านทาง registers](#ผ่านทาง-registers)
    - [ผ่านทาง memory location](#ผ่านทาง-memory-location)
    - [ผ่านทาง stack](#ผ่านทาง-stack)

## CALL

ใช้ในการเรียกใช้ procedure

format `CALL procedure_name`

การทำงานคือจะเก็บ return address ไว้ใน stack ในกรณีที่เป็น NEAR จะ push offset ของคำสั่งต่อไป (IP) ส่วนถ้าหากเป็น FAR จะ push (CS) และ (IP) คำสั่งนี้ไม่มีผลต่อ status flags

## RET

ใช้เพื่อส่ง controll กลับไปที่โปรแกรมล่าสุดที่เรียกใช้โปรแกรมนี้

format `RET`

format `RET n`

เมื่อ `n` เป็นเลขจำนวนเต็ม โดยที่จะ return และ pop `n` bytes ออกจาก stack

การทำงานคือจะ pop return address จาก stack ถ้าเป็น NEAR จะ pop 1 word ที่เก็บใน IP และถ้าเป็น FAR จะ pop 2 word ที่เก็บใน IP และ CS คำสั่งนี้ไม่มีผลต่อ flags

```asm
CALL ATOB
```

```asm
CODE SEGMENT
    ASSUME ...
  MAIN PROC
    ...
    CALL SUB
    ...
  MAIN ENDP

  SUB PROC
    ...
    RET
  SUB ENDP
CODE ENDS
  END MAIN
```

## การส่งต่อข้อมูล

การส่งผ่านข้อมูลระหว่าง calling procedure และ called procedure สามารถทำได้ดังนี้

1. ผ่านทาง registers
2. ผ่านทาง memory location
3. ผ่านทาง stack

### ผ่านทาง registers

ต้องรู้ว่าส่งผ่านทาง register ไหน

### ผ่านทาง memory location

ใช้เวลาต้องส่งผ่านค่าหลาย ๆ ค่า เช่น string โดยใส่ address ของข้อมูลใน register

### ผ่านทาง stack

push ข้อมูลเข้าไปก่อนการเรียก procedure และเมื่อจะใช้จะ pop ออกมาเลยไม่ได้เนื่องจากว่าบน top of stack มีค่าของ return address อยู่

สามารถใช้ BP เพื่อเข้าถึงข้อมูลใน stack โดยไม่ต้องดึงข้อมูลออกจาก stack โดย copy ข้อมูลใน SP ไว้ใน BP เพื่อใช้เป็น offset ของ stack โดยการ +2 กับ BP

ตัวอย่างการส่งข้อมูลผ่านทาง stack

ใน caller

```asm
PUSH ARG1
PUSH ARG2
CALL ROUTINE
```

ใน callee

```asm
MOV BP,SP
MOV BX,[BP]+2 ; get ARG1
MOV CX,[BP]+4 ; get ARG2
  ...
RET 4 ;return and pop 4 bytes off stack
```

# Directive

- [Directive](#directive)
- [Assembler directive](#assembler-directive)
  - [Directive `DB` `DW` `DD`](#directive-db-dw-dd)
  - [Directive `SEGMENT`, `ENDS`](#directive-segment-ends)
  - [Directive `ASSUME`](#directive-assume)
  - [Directive `PROC`, `ENDP`](#directive-proc-endp)
    - [Distance attribute](#distance-attribute)
  - [Directive `END`](#directive-end)
- [Listing directive](#listing-directive)
  - [Directive `PAGE`](#directive-page)
  - [Directive `TITLE`](#directive-title)
  - [Directive `SUBTTL`](#directive-subttl)

# Assembler directive

เป็นคำสั่งสำหรับ assembler ใช้เพื่อกำหนด segments, procedures, define symbols, allocate memory. ส่วนมากไม่ถูก generate เป็น object code

Macro Assembler has around 60 directives

## Directive `DB` `DW` `DD`

1. `DB` *define byte* each expression **using 1 byte**
2. `DW` *define word* each expression **using 2 bytes**
3. `DD` *define double* *word* each expression **using 4 bytes**

format `[name]? (DB|DW|DD) expression[,expresion]*`

การทำงาน จองเนื้อที่ในความจำโดย `name` เรียกว่าเป็น symbolic reference และสามารถจองโดยให้ค่าเริ่มต้น หรือไม่ให้ก็ได้

```asm
;byte
BU_MAX DB 255    ;max unsigned
BS_MAX DB 127    ;max signed
BS_MIN DB -128   ;min signed

;word
WU_MAX DW 65535  ;max unsigned
WS_MAX DW 32767  ;max signed
WS_MIN DW -32768 ;min signed
```

1. กรณีการให้ค่าเริ่มต้นมากกว่า 1 ค่า ให้คั่นแต่ละค่าด้วย `,` และจะเรียกว่าเป็น  table

```asm
B_TAB DB 0,1,2,3     ;byte talbe using 4 bytes
W_TAB DB -124,90,149 ;word talbe using 6 bytes
```

ให้ค่าเดียวกัน ซ้ำกันใช้ `DUP` duplicate operator `n DUP (expresion)?` n ≥ 1

```asm
DB 4 DUP (0) ;works like `DB 0,0,0,0`
```

1. กรณีการไม่ให้ค่าเริ่มต้น ให้ใช้ `?` แทน expression

```asm
COUNT DB ?
NUM   DW ?
TAB   DB 49 DUP (?)
```

1. กรณีให้ค่าเริ่มต้นเป็น string ใช้ DB (ไม่ใช้ DW) เพราะแต่ละ character ใช้ 1 byte

```asm
MSG DB 'MONOSODUIMGLUTAMATE'
FNS DB 'F','E','E','N'
```

## Directive `SEGMENT`, `ENDS`

ใช้เพื่อแบ่ง source program เป็น segment

format 

```asm
seg-name SEGMENT [align-type][combline-type]['class']
         ...
seg-name ENDS
```

- `align-type` กำหนดขอบเขตที่จะใส่ segment ไปที่ตำแหน่งไหนในความจำ อาจเป็น `PARA` คือ address ที่เป็น  multiple ของ 16 เป็น default, `PAGE` คือ address เป็น multiple ของ 256
- `combine-type` ในการที่จะเชื่อมโยง module เข้าด้วยกัน กำหนดว่า segment ที่จะมารวมกับ segment อื่นที่มีชื่อเดียวกันจะมารวมกันอย่างไร และหากไม่ระบุ `combine-type` แสดงว่าไม่ต้องการนำไปรวมกับ segment อื่น 1. `PUBLIC` ใช้กับ code, data, or extra segment to concatenate  the segment that has a same name into 1 physical segment when it linked 2. `COMMON` ใช้กับ code, data, extra segment เพื่อให้ segment ที่มีชื่อเดียวกันใช้เนื้อที่ร่วมกัน (overlapped) 3. `STACK` ใช้กับ stack segment และทุก stack segment ต้องมี `STACK`
- `'class'`  ใช้กำหนดลำดับการจัดกลุ่ม segment โดย segment ที่มีชื่อ class เหมือนกัน จะเก็บเรียงกันตามลำดับ ถ้าชื่อไม่เหมือนกันจะเก็บตาม linker กำหนด

```asm
DSEG SEMENT
;offset 0  2
     DW 11,12
;offset 4  5  6
     DB 45,46,47
DSEG ENDS
```

DSEG คือชื่อของ segment ที่กำหนด และจาก code ด้านบนใช้เนื้อที่ทั้งหมด 7 bytes

## Directive `ASSUME`

ใช้บอก assembler ว่า segment register ที่ใช้แต่ละ segment คือ segment register ชนิดใด การใช้นั้น `ASSUME` ต้องอยู่ถัดจาก `SEGMENT` directive และอยู่ใน code segment เท่านั้น

format `ASSUME seg-reg:seg-name(,seg-reg:seg-name)*`

example 1

```asm
SSEG SEGMENT STACK
     ...
SSEG ENDS

CSEG SEGMENT
     ASSUME CS:CSEG,SS:SSEG
     ...
CSEG ENDS
```

example 2

```asm
STORAGE SEGMENT STACK
     ...
STORAGE ENDS

DATA SEGMENT
     ...
DATA ENDS

CODE SEGMENT
     ASSUME SS:STORAGE,DS:DATA,CS:CODE
     ...
CODE ENDS
```

## Directive `PROC`, `ENDP`

ใช้กำหนดจุดเริ่ม และจุดสิ้นสุดของ procedure ที่เป็นบล็อกของคำสั่งที่อาจถูกเรียกใช้จากส่วนต่าง ๆ ของโปรแกรม และในทุกครั้ง ต้องมี distance attribute นั่นคือ `NEAR` หรือ FAR แต่มี NEAR เป็นค่าเริ่มต้น จึงไม่จำเป็นต้องใส่

format `name PROC [NEAR]?` หรือ `name PROC FAR` และปิดด้วย `name ENDP`

### Distance attribute

สำหรับ procedure ที่ติดธง `NEAR` นั้นจะสามารถถูกเรียกใน code segment ที่กำหนด procedure นี้และ code segment ที่ใน module อื่นที่มีชื่อ segment เหมือนกันเท่านั้น  แต่กลับกัน `FAR` นั้นจะถูกเรียกจาก code segment อื่นได้ไม่จำเป็นต้องมีชื่อเดียวกัน

```asm
CSEG SEGMENT
     ASSUME CS:CSEG,SS:STACK

FIRST PROC
     ...
FIRST ENDP

CSEG ENDS
```

ใช้คำสั่ง `CALL`  เพื่อเรียกใช้ procedure ที่กำหนดเอาไว้ และไม่ต้องกำหนดก่อนที่จะเรียกใช้

```asm
CSEG SEGMENT
     ASSUME CS:CSEG,SS:STACK

CLLR PROC
     ...
     CALL CLLE
CLLR ENDP

CLLE PROC
     ...
CLLE ENDP

CSEG ENDS
```

## Directive `END`

บอกจุดสิ้นสุดของโปรแกรม หลังจากคำสั่งนี้ จะไม่มีคำสั่งใดตามอีก

format `END main_pocedure`

```asm
SSEG SEGMENT STACK
     ...
SSEG ENDS

CSEG SEGMENT
     ASSUME SS:SSEG,CS:CSEG
NEW  PROC
     ...
NEW  ENDP
CSEG ENDS
     END NEW
```

# Listing directive

consist of directive `PAGE`, `TITLE`, and `SUBTTL`

## Directive `PAGE`

ใช้กำหนดความยาวและความกว้างของแต่ละหน้าของ Listing

format `PAGE [line]?[,columns]?`

1. *lines* คือจำนวนบรรทัดในหน้า listing เป็นได้ 10-155 และมีค่าเริ่มต้นเป็น 57 
2. *columns* คือจำนวนหลักเป็นได้ 60-132 และมีค่าเริ่มต้นเป็น 80

```asm
PAGE 60,70 ;60 lines and 70 columns
```

## Directive `TITLE`

ใช้พิมพ์ชื่อในแต่ละหน้าของ listing เหมือนเป็นหัวกระดาษ

format `TITLE text`

1. *text* อักขระยาวได้ 60 อักขระ

```asm
TITLE sample title
```

## Directive `SUBTTL`

ใช้พิมพ์ชื่อรองในแต่ละหน้าของ listing ต้องมี TITLE ก่อน

format `SUBTTL text`

1. *text* อักขระยาวได้ 60 อักขระ

```asm
SUBTTL sample subtitle
```

# Addressing Mode

ในการใช้คำสั่งนั้น ทุก operand ต้องมี addressing mode เพื่อใช้บอกกับ microprocessor ว่าให้ทำงานกับข้อมูลจากที่ใด โดยแบ่งได้ 7 กลุ่ม

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

# SYMDEB

ใช้ SYMDEB เพื่อ run โปรแกรมทีละคำสั่ง หรือแสดงข้อมูลที่ที่อยู่ที่ต้องการ หรือหยุดการทำงานของโปรแกรม

```sh
SYMDEB filename.exe
```

## G
format `G [offset]?[,offset]*`

ตัวอย่าง

```sh
SYMDEB filename.exe
G
```

```sh
SYMDEB filename.exe
G A
```

คำสั่งจากข้างบนจะทำงานจนถึงคำสั่งที่ offset 0AH

## T และ P

ececute 1 คำสั่งหรือมากกว่า แล้วแสดง content ของ registers และ flags

format `T [instruction-count]?`

ตัวอย่าง

```sh
SYMDEB filename.exe
T
```

```sh
SYMDEB filename.exe
T 5
```
ด้านบนจะทำ 5 คำสั่งถัดไป

สำหรับ P จะนับ procedure calls, interrupts, loop instructions, และ repeat string instructions เป็นเหมือน 1 คำสั่ง

format `P [instruction-count]?`

## Q

ออกจากโปรแกรม

format `Q`

## D

แสดงข้อมูลตัวเลขใน memory location

format `D (DS|SS|ES|CS):offset [offset]?`

`D (DS|SS|ES|CS):offset L byte_count`

ตัวอย่าง

แสดงข้อมูลจากตำแหน่งเริ่มต้นของ data segment (`DS`)

```sh
SYMDEB multiply.exe
D DS:0
```

แสดงข้อมูลจากตำแหน่ง 10H ถึง 1A ของ data segment (`DS`)

```sh
SYMDEB multiply.exe
D DS:10 1A
```

## R

จะแสดงข้อมูลของ 16-bit register หรือเปลี่ยนค่าใน 16-bit register [ดูที่นี่](register.md)

format `R [register-name [value]?]?`

ตัวอย่าง

แสดง content ของทุก registers

```
SYMDEB cal.exe
R
```

แสดง content ของ AX

```
SYMDEB cal.exe
R AX
```

ใส่ค่า 5DH ไว้ใน AX

```
SYMDEB cal.exe
R AX 5D
```

## เริ่มกันเลย

```
> masm filename
  ...
Object filename [filename.OBJ]:
source listing [NUL.LST]: filename.lst
Cross-reference [NUL.CRF]:
 ...
```

ทำการลิงก์

```
> link filename
  ...
Run File [finename.EXE]:
List File [NUL.MAP]:
Libraries [.LIB]:
```

ใช้ SYMDEB

```
> symdeb filename.exe
  ...
Processor is [80286]
-r
AX=FFFF BX=0000 CX=00E5 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
DS=0AA7 ES=0AA7 SS=0AB7 CS=0AC0 IP=0000 NV UP EI PL NZ NA PO NC
0ACO:0000 B8F0A        MOV   AX,0ABF
```

จาก

```
AX=FFFF BX=0000 CX=00E5 DX=0000 SP=0080 BP=0000 SI=0000 DI=0000
DS=0AA7 ES=0AA7 SS=0AB7 CS=0AC0 IP=0000 NV UP EI PL NZ NA PO NC
0ACO:0000 B8BF0A        MOV   AX,0ABF
```

ได้ว่า

`SP=0080` เนื่องจากว่าเราจองเนื้อที่ให้ stack segment จำนวน 64 words ซึ่งเท่ากับ 128 bits หรือ 80H bits ดังนั้น SP จะไปอยู่ที่บนสุดของ stack หรือที่ 80 ในกรณีนี้

`NV UP EI PL NZ NA PO NC` คือ status flags

`MOV AX,0ABF` คือคำสั่งถัดไป มี `segment:offset` คือ `0ACO:0000` และมี Object Code คือ `B8BF0A` มีขนาด 3 bytes ทำให้ offset ถัดไปจะ +3 หรือ เป็น 0003

และถัดมาจะทำ `MOV AX,0ABF`

```
-t
AX=0ABF BX=0000 CX=00E5 DX=0000 SP=0080 BP=0000 SI=0000 DI=0000
DS=0AA7 ES=0AA7 SS=0AB7 CS=0AC0 IP=0003 NV UP EI PL NZ NA PO NC
0ACO:0003 OED8         MOV   DS:AX
```

`MOV DS:AX` คือคำสั่งถัดไป

