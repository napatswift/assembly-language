# Procedure

เป็นกลุ่มของคำสั่งเพื่อทำงานบางอย่าง (คล้าย function) และคำสั่งเกี่ยวกับ procdure มีดังนี้

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
