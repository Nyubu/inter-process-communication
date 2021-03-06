# inter-process-communication
The project simulates a simple computer system consisting of a CPU and Memory.

## Project Overview:

The project simulates a simple computer system consisting of a CPU and Memory.
The CPU and Memory is simulated by separate processes that communicate.
Memory contains one program that the CPU will execute and then the simulation will end.

## Objectives

1)	Learn how multiple processes can communicate and cooperate.
2)	Understand low-level concepts important to an operating system. 

         a.	Processor interaction with main memory.
         b.	Processor instruction behavior.
         c.	Role of registers.
         d.	Stack processing.
         e.	Procedure calls.	
         f.	System calls.
         g.	Interrupt handling.
         h.	Memory protection.
         i.	I/O.
         j.	Virtualization/emulation

## Problem Details

### CPU
   It contains these registers:  PC, SP, IR, AC, X, Y.
   It supports the instructions shown at the bottom of this document
   It runs the user program at address 0.
   Instructions are fetched into the IR from memory.  The operand can be fetched into a local variable.
   Each instruction should be executed before the next instruction is fetched.
   The user stack resides at the end of user memory and grows down toward address 0.
   The system stack resides at the end of system memory and grows down toward address 0.
   There is no hardware enforcement of stack size.
   The program ends when the End instruction is executed.  The 2 processes should end at that time.
   The user program cannot access system memory (exits with error message).
   
### Memory
   It consists of 2000 integer entries, 0-999 for the user program, 1000-1999 for system code.
   It supports two operations:
       read(address) -  returns the value at the address
       write(address, data) - writes the data to the address
   Memory will initialize itself by reading a program file.
   Memory is simply storage that has no real logic beyond reading and writing.
 
   #### Timer
   A timer interrupts the processor after every X instructions, where X is a command-line parameter.

   #### Interrupt processing
  There are two forms of interrupts:  the timer and a system call using the int instruction.
  In both cases, the CPU enters kernel mode.
  The stack pointer is switched to the system stack.
  SP and PC registers is saved on the system stack.  (The handler may save additional registers). 
  A timer interrupt causes execution at address 1000.
  The int instruction should cause execution at address 1500.
  The iret instruction returns from an interrupt.
  Interrupts are disabled during interrupt processing to avoid nested execution.
  Interrupts are not allowed during system calls or vice versa.

### Instruction set
   
      1 = Load value          Load the value into the AC
      2 = Load addr           Load the value at the address into the AC
      3 = LoadInd addr        Load the value from the address found in the given address into the AC
                              (for example, if LoadInd 500, and 500 contains 100, then load from 100).
      4 = LoadIdxX addr       Load the value at (address+X) into the AC
                              (for example, if LoadIdxX 500, and X contains 10, then load from 510).
      5 = LoadIdxY addr       Load the value at (address+Y) into the AC
      6 = LoadSpX             Load from (Sp+X) into the AC (if SP is 990, and X is 1, load from 991).
      7 = Store addr          Store the value in the AC into the address
      8 = Get                 Gets a random int from 1 to 100 into the AC
      9 = Put port            If port=1, writes AC as an int to the screen
                              If port=2, writes AC as a char to the screen
      10 = AddX               Add the value in X to the AC
      11 = AddY               Add the value in Y to the AC
      12 = SubX               Subtract the value in X from the AC
      13 = SubY               Subtract the value in Y from the AC
      14 = CopyToX            Copy the value in the AC to X
      15 = CopyFromX          Copy the value in X to the AC
      16 = CopyToY            Copy the value in the AC to Y
      17 = CopyFromY          Copy the value in Y to the AC
      18 = CopyToSp           Copy the value in AC to the SP
      19 = CopyFromSp         Copy the value in SP to the AC 
      20 = Jump addr          Jump to the address
      21 = JumpIfEqual addr   Jump to the address only if the value in the AC is zero
      22 = JumpIfUnequal addr Jump to the address only if the value in the AC is not zero
      23 = Call addr          Push return address onto stack, jump to the address
      24 = Ret                Pop return address from the stack, jump to the address
      25 = IncX               Increment the value in X
      26 = DecX               Decrement the value in X
      27 = Push               Push AC onto stack
      28 = Pop                Pop from stack into AC
      29 = Int                Perform system call
      30 = Iret               Return from system call
      50 = End                End execution

  
