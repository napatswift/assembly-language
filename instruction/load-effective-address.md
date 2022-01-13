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