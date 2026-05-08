# Enhanced 9-Bit Processor (Verilog)

## Overview

An FSM-based 9-bit enhanced processor written in structural Verilog.
This version extends the basic processor by adding memory read/write support, program counter (PC), conditional branching (`mvnz`), and memory-mapped I/O using LEDs. 

---

## Features

### Supported Instructions

| Opcode | Instruction  | Description                 |
| ------ | ------------ | --------------------------- |
| `000`  | `mv Rx,Ry`   | Move data between registers |
| `001`  | `mvi Rx,#D`  | Load immediate data         |
| `010`  | `add Rx,Ry`  | Addition                    |
| `011`  | `sub Rx,Ry`  | Subtraction                 |
| `100`  | `ld Rx,[Ry]` | Load data from memory       |
| `101`  | `st Rx,[Ry]` | Store data to memory        |
| `110`  | `mvnz Rx,Ry` | Move if G register ≠ 0      |



## How It Works

### Instruction Fetch

* PC (`R7`) sends address to memory
* Instruction is loaded into `IR`
* PC automatically increments to next address 

### Execution Flow

```text id="g5xjmx"
PC → Memory → IR
        ↓
   FSM Decode
        ↓
MUX selects bus source
        ↓
ALU / Registers operate
        ↓
Write back result
```

### Memory Operations

#### Load (`ld`)

```text id="j5vlnn"
Ry → ADDR → Memory → DIN → Rx
```

#### Store (`st`)

```text id="srml4r"
Ry → ADDR
Rx → DOUT
W = 1 → Memory Write
```

### Conditional Move (`mvnz`)

```text id="26j2qa"
if (G != 0)
    Rx ← Ry
```

Used for loop and branch-like behavior. 

---

## Memory Map

| Address A8A7 | Function            |
| ------------ | ------------------- |
| `00`         | RAM access          |
| `01`         | LED output register |

When writing to address region `01`, data is sent to LEDs instead of RAM. 

---
