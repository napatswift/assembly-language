# Interrupt Intruction

ใช้เรียก interrupt routine โดยที่ใช้ interrupt type เป็นค่า 0-255 ในฐาน 10 หรือ 0-FFH มาคำนวณหาที่อยู่ของ interrupt vector ซึ่งเป็นที่เก็บที่อยู่ของ interrupt routine มีการทำงานคือ (1) Push flag register ไปใน stack ดังนั้นต้องมี stack segment ในโปรแกรม ต่อมา (2) clear `TF` และ `IF` เพื่อป้องกันการทำงานแบบ single-step และไม่ให้เกิด interrupt อื่น ๆ (3) push `CS` ใน stack (4) คำนวณหา address ของ interrupt vector โดยคำนวณจาก interrupt-type (5) โหลด word ที่ 2 ของ intrrupt vector ใน `CS` จากนั้น (6) push `IP` ลงใน stack และ (7) โหลด word แรกของ interrupt vector ใน `IP`

หลังจากทำงานใน interrupt routine จบหรือในอีกแง่คือ execute ไปถึงคำสั่ง `IRET` (interrupt return) จะ pop return address จาก stack แล้วทำงานต่อ

format `INT interrupt-type`

interupt-type คือ 0-255 หรือ 0-FFH

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
