# ASM

Assembly Language

- [ASM](#asm)
  - [MASM](#masm)
  - [Source Satement](#source-satement)
  - [Constants](#constants)
  - [Register](#register)
  - [Intruction](#intruction)
  - [Directive](#directive)
  - [Addressing Mode](#addressing-mode)
  - [File handling](#file-handling)
    - [การเปิดแฟ้ม (Open a file)](#การเปิดแฟ้ม-open-a-file)
    - [สร้างแฟ้ม (creat a file)](#สร้างแฟ้ม-creat-a-file)
    - [การปิดแฟ้ม (close a file)](#การปิดแฟ้ม-close-a-file)
    - [บันทึกลง file (write a file)](#บันทึกลง-file-write-a-file)
    - [อ่านจาก file (read a file)](#อ่านจาก-file-read-a-file)
    - [ลบ file (delete a file)](#ลบ-file-delete-a-file)
  - [External Reference Directives](#external-reference-directives)
    - [directive PUBLIC](#directive-public)
    - [directive INCLUDE](#directive-include)
    - [directive EXTRN](#directive-extrn)
  - [SYMDEB](#symdeb)

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

## Register

[Register](register.md)

## Intruction

คำสั่งในภาษาแอสเซมบลี

[Instruction](instruction)

## Directive

[Directive](directive.md)

## Addressing Mode

[Addressing Mode](addressing-mode.md)

## File handling
- ใช้ Dos interupt 21H โดยเรียกใช้ routine ต่าง ๆ
- ใซ้ชื่อของไฟล์จบด้วยเลข 0 เช่น `FNAME DB 'C:\DATA\PROG.DAT',0` ไม่จำเป็นต้องระบุ drive หรือ path ก็ได้เป็นแค่ชื่อไฟล์เฉย ๆ
- โดยที่ DOS จะเรียก string ที่ลงท้ายด้วยเลข 0 ว่า ASCII**Z** String หมายถึง 0 byte ตอนจบ

### การเปิดแฟ้ม (Open a file)
ให้ `AH = 3DH` เพื่อเปิดชื่อแฟ้มที่มีอยู่แล้ว และชื่อของแฟ้มให้เก็บใน `DS:DX` และ ใน `AL` ให้ใส่ Access code

| AL  | การทำงาน  |
| --- | ---  |
|  0  | เพื่ออ่าน |
|  1  | เพื่อเขียน |
|  2  | เพื่ออ่านและเขียน |

หลังการทำงานของ Routine `AX` จะมีตัวจัดการแฟ้ม (File Handle) ซึ่งเป็นตัวเลขที่ DOS กำหนดให้กับแฟ้มนี้

ถ้า `CF = 1` แสดงว่ามีข้อผิดพลาด (error) และใน `AX` จะมีรหัสข้อผิดพลาด

ตัวอย่างการเปิดแฟ้ม

```asm
HANDLE DW ?
FNAME  DB 'FILENAME.FN',0
...
    MOV  AH,3DH    ;OPEN FILE
    MOV  AL,0      ;TO READ
    LEA  DX,FNAME  ;STORE OFFSET OF A FILE NAME INTO DX
    INT  21H       ;CALL INTERUPT
    JC   WRONG     ;DOES IT HAS AN ERROR
    MOV  HANDLE,AX ;FILE HANDLE IN AX
```

### สร้างแฟ้ม (creat a file)
สำหรับการสร้างแฟ้มใหม่ให้กำหนด `AH=3CH` เพื่อสร้างแฟ้มใหม่หรือลบแฟ้มเดิมให้มีความยาวเป็น 0 (0 length) เพื่อบันทึกข้อมูลลงในแฟ้มโดยใส่ชื่อแฟ้มใน `DS:DX` และใล่ลักษณะประจำของแฟ้ม (file attribute) ใน `CX`

| CX  | ลักษณะประจำ  |
| --- | ---  |
| 0 | normal |
| 1 | read only |
| 2 | hidden |
| 4 | system file |

หลังจากการทำงานของ routine แล้้ว AX จะมีตัวจัดการแฟ้ม (File Handle) แฟ้มที่ถูกสร้างใหม่มีรหัสการเข้าถึงเป็นอ่านและเขียน

ถ้า `CF = 1` แสดงว่ามีข้อผิดพลาด และใน `AX` จะมีรหัสข้อผิดพลาด

หลักการทํางานของ routine คือ `AX` จะมี file handle ต่อมา file จะถูกสร้างโดยมี read/write access code ถ้า `CF` = 1 แสดงว่า เกิด error `AX` จะมี error code

```asm
HANDLE DW ?
FNAME DB '...', 0
...
    MOV AH,3CH     ; create file
    MOV CX,0       ; file attribute=normal
    LEA DX,FNAME
    INT 21H
    JC WRONG
    MOV HANDLE,AX  ; file handle in AX
```

### การปิดแฟ้ม (close a file)
ปิด file โดยให้ `AH=3EH`
ใช้ปิด file ที่เปิดไว้ หรือสร้างไว้โดยใส่ file handle ใน `BX`
ต้องปิด file ที่ใช้บันทึกข้อมูลเพื่อให้ข้อมูลทั้งหมดถูกบันทึกลง file

หลักการทำงานของ routine ถ้า `CF` = 1 แสดงว่า มี error และ `AX` จะมี error code

```asm
HANDLE DW ?
...
    MOV AH,3EH
    MOV BX,HANDLE ; handle of existing file
    INT 21H
    JC WRONG
```

### บันทึกลง file (write a file)

บันทึกลง file โดยให้ `AH=40H`
ใช้บันทึกข้อมูลลง file ที่เปิ ดไว้แล้ว โดย ใส่ file handle ใน BX. DS:DX มี address ของข้อมูลที่ต้องการบันทึกลง file. CX มีจำนวน byte ที่ต้องการบันทึกลง file

หลังการทำงานของ routine
AX จะมีจำนวน byte ที่บันทึกลง file
- ถ้า AX < CX แสดงว่าบันทึกข้อมูลไม่หมด อาจมี error
- ถ้า AX = 0 แสดงว่า disk full
- ถ้า CF = 1 แสดงว่า มี error และ AX จะมี error code
file pointer จะเลื่อนไปที่ตำแหน่งต่อไป

```asm
BUF DB 124 dup(?)
HANDLE DW ?
...
    MOV AH,40H ; write file
    MOV BX,HANDLE ; handle of file that is already opened
    MOV CX,124 ; number of characters to be writen
    LEA DX,BUF
    INT 21H
    CMP AX,124 ; write entire BUF
    JNE FULL
```

### อ่านจาก file (read a file)
อ่านจาก file โดยให้ `AH=3FH`
ใช้อ่านข้อมูลจาก file ที่เปิดไว้แล้วหรือสร้างไว้แล้ว โดย

ใส่ file handle ใน `BX`
`DS:DX` มี address ของ buffer เพื่อเก็บข้อมูล
`CX` มีจำนวน byte ที่ต้องการอ่าน

หลังการทำงานของ routine
AX จะมีจำนวน byte ที่อ่าน
- ถ้า AX = 0 แสดงว่าอ่าน past end of file
- ถ้า CF = 1 แสดงว่า มี error และ AX จะมี error code file pointer จะเลื่อนไปที่ตำแหน่งต่อไป

```asm
BUF DB 124 DUP(?)
HANDLE DW ?
...
    MOV AH,3FH
    MOV BX,HANDLE ; handle of file that is already opened
    MOV CX,124 ; number of characters
    LEA DX,BUF ; read buffer
    INT 21H
    JC WRONG
```

### ลบ file (delete a file)
ลบ file โดยให้ `AH=41H`
ใช้ลบ file ที่ไม่ต้องการใช้แล้ว โดย `DS:DX` มี address ของ ASCIIZ ชื่อ file ที่ต้องการลบ

หลังการทำงานของ routine
ถ้า CF = 1 แสดงว่า มี error และ AX จะมี error code

```asm
FNAME DB 'C:\DATA\PROG.DAT', 0
...
    MOV AH,41H ; delete a file
    LEA DX,FNAME ; address of ASCIIZ filename
    INT 21H
    JC  WRONG ; error
```

## External Reference Directives
directive PUBLIC, EXTRN, INCLUDE ใช้เพื่อ link modules เข้าด้วยกัน

### directive PUBLIC
รูปแบบ `PUBLIC symbol[,…]`
ใช้เพื่อให้ module อื่น สามารถใช้ symbol ใน directive PUBLIC ได้ symbol อาจเป็น symbolic reference,ชื่อprocedure
### directive INCLUDE
รูปแบบ `INCLUDE filespec ;filename.ext`
ใช้เพื่อนำ source file มารวมกับ current source file เวลา Assemble โปรแกรม
### directive EXTRN
รูปแบบ `EXTRN name:type[,…]`
ใช้คู่กับ PUBLIC เพื่อบอก linker ว่าต้องการใช้ name ซึ่ง defined ใน module อื่น name ต้องอยู่ใน PUBLIC ของ module อื่น
  - ถ้า name เป็น symbol ใน data หรือ extra segment type อาจเป็น BYTE, WORD หรือ DWORD
  - ถ้า name เป็นชื่อ procedure type อาจเป็น NEAR หรือ FAR

เวลาจะ link กับ module อื่น ต้องใส่ชื่อ procedure ไว้ใน directive PUBLIC เพื่อให้ procedure อื่นเรียกใช้ได้

global data หรือ procedure ที่ procedure นี้จะ เรียกใช้ต้องใส่ใน directive EXTRN

ใส่ directive PUBLIC, EXTRN, INCLUDE นอก segment

## SYMDEB

[Symbolic Debugger](symdeb.md)
