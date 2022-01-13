# Instruction Set

## MOV

ใช้คัดลอกข้อมูลจาก source operand ไปที่ destination operand ทั้งสอง operand ต้องมีขนาดเท่ากัน และทุก mode ที่ใช้เพื่อทำงานกับข้อมูลใน data segment ใช้ `DS` เป็น segment register

format `MOV destination,source`

## PUSH

ใช้กับ stack เพื่อเก็บข้อมูลชั่วคราว จะเก็บ content ของ 16-bit register or memory operand at the top of stack

format `PUSH source`

```asm
PUSH SI ;register addr
```

เอาข้อมูลที่อยู่ใน `SI`  ไปไว้ใน stack โดยที่ข้อมูลใน `SI` ยังคงเหมือนเดิม

```asm
PUSH DS ;register addr
```

```asm
PUSH WTAB[BX] ;base relative addr
```

`PUSH` จะเก็บข้อมูลใว้ที่ top of stack หรือที่ location ที่ `SP` ชี้ไป มี `SP` เป็น offset ของ stack segment เริ่มจากมากไปน้อย โดย `SP` จะชี้ไปที่ word ที่เก็บใน stack ล่าสุด เวลาที่ PUSH มันจะ -2 จาก `SP` แล้วจึงจะเก็บข้อมูล

เช่นเมื่อ `PUSH BX` จากที่ SP pointing to `01FE`,  มันก็เลื่อนไปที่ `01FC` แล้วจึงเก็บข้อมูล

ส่วน `SS` จะเก็บ address เริ่มต้นของ stack

ตัวอย่างการจอง stack segment ขนาด 128 words ว่าง ๆ เพื่อเก็บข้อมูล

```asm
STACK SEGMENT STACK 'STACK'
      DW 128 DUP (?)
STACK ENDS
```

## POP

คัดลอกข้อมูลจาก the top of stack ไปไว่ที่ memory location หรือ register และ `SP` จะ +2

format `POP destination`

```asm
PUSH AX
    ...
POP AX
```

```asm
POP [SI] ;เก็บที่ DS ตำแหน่งที่ SI
```

## XCHG

ใช้สลับข้อมูลใน destination กับ source ระหว่าง registers ที่ไม่ใช่  segment registers หรือระหว่าง register กับ memory location สำหรับคำสั่งนี้ไม่มีผลต่อ status flags

format `XCHG destination,source`

```asm
XCHG AX,BX ;word registers
XCHG AH,BL ;byte registers
XCHG AX,MWORD ;register and memory location
```