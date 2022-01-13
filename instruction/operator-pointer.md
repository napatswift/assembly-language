# Operator pointer

ใช้ในกรณีที่ operand เป็น memory location และไม่สามารถบอกขนาดของ operand ได้ จึงใช้ `PTR` เป็นตัวกำหนดขนาดของ operand ว่ามีขนาดเป็น byte หรือ word

format `(BYTE|WORD) PTR expression`

เช่น

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