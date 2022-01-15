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