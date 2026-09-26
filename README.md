# RISC-V Fundamentals — Overview

A short overview of core RISC-V and processor concepts: how instructions execute, how data is represented, and how registers and function calls work.

## Topics Covered

- Why modern devices rely on multiple specialized processors
- The fetch–decode–execute–writeback cycle
- Binary representation and two's complement
- RISC-V's register set and the ABI naming convention
- The difference between an ISA and a physical processor
- The function calling convention (prologue / body / epilogue)

---

## 1. Processors in Everyday Devices

A single action like taking a phone photo involves several specialized processors working together — a touch controller, the main CPU, an image signal processor (ISP), a GPU, and often a machine-learning accelerator — rather than one general-purpose chip. This reflects a broader pattern: modern systems (phones, cars, appliances) typically contain many small, purpose-built processors rather than a single all-purpose one.

## 2. How a Processor Executes Instructions

At its core, a processor performs a small set of operations — arithmetic, logic, comparisons, memory access, and decision-making — repeated through a four-stage cycle:

```
Fetch → Decode → Execute → Writeback
```

Each instruction (e.g. `add x3, x1, x2`) is fetched as binary, decoded to determine the operation and registers involved, executed by the ALU, and the result is written back to a register.

Performance is commonly described using **clock speed** (cycles per second) and **IPC** (instructions completed per cycle) — a higher clock speed alone doesn't guarantee better performance.

## 3. Binary and Data Representation

Digital circuits use two voltage levels (high/low) to represent `1` and `0`, chosen for reliability, noise immunity, speed, and power efficiency. Negative numbers are represented using **two's complement**, allowing the same hardware to handle addition and subtraction uniformly.

## 4. Registers

Registers are small, extremely fast storage locations built into the processor from flip-flop circuits. RISC-V defines **32 general-purpose registers** (x0–x31). By convention (the ABI), each register is also given a role-based nickname — e.g. ra- (return address), sp-(stack pointer), a0–a7-(function arguments), t0–t6- (temporary), s0–s11- (saved).

- **Caller-saved (t0–t6):** may be freely overwritten by any function.
- **Callee-saved (s0–s11):** must be preserved and restored by any function that uses them.

## 5. ISA vs. Processor

RISC-V is an **Instruction Set Architecture (ISA)** — a specification defining required instructions and register behavior, not physical hardware. A **RISC-V processor** is the physical chip (built by vendors such as SiFive or Western Digital) that implements this specification in silicon. A register's flip-flop circuit doesn't connect to the ISA — it *is* the hardware realization of what the ISA requires.

## 6. The Function Calling Convention

When a function uses callee-saved registers, it follows a three-part structure to avoid corrupting the caller's data:

1. **Prologue** — save the registers about to be used, onto the stack.
2. **Body** — perform the actual computation.
3. **Epilogue** — restore the saved registers and return to the caller.

This convention lets independently written functions call each other safely, since every function guarantees it will leave the register state as it found it (except for designated return values).

## Reference

Term Meaning 
ISA -Instruction Set Architecture — a specification 
ABI - Application Binary Interface — register usage convention 
Register - Fast on-chip storage, built from flip-flops 
Stack - Scratch memory for temporarily saving register values 
Prologue / Epilogue - Setup and cleanup code around a function body 
Status

🚧 Actively learning — this repo will grow as I work through more exercises and start writing my own programs instead of just following examples.
