[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=9175436&assignment_repo_type=AssignmentRepo)
# RISC-V BASED CPU DESIGN
***
This repository is the outcome of RISC-V MYTH (Microprocessor for You in Thirty Hours) workshop conducted by VSDFLOW and REDWOOD-EDA. It contains all the information needed to build your RISC-V pipelined core, which has support of base interger RV32I instruction format using TL-Verilog on makerchip platform.

## TABLE OF CONTENTS
1. [Introduction to RISC-V ISA](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#1-introduction-to-risc-v-isa)
2. [GNU Compiler Toolchain](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#gnu-compiler-toolchain)
3. [Introduction to ABI](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#3-introduction-to-abi)
4. [Digital Logic with TL-Verilog and Makerchip](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#4-digital-logic-with-tl-verilog-and-makerchip)
  
      4.1 [Playing with Makerchip](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#41-playing-with-makerchip)
    
      4.2 [Calculator - Combinational Logic](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#42-calculator---combinational-logic)


      4.3 [Calculator - sequential Logic](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/README.md#43-calculator---sequential-logic)
      
      4.4 [Pipeline in Calculator](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/README.md#44-pipelined-logic)


      4.5 [Validity in calculator](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#45-validity)
      
5. [Basic RISC-V CPU micro-architecture](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#5-basic-risc-v-cpu-micro-architecture)


6. [Acknowledgements](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/edit/master/README.md#acknowledgements)

## 1. Introduction to RISC-V ISA
  ISA (Instruction Set Architecture ) defines the set of basic operations defines how the CPU is controlled by software. It is an interface between hardware and software. 
  
  
  RISC-V is an ISA developed with RISC (Reduced Instruction Set Computer) principle. It is revolutionary because of it's open source nature and it has smaller set of instruction compared to [CISC](https://en.wikipedia.org/wiki/Complex_instruction_set_computer)(Complex Instruction Set Computer).
  
  More details on RISC-V ISA can be obtained [here](https://github.com/riscv/riscv-isa-manual/releases/download/draft-20200727-8088ba4/riscv-spec.pdf)
  
## 2. GNU Compiler Toolchain

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
![Ofast_command](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Ofast.PNG)
More details on compiler options can be obtained here

To view assembly code use the below command,
```
riscv64-unknown-elf-objdump -d <object filename>
```
Below is the output of object dump captured when debugging the printf address. 
![Obj_when_debugging_printf](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Obj.png)


To use SPIKE simualtor to run risc-v obj file use the below command,
```
spike pk <object filename>
```
To use SPIKE as debugger
```
spike -d pk <object Filename> with degub command as until pc 0 <pc of your choice>
```  

To view the registers we can use command as 
```
reg <core> <register name>.
````
![spike_debugger_in_action](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Spike-debugger.PNG)
## 3. Introduction to ABI

   In computer software, an application binary interface (ABI) is an interface between two binary program modules. Often, one of these modules is a library or operating system facility, and the other is a program that is being run by a user.Words from [wiki](https://en.wikipedia.org/wiki/Application_binary_interface)
ABI is also called as system call interface used by the application program to access the registers specific to architecture. Since the MYTH workshop is using RISC-V architecture, it is good to know the register details in detail. 


![ABI](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/ABI-Image.png)

There are around 32 registers in RISCV ISA, since only 5-bits are alloted for registers. Other than x0 (which is hardwired to zero) remaining registers are good to use.

## 4. Digital Logic with TL-Verilog and Makerchip

  [Makerchip](https://makerchip.com/) is a free online environment for developing high-quality integrated circuits. You can code, compile, simulate, and debug Verilog designs, all from your browser. Your code, block diagrams, and waveforms are tightly integrated.

All the examples shown below are done on Makerchip IDE using TL-verilog. Also there are other tutorials present on IDE which can be found [here](https://makerchip.com/sandbox/) under Tutorials section.

### 4.1 Playing with Makerchip
  Before start playing the game (Designing a RISC-V CPU) it is good to know/familiarize with the ground (IDE). So we were tasked to design some basic logic gates using TL-Verilog and explore the options/features in the makerchip platform.   
Designed gates such as Inverted, Operations on gates, vector ,counter and Mux. 

Inverter 
![Inverter](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/A-Inverter.PNG)

Operations on gates
![logic_operation](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/B-Logic_gates.PNG)

Vector 
![vector](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/C-Vector.PNG)

MUX
![mux](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Mux.PNG)

MUX with Vector
![mux_with_vector](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Mux_vector.PNG)

Free running counter 
![counter](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Free-running-counter.PNG)

## 4.2 Calculator - Combinational Logic
  So the real learning started by designing a calculator. Beauty of the makerchip is there is no need for declaration for the variable used in the program unlike verilog. There is also no need to create testbench for simple logic such as calculator, makerchip provides random stimulus to the inputs. As you can observe the random values in the wavefor (bottom right corner).
  
  
Below is the snapshot of calculator with combinational logic.
![combinational_logic](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Calculator_coninational_logic.PNG)

## 4.3 Calculator - sequential Logic
  To get started on the sequential logic, before implementing in calculator we implemented in fibonacci series as below: 
  
  ![Fibonacci_series](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Fibonacci_tlv.PNG)
  
  Interesting thing about one of the operators used above ">>n" is ahead operator. which will provide the value of that signal n cycle before.
  Coming back to sequential calculator, used the above mentioned ahead operator to calculate the previous output in as input (Line 16)
  
  ![seqential_calculator](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Sequential_calculator.PNG)
  
## 4.4 Pipelined logic
  Timing abstract powerful feature of TL-Verilog which converts a code into pipeline stages easily. Whole code under |pipe scope with stages defined as @?

Below is snapshot of 2-cycle calculator which clears the output alternatively and output of given inputs are observed at the next cycle.

![pipline_cal](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/cycle_calculator.PNG)

## 4.5 Validity 
  
  ```
  ?valid 
  ```
  This validty executes the block (whithin their scope) when it is true. This operator provides easier debug, cleaner design, better error checking, automated clock gating.
  Below is 2-cycle with validity 

  ![validity](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/cycle_calculator_with_validity.PNG)

## 5. Basic RISC-V CPU micro-architecture

  Designing the basic processor of 3 stages Fetch -> Decode -> Execute based on RISC-V ISA.
  If a processor is mentioned as single cycle processor then it is capable of executing **Fetch -> Decode -> Execute** all instruction in a single cycle. 
  ![CPU micro-architecture](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/riscv_uA.PNG)
  
  ### Fetch 
  
    Program Counter (PC): Holds the address of next Instruction
    Instruction Memory (IM): Holds the set of instructions to be executed
  
  During fetch stage, processor fetch instruction from IM directed by PC.
  ![Fetch](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Day4-Imem.PNG)
  
  ### Decode 
    
    
   In this stage, the instruction fetched from the previous stage is dismantled in decode stage. It will decode into opcode, immediate value, source address and destination address. decode happens based on the instruction format and types of instruction.
   
   
  ![Decode](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Day-4-Decode.PNG)
  
  
  ### Execute
  
   In this stage, based on the opcode, respectice operation is performed on the sorce and destination register. Initally we implemented ADD and ADDI to make the sum of 1 to n C program running on the top of the RSIC V cpu that we are developing. 
   
  ![ALU](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Day4-Alu.PNG)

After implementing the Branch, control logic and after some debugging got the expected output from the C program which is 45 (sum of numbers from 1 to 9).

Then we started updating the code with pipeline logic and self checking condition
   *passed = |cpu/xreg[15]>>5$value == (1+2+3+4+5+6+7+8+9);

Finally added the remaining Arithmetic, Branch, Load, Jump instruction we completed the RISC V CPU design. 


Makechip IDE: [RISC V CPU](https://makerchip.com/sandbox/04xfJhN3W/0zmhGmo#)


Final Code : [CPU](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Day3_5/risc-v_solutions.tlv)

![VIZ](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Final_VIZ.PNG)
![waveform](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Final-wave.PNG)
![dig1](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Final_dig_1.PNG)
![dig2](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Final_dig_2.PNG)
![dig3](https://github.com/RISCV-MYTH-WORKSHOP/riscv_myth_workshop_nov22-rpjayaraman/blob/master/Images/Final_dig_3.PNG)


# Acknowledgements

[Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.


[Steve Hoover](https://github.com/stevehoover), Founder, Redwood EDA


[Shivam Potdar](https://github.com/shivampotdar), GSoC 2020 @fossi-foundation

