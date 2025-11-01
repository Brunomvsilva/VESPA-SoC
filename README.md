# VeSPA - System on Chip

_An educational custom RISC-based processor implemented on FPGA with the purpose of exploring and learning computer architecture and digital design._

---

## Table of Contents

- [VeSPA - System on Chip](#vespa---system-on-chip)
  - [TEAM](#team)
  - [Problem Statement](#problem-statement)
  - [VeSPA Single Cycle](#vespa-single-cycle)
    - [VeSPA Instruction Set Architecture](#vespa-instruction-set-architecture)
    - [Baseline Design – VeSPA Single-Cycle CPU](#baseline-design--vespa-single-cycle-cpu)
  - [VeSPA Pipeline](#vespa-pipeline)
    - [Key improvements implemented in this project](#key-improvements-implemented-in-this-project)
  - [Tools, Hardware and Language](#tools-hardware-and-language)
  - [BUS Between Peripherals and CPU](#bus-between-peripherals-and-cpu)
  - [New Datapath](#new-datapath)
  - [VeSPA ABI – Application Binary Interface](#vespa-abi--application-binary-interface)
  - [How to Run](#how-to-run)
    - [Option 1 – Using the VeSPA Compiler or Assembler](#option-1--using-the-vespa-compiler-or-assembler)
    - [Option 2 – Using the Python COE Generator Script](#option-2--using-the-python-coe-generator-script)
  - [RocketSim - VeSPA Simulator](#rocketsim---vespa-simulator)
  - [Results](#results)
    - [Performance & Timing Analysis](#performance--timing-analysis)
    - [Clock Frequency Calculation](#clock-frequency-calculation)
  - [My Contributions to the VeSPA SoC](#my-contributions-to-the-vespa-soc)


---

## TEAM  

➡️ This project was developed by a team of 20 students as part of the Computer Architecture course. 

➡️ The class was organized into three groups, which rotated roles throughout the semester to cover different stages of the development process

➡️ The roles were:
- CPU Pipeline (SoC Team) - Current Repository
- [VESPA C Compiler Frontend](https://github.com/Brunomvsilva/RocketC-VeSPA-Compiler.git)  
- [VESPA C Compiler Backend](https://github.com/Brunomvsilva/RocketC-VeSPA-Compiler.git)

---

## Problem Statement

The goal of this project was to enhance a pre-existing version of the VeSPA CPU.  
This version of the VeSPA processor was originally developed in a previous semester and is documented in the report [VeSPA Single-Cycle CPU](VeSPA_SingleCycle.pdf).

---

## VeSPA Single Cycle

### VeSPA Instruction Set Architecture

<p align="center">
  <img src="Reports%20and%20docs/Images/VESPA_ISA_Table.png" width="950">
</p>

### Baseline Design – VeSPA Single-Cycle CPU

This project builds upon this previously developed single-cycle version of the VeSPA processor, presented below.

<p align="center">
  <img src="Reports%20and%20docs/Images/Single-Cycle-Design.png" width="850">
</p>

---

## VeSPA Pipeline

### **Key improvements implemented in this project:**

- Implementation of a **5-stage pipelined datapath** (IF, ID, EX, MEM, WB) — **Main Goal**
  - Mitigate **structural hazard** by Adopting a **Harvard architecture**, replacing the previous **Von Neumann** design
  - Introduction of **pipeline registers** between stages to enable instruction-level parallelism
  - Extension of the **control unit** to generate and manage new pipeline control signals
  - Integration of **hazard detection** units to identify data and control dependencies
  - Implementation of **data hazard mitigation** mechanisms, including **data forwarding** and **pipeline stalls**
  - Addition of **branch prediction** mechanisms to minimize control hazards
  - Implementation of **pipeline flush** procedures to handle branch mispredictions
- Addition of **shift instructions** supported by a **barrel shifter**
- Fix, Test, and integrate previosly developed peripherals with the CPU, including:
  - **Interrupt Controller**
  - **GPIO**
  - **PS/2 Keyboard Controller**
  - **Timer**
  - **UART**
- Implement and integrate a **VGA Controller** peripheral 

---

## Tools, Hardware and Language

- Verilog
- Xilinx Vivado
- Zybo Board Z7-10

---

## BUS Between Peripherals and CPU

To achieve full integration of the peripherals, a **data bus** was implemented to connect the **CPU (Master)** with all **peripheral modules (Slaves)**.  
The following diagrams illustrate the **bus interconnection architecture** and the corresponding **peripheral memory mapping** within the system’s address space.

<p align="center">
  <img src="Reports%20and%20docs/Images/VeSPA_BUS.png" alt="VeSPA Bus Architecture" width="47%">
  <img src="Reports%20and%20docs/Images/PeripheralMapping.png" alt="Peripheral Mapping" width="22%">
</p>


## New Datapath

The diagram below presents the full pipelined datapath implemented in this project.  
For better visibility and detail, it is also available in:
- **High-resolution PNG image** → [View Datapath Image](Reports%20and%20docs/Images/VeSPAPipeline.png)
- **Final report** → [View final report](Reports%20and%20docs/VesPA_SOC.pdf)
- **Presentation slides** → [View final presentation](Presentations/SoC_Final.pptx)

<p align="center">
  <img src="Reports%20and%20docs/Images/VeSPAPipeline.png" width="850">
</p>

## VeSPA ABI – Application Binary Interface

The **Application Binary Interface (ABI)** defines how the code generated by the compiler interacts with the VeSPA hardware.  
It acts as an agreement between the **CPU architecture** and the **compiler**, ensuring that both interpret function calls, register usage, data formats and memory structures in a consistent manner.

This ABI specifies:

- Memory organization
- Register usage conventions
- Function calling conventions and stack frame layout
- Data representation (endianness, data sizes and alignment)
- Interrupt handling rules

**Full ABI specification** [here](Reports%20and%20docs/VeSPA__ABI.pdf).

### Register Usage (as defined in the ABI)

| Register | Purpose                                  | Caller-Saved |
|----------|-------------------------------------------|--------------|
| **R0**   | Constant zero (hardwired)                | –            |
| **R1**   | Return address for function calls        | Yes          |
| **R2**   | Frame Pointer (FP)                       | Yes          |
| **R3**   | Stack Pointer (SP)                       | Yes          |
| **R4–R5**| Return registers and special registers used for Mul/Div operations | No        |
| **R6–R31** | General-purpose temporary registers     | No           |

---

## How to Run

To test or deploy this project on hardware or simulation, you will need the [development tools and hardware ](##Tools,-Hardware-and-Language) referenced (or similar).

You will first need to generate a `.coe` file (memory initialization file), which can be done in one of the following ways.

After generating the  `.coe` file you will need to input it manually in the VeSPA code memory inside Vivado Tool

---

### **Option 1 – Using the VeSPA Compiler or Assembler**

You can compile C or Assembly code into VeSPA machine code using the following repositories:

- **C Compiler (C to VeSPA Assembly and Binary/COE):** [RocketC – VeSPA C Compiler](https://github.com/Brunomvsilva/RocketC-VeSPA-Compiler.git)

- **Assembler / Backend (Assembly to Binary/COE):** [RocketC – VeSPA C Compiler](https://github.com/Brunomvsilva/RocketC-VeSPA-Compiler.git)

---

### **Option 2 – Using the Python COE Generator Script**

The Python script is located here [`Python COE Script`](GenerateCOE/)

- Write the VeSPA Assembly program inside the `code.txt` file  
- Run the script using:
   ```bash
   python3 VeSPA_binary.py

---

## RocketSim - VeSPA Simulator

Because deploying software directly to the **Zybo Z7 FPGA** can be time-consuming, and debugging capabilities on hardware are limited, a **virtual runtime environment** was created.  
 
This virtual environment allows developers to **run and debug VeSPA programs**, without requiring immediate execution on the FPGA.

RocketSim details and implementation can be found **[here](https://github.com/Brunomvsilva/RocketC-VeSPA-Compiler/tree/main/Instruction%20Scheduler)**

---

## Results

Functional tests can be found in the [Report](VesPA_SOC.pdf) and the [Final Presentation](Presentations/SoC_Final.pptx)

### Performance & Timing Analysis

<p align="center">
  <img src="Reports%20and%20docs/Images/TimingResults.png" width="550">
</p>

From the timing table, the longest delay occurs in the **Execute stage of the SUB instruction**, with a latency of **6.93 ns**. This makes it the critical path of the pipeline and determines the maximum clock frequency.

### Clock Frequency Calculation

The CPU clock frequency is limited by the longest stage delay -> **1/6.93 ns**

**→ Maximum clock frequency: ≈ 144 MHz**
**Clock Frequency of Single cycle version was 100 MHz**

Even though the CPI (cycles per instruction) was not experimentally measured, theoretical analysis already allows meaningful conclusions. The single-cycle VeSPA processor runs at 100 MHz, while the pipelined version can operate at 144 MHz due to a shorter critical path. In an ideal scenario (CPI = 1), this represents a 44% increase in instruction throughput.

In practice, pipeline hazards (data and control hazards) introduce occasional stalls and flushes, increasing CPI slightly above 1. However, since the clock frequency is significantly higher, the overall throughput of the pipelined CPU is still superior to the single-cycle implementation. Additionally, the single-cycle CPU suffers CPI > 1 in load/store instructions, while the pipeline can sustain close to one instruction per cycle under normal execution.

Therefore, even without experimental CPI values, it is reasonable to conclude that the pipelined VeSPA CPU offers higher throughput than the single-cycle version.


---

## My Contributions to the VeSPA SoC

- Played a major role in designing and implementing the **pipelined datapath**
- Actively participated in team discussions on data, control, and structural hazard mitigation strategies
- Contributed to the design and development of the **Hazard Unit**
- Integrated and tested the **UART** and **GPIO** peripherals with the **CPU**
- Assisted in writing the **technical documentation and final report**

---
