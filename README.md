[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=9175436&assignment_repo_type=AssignmentRepo)
# RISC-V BASED CPU DESIGN
***
This repository is the outcome of RISC-V MYTH (Microprocessor for You in Thirty Hours) workshop conducted by VSDFLOW and REDWOOD-EDA. It contains all the information needed to build your RISC-V pipelined core, which has support of base interger RV32I instruction format using TL-Verilog on makerchip platform.

## TABLE OF CONTENTS
1. [Introduction to RISC-V ISA](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#1-introduction-to-risc-v-isa)
2. [GNU Compiler Toolchain](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#gnu-compiler-toolchain)

## 1. Introduction to RISC-V ISA
  ISA (Instruction Set Architecture ) defines the set of basic operations defines how the CPU is controlled by software. It is an interface between hardware and software. 
  
  
  RISC-V is an ISA developed with RISC (Reduced Instruction Set Computer) principle. It is revolutionary because of it's open source nature and it has smaller set of instruction compared to [CISC](https://en.wikipedia.org/wiki/Complex_instruction_set_computer)(Complex Instruction Set Computer).
  
  More details on RISC-V ISA can be obtained [here](https://github.com/riscv/riscv-isa-manual/releases/download/draft-20200727-8088ba4/riscv-spec.pdf)
  
## GNU Compiler Toolchain

The GNU Toolchain is a set of programming tools in Linux systems that programmers can use to make and compile their code to produce a program or library.
Following is the list of command used to debug the program:

To use the risc-v gcc compiler use the below command:
```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o <object filename> <C filename>
```
More generic command with different options:
```
riscv64-unknown-elf-gcc <compiler option -O1 ; Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C filename>
```
More details on compiler options can be obtained here

To view assembly code use the below command,
```
riscv64-unknown-elf-objdump -d <object filename>
```
To use SPIKE simualtor to run risc-v obj file use the below command,
```
spike pk <object filename>
```
To use SPIKE as debugger
```
spike -d pk <object Filename> with degub command as until pc 0 <pc of your choice>
```  

  
