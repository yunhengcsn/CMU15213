# Lecture 01 Course Overview

##### Book: *Computer Systems: A Programmer's Perspective, Third Version(CS:APP3e),Pearson, 2016*

- Great Reality #1: Ints are not Integers, Floats are not Reals.
  - 整数运算发生溢出时，值可能变为整数或者复数，整数运算符合交换律、结合律
  - 浮点数运算不符合交换律、结合律，有舍入问题，可能会丢掉不重要的数字
- Great Reality #2: You've Got to Know Assembly（汇编语言）
  - Assembly is key to machine-level execution model
- Great Reality #3: Memory Matters - RAM is an unphysical abstraction
  - Memory Referencing Errors:
    - C and C++ do not provide any memory protection
    - can lead to nasty bugs
- Great Reality #4: There is more to performance than asymptotic complexity
  - Hierarchical memory organization
  - Performance depends on access patterns
- Great Reality #5: Computers do more than execute programs
  - get data in and out
  - communicate with each other over networks.

##### Role within CS/ECE Curriculum

- 213 is foundation of computer systems, underlying principles for hardware, software, and networking 
- network prog concurrency ----- CS 440 Distributed Systems
- Data Reps. Memory Model ----- CS 415 Databases
- Network Protocols ----- CS 441 Networks
- Processes Mem.Mgmt ----- CS 410 Operating Systems Cs 412 OS practicum
- Machine Code ----- CS 411 Compilers
- Arithmetic ----- ECE 340 Digital Computation
- Execution Model Memory System ----- ECE 447 ECE 349 ECE 545/549 ECE 348