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

