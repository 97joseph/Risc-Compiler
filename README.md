# Risc-Compiler

Risc architecture implementation

The
fields  **rs ** (instr[25:21]),
**rt** (instr[20:16]),
**rd** (instr[15:11])
are pointers to operands. The field **op** (instr[31:26])
is used for operations. In this Lab, we will
not use the **shamp** and **funct** fields so we simply set their bits as logic
0(instr[5:0] = 6’b000000, instr[10:6] = 5’b00000).

**[Important]. **The values stored in  **rs** ,  **rt** ,
and **rd** are  ** address are 5 bit fields that address one of
32 registers in the register  file (R)** .

**op **in R-type instructions** **tells us to **ADD** or  **SUB** . An example will be given after we explain the  **single datapath RISC architecture** .

**B.**** I-type Instruction **

I-types instructions can be used to achieve **LW**
and **SW
**operations. The Figure 6.8 is the format of I-type instruction.

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image003.gif)** **

**Figure 6.8 I-type  instruction format **

Within these **32** bits,
the field  **rs
** (instr[25:21]),  **rt** (instr[20:16]),  **imm** (instr[15:0]) are used to
point to operands. The field **op** (instr[31:26])
is used for operations.

**[Important ]** The values stored in  **rs** , **rt** also point to registers ** in the RF** .

O**p **in I-type instructions** **tells us to **LW** or  **SW** . An example will be given after we explain the  **single datapath RISC architecture** .

**1.
Introduction of Single Datapath RISC Architecture** The general
components in RISC architecture are shown in below:

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image005.gif)

**PC** (Program
Counter) contains the address of the instruction to execute.

**Instruction Memory **stores all the
instructions. Port **A **requires  the address of instructions. Port **RD **reades the instruction data. As
shown here, **PC** connect to **A **and the **Instruction Memory **reads out the instruction from  **RD** , labelled **Instr. **

---

**Register
File** can read 2 registers and write 1 register simultaneously. For  **Register File** , Port **A1, A2, A3** are the input ports for
address. Port **RD1, RD2 **are the
output ports for read registers pointed by A1 and A2, respectively. **WD3 **is the input port for writing data
into a RF at the rising edge of the clock. **WE3**
is the write enable signal port.

**Data Memory **stores
the data. Port **A **is the address
input port. Port** RD **is the data
output port. Port **WD **is the data
input port. Port **WE **is the write
enable port.

---

---

**[Important] **The above figure only shows
the main component of a RISC architecture. A complete single datapath RISC
architecture is given in  **Figure 7.9** .
below.

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image007.gif)

# 3. How LW, SW, ADD, SUB works in RISC Architecture

---

Please refer to Chapter 7, Single-cycle
Datapath(7.3.1) in textbook to see how  **LW** ,  **SW** , **ADD** and **SUB **work in the single path RISC architecture. Here we give you a
brief description on how those operations work in the RISC architecture(**For details, you still need to go over the
chapters/sections in textbook stated in the beginning) **

|   ***Note*** *: To better describe, data in memory is represented as

| Memory[address]. E.g. the 1^st^data in*                                      |
| ---------------------------------------------------------------------------- |
| *Register                                                                    |
| File is Register_File[0], the second data in Data memory is Data_Memory[1].* |

**LW **and **SW** achieved by I-type instruction, are
mainly data communications between Register

File and Data Memory. For **LW, ** **rs
** (instr[25:21]) are inputted in port A1 of Register File and  **Register_File[rs]** (32-bit) is read out
from port RD1, at the same time,  **imm** (instr[15:0]) is extended through Sign
Extender and  **SignImm** (32-bit) is
obtained. **Register_File[rs]** and **SignImm **are added together through ALU
to get the memory address for data memory. So you can see the ALU result, in
this case, is inputted to port A(see  **Figure
7.6 in Textbook** ) and the Data Memory reads out **Data_Memory[Register_File[rs] + SignImm] **from port RD of data
memory and inputted in port WD3 of register file. Meanwhile  **rt** (instr[20:16])
is inputted in port A3 as the destination address for register file. So **Data_Memory[Register_File[rs] + SignImm] **is
loaded from Data Memory to  **Register_File[rt]** .
For  **SW** , it is very similar, please
go through the section 7.3.1.

**ADD **and **SUB** achieved by R-type instruction, are mainly operated for the
registers in Register File. For  **ADD** ,
**rs ** (instr[25:21])
is inputted in port A1 of Register File,  **rt** (instr[20:16]) is inputted in port A2 of Register
File. So **Register_File[rs]** and **Register_File[rt]** are read out from
port RD1 and RD2 respectively to ALU to finish the addition. The ALUResult will
be inputted back to Register File(Here MemtoReg = 0) from port WD3. At the same
time,  **rd** (instr[15:11])
is inputted in port A3 of Register File. So **Register_File[rs]+Register_File[rt] **is stored in **Register_File[rd]. **For  **SUB** , it is very similar, please go
through the section 7.3.1.

---

**The only thing we modify for our Lab** ,
which is a little bit different from the textbook, is the **op**** **field
for I-types and R-types instruction. Here to control the 3 MUXs and ALU(RegDst,
ALUSrc, ALUControl ~2:0~ , MemtoReg), we use  **op** . The fields
of **op** is shown
below.

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image009.jpg)

Now we give some examples for you how to represent
our operations(**LW, SW, ADD, SUB)**
into I-type and R-type instruction and how the instruction work in RISC datapth
architecture.

***Note*** *: Assume all the data in Register File are initialized as 0.*

1. ```
     **LW** (Load) in I-type instruction 
   ```

Load Data_Memory[5] to Register_File[1]. The base
address **rs** = 0, offset **imm** = 5. So
Address of Data_Memory  =
Register_File[rs] + imm = 5

The address of Register File **rt** = 1

In this case, RegDst = 0, ALUSrc = 1, ALUControl[2:0] =
010(ADD), MemtoReg = 1, so: op = 6'b010101 rs = 5'd0 = 5'b00000 rt = 5'd1 =
5'b00001 imm = 16'd5 = 16'b0000_0000_0000_0101

So the I-type instruction should be:
32’b010101_00000_00001_0000_0000_0000_0101.

2. ```
    **ADD** in R-type instruction 
   ```

ADD Register_File[1] and Register_File[2], and store
the result to Register_File[6]. The first

Address **rs** = 1, The
second address **rt** = 2, the destination address **rd** =
6; In this case, RegDst = 1, ALUSrc = 0, ALUControl[2:0] = 010, MemtoReg = 0,
so: op = 6'b100100 rs = 5'b00001 rt = 5'b00010 rd = 5'b00110 rest =
11'b00000_000000(**shamp **and** ****funct**
are set as logic 0)

So the R-type instruction should be:
32’b100100_00001_00010_00110_00000_000000.

3. ```
     **SW** (Store) in I-type instruction** **
   ```

---

Store Register_File[6] to Data_Memory[2]. The base
address **rs** = 0, offset **imm** = 2. So
Address of Data_Memory = Register_File[rs] + imm = 2.  The address of Register file **rt** =
6

In this case, RegDst = 0, ALUSrc = 1, ALUControl[2:0]
= 010, MemtoReg = 0, so:

op = 6'b010100 rs = 5'b00000  rt = 5'b00110

imm = 16'b0000_0000_0000_.0010

So the I-type instruction should be:
32’b010100_00000_00110_0000_0000_0000_0010.

4. ```
    **SUB** in R-type instruction** **
   ```

---

For SUB operation, it is similar to ADD in R-type
instruction. Notice that ALUControl for SUB should be 110 for ALU to do
substraction.

# Assignment in Inlab 6

In Prelab6 assignment, we translate LW, SW, ADD, SUB
operations into I-types and R-types instruction. In Inlab6 we are going
implement 2 key component of single path RISC architecture, **Register File** and **ALU** by writing the System Verilog code for 2 modules, **Register File** and **ALU. **And you need to write testbench to verify your Register File and
ALU.

1. ```
    Register
   ```

File

The module of register file should look like
this,

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image011.jpg)

|  Notice for the Register File used here,

| there are totally 32 32-bit registers, which means the |
| ------------------------------------------------------ |
| width(number of bits for each register) and            |
| depth(number of registers) are both 32.                |

You should initialize the Register File first. There
are a lot of ways to initialize a register file. Here we provide the simplest
way to initialize it, which is hardcode it. Below is an example of how to
initialize the register file:

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image012.jpg)

Please follow the
example and initialize the Register File as Register_File[i] = i. (It
means for the 1^st^ register, the value it stores is 1, for the 3^rd^
register, the value it stores is 3, etc...)

2. ```
    ALU
   ```

The module of ALU should look like this:

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image013.jpg)

Notice that you only need to operate ADD and SUB for ALU.

ALUControl = 010, do ADD, ALUControl = 110, do SUB,
(see Table 5.1 in Textbook)

3. ```
    Testbench
   ```

Notice that there are 2 testbenches you need to
write, one for Register File, one for ALU.

a.       Testbench
for Register File

In testbench for Register File, you should be able to
show that(There is not specific input values for A1, A2, A3, WD3, but you need
to explain in the report what are these values in your testbench)

While Reading(WE3 = 0)

RD1 = Register_File[A1], RD2 = Register_File[A2],

While Writing(WE3 = 1)

Register_File[A3] = WD3.

|  Put your waveforms result in

| your report, also explain during reading, what values are read out |
| ------------------------------------------------------------------ |
| from Register File, during                                         |
| writing, what values is written into the Register File             |

b.      Testbench
for ALU

In testbench for ALU, there is no specific input
values and output values required. But you should be able to perform one ADD and one SUB (Also there is not
specific input values for SrcA and SrcB, but you need to explain in the report
what are these values in your testbench, besides make sure the SrcA and SrcB is
not too large to cause overflow).

|  Put your waveforms result in your report,

| also explain it(What is your input values, what is the |
| ------------------------------------------------------ |
| result)                                                |

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image015.jpg)

# 1. Design, and Simulation

[Picking up from Lab 6]

In Lab 6, we have designed “ **Register File** ” and “ **ALU** ”, Now we need to implement the rest
components in **Figure 7.9 **to create a
complete single datapath RISC architecture.

1. Data memory

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image017.jpg)

Note
1: For the size of the data memory, please set it the same size of Register
File: which means there are totally 32 32-bit registers, or to say, the width
and depth are both 32.

Note
2: Also initialize your data memory as  **data_memory[i]
= i** .(In terms of How to initialize it, it is the same as initialization of
register file as shown below). Also again, initialize your Register File as  **Register_File[i] = i** (Same as Lab 6)

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image019.jpg)

Note
3: For **prode **output usage, Please
check the appendix in Lab 6.

Note
4: Please also add **prode **signal for **Register File **module so we can check
the register value in **Register File**
too!

2. ```
    MUX
   ```

There are three MUXs here. MUX_MemtoReg,
MUX_ALUSrc, MUX_RegDst

They are very similar. Here is an example
of module definition of MUX_MemtoReg:

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image021.jpg)

3. ```
    Sign
   ```

Extend

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image023.jpg)

4. ```
    Instruction
   ```

memory

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image025.jpg)

Here we give you almost the whole part of
instruction memory. But you still need to fill up the 4 32-bit instruction
codes as it is told in notation, also you need to fill up the logic left blank
after “else”.

5. ```
    PC
   ```

PC is the top module of your design,

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image027.jpg)

6. ```
    Simulation
   ```

Make PC as your top-level entity. Write
the testbench for it and verify the result through Modelsim waveforms. For the
waveforms, you need to show the 4
results(preferred in decimal raidx) of
the 4 instructions in instruction memory.

# 2. What to Turn In

2.1. For Lab 7 Inlab Report

- **Due Date:** Lab 7 in-lab report is due in
  the week of November 15.
- **Points assigned to inlab Assignment:** 10
  points for Lab 7 come from inlab assignment report submission
- **Penalty for late submission:** You have
  one extra week to submit the in-lab report late. But this will attract a
  penalty of 4 points. So, if you miss your deadline in the week of November 15,
  you can still submit it a week later, in the week of November 22 but with a
  penalty.
- **Content of the in-lab report: **

You must submit an electronic copy of the
following items (in the correct order, and each part clearly labeled) via
Sakai. Submit it to the Lab 7 In-Lab Assignment. All
items should all be included in a single
file (.pdf).

![](file:///C:/Users/97jos/AppData/Local/Packages/oice_16_974fa576_32c1d314_3ac0/AC/Temp/msohtmlclip1/01/clip_image028.jpg)
