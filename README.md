# VeSPA - System on Chip

_An educational custom RISC-based processor implemented on FPGA with the purpose of exploring and learning computer architecture and digital design._

---
## TEAM  

➡️ This project was developed by a team of 20 students as part of the Computer Architecture course. 

➡️ The class was organized into three groups, which rotated roles throughout the semester to cover different stages of the development process

➡️ The roles were:
- CPU Pipeline (SoC Team) - Current Repository
- [VESPA C Compiler Frontend](https://github.com/your-username/vespa-compiler-frontend)  
- [VESPA C Compiler Backend](https://github.com/your-username/vespa-compiler-backend)

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
  - Adoption of a **Harvard architecture**, replacing the previous **Von Neumann** design
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

## New Datapath








