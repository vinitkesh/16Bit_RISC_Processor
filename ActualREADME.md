How does one implement a RISC architecture?

The NITC-RISC18 is very simple, but it is general enough to solve complex problems with three machine-code instruction formats (R, I, and J type) and a total of six instructions shown below.

### 1. ADD
### 2. NDU
### 3. LW
### 4. SW
### 5. BEQ
### 6. JAL

- We need to have 8 registers each of which has a 16 bit value.
- Each instruction is 16 bits long.
- Which means that the test bench will input 16 bits.

We need to implement the following modules:

1. Register File : 8 registers each of which has a 16 bit value.
2. ALU
3. Control Unit
4. Data Memory
5. Instruction Memory
6. Program Counter

### Register File

The register file is a module that has 8 registers each of which has a 16 bit value. 

- The module has 3 read ports and 1 write port. 
- Each of the read ports has a 3 bit address and the write port has a 3 bit address and a 16 bit data input.
  
The module has the following ports:

- clk : The clock signal
- readRegister1 : The address of the first register to be read
- readRegister2 : The address of the second register to be read
- writeRegister : The address of the register to be written to
- writeEnable : A signal that enables writing to the register
- writeData : The data to be written to the register

```verilog
module RegisterFile(
    input wire clk,
    input wire [2:0] readRegister1,
    input wire [2:0] readRegister2,
    input wire [2:0] writeRegister,
    input wire writeEnable,
    input wire [15:0] writeData,

    output reg [15:0] readData1,
    output reg [15:0] readData2
);

    reg [15:0] registers [0:7]; // 8 registers each of which has a 16 bit value

    always @(posedge clk) begin
        readData1 <= registers[readRegister1];
        readData2 <= registers[readRegister2];
        if(writeEnable) begin
            registers[writeRegister] <= writeData;
        end
    end

endmodule
```

### ALU

The ALU is a module that performs arithmetic and logical operations.

- The module has 2 16 bit inputs and a 16 bit output.
- The module has the following ports:
- input1 : The first 16 bit input
- input2 : The second 16 bit input
- operation : A 4 bit signal that specifies the operation to be performed

```verilog

module ALU(
    input wire [15:0] input1,
    input wire [15:0] input2,
    input wire [3:0] operation,
    output reg [15:0] output
);

    always @(*) begin
        case(operation)
            4'b0000: output = input1 + input2; // ADD
            4'b0010: output = input1 & input2; // NDU
            default: output = 16'b0;
        endcase
    end

endmodule
```

### Control Unit

The control unit is a module that generates control signals for the processor.

- The module has a 6 bit input and 6 bit output.
- The module has the following ports:
- input : The 6 bit input
- output : The 6 bit output

```verilog
module ControlUnit(
    input wire [5:0] input,
    output reg [5:0] output
);

    always @(*) begin
        case(input)
            6'b000000: output = 6'b000000; // ADD
            6'b000010: output = 6'b000010; // NDU
            6'b010000: output = 6'b010000; // LW
            6'b010001: output = 6'b010001; // SW
            6'b110000: output = 6'b110000; // BEQ
            6'b100000: output = 6'b100000; // JAL
            default: output = 6'b000000;
        endcase
    end

endmodule
```

### Data Memory

The data memory is a module that stores data.

- The module has a 16 bit address and a 16 bit data input and output.
- The module has the following ports:
- address : The 16 bit address
- writeEnable : A signal that enables writing to the memory
- writeData : The data to be written to the memory
- readData : The data read from the memory

```verilog
module DataMemory(
    input wire [15:0] address,
    input wire writeEnable,
    input wire [15:0] writeData,
    output reg [15:0] readData
);

    reg [15:0] memory [0:65535]; // 64K memory

    always @(*) begin
        if(writeEnable) begin
            memory[address] <= writeData;
        end
        readData = memory[address];
    end

endmodule
```

### Instruction Memory

The instruction memory is a module that stores instructions.

- The module has a 16 bit address and a 16 bit data output.
- The module has the following ports:
- address : The 16 bit address
- readData : The data read from the memory

```verilog
module InstructionMemory(
    input wire [15:0] address,
    output reg [15:0] readData
);

    reg [15:0] memory [0:65535]; // 64K memory

    always @(*) begin
        readData = memory[address];
    end

endmodule
```

### Program Counter

The program counter is a module that stores the address of the next instruction to be executed.

- The module has a 16 bit input and output.
- The module has the following ports:
- input : The 16 bit input
- output : The 16 bit output

```verilog
module ProgramCounter(
    input wire [15:0] input,
    output reg [15:0] output
);

    always @(*) begin
        output = input;
    end

endmodule
```

### NITC-RISC18

The NITC-RISC18 is a module that implements the NITC-RISC18 architecture.

- The module has the following ports:
  - clk : The clock signal
  - reset : A signal that resets the processor
  - instruction : The 16 bit instruction
  - dataMemoryAddress : The 16 bit address for the data memory
  - dataMemoryWriteEnable : A signal that enables writing to the data memory
  - dataMemoryWriteData : The data to be written to the data memory
  - dataMemoryReadData : The data read from the data memory
  - instructionMemoryReadData : The data read from the instruction memory
  - programCounterOutput : The address of the next instruction to be executed

Here's the code for the NITC-RISC18 module with all the previously defined modules in the same file:

```verilog

module RegisterFile(
    input wire clk,
    input wire [2:0] readRegister1,
    input wire [2:0] readRegister2,
    input wire [2:0] writeRegister,
    input wire writeEnable,
    input wire [15:0] writeData,

    output reg [15:0] readData1,
    output reg [15:0] readData2
);

    reg [15:0] registers [0:7]; // 8 registers each of which has a 16 bit value

    always @(posedge clk) begin
        readData1 <= registers[readRegister1];
        readData2 <= registers[readRegister2];
        if(writeEnable) begin
            registers[writeRegister] <= writeData;
        end
    end

endmodule

module ALU(
    input wire [15:0] input1,
    input wire [15:0] input2,
    input wire [3:0] operation,
    output reg [15:0] output
);

    always @(*) begin
        case(operation)
            4'b0000: output = input1 + input2; // ADD
            4'b0010: output = input1 & input2; // NDU
            default: output = 16'b0;
        endcase
    end

endmodule

module ControlUnit(
    input wire [5:0] input,
    output reg [5:0] output
);

    always @(*) begin
        case(input)
            6'b000000: output = 6'b000000; // ADD
            6'b000010: output = 6'b000010; // NDU
            6'b010000: output = 6'b010000; // LW
            6'b010001: output = 6'b010001; // SW
            6'b110000: output = 6'b110000; // BEQ
            6'b100000: output = 6'b100000; // JAL
            default: output = 6'b000000;
        endcase
    end

endmodule

module DataMemory(
    input wire [15:0] address,
    input wire writeEnable,
    input wire [15:0] writeData,
    output reg [15:0] readData
);

    reg [15:0] memory [0:65535]; // 64K memory

    always @(*) begin
        if(writeEnable) begin
            memory[address] <= writeData;
        end
        readData = memory[address];
    end

endmodule

module InstructionMemory(
    input wire [15:0] address,
    output reg [15:0] readData
);

    reg [15:0] memory [0:65535]; // 64K memory

    always @(*) begin
        readData = memory[address];
    end

endmodule

module ProgramCounter(
    input wire [15:0] input,
    output reg [15:0] output
);

    always @(*) begin
        output = input;
    end

endmodule

module NITC_RISC18(
    input wire clk,
    input wire reset,
    input wire [15:0] instruction,
    input wire [15:0] dataMemoryAddress,
    input wire dataMemoryWriteEnable,
    input wire [15:0] dataMemoryWriteData,
    output reg [15:0] dataMemoryReadData,
    output reg [15:0] instructionMemoryReadData,
    output reg [15:0] programCounterOutput
);

    wire [2:0] registerFileReadRegister1;
    wire [2:0] registerFileReadRegister2;
    wire [2:0] registerFileWriteRegister;
    wire registerFileWriteEnable;
    wire [15:0] registerFileWriteData;

    wire [15:0] aluInput1;
    wire [15:0] aluInput2;
    wire [3:0] aluOperation;
    wire [15:0] aluOutput;

    wire [5:0] controlUnitInput;
    wire [5:0] controlUnitOutput;

    wire [15:0] dataMemoryAddressWire;
    wire dataMemoryWriteEnableWire;
    wire [15:0] dataMemoryWriteDataWire;
    wire [15:0] dataMemoryReadDataWire;

    wire [15:0] instructionMemoryAddress;
    wire [15:0] instructionMemoryReadDataWire;

    wire [15:0] programCounterInput;
    wire [15:0] programCounterOutputWire;

    RegisterFile registerFile(
        .clk(clk),
        .readRegister1(registerFileReadRegister1),
        .readRegister2(registerFileReadRegister2),
        .writeRegister(registerFileWriteRegister),
        .writeEnable(registerFileWriteEnable),
        .writeData(registerFileWriteData),
        .readData1(aluInput1),
        .readData2(aluInput2)
    );

    ALU alu(
        .input1(aluInput1),
        .input2(aluInput2),
        .operation(aluOperation),
        .output(aluOutput)
    );

    ControlUnit controlUnit(
        .input(controlUnitInput),
        .output(controlUnitOutput)
    );

    DataMemory dataMemory(
        .address(dataMemoryAddressWire),
        .writeEnable(dataMemoryWriteEnableWire),
        .writeData(dataMemoryWriteDataWire),
        .writeData(dataMemoryReadDataWire)
    );

    InstructionMemory instructionMemory(
        .address(instructionMemoryAddress),
        .readData(instructionMemoryReadDataWire)
    );

    ProgramCounter programCounter(
        .input(programCounterInput),
        .output(programCounterOutputWire)
    );

    assign registerFileReadRegister1 = instruction[11:9];
    assign registerFileReadRegister2 = instruction[8:6];
    assign registerFileWriteRegister = instruction[5:3];

    assign registerFileWriteEnable = controlUnitOutput[5];
    assign registerFileWriteData = aluOutput;
    assign aluInput1 = registerFile.readData1;
    assign aluInput2 = registerFile.readData2;
    assign aluOperation = controlUnitOutput[4:0];
    assign controlUnitInput = instruction[15:10];
    assign dataMemoryAddressWire = aluOutput;
    assign dataMemoryWriteEnableWire = controlUnitOutput[5];
    assign dataMemoryWriteDataWire = registerFile.readData2;
    assign instructionMemoryAddress = programCounterOutputWire;
    assign programCounterInput = instructionMemoryReadDataWire;

endmodule
```