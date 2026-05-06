# NJVM - Ninja Virtual Machine

## Introduction

The NJVM (Ninja Virtual Machine) is a complete implementation of a stack-based virtual machine developed as the final homework assignment for the KSP (Kompilerseminar Praktikum) course. This project demonstrates the design and implementation of a modern virtual machine from the ground up, featuring a comprehensive instruction set, automatic memory management through garbage collection, and support for both primitive and complex data types.

Built incrementally across nine assignments, the NJVM executes bytecode programs compiled from custom assembly language or higher-level Ninja source code. It includes advanced features such as dynamic heap allocation, stop-and-copy garbage collection, big integer arithmetic, and object-oriented programming constructs, making it a fully functional virtual machine capable of running complex programs.

The implementation is written in C99, following strict coding standards and utilizing external libraries for specialized functionality like arbitrary-precision arithmetic.

## Basic CLI handling (njvma0.c)

This is the foundation of our virtual machine - a basic command-line interface that handles version information, help messages, and initial program execution signaling.
Version number **(0)**, help center and terminal handling are handled at this stage.
Below is an image of the tests...
![test00](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test0_0.png))


## Stack and basic arithmetic/Input/Output instructions (njvma1.c)

Assignment 1 introduces the core execution engine with a stack-based architecture and fundamental arithmetic operations, establishing the VM's ability to perform basic computations and I/O. VM version **(1)**.
![test11](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test1.png)


## Handling Stack frames as well as Global and Local variables (njvma2.c)
Here we add support for global and local variables through the static data area and stack frames, enabling more complex program structures and data management. This addition permits from the execution of asssembler programs on my VM. Below a test image(MOD, DIV,..).
![test2](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test2.png)

## Comparisons and control flow introduction (njvma3.c)
This section brings control flow capabilities with comparison operations and conditional branching, allowing programs to make decisions and execute different code paths.
Below an image of some tests(HCF).
![test3](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test3.png)

## Function calls and return values (njvma4.c)
This version introduces function call mechanics with CALL, RET, DROP and DUP instructions, plus a return value register(pushr, popr), enabling modular program design and code reuse.
![test4](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test4.png)

## Handling the Object system (njvma5.c)
Assignment 5 establishes the object system foundation with a new data model that can handle both primitive values and object references on the stack. Better, it is the direct conversion of the 4th version and doesn't handle primitive types only but objects and primitive types and most importantly big integers are introduced and managed here. The test image is follwed:
![test5](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test5.png)

## Arbitrary-Precision Integer Support (njvma6.c)
Extend the NJVM to handle arbitrary-precision integers (bigints) instead of 32-bit integers, enabling computations with very large numbers without overflow.Numbers are now heap-allocated objects with a size field (indicating byte count) and a flexible array of bytes (digits in base-256). The stack, static data area (SDA), and return value register hold pointers to these objects.
Arithmetic Operations: All math (add, sub, mul, div, mod) delegates to the provided bigint library (from Knuth's algorithms). For example:
ADD: Pops two bigint objects, adds them via bigAdd(), pushes the result.
MOD: Uses bigDiv() and pushes the remainder (bip.rem).
![test6](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test6.png)

## Records and Arrays as Objects (njvma7.c)
Objective: Add support for records (objects with fields) and arrays as heap-based objects, enabling object-oriented programming in Ninja.   Implementation of my personal malloc(myMalloc(...)) and so on...
![test7](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test7.png)


## Garbage Collection with Stop-and-Copy (njvma8.c)
Objective: Implement a compacting garbage collector (GC) using the stop-and-copy algorithm to manage heap memory dynamically.
Key Changes/Features:
Dynamic Allocation: Heap and stack allocated via malloc() with command-line options (--heap n for KiB, default 8192; --stack n for KiB, default 64).
Heap Structure: Split into two equal halves; allocation from one half until full, then trigger GC.
GC Algorithm: Stop-and-copy: Copy live objects to the other half, update all pointers (roots: SDA, return register, stack references, bigint processor components).
Broken Heart Flag: Use the second-highest bit of size for forwarding pointers (byte offset in target half).
![test8](https://github.com/ThierryD777/Ninja-Virtual-Machine-with-C/blob/21c5f68a0b086afc998997c2258d447ce3c1c18c/images/test8.png)
