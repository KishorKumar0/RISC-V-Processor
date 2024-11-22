# RISC-V Processor

This repository contains the implementation of a 64-bit pipelined RISC-V processor using Verilog HDL. The design follows the RV64I base integer instruction set architecture (ISA) and incorporates a 5-stage pipeline: Instruction Fetch (IF), Instruction Decode (ID), Execution (EX), Memory Access (MEM), and Write Back (WB).

## Table of Contents
- Features
- What’s Different in This Design
- Pipeline Stages
- Modules
- How to Use
- Project Structure
- Future Enhancements
- Contributing
- License

This is a verilog code for a 5-stage pipelined RISC-V Processor with forwarding functionality. Here is the circuit diagram of the processor.

![The circuit diagramme of the processor.](CircuitDiagramme.png)

Features
- Implements the RV64I instruction set.
- Fully pipelined design with 5 stages:
  - **IF:** Fetches the instruction from memory.
  - **ID:** Decodes the instruction and reads operands.
  - **EX:** Executes operations using the ALU.
  - **MEM:** Accesses memory for load/store instructions.
  - **WB:** Writes results back to the register file.
- Modular Verilog design for easy understanding and testing.
- Includes hazard handling mechanisms:
  - Forwarding unit for resolving data hazards.
  - Basic branch prediction and control hazard resolution.

### What’s Different in This Design
This RISC-V processor design introduces several unique aspects that differentiate it from other implementations:

  1. **Customizable Register Initialization:**
      - The `RegisterFile` module allows preloading specific registers with initial values, which can simplify testing and debugging for specific instructions and use cases.
  2. **Enhanced Debugging Features:**
      - Designed with modular pipeline registers (`IDRegister`, `EXRegister`, `MEMRegister`, `WBRegister`) that enable detailed waveform analysis of each stage during simulation.
  3. **Explicit Forwarding Mechanism:**
      - A separate `ForwardingUnit` is implemented to ensure seamless resolution of data hazards between pipeline stages.
  4. **Minimalistic Control Signal Design:**
      - The `Control_Unit` and `ALU_Control` modules generate essential control signals, minimizing redundancy while supporting all RV64I instructions.
  5. **Seamless Simulation Workflow:**
      - Testbench (`tb.v`) is designed to toggle clock and reset signals automatically, enabling rapid validation without manual intervention.
  6. **Modular MUX Design:**
      - Reusable MUX modules (`mux` and `mux2`) streamline data selection, making the design efficient and adaptable.
  7. **Educational Focus:**
    - The design is structured for learning, with clear modular boundaries that allow students and developers to test and modify individual components without affecting the entire processor.
  8. **Compatibility with IoT Applications:**
    - The modularity and scalability of this design make it well-suited for lightweight IoT and edge-computing systems.

### Pipeline Stages
1. **Instruction Fetch (IF):**
    - Fetches instructions from the Instruction Memory.
    - Maintains the current Program Counter (PC).
    - Calculates the next PC using an adder.
2. **Instruction Decode (ID):**
     - Decodes the opcode, funct3, funct7, and other fields.
    - Reads source registers (rs1 and rs2) from the Register File.
    - Generates immediate data based on the instruction type.
    - Control unit determines control signals based on the opcode.
3. **Execution (EX):**
    - Performs arithmetic and logical operations using the ALU.
    - Computes branch target addresses.
    - Resolves data hazards using the Forwarding Unit.
4. **Memory Access (MEM):**
    - Handles load and store operations with Data Memory.
    - Passes non-memory results directly to the Write Back stage.
4. **Write Back (WB):**
    - Writes ALU or memory results back to the Register File.

### Modules
Below is the list of modules in the design:

1. **Top-Level Module:** RISC_V_Processor (the main processor module).
2. **Program_Counter:** Maintains and updates the Program Counter.
3. **Adder:** Computes PC + 4 and branch addresses.
4. **Instruction_Memory:** Stores instructions.
5. **RegisterFile:** Holds 32 general-purpose registers.
6. **ALU:** Executes arithmetic and logical operations.
7. **Control_Unit:** Generates control signals based on opcode.
8. **parser:** Decodes instructions into components (opcode, funct3, funct7, etc.).
9. **mux and mux2:** Handle data selection for control flow and forwarding.
10. **ALU_Control:** Generates ALU operation codes.
11. **ForwardingUnit:** Resolves data hazards.
12. **Pipeline Registers:**
    - `IDRegister` (IF/ID)
    - `EXRegister` (ID/EX)
    - `MEMRegister` (EX/MEM)
    - `WBRegister` (MEM/WB)
13. **Data_Memory:** Implements memory access for load/store instructions.

### Project Structure

risc-v-processor/
```
├── src/
│   ├── RISC_V_Processor.v       # Top-level module
│   ├── Program_Counter.v
│   ├── Adder.v
│   ├── Instruction_Memory.v
│   ├── RegisterFile.v
│   ├── ALU.v
│   ├── Control_Unit.v
│   ├── parser.v
│   ├── mux.v
│   ├── mux2.v
│   ├── ALU_Control.v
│   ├── ForwardingUnit.v
│   ├── Pipeline Registers/
│   │   ├── IDRegister.v
│   │   ├── EXRegister.v
│   │   ├── MEMRegister.v
│   │   └── WBRegister.v
│   └── Data_Memory.v
├── tb/
│   └── tb.v                     # Testbench
├── Makefile                     # Makefile for simulation
└── README.md                    # Project documentation
```

### Future Enhancements
- Extend support to other RISC-V ISA extensions (e.g., RV64M for multiplication and division).
- Add advanced branch prediction mechanisms.
- Implement a cache system for improved memory access performance.
- Explore integrating the processor into IoT or AI projects.
