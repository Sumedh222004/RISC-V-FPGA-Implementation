# RISC-V Processor Implementation on FPGA

A Verilog implementation of a pipelined RISC-V CPU core, integrated into a simple System-on-Chip (SoC) framework. Developed as a summer project to explore RISC-V architecture on hardware, covering pipelining, hazard detection, memory-mapped I/O, and serial output.

## Project Overview

This project implements a functional RISC-V processor from the ground up in Verilog, with a focus on modularity and hardware efficiency. It includes the core SoC components and a data hazard detection mechanism for reliable pipelined execution, along with a matrix multiplication module built on top of the same core.

## Key Features

- **RISC-V CPU Core** — a pipelined CPU core implementing the RISC-V instruction set.
- **Pipelining** — Fetch, Decode, Execute, Memory, and Write-back stages for improved instruction throughput.
- **Data Hazard Detection** — a dedicated unit (`DataHazard.sv`) detects load-use hazards and stalls the pipeline to ensure correct execution.
- **Configurable Core** — parameters allow enabling or disabling features such as multiplication/division and pipeline bypassing.
- **Instruction Memory** — a hardcoded ROM (`instruction_memory.v`) supplying instructions, easily customizable for different programs.
- **Data Memory (BRAM)** — block-RAM-based data memory (`memory.v`, `data_BRAM.v`) with byte-enable support, communicating with the CPU via a handshake protocol.
- **UART Transmitter** — a UART module (`uart.v`) for serial output, used for debugging and viewing program results.
- **Matrix Multiplication Module** — a separate module demonstrating a compute workload running on the core.
- **Test Programs** — example programs (sorting, Fibonacci, arithmetic) preloaded into instruction memory for functional verification.

## Repository Structure

```
.
├── ppt/
│   └── RISC-V implementation on FPGA.pptx      Project presentation
├── verilog code/
│   ├── riscv/                                  CPU core, pipeline stages, memory, UART
│   └── matrix_multiplication/                  Matrix multiplication module
└── verilog_testbench/
    ├── riscv_tb/                               Testbenches for the CPU core
    └── matrixmul_tb/                           Testbenches for matrix multiplication
```

## How It Works

1. **Instruction Fetch** — the CPU fetches instructions from `instruction_memory.v`.
2. **Pipelined Execution** — instructions move through Fetch, Decode, Execute, Memory, and Write-back stages.
3. **Data Memory Access** — Load/Store instructions interact with `data_BRAM.v` and `memory.v`.
4. **Hazard Resolution** — `DataHazard.sv` monitors the pipeline for hazards (e.g. an instruction needing data not yet available from a previous load) and inserts stalls as needed.
5. **UART Output** — results and debug information are serialized by `uart.v` and transmitted for observation on a host machine.

## Customization

- **Programs:** Modify `instruction_memory.v` — update the Verilog `initial` blocks with compiled RISC-V assembly.
- **Memory Size:** Adjust BRAM parameters in `data_BRAM.v` or `memory.v`.
- **UART Baud Rate:** Modify the baud rate divisor in `uart.v`.

## Getting Started

### Clone the repository
```bash
git clone https://github.com/Sumedh222004/RISC-V-FPGA-Implementation.git
cd RISC-V-FPGA-Implementation
```

### Prerequisites
- A Verilog simulator, e.g. Xilinx Vivado Simulator, ModelSim, or Icarus Verilog
- For FPGA deployment: Vivado or your board's vendor toolchain

### Simulation
1. Open the testbench for the module to be tested (`verilog_testbench/riscv_tb/` or `verilog_testbench/matrixmul_tb/`).
2. Run it in a simulator of choice, e.g. with Icarus Verilog:
   ```bash
   iverilog -o sim.out <testbench_file>.v <source_files>.v
   vvp sim.out
   ```
3. Monitor the UART output or waveform for program results.

### Synthesis and FPGA Deployment
- The design is fully synthesizable and can target FPGA boards such as Xilinx Artix-7 or Zynq.
- Connect the UART TxD pin from the FPGA to a serial-to-USB adapter or logic analyzer to view output on a PC terminal.

## Demo

A demo video and presentation covering the design and results are included in this repository (`ppt/`). 

## Contributors

- [Sumedh Wankhede](https://github.com/Sumedh222004)
- [Anvesh Jadhav](https://github.com/jadhav-anvesh)
- Sandeep Boda

## Contact

For questions about this implementation, please open an issue on this repository.
