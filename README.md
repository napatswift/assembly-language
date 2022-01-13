# ASM

Content

[Register](ASM%20517b6b565be14944823217be2eb62014/Register%204a32276b12e64566a0a6b8c4d008a9ce.md)

[Internal Units and Bus Unit](ASM%20517b6b565be14944823217be2eb62014/Internal%20Units%20and%20Bus%20Unit%201e0e5aa1834f4f1cb4949e7458a71016.md)

[Instruction](ASM%20517b6b565be14944823217be2eb62014/Instruction%20aa065a9caf6a4fbcb8e4e2a01c397f02.md)

[Directive](ASM%20517b6b565be14944823217be2eb62014/Directive%20b82ba5c1e47446539817982c738781e3.md)

[Addressing Mode](ASM%20517b6b565be14944823217be2eb62014/Addressing%20Mode%2053a46008d6e84148b9cfc76caa15d22c.md)

[Instruction Set](ASM%20517b6b565be14944823217be2eb62014/Instruction%20Set%20031628dbe197471283dac1d54c7a5ac2.md)

[Arithmetic Instructions](ASM%20517b6b565be14944823217be2eb62014/Arithmetic%20Instructions%2031a7b6caa4484d1aa7a8eee1384ffad8.md)

[control-transfer-intructions](ASM%20517b6b565be14944823217be2eb62014/control-transfer-intructions%20592853bcda8a4fde9bf7770c974ea071.md)

## Operator pointer

ใช้ในกรณีที่ operand เป็น memory location และไม่สามารถบอกขนาดของ operand ได้ จึงใช้ `PTR` เป็นตัวกำหนดขนาดของ operand ว่ามีขนาดเป็น byte หรือ word

format `(BYTE|WORD) PTR expression`

```
NEG [SI]
CMP [BX],37H
```

แต่ไม่รู้ขนาดว่าเป็น byte หรือ word จึงใช้ `PTR`

```
NEG BYTE PTR [SI]
CMP WORD PTR [BX],37H
CMP BYTE PTR [BX],37H
```

## Load Effective Address

ใช้โหลดค่าออฟเซ็ตไว้ใน register ขนาด 16 บิต

 format `LEA register,memory-location`

```
MOV BX,8
LEA DI,TAB[BX]
```

```
MOV BX,10
MOV DI,4
LEA SI,[BX][DI]+7
```

หลังจบคำสั่ง `SI` จะมีค่า offset คือ 10 + 4 + 7 = 21