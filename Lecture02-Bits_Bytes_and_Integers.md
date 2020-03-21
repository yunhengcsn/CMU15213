

# Lecture 02 Bits, Bytes, and Integers

### Representing information as bits

##### Everything is bits(*0 or 1*)

- By encoding/interpreting sets of bits in various way, computers determine what to do(instructions) and representation and manipulation of numbers,sets,strings,etc.
- Why bits? Electronic Implementation
  - Easy to store with bistable elements
  - Reliably transmitted on noisy and inaccurate wires

##### Base 2 number representation

_  _  .   _     _

1 0     -1   -2                                [*core of float number representations*]

2 1   1/2 1/4

##### Encoding Byte values

- group collections of 4 bits at a time and then represent that in base 16(hexadecimal representation, using A ~F as values 10 through 15)
- A byte is 8 bits

| Hex  | Decimal | Binary |
| ---- | ------- | ------ |
| 0    | 0       | 0000   |
| 1    | 1       | 0001   |
| 2    | 2       | 0010   |
| 3    | 3       | 0011   |
| 4    | 4       | 0100   |
| 5    | 5       | 0101   |
| 6    | 6       | 0110   |
| 7    | 7       | 0111   |
| 8    | 8       | 1000   |
| 9    | 9       | 1001   |
| A    | 10      | 1010   |
| B    | 11      | 1011   |
| C    | 12      | 1100   |
| D    | 13      | 1101   |
| E    | 14      | 1110   |
| F    | 15      | 1111   |

##### Example Data Representation(byte)

| C Data Type | Typical 32-bit | Typical 64-bit | x86-64                                  |
| ----------- | -------------- | -------------- | --------------------------------------- |
| char        | 1              | 1              | 1                                       |
| short       | 2              | 2              | 2                                       |
| int         | 4              | 4              | 4                                       |
| long        | 4              | 8              | 8                                       |
| float       | 4              | 4              | 4                                       |
| double      | 8              | 8              | 8                                       |
| long double | -              | -              | 10/16（16字节为了64位机器上的增量对齐） |
| pointer     | 4              | 8              | 8                                       |

- Any address is defined to the sort of the word size of the machine. (*虚拟地址空间由字长决定*)
- When they say it's a  64-bit machine, what they really mean is that the addresses are 64-bit values or 8-byte values.

### Bit-level manipulations

*基于布尔运算来考虑位级运算*

##### Boolean Algebra

- Algebraic representation of logic: Encode 'True' as 1 and 'False' as 0
- and &          or |             not ~         Exclusive-or(Xor) ^

##### General Boolean Algebras

- Operate on bit vectors
- All of the properties of boolean algebra apply

##### Bit-level Operations in C

- Operations &, |, ~, ^ available in C
  - Apply to any "Integral" data type(long, int, short, char, unsigned)
  - View arguments as bit vectors
  - Arguments applied bit-wise

##### Contrast: logic operations in C

- logical operations: && || !
  - view 0 as "False", anything nonzero as "True"
  - Always return 0 or 1
  - ***Early termination***
    - p && *p  (avoids null pointer access)

##### Shift Operations

- left shift :   x  <<y     fill with 0 on right
- right shift : x>>y
  - logical shift: fill with 0 on left
  - arithmetic shift : replicate most significant bit on left
- undefined behavior: shift amount < 0 or >= word size

| Argument x | 01100010 |
| ---------- | -------- |
| << 3       | 00010000 |
| Log. >>2   | 00011000 |
| Arith. >>2 | 00011000 |

| Argument x | 10100010 |
| ---------- | -------- |
| << 3       | 00010000 |
| Log. >>2   | 00101000 |
| Arith. >>2 | 11101000 |

### Integers

##### Representation: unsigned and signed

- Encoding integers

  - Unsigned   
    $$
    B2U(X)=\sum_{i=0}^{w-1}{x_i*2^i}
    $$

  - Two's Complement（补码） : $x_{w-1}$ is sign bit
    $$
    B2T(X)=-x_{w-1}*2^{w-1}+\sum_{i=0}^{w-2}x_i*2^i
    $$

- Numeric Ranges

  - Unsigned values:  0 ~ $2^w-1$ ($U_{max}$)
  - Two's Complement values: $-2^{w-1}  (T_{min}) $     ~    $2^{w-1}-1  (T_{max})$ , 1.....1111 represents -1

- Values for different word sizes

|           | 8    | 16     | 32          |
| --------- | ---- | ------ | ----------- |
| $U_{max}$ | 255  | 65535  | 4294967295  |
| $T_{max}$ | 127  | 32767  | 2147483647  |
| $T_{min}$ | -128 | -32768 | -2147483648 |

$|T_{min}|=T_{max}+1$              $U_{max}=2*T_{max}+1$ 

- Unsigned & Signed Numeric Values
  - Equivalence : Same encodings for nonnegative values
  - Uniqueness
  - can invert mapping

| X    | B2U(X) | B2T(X) |
| ---- | ------ | ------ |
| 0000 | 0      | 0      |
| 0001 | 1      | 1      |
| 0010 | 2      | 2      |
| 0011 | 3      | 3      |
| 0100 | 4      | 4      |
| 0101 | 5      | 5      |
| 0110 | 6      | 6      |
| 0111 | 7      | 7      |
| 1000 | 8      | -8     |
| 1001 | 9      | -7     |
| 1010 | 10     | -6     |
| 1011 | 11     | -5     |
| 1100 | 12     | -4     |
| 1101 | 13     | -3     |
| 1110 | 14     | -2     |
| 1111 | 15     | -1     |

##### Conversion, casting

- Signed vs Unsigned in C

  - Constants

    - By default are considered to be signed integers
    - unsigned if have"U" as suffix

  - Casting

    - Explicit : 

      ```c
      tx=(int)ux;
      uy=(unsigned)ty;
      ```

    - Implicit:

      ```c
      tx=ux;
      uy=ty;
      ```

    - If there is a mix of unsigned and signed in single expression, signed values implicitly cast to unsigned,including comparison operations.

    ​       Basic Rules: Bit pattern is maintained but reinterpreted.

  - casting can have unexpected effects : adding or subtracting $2^w$

  - Don't use unsigned without understanding implications
  
    - easy to make mistakes
  
      ```c
      unsigned i;
      for(i = cnt - 2; i >= 0; i--)
      	a[i] += a[i+1]; //i会减到0，之后变为最大的无符号数，超过数组索引范围
      ```
  
    - can be very subtle
  
      ```c
      #define DELTA sizeof(int)
      int i;
      for(i = CNT; i - DELTA >= 0; i -= DELTA) //减法运算时将i转为无符号数
      ......
      ```
  
      *the result of **sizeof** is considered to be unsigned long*
  
  - Do Use Unsigned
  
    - When performing modular arithmetic
    - When using bits to represent sets
  
  - Counting Down with Unsigned
  
    - Proper way to use unsigned as loop index
  
      ```c
      unsigned i;
      for(i = cnt - 2; i < cnt; i--)
      	a[i] += s[i+1];
      ```
  
      - C standard guarantees that unsigned addition will behave like modular arithmetic   $0-1$ -->$Umax$  (See Robert Seacord, *Secure Coding in C and C++*)
  
    - Even better : unsigned is on our machines just a 32-bit value, but `size_t` is a 64-bit value.
  
      ```c
      size_t i;
      for(i = cnt - 2; i < cnt; i--)
      	a[i] += a[i+1];
      ```
  
      - Data type `size_t` defined as unsigned value with length = word size
      - Code will work even if `cnt=Umax` 

##### Expanding, truncating 

- Expanding(e.g.,short int to int)
  - Unsigned: zeros added
  - Signed: sign extension
  - Both yield expected result
  - Converting from smaller to larger integer data type, C automatically performs sign extension.
- Truncating(e.g.,unsigned to unsigned short)
  - Unsigned/signed: bits are truncated, result reinterpreted
  - Unsigned: mod operation & Signed: similar to mod
  - For small numbers yields expected behavior

##### Addition, negation, multiplication, shifting

- Addition    $w$ bit to $w+1$ bit
  - Unsigned
    - standard addition function：
      - *ignores carry output*
    - implements modular arithmetic  $s=UAdd_w(u,v)=(u+v)mod 2^w$
  - Two's Complement 
    - TAdd and UAdd have Identical Bit-level behavior
    - discard carry bit
    - TAdd Overflow : negative overflow（负数相加溢出得正数） or positive overflow（正数相加溢出得负数）
      - True sum requires $w+1$ bits
      - drop off MSB
- Multiplication   $w$ bit to $2w$ bit
  - Exact results
    - Unsigned : up to $2w$ bits
    - Two's Complement min : up to $2w-1$ bits , max : up to $2w$ bits,but only for $(TMin_w)^2$
    - So, maintaining exact results would need to keep expanding word size
  - Unsigned in C
    - standard multiplication function
      - *Ignores high order $w$ bits*
    - Implements modular Arithmetic
      - $UMult_w(u,v)=(u*v) mod 2^w$
  - Signed in C
    - Standard Multiplication Function
      - *Ignores high order $w$ bits*
      - completely irrespective of the signs of the original two operands
      - ***overflow***
      - Some of which are different for signed vs. unsigned multiplication , lower bits  are the same 
- Shift
  - Power-of-2 Multiply with Shift
    - Both signed and unsigned : $u<<k$ gives $u*2^k$
    - multiplication instruction took a lot longer than a shift or add instruction (most machines)
      - compilers generates this multiply-to-shift code automatically
  - Unsigned Power-of-2 Divide with Shift
    - Quotient of Unsigned by Power of 2
      - $u>>k$ gives $\lfloor u/2^k \rfloor$ ,向0下取整
      - uses logical shift
    - Quotient of Signed by Power of 2
      -  $u>>k$  gives $\lfloor u/2^k \rfloor$ ，向负无穷下取整
      - uses arithmetic shift (Most machines)
      - C standards says that there's no fixed definition
    - Division is really really slow even on a modern computer
  - In Java: only use two's complement
    - $>>>$  logical right shift 
    - $>>$ arithmetic right shift
- negate 取负 ：complement and increment取反加一 
  - Tmin取反加一的结果还是Tmin
  - 每个正数取反加一都能得到负数

### Representations in memory, pointers,strings

##### Byte-Oriented Memory Organization

*Memory is just this big array of bytes, numbered from 0 up to some maximum number. So for example in the machines we're using the 64-bit machines, an address is represented  in 64 bits, but in fact the  maximum address you're allowed to use in current machines is 47 bits.* $2^{47}$*约等于128 TB*

- Programs refer to data by address
- system provides private address spaces to each "process"
- Machine Words
  - Any given computer has a "Word Size"
    - **Word Size is nominal size of integer-valued data and of addresses** 
    - It's a combination of the hardware and the compiler that determines what is the word size being used in this particular program. ( GCC compiler can choose word size of 32 bit or 64 bit.)
    - Until recently, most machines used 32 bits(4 bytes) as word size, limiting addresses to 4 GB($2^{32} bytes$)
    - Increasingly, machines have 64-bit word size, which could have 18 PB of addressable memory.
    - Even though it's a 64-bit word size, the data type `int` without any other qualifiers to it is just 32 bit.
    - word or word size : we'll sort of throw it around when we mean sort of a generic chunk of bits without trying to assume that it has a particular number of bits to it.

##### Word-Oriented Memory Organization : group memory into blocks of words of different word sizes

- Addresses Specify Byte Locations
  - Address of the word is the lowest value address in it
  - Addresses of successive words differ by 4(32-bit) or 8(64-bit)
  - Aligned words: the compiler works pretty hard to keep things aligned because the hardware runs more efficiently that way.

##### Byte Ordering

- How are the bytes within a multi-byte word ordered in memory?
  - Big Endian (大端序) : Sun , PPC Mac, Internet
    -  Least significant byte has highest address
  - Little Endian (小端序)：x86, ARM processors running Android, iOS, and WIndows
    - Least significant byte has lowest address
  - Example : x has 4-byte value of 0x01234567
    - Big Endian : 01 23 45 67
    - Small Endian: 67 45 23 01

##### Representing Pointers

```
int B = -15123;
int *P = &B;
```

*Different compilers & machines assign different locations.*

##### Representing Strings

*Regardless of byte ordering, the ordering of characters is the same.*

- Strings in C is always represented by array of characters / a series of bytes, where the final byte is 0 called null terminator
- Each character encoded in ASCII format
  - standard 7-bit encoding of character set 
  - Character "0" has code 0x30