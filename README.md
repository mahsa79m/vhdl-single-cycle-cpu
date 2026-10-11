# 32-bit Single-Cycle CPU in VHDL

An educational CPU design with a MIPS-style instruction format, developed as a university Digital Computer Design course project.

## Authors

Mahsa Maleki 

## Overview

The design explores instruction fetching, opcode decoding, register access, arithmetic and logic operations, and writeback through modular VHDL components.

## Included modules

| File | Purpose |
| --- | --- |
| `src/PC.vhd` | 32-bit program counter with asynchronous reset |
| `src/InstructionMemory.vhd` | 32-word instruction ROM with asynchronous read |
| `src/RegisterFile.vhd` | 32 registers of 32 bits, two read ports, synchronous write, protected register zero |
| `src/ALU.vhd` | AND, OR, addition, subtraction and unsigned less-than comparison |
| `src/ControlUnit.vhd` | Control signals for R-type, LW, SW, BEQ and ADDI opcodes |
| `src/SingleCycleCPU.vhd` | Structural integration, immediate sign extension and writeback selection |
| `docs/project-report-fa.pdf` | Original Persian university project report |

## Example ROM program

| Address | Machine code | Decoded instruction |
| --- | --- | --- |
| 0 | `00221820` | `add $3, $1, $2` |
| 4 | `00222022` | `sub $4, $1, $2` |
| 8 | `AC030008` | `sw $3, 8($0)` |
| 12 | `8C050008` | `lw $5, 8($0)` |

## Current snapshot and limitations

This repository preserves the six submitted VHDL files without source-code changes. It documents the supplied snapshot rather than claiming a complete MIPS implementation.

- `SingleCycleCPU.vhd` references `DataMemory`, but `DataMemory.vhd` was not supplied. The report includes its listing; the original source should be added before running a full CPU simulation.
- Testbenches are discussed and a CPU testbench is listed in the report, but separate testbench files were not supplied.
- Next-PC logic currently always advances by four. BEQ is decoded, but branch target selection is not connected. Jump is not implemented.
- The ALU supports five operations, but the top-level function decoder explicitly selects ADD and SUB only; other R-type functions default to AND. OR and SLT need explicit decoding before they can execute correctly as CPU instructions.
- SLT compares unsigned operands and should not be described as signed MIPS SLT.
- Register values 10 and 5 are initialized for the example, but asserting reset clears all registers. A reset-based test must arrange suitable input values to demonstrate nonzero arithmetic results.
- ROM addressing uses only bits 6 through 2, so execution wraps after 32 instruction words. Zero-filled words are not explicitly decoded as NOP, although register-zero write protection prevents them from changing registers.

## Verification status

Source review and report comparison were performed during portfolio preparation. No VHDL compiler or simulator was available in that environment, so compilation, simulation, synthesis, timing and FPGA deployment have not been independently verified for this snapshot. The report's simulation claims should be read as the authors' original course documentation.

## Running the project

Restore the original data-memory source and testbenches first. Then add sources to your VHDL simulator in this dependency order: PC, InstructionMemory, ControlUnit, RegisterFile, ALU, DataMemory, SingleCycleCPU, and the testbench. Set the testbench as the simulation top level. Select a VHDL standard compatible with the original project; no tool/version metadata was supplied.

## Skills demonstrated

VHDL, digital logic, computer architecture, modular hardware design, datapath integration, instruction decoding, control signals and technical documentation.

## Attribution

This is a joint university project. Preserve both authors' attribution. No distribution license has been chosen in this prepared snapshot.
