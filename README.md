# ASM

# register

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

# Instruction Set and Memory

### Instruction Set

8088 สามารถทำได้ประมาณ 100 คำสั่ง ทำงานกับเลขจำนวนเต็ม

### Memory

8088 สามารถ address ได้ 1 Mbytes (0000H → FFFFFH)

ไม่สามารถใช้ความจำทั้งหมดเพื่อเก็บโปรแกรมได้ เนื่องจากบางส่วนใช้เก็บบ  system programs เช่น systrm reset, คำสั่งที่ใช้ทำงานเวลาเปิดเครื่อง, เก็บ address ของโปรแกรมเพื่อทำงานเวลาเกิด **interrrupts**

### Interrupts

นอกจาทที่ microprocessor ทำงานตามคำสั่งในโปรแกรมแล้ว ก็ยังต้องทำงานอย่างอื่นด้วย เช่น เวลามีการกด keyborad ต้องรู้ว่าต้องทำอะไร ดังนั้นอุปกรณ์ต่าง ๆ ที่ต้องการใช้ sevice จาก microprocessor จะส่งสัญญาณ (interrupt signal) และพร้อมกับ identification number (ว่าเรียก type code มีทั้งหมด 256 code) เพื่อบอกว่าอุปกรณ์ตัวไหนที่ต้องการใช้ service

type code ใช้เพื่อคำนวณหา address ของโปรแกรมเพื่อทำงาน สำหรับ interrupt ชนิดต่าง ๆ

โปรแกรมที่ทำเวลาที่เกิด interrupt นั้น ๆ เรียกว่า interrupt (service) routine ISR หรือ interrupt handler

ISR ส่วนมากอยู่ใน ROM chip

หลังจากคำนวณ address ของ ISR เสร็จ ก็จะทำตามคำสั่งใน routine จนเสร็จแล้วจึงกลับไปทำงานที่ทำค้างอยู่ต่อ

บางคำสั่งจากโปรแกรมก็อาจทำให้เกิด intterrupt ได้เช่น การหารด้วย 0 การเกิด overflow

# MASM

MASM ย่อมาจาก Macro Assembler Language จาก Microsoft

assemble → *object code* → link → *executable program*

programmer *-assembly-lang-program→* assembler *-object-prog→* liker *-run-module→* execute

# Source Satement

คำสั่งในภาษา Assembly 

- **Assembly language** เป็นคำสั่งของ processor ที่จะทำตอน execute
- **Assembler directive** หรือ pseudo-operation เป็นคำสั่งของ assembler ที่ทำตอน assemble

## constants

ค่าคงที่ในภาษา

1. binary constant: sequence of `01` followed by B e.g. 1010B
2. decimal constant: sequence of `0-9`  optionally followed by D e.g. 12D or 12
3. hexadecimal constant: sequence of `0-9A-F` followed by H and MUST start with `0-9` e.g. 12H, 0A4H 
4. character and string: sequence of letters, numbers, or special characters that surrounded by `'` or `"` e.g. ‘a’, “drop”

Negative number

1. decimal: add - in front of a number
2. binary and hexadecimal: in [2’s complement](https://en.wikipedia.org/wiki/Two%27s_complement)

## Instruction

`[label:]? mnemonic [operand]? [;comment]?`

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

# Assembler directive

เป็นคำสั่งสำหรับ assembler ใช้เพื่อกำหนด segments, procedures, define symbols, allocate memory. ส่วนมากไม่ถูก generate เป็น object code

Macro Assembler has around 60 directives

## Directive `DB` `DW` `DD`

1. `DB` *define byte* each expression **using 1 byte**
2. `DW` *define word* each expression **using 2 bytes**
3. `DD` *define double* *word* each expression **using 4 bytes**

format `[name]? (DB|DW|DD) expression[,expresion]*`

การทำงาน จองเนื้อที่ในความจำโดย `name` เรียกว่าเป็น symbolic reference และสามารถจองโดยให้ค่าเริ่มต้น หรือไม่ให้ก็ได้

```assembly
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

```assembly
B_TAB DB 0,1,2,3     ;byte talbe using 4 bytes
W_TAB DB -124,90,149 ;word talbe using 6 bytes
```

ให้ค่าเดียวกัน ซ้ำกันใช้ `DUP` duplicate operator `n DUP (expresion)?` n ≥ 1

```assembly
DB 4 DUP (0) ;works like `DB 0,0,0,0`
```

1. กรณีการไม่ให้ค่าเริ่มต้น ให้ใช้ `?` แทน expression

```assembly
COUNT DB ?
NUM   DW ?
TAB   DB 49 DUP (?)
```

1. กรณีให้ค่าเริ่มต้นเป็น string ใช้ DB (ไม่ใช้ DW) เพราะแต่ละ character ใช้ 1 byte

```assembly
MSG DB 'MONOSODUIMGLUTAMATE'
FNS DB 'F','E','E','N'
```

## Directive `SEGMENT`, `ENDS`

ใช้เพื่อแบ่ง source program เป็น segment

format 

```assembly
seg-name SEGMENT [align-type][combline-type]['class']
         ...
seg-name ENDS
```

- `align-type` กำหนดขอบเขตที่จะใส่ segment ไปที่ตำแหน่งไหนในความจำ อาจเป็น `PARA` คือ address ที่เป็น  multiple ของ 16 เป็น default, `PAGE` คือ address เป็น multiple ของ 256
- `combine-type` ในการที่จะเชื่อมโยง module เข้าด้วยกัน กำหนดว่า segment ที่จะมารวมกับ segment อื่นที่มีชื่อเดียวกันจะมารวมกันอย่างไร และหากไม่ระบุ `combine-type` แสดงว่าไม่ต้องการนำไปรวมกับ segment อื่น 1. `PUBLIC` ใช้กับ code, data, or extra segment to concatenate  the segment that has a same name into 1 physical segment when it linked 2. `COMMON` ใช้กับ code, data, extra segment เพื่อให้ segment ที่มีชื่อเดียวกันใช้เนื้อที่ร่วมกัน (overlapped) 3. `STACK` ใช้กับ stack segment และทุก stack segment ต้องมี `STACK`
- `'class'`  ใช้กำหนดลำดับการจัดกลุ่ม segment โดย segment ที่มีชื่อ class เหมือนกัน จะเก็บเรียงกันตามลำดับ ถ้าชื่อไม่เหมือนกันจะเก็บตาม linker กำหนด

```assembly
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

```assembly
SSEG SEGMENT STACK
     ...
SSEG ENDS

CSEG SEGMENT
     ASSUME CS:CSEG,SS:SSEG
     ...
CSEG ENDS
```

example 2

```assembly
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

```assembly
CSEG SEGMENT
     ASSUME CS:CSEG,SS:STACK

FIRST PROC
     ...
FIRST ENDP

CSEG ENDS
```

ใช้คำสั่ง `CALL`  เพื่อเรียกใช้ procedure ที่กำหนดเอาไว้ และไม่ต้องกำหนดก่อนที่จะเรียกใช้

```assembly
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

```assembly
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

```assembly
PAGE 60,70 ;60 lines and 70 columns
```

## Directive `TITLE`

ใช้พิมพ์ชื่อในแต่ละหน้าของ listing เหมือนเป็นหัวกระดาษ

format `TITLE text`

1. *text* อักขระยาวได้ 60 อักขระ

```assembly
TITLE sample title
```

## Directive `SUBTTL`

ใช้พิมพ์ชื่อรองในแต่ละหน้าของ listing ต้องมี TITLE ก่อน

format `SUBTTL text`

1. *text* อักขระยาวได้ 60 อักขระ

```assembly
SUBTTL sample subtitle
```

# Instruction Set

ในใช้คำสั่งนั้น ทุก operand ต้องมี addressing mode เพื่อใช้บอกกับ microprocessor ว่าให้ทำงานกับข้อมูลจากที่ใด โดยแบ่งได้ 7 กลุ่ม

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

```assembly
MOV AX,CX
```

### Immediate addressing

format: ค่าคงที่ขนาด 8 หรือ 16 บิตเป็น source opeand

segment register: none

```assembly
MOV CX,59 ;59 is immediate addressing
```

```assembly
MOV CL,'STRING' ;'STRING' is immediate addressing
```

### Direct addressing

format: symbolic reference (คล้าย ๆ ชื่อตัวแปร)

segment register: `DS`

เป็น offset โดย microprocessor จะคำนวณ 20-bit address เติม 0H จาก `DS` แล้วบวกกับ offset

```assembly
MOV AX,TAB ;TAB is direct addressing
```

### Register indirect addressing

ใช้กับ register ที่กำหนดเท่านั้น ใช้เป็นค่า offset

ทำงานกับข้อมูลใน `DS`: `[BX]`, `[DI]`, and `[SI]`

ทำงานกับข้อมูลใน SS: `[BP]`

```assembly
MOV BX,2
MOV AX,[BX] ;offset 2 in AX register
```

### Base relative addressing

| operand format | segment register |
| --- | --- |
| [BX]+displacement | DS |
| [BP]+displacement | SS |

displacement คือ ค่า 16-bit signed displacement value หรือ symbolic reference อาจเขียนได้หลายวิธี เช่น `[BP]+4`, `4[BP]`, `[BP+4]`, `NAME[BP]`

```assembly
MOV AX,[BX]+4
```

```assembly
MOV AX,TABLE[BX]
```

### Direct indexed addressing

| operand format | segment register |
| --- | --- |
| [DI]+displacement | DS |
| [SI]+displacement | DS |

```assembly
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

```assembly
MOV AX,[BX][SI]
```

```assembly
MOV CX,NEXT[BP][SI]
```

## MOV

ใช้คัดลอกข้อมูลจาก source operand ไปที่ destination operand ทั้งสอง operand ต้องมีขนาดเท่ากัน และทุก mode ที่ใช้เพื่อทำงานกับข้อมูลใน data segment ใช้ `DS` เป็น segment register

format `MOV destination,source`

## PUSH

ใช้กับ stack เพื่อเก็บข้อมูลชั่วคราว จะเก็บ content ของ 16-bit register or memory operand at the top of stack

format `PUSH source`

```assembly
PUSH SI ;register addr
```

เอาข้อมูลที่อยู่ใน `SI`  ไปไว้ใน stack โดยที่ข้อมูลใน `SI` ยังคงเหมือนเดิม

```assembly
PUSH DS ;register addr
```

```assembly
PUSH WTAB[BX] ;base relative addr
```

`PUSH` จะเก็บข้อมูลใว้ที่ top of stack หรือที่ location ที่ `SP` ชี้ไป มี `SP` เป็น offset ของ stack segment เริ่มจากมากไปน้อย โดย `SP` จะชี้ไปที่ word ที่เก็บใน stack ล่าสุด เวลาที่ PUSH มันจะ -2 จาก `SP` แล้วจึงจะเก็บข้อมูล

เช่นเมื่อ `PUSH BX` จากที่ SP pointing to `01FE`,  มันก็เลื่อนไปที่ `01FC` แล้วจึงเก็บข้อมูล

ส่วน `SS` จะเก็บ address เริ่มต้นของ stack

ตัวอย่างการจอง stack segment ขนาด 128 words ว่าง ๆ เพื่อเก็บข้อมูล

```assembly
STACK SEGMENT STACK 'STACK'
      DW 128 DUP (?)
STACK ENDS
```

## POP

คัดลอกข้อมูลจาก the top of stack ไปไว่ที่ memory location หรือ register และ `SP` จะ +2

format `POP destination`

```assembly
PUSH AX
    ...
POP AX
```

```assembly
POP [SI] ;เก็บที่ DS ตำแหน่งที่ SI
```

## XCHG

ใช้สลับข้อมูลใน destination กับ source ระหว่าง registers ที่ไม่ใช่  segment registers หรือระหว่าง register กับ memory location สำหรับคำสั่งนี้ไม่มีผลต่อ status flags

format `XCHG destination,source`

```assembly
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

```assembly
ADD AX,BX
```

```assembly
ADD AX,MEM_WORD
```

```assembly
ADD MEM_WORD,AX
```

```assembly
ADD AL,10
```

```assembly
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

```assembly
MUL CX ;CX * AX
MUL MBYTE ;MBYTE * AL
```

```assembly
MOV CL,2
MOV AL,3
MUL CL ;CL * AL
```

## IMUL

integer multiply signed

format `IMUL source`

```assembly
IMUL DL ;DL*AL
IMUL MWORD ;MWORD*AX
```

```assembly
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

```assembly
DIV SI ;(DX,AX)/SI
DIVE AX;AX/MBYTE
```

```assembly
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

```assembly
MOV CL,5BH
DEC CL ;CL=5AH
```

## INC

increment destination value by 1 มีผลกับ OF, SF, ZF, AF, PF

format `INC destination`

```assembly
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

```assembly
MOV BL,32
CMP BL,20
```

```assembly
MOV BX,123
MOV AX,32
CMP AX,BX ;32 CMP to 123
```
