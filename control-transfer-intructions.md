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

## JA
"jump if above" เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

## JNBE
"jump if not below nor equal" เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

## JAE
"jump if above or equal" เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

## JNB
"jump if not below" เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

## JB
"jump if below เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags

## JNAE
"jump if not above or equal" เป็นแบบมีเงื่ยนไข (conditional transfer) และไม่มีผลต่อ flags
