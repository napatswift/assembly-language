# Control Transfer Instructions

คำสั่งที่ใช้ในการควบคุมการไหลของโปรแกรม แบ่งได้เป็น condtitional transfer และ unconditional transfer

1. JMP

เป็นแบบไม่มีเงื่อนไข (unconditional transfer) และไม่มีผลต่อ flags

format `JMP label`

```
JMP NEXT
    ...
NEXT: ...
```

```
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

```
ADD AL,CL
JC BIG
```

```
ADD BX,AX
JO OVER
```

```
SUB AL,BL
JZ NULL
```

```
DEC COUNT
JNZ MORE
```

```
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

```
CMP BX,CX ;unsigned
JA BX_MORE
```

```
CMP AL,BL ;signed
JG ...
```

```
CMP CX,123 ;signed
JLE ..
```

```
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

```
	MOV CX,10
NEXT: ...
	...
	LOOP NEXT ;
```

จะจบการวนรอบ ถ้าใน `CX` เป็น 0
