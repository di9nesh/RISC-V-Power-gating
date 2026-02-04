# RISC-V-Power-gating
COARSE-GRAINED VS. FINE-GRAINED POWER GATING TECHNIQUES FOR RISC-V PROCESSOR
# Coarse-Grained vs. Fine-Grained Power Gating Techniques for a 5-Stage RISC-V Processor (RTL → GDSII)

**IEEE (2026)** — 979-8-3315-7097-2/26/$31.00 ©2026 IEEE  

This repository presents a complete **RTL-to-GDSII implementation and comparison** of **coarse-grained** and **fine-grained power gating techniques** applied to a **5-stage pipelined RISC-V (RV32I) processor**.  
The objective is to evaluate the **power, area, and performance trade-offs** of both approaches using **industry-standard EDA tools** and **UPF-based power intent**.

---

## 📌 Project Overview

As transistor scaling continues according to Moore’s Law, **power consumption and leakage** have become dominant challenges in modern VLSI design. Power gating is a widely adopted low-power technique that reduces leakage and dynamic power by disconnecting power from inactive modules using sleep transistors.

This work evaluates:
- **Coarse-Grained Power Gating**: Power gating applied to large functional blocks or entire pipeline stages.
- **Fine-Grained Power Gating**: Power gating applied to smaller sub-modules such as ALU, decoder, coprocessor, and control logic.

Both approaches are implemented, synthesized, physically realized, and compared using a **32-bit RV32I RISC-V processor**.

---

## 🎯 Objectives

- Design a 5-stage pipelined RISC-V processor (RV32I)
- Apply **coarse-grained and fine-grained power gating**
- Implement power intent using **Unified Power Format (UPF)**
- Perform **RTL simulation, synthesis, and physical implementation**
- Compare designs based on **power, area, and number of power gates**
- Identify the most efficient power gating strategy for low-power RISC-V cores

---

## 🧠 Background Concepts

### Power Gating
Power gating reduces power by cutting off either **VDD or VSS** to inactive modules.  
This is achieved using:
- **Header switches (PMOS)** between VDD and logic
- **Footer switches (NMOS)** between logic and VSS  

Header switches are often preferred due to lower leakage characteristics.

---

### Types of Power Gating

#### Coarse-Grained Power Gating
- Large functional blocks or pipeline stages are gated as a whole
- Fewer control signals
- Larger power domains
- Less flexibility
- Blocks often remain ON even when partially idle

#### Fine-Grained Power Gating
- Smaller functional units are gated individually
- More selective shutdown
- Smaller power domains
- Higher efficiency with minimal area overhead
- Better suited for modules that are idle frequently

---

## 🏗 RISC-V Processor Architecture

- **ISA**: RV32I (32-bit Integer Base)
- **Pipeline Stages**:
  1. Instruction Fetch (IF)
  2. Instruction Decode (ID)
  3. Execute (EX)
  4. Memory Access (MEM)
  5. Write Back (WB)

### Major Submodules
- ALU
- Decoder
- Controller
- Coprocessor
- Register File
- Instruction Memory
- Data Memory
- Program Counter
- Stall Unit
- Pipeline Registers (IF/ID, ID/EX, EX/MEM, MEM/WB)

---

## 🔌 Power Gating Strategy

### Fine-Grained Power Gating
Power-gated domain includes smaller submodules:
- ALU
- Decoder
- Stall logic
- Coprocessor
- Virtual/auxiliary stages

These modules are not active at all times, making them ideal candidates for fine-grained gating.

---

### Coarse-Grained Power Gating
Power-gated domain includes larger blocks such as:
- Entire MEM/WB pipeline stage
- Large grouped logic regions

These blocks tend to remain active more frequently, limiting power savings.

---

## 🔁 Design Flow (RTL → GDSII)

1. **RTL Design (Verilog)**
   - Introduced `pwr_sig` signal to control power gating behavior
   - Enables functional simulation of power OFF/ON behavior

2. **Simulation & Verification**
   - Performed using ModelSim
   - Functional correctness verified using testbench
   - Switching activity captured using VCD (`$dumpfile`, `$dumpvars`)

3. **Synthesis (Synopsys Design Compiler)**
   - RTL synthesized using **Nangate 45nm Open Cell Library**
   - Separate UPF files applied for coarse-grained and fine-grained designs
   - Area, power, and timing reports generated

4. **Physical Implementation (Cadence Innovus)**
   - Netlist and UPF imported
   - Power domains committed
   - Floorplanning, power planning, placement, CTS, routing performed
   - Power switches inserted using HEADER_X1 cells
   - Activity-based power analysis using VCD

---

## 🧾 UPF & Innovus Power Intent (Key Concepts)

- Power domains defined for gated and always-on regions
- Power switches mapped using UPF
- Power State Tables (PST) used to define legal power states
- Physical boundaries assigned to power domains for clean clustering
- Column-style power switch insertion
- IR-drop limited to ≤3% of VDD

---

## 📊 Results and Analysis

### Power Comparison (Innovus)

| Power Component | Fine-Grained | Coarse-Grained |
|----------------|-------------|----------------|
| Internal Power | 1.81 mW | 2.16 mW |
| Switching Power | 1.00 mW | 2.24 mW |
| Leakage Power | 0.74 mW | 0.79 mW |
| **Total Power** | **3.55 mW** | **5.18 mW** |

➡️ Fine-grained power gating reduces post-layout total power by **~46%**.

---

### Overall Comparison

| Parameter | Fine-Grained | Coarse-Grained |
|---------|--------------|----------------|
| Area | 196,284.58 | 196,385.40 |
| Power (DC) | 17.3 mW | 22.7 mW |
| Power (Innovus) | 3.5 mW | 5.1 mW |
| Power Domain Size | 2.5 μm² | 8.1 μm² |
| Number of Power Gates | 140 | 384 |

➡️ Area overhead difference is **~0.05%**, which is negligible.

---

## ✅ Key Findings

- Fine-grained power gating:
  - Achieves **lower dynamic and leakage power**
  - Requires **fewer power gates**
  - Uses **smaller power domains**
  - Maintains almost identical area

- Coarse-grained power gating:
  - Simpler conceptually
  - Less effective for frequently active pipeline stages

---

## 🧠 Conclusion

Both coarse-grained and fine-grained power gating techniques were successfully implemented from **RTL to GDSII** using UPF-driven power intent. While silicon area remains nearly identical, **fine-grained power gating demonstrates significantly better power efficiency** due to its ability to selectively shut down smaller, frequently idle modules.

Fine-grained power gating proves to be the **preferred strategy for low-power RISC-V processors**, especially in **IoT, edge computing, and energy-constrained applications**, where minimizing power consumption directly impacts battery life and system reliability.

---

## 🚀 Future Work

- Extend design to **64-bit and 128-bit RISC-V processors**
- Explore **dynamic and adaptive power gating**
- Combine power gating with **clock gating and DVFS**
- ASIC fabrication and silicon validation
- Introduce **state retention and fault-tolerant power gating**

---

## 👥 Authors

- **Dinesh Reddy Munnangi**  
  Electrical and Computer Engineering  
  California State University, Fresno  
  📧 dineshm@mail.fresnostate.edu  

- **Aaron Stillmaker**  
  Electrical and Computer Engineering  
  California State University, Fresno  
  📧 astillmaker@csufresno.edu  

---

## 📚 References

Includes IEEE and academic references on power gating, UPF methodology, RISC-V processor design, and low-power VLSI techniques (as cited in the associated paper).
