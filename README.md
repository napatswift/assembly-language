# ASM

Assembly Language

## MASM

MASM ย่อมาจาก Macro Assembler Language จาก Microsoft

assemble → *object code* → link → *executable program*

programmer *-assembly-lang-program→* assembler *-object-prog→* liker *-run-module→* execute

## Source Satement

คำสั่งในภาษา Assembly 

- **Assembly language** เป็นคำสั่งของ processor ที่จะทำตอน execute
- **Assembler directive** หรือ pseudo-operation เป็นคำสั่งของ assembler ที่ทำตอน assemble

## Constants

ค่าคงที่ในภาษา

1. binary constant: sequence of `01` followed by B e.g. 1010B
2. decimal constant: sequence of `0-9`  optionally followed by D e.g. 12D or 12
3. hexadecimal constant: sequence of `0-9A-F` followed by H and MUST start with `0-9` e.g. 12H, 0A4H 
4. character and string: sequence of letters, numbers, or special characters that surrounded by `'` or `"` e.g. ‘a’, “drop”

ค่าคงที่จำนวนลบ

1. decimal: add - in front of a number
2. binary and hexadecimal: in [2’s complement](https://en.wikipedia.org/wiki/Two%27s_complement)

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
