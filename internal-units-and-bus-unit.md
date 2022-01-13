# Internal Units and Bus Unit

### internal units

8086/8088 ทำงามตามคำสั่งโดยมี bus unit, instruction unit และ execution unit ซึ่งเป็น special-purpose units ในชิปเป็นตัวช่วย

### bus unit

อ่านคำสั่งจาก memory

ส่งผ่านข้อมูลระหว่าง processor กับภายนอก

นำคำสั่งไปเก็บใน code queue เพี่อให้ instruction unit นำไปใช้ 

### instruction unit

นำคำสั่งจาก bus unit code queue แปลคำสั่ง แล้วนำคำสั่งไปเก็บใน instruction queue หรือ pipeline

### execution unit (EU)

ทำงานตามคำสั่ง (execute) ที่อยู่ใน instruction queue

### instruction pointer (IP)

is register

stores offset of instruction that `EU` will execute next