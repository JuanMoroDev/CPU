# Microprogrammed CPU & ALU in Logisim

A microprogrammed 8-bit processor and an x86-style ALU, designed and simulated in **Logisim-evolution** for the *Computer Architecture* course.

![Logisim-evolution](https://img.shields.io/badge/Logisim--evolution-3.9-blue)
![Topic](https://img.shields.io/badge/topic-computer_architecture-informational)

## Processor (`1.circ`)

Main circuit: `procesador`.

- **Datapath**: program counter (`PC`), instruction register (`IR`), memory address and data registers (`MAR`, `MBR`), general registers `R0` and `R1`, accumulators, stack pointer (`SP`) and flags register.
- **Memory**: RAM for program and data, connected through `MAR`/`MBR`.
- **Control unit**: microprogrammed. Each instruction is a sequence of micro-operations stored in a **control ROM** as 32-bit control words; each bit drives one control signal (register enables, RAM read/write, PC increment, end of instruction, ...).
- **Instruction set**: opcodes `00`–`0B`, 2 bytes each (opcode + operand, hence `PC += 2`), covering instruction fetch, loads and stores between RAM and registers, register-to-register moves, jumps and ALU operations.

### Microcode

| File | Content |
|---|---|
| `Instrucciones.ods` | Microcode design table: for every opcode, its micro-steps (e.g. `MAR ← PC`, `MBR ← RAM`, `IR ← MBR`, `PC += 2`, `FIN`), the state of each control bit and the resulting hex control word |
| `BIOS` | Control ROM image in Logisim format (`v3.0 hex words addressed`), generated from the table |

Example: the fetch cycle shared by all instructions.

| Step | Micro-operation | Control word |
|---|---|---|
| 00 | `MAR ← PC` | `20400000` |
| 01 | `RAMD ← RAM[MAR]` | `40000000` |
| 02 | `MBR ← RAMD` | `12000000` |
| 03 | `IR ← MBR` | `04800000` |

## ALU (`ArquitecturaDeComputadoras-23130490-JuanMoroyoqui-ALU/`)

One sub-circuit per operation, modelled on the x86 instruction set, plus the `ALU.circ` top level that selects among them:

| Group | Operations |
|---|---|
| Arithmetic | `ADD`, `ADC`, `SUB`, `SBB`, `INCREMENTO`, `DECREMENTO`, `NEGADO` (NEG), `SUMA8BITS`, `RESTA8BITS`, `SUMACOLUMNA`, `RESTACOLUMNA` |
| Multiply / divide | `MULTIPLICACION8BITS`, `IMULTI`, `DIVISION8BITS`, `DIVISIONCICLO8BITS`, `IDIV` |
| Logic | `AND`, `OR`, `XOR`, `NOT` |
| Shifts / rotations | `SHL`, `SHR`, `SAL`, `SAR`, `ROL`, `ROR`, `RCL`, `RCR` |
| Data | `XCHG`, `ConvByteWord` (CBW) |

The assignment brief is in `Tarea de ALU.pdf`. A zipped copy of the folder is included for submission.

## How to open

1. Install [Logisim-evolution](https://github.com/logisim-evolution/logisim-evolution) 3.9 or newer (needs Java).
2. Open `1.circ` for the processor, or `ALU.circ` inside the ALU folder.
3. In the processor, load `BIOS` into the control ROM if it is empty (*right-click the ROM → Load Image...*).
4. Use the clock (*Simulate → Ticks Enabled*) or step manually with *Ctrl+T*.

## Repository structure

```
1.circ                    Processor (Logisim-evolution)
BIOS                      Control ROM image (microcode)
Instrucciones.ods         Microcode design table
ArquitecturaDeComputadoras-23130490-JuanMoroyoqui-ALU/
    ALU.circ              ALU top level
    *.circ                One circuit per operation
    Tarea de ALU.pdf      Assignment brief
```

## Author

Juan Moroyoqui, Computer Architecture course.
