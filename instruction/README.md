# Instruction

format

```
[label:]? mnemonic [operand]? [;comment]?
```

- อยู่ที่ column ไหนก็ได้
- แยกกันอย่างน้อย 1 วรรค
- ไม่ยาวเกิน 132 columns

### Label

- ใช้เวลามีการอ้างอิงคำสั่ง
- ยาวได้มากสุด 31 characters
- ลงท้ายด้วย `:`
- `A-Z 0-9 ? . @ _ $`
- ห้ามขึ้นต้นด้วยตัวเลข
- หากมี `.` ต้องให้เป็นตัวแรก
- ห้ามซ้ำกับ registers

### Mnemonic

- 2-7 characters
- บางคำสั่งต้องมี operand ตามหลัง

### Operand

- บอกกับ microprocessor ว่าต้องทำงานกับข้อมูลจากใหน ไปเก็บไว้ที่ไหน
- อาจมี 0 1 หรือ 2 operands
- สำหรับ 2 operands ตัวแรกเป็น destination และตัวที่สองเป็น source

### Comment

- start with `;`
- a line that start with `;` is a comment line

## Content
  1. [Instruction Set](instruction-set.md)
  2. [Arithmetic Instructions](arithmetic-instructions.md)
  3. [Control Transfer Intruction](control-transfer-intructions.md)
  4. [Load Effective Address](load-effective-address.md)
  5. [Operator Pointer](operator-pointer.md)