# สถาปัตยกรรมคอมพิวเตอร์
สถาปัตยกรรมคอมพิวเตอร์ (Computer Architecture) คือการออกแบบของคอมพิวเตอร์มี instruction set, hardware components, and system organization มี 2 ส่วนคือ
1. Instruction-set architecture (ISA) กำหนด conputational charactistics ของ computer และ ISA จะประสบความสำเร็จได้ต้องมีหลากหลาย implementations เช่น personal computor มีหลาย specification คือมีหลายขนาด, performance, reliabilities, และ HSA) แต่มี ISA เหมือนกัน คือเข้าใจคำสั่งเหมือนกันแม้ว่า hardware จะต่างกันก็ตาม อย่างน้อยในตระกูล (computer-family architecture) เดียวกัน
2. Hardware-system architecture (HSA) จัดการกับ hardware subsystems รวม central processing unit (CPU), storage system หรือ memory, และ input-output system (I/O) ดังนั้น HSA จะกำหนดประสิทธิภาพของคอมพิวเตอร์

Computer-family architecture หมายถึง set of implementation ที่มี ISA เหมือนกันหรือคล้ายกัน

Computer architects หรือสถาปนิกคอมพิวเตอร์มีหน้าที่สร้างตระกูลของคอมพิวเตอร์โดยใช้หลากหลายเทคโนโลยี หลายขนาดของความจำ และความเร็วต่างกัน ทำให้ performance สำหรับแต่ละโมเดลในตระกูลเดียวกันแตกต่างกัน แต่สามารถใช้งานโปรแกรมเดียวกันได้

Compatiblitity คือความสามารถของคอมพิวเตอร์ที่ต่างกันในการใช้งานโปรแกรมเดียวกัน สถาปนิกโดยทั่วไปออกแบบสำหรับการเข้ากันได้กับรุ่นหน้า (upward compatibility) คือการออกแบบใซ้รุ่นหน้าหรือรุ่นถัดไปสามารถใช้งานโปรแกรมได้ ซึ่งทำให้สมาชิกในตระกูลที่มีประสิทธิภาพสูงกว่า (heigh-performace family) สามารถทำงานโปรแกรมเดียวกันกับสมาชิกตระกูลที่มีประสิทธิภาพน้อยกว่า (lower-performance member)

# Historical perspective
เทคโนโลยีคอมพิวเตอร์ (computer technology) มีการพัฒนาอย่างรวดเร็ว ตั้งแต่ปลายปี ค.ศ. 1940s เมื่อ John Atanasoff ที่ Iowa State University สร้าง special – purpose electronic digital computer ชื่อ ABC
first-generation computer เป็น Laboratory machines
ENIAC ซึ่งไม่ใช่ stored-program computer (University of Pennsylvania) EDSAC (University of Cambridge) MARK-I, MARK-II, MARK-III, MARK-IV (Harvard University) MARK-I, MARK-II ใช้ electromechanical relays แต่
คอมพิวเตอร์ส่วนใหญ่ในยุคนั้นใช้ vacuum-tube และยังไม่มี transistor
Univac ซึ่งเป็นคอมพิวเตอร์ผลิตเพื่อการค้าเครื่องแรกใช้ vacuum-tube ทําให้เปลืองไฟ และ เกิดความร้อนสูง ในปี ค.ศ. 1948 John Bardeen, Walter Brattain และ William Shockley ที่ Bell Laboratories สร้าง transistor
transistors ใช้พลังงานน้อยกว่า vacuum-tube มีขนาดเล็กกว่าและ น่าเชื่อถือกว่า (more reliable)

ต้นปี ค.ศ.1960s คอมพิวเตอร์ใช้ transistor technology ซึ่งเป็น second-generation computer
นอกจากใช้ transistors แล้วยังใช้ magnetic-core memory ซึ่ง สามารถเก็บข้อมูลโดยไม่ต้องใช้ไฟฟ้า เพราะcore memory เป็น permanent magnets แต่ core memory มี limited access time และมีราคาแพง
เนื่องจากคอมพิวเตอร์มีราคาแพงดังนั้นต้องใช้ CPU time อย่างคุ้มค่า เช่นมี special I/O processors เพื่อที่ CPU จะได้ไม่ต้องทํา I/O operations
batch-processing operating systems ป้องกัน operator intervention โดย load และ execute program แบบอัตโนมัติ
multi-programming operating systems ใช้ CPU อย่างมี
ประสิทธิภาพ โดยโปรแกรมหนึ่งอาจใช้ CPU ขณะท่ีอีกโปรแกรมหน่ึงรอ I/O operations

third-generation computer ใช้ small-scale integration (SSI) ซึ่งมีหลาย transistors บน chip เดียวและใช้ solid-state หรือ magnetic-core memory กับ batch operating systems ประมาณปี ค.ศ.1963 – 1975

ประมาณ ค.ศ.1973 Intel Corporation ผลิต microprocessor ตัว แรก ซึ่งมี CPU ทั้งหมดบน chip ตัวเดียว ค.ศ. 1990 โดยใช้ very-large-scale integration (VLSI) บริษัท Intel ผลิต CPU chip ซึ่งประกอบด้วยมากกว่า 1 ล้าน transistors
fourth-generation computer มีmemory ที่มีความน่าเชื่อถือ มากขึ้น มีความเร็วมาก มีราคาถูกลงมาก มี virtual memory สามารถรองรับการ โปรแกรมท่ีหลากหลายโดยใช้ VLSI components, solid-state memories
และ time-sharing operating systems

computer ประกอบด้วย

-	CPU ที่ประกอบด้วย
      - control unit ซึ่งควบคุมการทํางานของ computer
      - arithmetic and logic unit (ALU) ทํา arithmetic, logical และ shift operations
      - register set ซึ่งเป็นที่เก็บของมูลในขณะที่คอมพิวเตอร์ทํางาน
      - program counter(PC) บางทีเรียก Instruction Pointer(IP) เป็นท่ีเก็บ address ของคําสั่ง โดย PC เป็นส่วนของ register set
-	main-memory
-	I/O system

สามารถเก็บโปรแกรมได้ และทํางานตามคําสั่งในโปรแกรมได้

Instructions
คําสั่งทุกคําสั่งมี instruction fields เพื่อบอกรายละเอียดกับ control unit และทุกคําสั่ง มี instruction forma9 t

instruction size บอกขนาดของคําสั่ง ท่ัว ๆ ไปมีหน่วยเป็น byte
คําสั่งที่ทํางานกับข้อมูล(data) โดยมี data เป็น operand ของ operation และ sequence ของ data ที่ CPU ทํางานด้วย เรียก data
stream

instruction set คือ set ของคําสั่งที่ computer สามารถ execute
ทุกคําสั่งมี operation code (op code) เป็นตัวบอกว่าจะทํา operation อะไร ส่วนอื่นของคําสั่งกําหนดว่าใช้ register ไหนหรือบอก operation address specification คําสั่งอาจ set status bits

โปรแกรม คือ sequence ของคําสั่ง แต่ละคําสั่งมีลําดับในโปรแกรมเรียก logical address เม่ือโปรแกรมอยู่ใน main memory แต่ละคําสั่งมี physical address

ลําดับคําสั่งที่ถูก execute อยู่ใน instruction stream
ในการทํางาน control units ทํา operation 2 operations ได้แก่ instruction fetch และ instruction execution เรียก machine
cycle
ใน instruction fetch control unit นําคําสั่งที่จะทําเป็นคําสั่งถัดไป
จาก main memory โดยนํา address ของคําสั่งซึ่งอยู่ใน PC (program
counter)
PC เก็บ address ของคําสั่งที่จะทําเป็นคําสั่งถัดไป
เมื่อได้ address ของคําสั่งแล้ว control unit จะ execute คําสั่ง เมื่อทําคําสั่งเสร็จก็จะเริ่ม fetch-execute cycle สําหรับคําสั่งถัดไป

# หมวดหมู่ computer architecture
อาจจัดหมวดหมู่ computer architecture ได้ดังนี้
 - single instruction stream, single data stream (SISD) computer มี 1 CPU ที่ execute ทีละคําสั่ง และ fetch หรือ store 1 data item ต่อครั้ง 
 - single instruction stream, multiple data stream (SIMD) computer มีมากกว่า 1 processing element โดย control unit ส่ง control signal สําหรับ processing element เพื่อทํา operation เหมือนกันกับข้อมูลต่างกัน (different data items)
 - multiple instruction stream, single data stream (MISD) ทําหลายโปรแกรมกับ data item เดียวกัน
 - multiple instruction, multiple data stream (MIMD) เรียก multiprocessors มีมากกว่า 1 independent processor แต่ละ processor สามารถ execute โปรแกรม12ท่ีต่างกันได้

multiprocessor architecture แบ่งเป็น 2 categories ตามการ จัดการ memory
1. global memory (GM) system architecture ใช้ memory ร่วมกันสําหรับ processors ทั้งหมด
2. local memory (LM) system architecture มี storage system สําหรับแต่ละ processor

SIMD machines มี 1 CU และหลาย PE (processing elements)

MIMD machines มี independent processors ซึ่งใช้ resources ร่วมกัน รวมถึงใช้ main memory ร่วมกัน แต่ละ processor ทํางานโดยเป็นอิสระต่อกัน multiple-processor computers อาจเป็นแบบ tightly coupled หรือ loosely coupled ขึ้นกับการ access memory tightly coupled multiprocessor ใช้ memory system ร่วมกัน และ loosely coupled multiprocessor ใช้ทั้ง memory ร่วม และแต่ ละprocessor มี local memory ของตัวเอง

# Computer architecture ท่ีดี
Computer architecture ท่ีดีต้องมีคุณลักษณะดังนี้

1. Generality สามารถทํางานได้กับหลากหลาย applications เช่น scientific และ engineering applications ซึ่งต้องใช้ floating-point arithmetic, business applications ใช้ decimal arithmetic การมี addressing mode ที่หลากหลายก็เป็นอีกหนึ่งตัวที่บอก generality
2. Applicability ความเหมาะสมในการใช้งาน สําหรับ scientific และ engineering applications เป็น computation-intensive applications เพราะฉะนั้นจึงมีสัดส่วนของ CPU operations to memory และ I/O operations สูงกว่างานด้านอื่น commercial applications ใช้งานทั่วไปเช่น compiling, accounting, spreadsheet usage, word processing 
3. Efficiency วัดปริมาณการทํางานเฉลี่ยของ hardware ในการทํางานปกติ เนื่องจากปัจจุบันราคาของคอมพิวเตอร์ถูกลง efficiency จึงไม่ค่อยเป็นส่วน สําคัญเหมือนในอดีต efficient architecture มีโครงสร้างที่ไม่ซับซ้อน มีราคาถูกกว่า และทํางานได้เร็ว
4. Ease of Use วัดความง่ายสําหรับ system programmer ในการพัฒนา software เช่น operating system, compiler เพราะนั้นขึ้นกับ ISA
5. Malleability วัดความง่ายในการสร้างคอมพิวเตอร์ที่มีความหลากหลาย เช่น ขนาด, performance สําหรับ architecture เดียวกัน ถ้า architecture มีความเฉพาะเจาะจงมาก ความหลากหลายจะลดลง
6. Expandabilityวัดความง่ายสําหรับคนออกแบบในการเพิ่มความสามารถ ของ architecture เช่น ความจุสูงสุดของmemory หรือความสามารถในการ คํานวณ (arithmetic capabilities) สําหรับคอมพิวเตอร์ที่มีได้มากกว่า 1 CPU expandability รวมถึงจํานวน สูงสุดของ CPU ท่ีเป็นไปได้

## Factors Influencing the Success of a Computer Architecture
ปัจจัยท่ีมีผลต่อความสําเร็จของ computer architecture
### 1. Architectural Merit
- Applicability ความเหมาะสมในการใช้งาน
- Malleability ความง่ายในการใช้งาน
- Expandability ความสามารถในการขยาย memory size, I/O capacity, number of processors
- Compatibility การใช้แทนกันได้กับคอมพิวเตอร์รุ่นก่อน ๆ ใน family เดียวกัน

### 2. System Performance
กําหนดโดยความเร็ว ของคอมพิวเตอร์ ในการวัด computer’s performance สถาปนิกจะ run standardized โปรแกรม เรียก benchmarks

#### CPU Performance Measures
- MIPS millions of instructions per second ทําได้กี่ล้านคําสั่งต่อวินาที
- MFLOPS millions of floating-point operations per second
- GFLOPS หรือ gigaflops billions of floating-point operations per second

ตัวอย่างเช่น Spec Benchmark Suite, Perfect Club Suite ประกอบด้วย programs ทางวิทยาศาสตร์เพื่อตรวจสอบความเร็วในการทํางาน

#### I/O Performance Measures
- bandwidth เป็นความเร็วในการทํางานของ I/O มีหน่วยเป็น MBS (megabytes per second) อุปกรณ์ I/O ส่วนใหญ่มี specific transfer rate
- I/O operations per second วัดจํานวน I/O operations ที่ระบบสามารถทําได้ต่อวินาที

#### Other Performance Measure
- Memory bandwidth เป็น megabyte/second ที่หน่วยความจําสามารถส่งข้อมูลให้ processor
- Memory access time เป็นเวลาเฉลี่ยในการเข้าถึงข้อมูลใน memory โดย CPU
- Memory size (ขนาดของ memory)

#### Instruction-set Architecture (ISA)
- data type หมายถึงชนิดของข้อมูลและการทํางานกับข้อมูลนั้น ๆ เช่น integer รวม set ของเลขจํานวนเต็ม และการทํางาน(operations) การบวก ลบ คูณ หารแบบจํานวนเต็ม
- hardware มีส่วนในการกําหนด data types โดยการเลือก data type เป็นส่วนสําคัญในการออกแบบ ISA ของคอมพิวเตอร์
- register เป็นอุปกรณ์ hardware เพื่อเก็บข้อมูล registers ที่ โปรแกรมเมอร์ควบคุมเรียก operational registers
- machine instruction กําหนดการทํางานและ operand ในการ ทํางานนั้น ๆ

### 3. System Cost
นอกจากในส่วนของราคา hardware แล้ว ยังมี cost ด้านอื่น ๆ ด้วย
- Reliability ความน่าเชื่อถือ เช่น ใน flight-critical control ของเครื่องบิน, safety-critical control ของโรงงาน
- Ease of repair
- Power consumption หรือ heat generation
- Weight สําหรับบางงานเช่น ในเครื่องบิน , ยานอวกาศ
- Ruggedness คือความสามารถในการทนต่อสภาพแวดล้อม เช่นกระแสไฟตก สภาพแวดล้อมไม่ปกติ เช่นใช้ในหน่วยงานทหาร
- Software system interface วัดความสะดวกในการใช้งาน (user friendly) และความเป็นมาตรฐาน (degree of standardization)

# Data Representations
units of information ส่วนใหญ่ใช้ bytes เป็นหน่วยนับ 1 byte = 8 bits เก็บ character 1 ตัว หรือเลขจํานวนเต็มขนาดเล็ก word size คือ ขนาดของ operational register 16-bit computer มี 16-bit registers และ word ขนาด 2 bytes สําหรับ
- 32-bit computer 1 word มีขนาด 4 bytes 
- 16-bit computer double word = 4 bytes 
- 32-bit computer double word = 8 bytes 
- quad word มีขนาด 4 words 
- octet word มีขนาด 8 words จํานวน byte ขึ้นกับขนาดของคอมพิวเตอร์

โดยทั่วไปเมื่อใช้หนึ่ง word เพื่อเก็บข้อมูลเรียก single precision ถ้าใช้ 2 words เรียก double precision ถ้ามี 32-bit word double precision floating point ใช้เนื้อที่ 64 bit

## character
ใช้เนื้อที่ 1 byte และส่วนใหญ่แทนด้วยรหัส ASCII (personal computers และ minicomputers) รหัส EBCDIC (Extended Binary Coded Decimal Interchange Code) ใช้กับเครื่อง IBM mainframes

## Numeric data
อาจเก็บได้หลายรูปแบบ
- BCD (Binary Coded Decimal) เก็บเลขฐานสิบ 1 หลักโดยใช้ 8 bit หรือ 4 bit ตัวเลขหนึ่งจํานวนใช้เนื้อที่หลาย bytes
- integer ใช้เนื้อที่หลาย bytes เพ่ือเก็บข้อมูล
- floating-point number

# Data Structures
คอมพิวเตอร์บางชนิดมี hardware เพื่อช่วย programmer ทํางานกับ string ของ characters, stacks, arrays และ structures สําหรับ parameter passing (การส่งผ่านข้อมูล)

## character strings
เป็น sequence ของ 0 หรือมากกว่า characters การทํางาน เช่น
`LENGTH` หาความยาวของ string
`EQUAL` ตรวจสอบว่า string เท่ากันหรือไม่
`CONCAT` เชื่อม string
`SUBSTRING` หาส่วนของ string

## stacks
เป็น data structure ท่ีใช้ในการเก็บข้อมูลแบบ LIFO การทําการกับ stack เช่น `push`, `pop`, `top`, และ `empty`

## arrays
เป็นกลุ่มของข้อมูลที่อ้างถึงโดยใช้ชื่อเดียวกัน ข้อมูลแต่ละ item เรียก array element เมื่อมีการกําหนด array โปรแกรมเมอร์ต้องกําหนดชื่อ, subscript positions และ subscript values สําหรับแต่ละ subscript position

การใช้งาน array ส่วนใหญ่คือ array ชนิด vectors และ matrices โดย vector เป็น array 1 มิติ vector element เก็บใน memory ตามลําดับ เช่นท่ี $location v, v+I ,v+2I , v+3I , … v+(N-1)I$ โดย $I$ คือ increment และ $N$ คือ vector length