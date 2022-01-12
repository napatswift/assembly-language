# Control Transfer Instructions

คำสั่งที่ใช้ในการควบคุมการไหลของโปรแกรม แบ่งได้เป็น condtitional transfer และ unconditional transfer

## JMP

เป็นแบบไม่มีเงื่นไข (unconditional transfer) และไม่มีผลต่อ flags

format `JMP label`

```assembly
JMP NEXT
    ...
NEXT: ...
```

```aseembly
BACK: ...
      ...
      JMP BACK
```
## Conditional Transfer

เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

### JA
"jump if above" มากกว่า

format `JA label`

### JNBE
"jump if not below nor equal" ไม่น้อยกว่าเท่ากับ

format `JNBE label`

### JAE
"jump if above or equal" มากกว่าหรือเท่ากับ

format `JAE label`

### JNB
"jump if not below" ไม่น้อยกว่า

format `JNB label`

### JB
"jump if below" มากกว่า

format `JB lable`

### JNAE
"jump if not above or equal" ไม่มากกว่าเท่ากับ

format `JNAE label`

### JC
"jump if carray" ถ้า CF=1

format `JC label`

### JNC
"jump if not carry" ถ้า CF=0

format `JNC label`

### JBE
"jump if below or equal" ถ้า C=1 or ZF=1

format `JBE label`

### JNA
"jump if not above" ถ้า C=1 or ZF=1

format `JNA label`

### JCXZ
"jump if CX is zero" ถ้า CX=0

format `JCXZ label`

### JE
"jump if equal" ถ้า ZF=1

format `JE label`

### JZ
"jump if zero" ถ้า ZF=1

format `JZ label`

