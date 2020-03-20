# Lecture 02 Bits, Bytes, and Integers

### Representing information as bits

###### Everything is bits(*0 or 1*)

- By encoding/interpreting sets of bits in various way, computers determine what to do(instructions) and representation and manipulation of numbers,sets,strings,etc.
- Why bits? Electronic Implementation
  - Easy to store with bistable elements
  - Reliably transmitted on noisy and inaccurate wires

###### Base 2 number representation

_  _  .   _     _

1 0     -1   -2                                [*core of float number representations*]

2 1   1/2 1/4

###### Encoding Byte values

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

###### Example Data Representation(byte)

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

###### Boolean Algebra

- Algebraic representation of logic: Encode 'True' as 1 and 'False' as 0
- and &          or |             not ~         Exclusive-or(Xor) ^

###### General Boolean Algebras

- Operate on bit vectors
- All of the properties of boolean algebra apply

###### Bit-level Operations in C

- Operations &, |, ~, ^ available in C
  - Apply to any "Integral" data type(long, int, short, char, unsigned)
  - View arguments as bit vectors
  - Arguments applied bit-wise

###### Contrast: logic operations in C

- logical operations: && || !
  - view 0 as "False", anything nonzero as "True"
  - Always return 0 or 1
  - ***Early termination***
    - p && *p  (avoids null pointer access)

###### Shift Operations

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

###### Representation: unsigned and signed

- Encoding integers

  - Unsigned   
    $$
    B2U(X)=\sum_{i=0}^{w-1}{x_i*2^i}
    $$

  - Two's Complement : $x_{w-1}$ is sign bit
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

###### Conversion, casting

- Signed vs Unsigned in C

  - Constants

    - By default are considered to be signed integers
    - unsigned if have"U" as suffix

  - Casting

    - Explicit : 

      ```
      tx=(int)ux;
      uy=(unsigned)ty;
      ```

    - Implicit:

      ```
      tx=ux;
      uy=ty;
      ```

    - If there is a mix of unsigned and signed in single expression, signed values implicitly cast to unsigned,including comparison operations.

    ​       Basic Rules: Bit pattern is maintained but reinterpreted.

  - casting can have unexpected effects : adding or subtracting $2^w$

    *the result of **sizeof** is considered to be unsigned*

###### Expanding, truncating 

- Expanding(e.g.,short int to int)
  - Unsigned: zeros added
  - Signed: sign extension
  - Both yield expected result
  - Converting from smaller to larger integer data type, C automatically performs sign extension.
- Truncating(e.g.,unsigned to unsigned short)
  - Unsigned/signed: bits are truncated, result reinterpreted
  - Unsigned: mod operation & Signed: similar to mod
  - For small numbers yields expected behavior

###### Addition, negation, multiplication, shifting

###### Summary

### Representations in memory, pointers,strings