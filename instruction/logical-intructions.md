# Logical Instruction

ทำงานกับ bits ของ operands

1. AND
2. OR
3. XOR

format `AND destination,source`

ีมีผลต่อ flags `SF`, `ZF`, `PF` โดย `OF`=0 `CF`=0 `AF`=undifined


4. NOT

format `NOT destinatin`

5. TEST ทำงานเหมือน AND แต่จะไม่เปลี่ยน destination operand และใช้เพื่อ set flags