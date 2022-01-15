# SYMDEB
 ใช้ SYMDEB เพื่อ run โปรแกรมทีละคำสั่ง หรือแสดงข้อมูลที่ที่อยู่ที่ต้องการ หรือหยุดการทำงานของโปรแกรม

```sh
SYMDEB filename.exe
```

## G
format `G [offset]?[,offset]*`

ตัวอย่าง

```sh
SYMDEB filename.exe
G
```

```sh
SYMDEB filename.exe
G A
```

คำสั่งจากข้างบนจะทำงานจนถึงคำสั่งที่ offset 0AH

## T และ P

ececute 1 คำสั่งหรือมากกว่า แล้วแสดง content ของ registers และ flags

format `T [instruction-count]?`

ตัวอย่าง

```sh
SYMDEB filename.exe
T
```

```sh
SYMDEB filename.exe
T 5
```
ด้านบนจะทำ 5 คำสั่งถัดไป

สำหรับ P จะนับ procedure calls, interrupts, loop instructions, และ repeat string instructions เป็นเหมือน 1 คำสั่ง

format `P [instruction-count]?`

## Q

ออกจากโปรแกรม

format `Q`

## D

แสดงข้อมูลตัวเลขใน memory location

format `D (DS|SS|ES):offset [offset]?`

`D (DS|SS|ES):offset L byte_count`

ตัวอย่าง

แสดงข้อมูลจากตำแหน่งเริ่มต้นของ data segment (`DS`)

```sh
SYMDEB multiply.exe
D DS:0
```

แสดงข้อมูลจากตำแหน่ง 10H ถึง 1A ของ data segment (`DS`)

```sh
SYMDEB multiply.exe
D DS:10 1A
```

# R

จะแสดงข้อมูลของ 16-bit register หรือเปลี่ยนค่าใน 16-bit register [ดูที่นี่](register.md)

format `R [register-name [value]?]?`

ตัวอย่าง

แสดง content ของทุก registers

```
SYMDEB cal.exe
R
```

แสดง content ของ AX

```
SYMDEB cal.exe
R AX
```

ใส่ค่า 5DH ไว้ใน AX

```
SYMDEB cal.exe
R AX 5D
```

## เริ่มกันเลย

```
> masm filename
  ...
Object filename [filename.OBJ]:
source listing [NUL.LST]: filename.lst
Cross-reference [NUL.CRF]:
 ...

> link filename
  ...
Run File [finename.EXE]:
List File [NUL.MAP]:
Libraries [.LIB]:

> symdeb filename.exe
  ...
Processor is [80286]
-r
AX=FFFF BX=0000 CX=00E5 DX=0000 SP=0000 BP=0000 SI=0000 DI=0000
DS=0AA7 ES=0AA7 SS=0AB7 CS=0AC0 IP=0000 NV UP EI PL NZ NA PO NC
0ACO:0000 B8F0A        MOV   AX,0ABF
```

จาก

```
AX=FFFF BX=0000 CX=00E5 DX=0000 SP=0080 BP=0000 SI=0000 DI=0000
DS=0AA7 ES=0AA7 SS=0AB7 CS=0AC0 IP=0000 NV UP EI PL NZ NA PO NC
0ACO:0000 B8BF0A        MOV   AX,0ABF
```

`SP=0080` เนื่องจากว่าเราจองเนื้อที่ให้จำนวน 64 words ซึ่งเท่ากับ 128 bits หรือ 80H bits ดังนั้น SP จะไปอยู่ที่บนสุดของ stack หรือที่ 80 ในกรณีนี้




