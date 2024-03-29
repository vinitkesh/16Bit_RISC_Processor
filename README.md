# 16Bit_RISC_Processor

Repo for by implementation of NITC-RISC18 Instruction Set Architecture


The Little-Endian Computer Architecture serves as the foundation for the development of the 16-bit, very simple NITC-RISC18 computer for teaching. It (NITC-RISC18) is an 8-register, 16-bit computer system. It uses registers R0 to R7 for general purposes. However, Register R7 always stores the program counter. This architecture also uses a condition code register, which has two flags: the Carry flag (C) and the Zero flag (Z).

The NITC-RISC18 is very simple, but it is general enough to solve complex problems with three machine-code instruction formats (R, I, and J type) and a total of six instructions shown below.

R Type Instruction format

| Opcode| Register A (RA)| Register B (RB)| Register B (RB)| Unused |Condition (CZ) |
|---|---|---|---|---|---|
 |(4 bit) |(3 bit) |(3-bit) |(3-bit) |(1 bit) |(2 bit) |

## I Type Instruction format
| Opcode| Register A (RA)| Register C (RC)| Immediate| 
|---|---|---|---|
| (4 bit)| (3 bit)| (3-bit)| (6 bits signed)| 
 

## J Type Instruction format:

| Opcode| Register A (RA)| Immediate| 
|---|---|---|
| (4 bit)| (3 bit)| (9 bits signed)| 

## Instructions Encoding: 

### ADD:
|||||||
|---|---|---|---|---|---|
| 00_00 | RA | RB | RC | 0 | 00 | 
### NDU:
|||||||
|---|---|---|---|---|---|
| 00_10| RA| RB| RC| 0| 00| 

### LW:

|||||
|---|---|---|---|
 |01_00 |RA |RB |6 bits Immediate |


### SW:


||||
|---|---|---|
| 01_01| RA| RB| 6 bits Immediate| 


### BEQ:

||||
|---|---|---|
| 11_00| RA| RB| 6 bits Immediate|

### JAL:

||||
|---|---|---| 
| 10_00 | RA | 9-bit Immediate offset |


> RA: Register A

> RB: Register B

> RC: Register C
