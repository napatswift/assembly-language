# ASM
Assembly Language

## Register

[Register](register.md)

## Units

[Internal Units and Bus Unit](internal-units-and-bus-unit.md)

## Intruction

คำสั่งในภาษาแอสเซมบลี

[Instruction](instruction)

## Directive

[Directive](directive.md)

## Addressing Mode

[Addressing Mode](addressing-mode.md)

## Operator pointer

ใช้ในกรณีที่ operand เป็น memory location และไม่สามารถบอกขนาดของ operand ได้ จึงใช้ `PTR` เป็นตัวกำหนดขนาดของ operand ว่ามีขนาดเป็น byte หรือ word

format `(BYTE|WORD) PTR expression`

```asm
NEG [SI]
CMP [BX],37H
```

แต่ไม่รู้ขนาดว่าเป็น byte หรือ word จึงใช้ `PTR`

```asm
NEG BYTE PTR [SI]
CMP WORD PTR [BX],37H
CMP BYTE PTR [BX],37H
```

## Load Effective Address

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
