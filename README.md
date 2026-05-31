# RISC-V Emulator

> A RISC-V instruction set simulator written in C — implements the RV32I base integer instruction set, register file, and memory subsystem from scratch.

## Overview

An educational RISC-V emulator that takes a compiled RISC-V binary and executes it instruction-by-instruction in software. Built to understand CPU internals — the fetch-decode-execute cycle, two's-complement arithmetic, memory addressing, and how high-level constructs (function calls, loops) compile down to RISC-V assembly.

## Supported

- **ISA:** RV32I (32-bit base integer instruction set)
- **Instruction formats:** R-type, I-type, S-type, B-type, U-type, J-type
- **Memory:** Word-addressable simulated RAM
- **System calls:** Basic ECALL handling for I/O and program exit

## Tech

- **Language:** C
- **Build:** Makefile
- **Test programs:** Compiled with the RISC-V GNU toolchain (`riscv64-unknown-elf-gcc`)

## Architecture

```
ELF Binary  ──▶  Loader  ──▶  Memory[]
                                 │
                                 ▼
                       ┌──────────────────┐
                       │  Fetch (PC)      │
                       │     │            │
                       │     ▼            │
                       │  Decode          │
                       │     │            │
                       │     ▼            │
                       │  Execute  ───────┼──▶  Registers[32]
                       │     │            │
                       │     ▼            │
                       │  Update PC       │
                       └──────────────────┘
```

## Running

```bash
git clone https://github.com/SahilJ10319/RISCV-Emulator.git
cd RISCV-Emulator
make

# Run a compiled RISC-V program
./emulator path/to/program.bin
```

## What I Learned

- **Decoding instructions cleanly** is the hardest part — extracting the right bit slices for opcode, rs1, rs2, rd, funct3, funct7, and immediates without writing 40 magic numbers takes thought. Bitfield macros help.
- **Sign extension** trips up beginners frequently. Immediates in B-type and J-type are not just "the low N bits."
- **Two's complement arithmetic in C** mostly Just Works because of how C casts signed/unsigned integers, but the few places it doesn't (right-shift on signed types, overflow in add) are subtle.
