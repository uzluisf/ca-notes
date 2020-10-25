# Computer Architecture II Notes

# Resources

* Textbook: *Computer Organization and Design*
* [MIPS instruction
  reference](http://alumni.cs.ucr.edu/~vladimir/cs161/mips.html)

# Introduction

## Measuring Performance

### Throughput and latency

* **response time**
* **throughput**
* **latency**

### Performance and execution time

To maximize performance, we want to minimize **response time** (or execution
time) for some task. Thus, we can relate performance and execution time for a
computer X:

$$\text{Perfomance}_{X} = \frac{1}{\text{Execution time}}_{X}$$

We also want to relate the performance of two different computers
quantitatively. We will use the phrase “X is n times faster than Y” (or
equivalently “X is n times as fast as Y”) to mean

$$\frac{\text{Performance}_{X}}{\text{Performance}_{Y}} = n$$

If $X$ is n times as fast as $Y$, then the execution time on $Y$ is $n$ times as
long as it is on $X$:

$$\frac{\text{Performance}_{X}}{\text{Performance}_{Y}} =
\frac{1/\text{Execution time}_{X}}{1/\text{Execution time}_{Y}} =
\frac{\text{Execution time}_{Y}}{\text{Execution time}_{X}} = n$$

**Example:** If computer A runs a program in 10 seconds and computer B runs the
same program in 15 seconds, how much faster is A than B?

We know that A is n times as fast as B if

$$\frac{\text{Performance}_{A}}{\text{Performance}_{B}} = \frac{\text{Execution
time}_{B}}{\text{Execution time}_{A}} = n$$

Thus,

$$\frac{15}{10} = 1.5$$

meaning than $A$ is $1.5$ times as fast as $B$.

### CPU time and performance

Time is the measure of computer performance: the computer that performs the same
amount of work in the least time is the fastest. Program execution time is
measured in seconds per program.

The **CPU execution time** is the time the CPU spends computing for a given
task and does not include time spent waiting for I/O or running other programs.
Further divided into:

* **user CPU time** is the CPU time spent in a program itself (i.e., the user).
* **system CPU time** is the CPU time spent in the operating system (OS)
  performing tasks on behalf of the program. 

---

Even though as computer users we care about time, it’s convenient to think about
performance in other metrics such as a measure relating how fast the hardware
can perform basic functions. Almost all computers are constructed using a clock;
this **computer clock** runs at constant speed and determines when events take
place in the hardware.

* The **clock cycle time** is the amount of time between two pulses in an
  oscillator. For example, $5ns$ means that $5$ nanoseconds elapsed between two
  pulses. In other words, $5$ nanoseconds per cycle.
* What about if we're interested in *the number of cycles per second* instead?
  We just invert the clock cycle which gives us the **clock rate**, i.e., the
  inverse of the clock cycle. For example, if a computer has a clock cycle of
  $5$ nanoseconds, the clock rate is

$$\frac{1}{5ns} = \frac{1}{5\times 10^{-9} \text{sec}} = 200\text{ MHz}$$

We know that $1\text{ MHz} = 1000000\text{ cycles/second}$, thus the
aforementioned computer performs $20000000$ cycles per second.

These discrete time intervals are called clock cycles (or ticks, clock ticks,
clock periods, clocks, cycles). Designers refer to the length of a

### Clock cycles, clock cycle time, and CPU time

A simple formula that relates the most basic metrics (clock cycles and clock
cycle time) to CPU time is:

$$\text{CPU time} = \text{CPU clock cycles}\times \text{CPU cycle time}$$

Alternatively, because clock cycle time and clock rate are inverses,

$$\text{CPU time} = \frac{\text{CPU clock cycles}}{\text{clock rate}}$$

This formula makes it clear that the hardware designer can improve performance
by reducing the *number of clock cycles* required for a program or *the length
of the clock cycle*.

**Example:** A computer program runs in $10$ seconds on computer A, which has a
$2\text{ GHz}$ clock. We want to build a computer, B, that will run the same
program in $6$ seconds. Increasing in the clock rate is possible, but this
increase will affect the rest of the CPU design, causing computer B to require
$1.2$ times as many clock cycles as computer A for this program. What clock rate
should we target?

   1. Find the number of clock cycles required for the program on A:
   \begin{align*} \text{CPU time}_A &= \frac{\text{CPU clock
   cycles}_{A}}{\text{clock rate}_A} \\ 10 \text{ seconds} &= \frac{\text{CPU
   clock cycles}_{A}}{2\times 10^{9} \frac{\text{cycles}}{\text{second}}} \\
   \text{CPU clock cycles}_{A} &= 10 \text{ seconds} \times 2\times 10^{9}
   \frac{\text{cycles}}{\text{second}} \\ &= 20 \times 10^{9} \text{cycles}
   \end{align*}
   2. Find CPU time for B: \begin{align*} \text{CPU time}_{B} &= \frac{1.2\times
   \text{CPU clock cycles}_A}{\text{clock rate}_B }\\ 6 \text{ seconds} &=
   \frac{1.2\times 20\times 10^{9}\text{ cycles}}{\text{clock rate}_B } \\
   \text{Clock rate}_B &= \frac{1.2\times 20\times 10^{9}\text{ cycles}}{6
   \text{ seconds}} \\ &= \frac{4\times 10^{9}\text{ cycles}}{\text{second}} =
   4\text{ GHz} \end{align*}

Thus to run the program in $6$ seconds, B must have twice the clock rate of A.

### Instruction performance

Compilers generate instructions to execute, and the computer had to execute
those instructions to run the program, thus the execution time must depend on
the number of instructions in a program. The execution time can be expressed as
the number of instructions executed multiplied by the average time per
instruction:

$$\text{CPU clock cycles} = \text{instructions} \times \text{avg clock cycles
per instruction}$$

The **clock cycles per instruction (CPI)** is the average number of clock cycles
per instruction for a program or program fragment.

**Example:** Suppose we have two implementations of the same instruction set
architecture. Computer A has a clock cycle time of 250ps and a CPI of 2.0 for
some program, and computer B has a clock cycle time of 500ps and a CPI of 1.2
for the same program. Which computer is faster for this program and by how much?

Let's $I$ be the number of instructions executed for the program; it's the same
for both computers. First, find the number of CPU clock cycles for each
computer:

$$\text{CPU clock cycles}_A = I \times 2.0$$ $$\text{CPU clock cycles}_B = I
\times 1.2$$

Now compute the CPU time for each computer:

\begin{align*} \text{CPU time}_B &= \text{CPU clock cycles}\times \text{clock
cycle time} \\ &= I\times 2.0 \times 250ps = 500 \times I ps \end{align*}

Similarly for B:

$$\text{CPU time}_B = I\times 1.2 \times 500ps = 600 \times I ps$$

Clearly, computer A is faster. The amount faster is given by the ration of the
execution times:

$$\frac{\text{CPU performance}_A}{\text{CPU performance}_B} =
\frac{\text{Execution time}_B}{Execution time}_A = \frac{600\times I
ps}{500\times I ps} = 1.2$$

Thus, computer A is 1.2 times as fast as computer B.

### The classic CPU performance equation

We can now express the basic performance equation using **instruction count**
(i.e., the nunmber of instructions executed by the program), CPI, and clock
cycle time:

$$\text{CPU time} = \text{instruction count}\times \text{CPI} \times \text{clock
cycle time}$$

**Example:** A compiler designer is trying to decide between two code sequences
for a particular computer. The hardware designers have supplied the following
facts:

|  Instruction class | CPI | Code sequence 1 | Code sequence 2 |
|--------------------| --- | --------------- | --------------- |
|  A                 |  1  | 2               | 4               |
|  B                 |  2  | 1               | 1               |
|  C                 |  3  | 2               | 1               |

1. Which code sequence executes the most instructions?

Sequence 1 executes $2 + 1 + 2 = 5$ instructions. Sequence 2 executes $4 + 1 + 1
= 6$ instructions. Therefore, sequence 2 executes the most instructions.

2. Which will be faster?

We can use the equation for CPU clock cycles based on instruction count and CPI
to find the total number of clock cycles for each sequence:

$$\text{CPU clock cycles} = \sum^{n}_{i=1}(\text{CPI}_i\times C)$$

This yields

$$\text{CPU clock cycles}_1 = (1\times 2) + (2\times 1) + (3\times 2) = 10\text{
cycles}$$ $$\text{CPU clock cycles}_2 = (1\times 4) + (2\times 1) + (3\times 1)
= 9\text{ cycles}$$

Thus code sequence 2 is faster (i.e., fewer cycles), even though it executes
more instructions.

3. What is the CPI for each sequence?

We know that $\text{CPU clock cycles} = \frac{\text{CPU clock
cycles}}{\text{instruction count}}$. Thus,

$$\text{CPI}_1 = \frac{\text{CPU clock cycles}_1}{\text{instruction count}_1} =
\frac{10}{5} = 2.0$$ $$\text{CPI}_2 = \frac{\text{CPU clock
cycles}_2}{\text{instruction count}_2} = \frac{9}{6} = 1.5$$

**Example:** A given application written in Java runs 15 seconds on a desktop
processor. A new Java compiler is released that requires only 0.6 as many
instructions as the old compiler. Unfortunately, it increases the CPI by 1.1.
How fast can we expect the application to run using this new compiler?

# Assembly language programming

## Intro to MIPS assembly Language

To command a computer’s hardware, you must speak its language. The words of a
computer’s language are called **instructions**, and its vocabulary is called an
**instruction set**. MIPS is an instruction set comes from MIPS Technologies,
and is an elegant example of the instruction sets designed since the 1980s.
Among other popular instruction sets there are:

* ARMv7 is similar to MIPS. More than 9 billion chips with ARM processors were
  manufactured in 2011, making it the most popular instruction set in the world.
* The second example is the Intel x86, which powers both the PC and the cloud of
  the PostPC Era. 
* The third example is ARMv8, which extends the address size of the ARMv7 from
  32 bits to 64 bits. Ironically, as we shall see, this 2013 instruction set is
  closer to MIPS than it is to ARMv7.

A **computer architecture** is a set of rules and methods that describe the
functionality, organization, and implementation of computer systems. Some
definitions of architecture define it as describing the capabilities and
programming model of a computer but not a particular implementation. The **MIPS
architecture** specifically refers to a special computer architecture that
features reduced instruction sets, which is what it's used in this course.

## Operands and memory access

Operands of arithmetic instructions are restricted, and thus they're based on a
limited number of special locations built directly in hardware called
**registers**. The size of a register in the MIPS architecture is 32 bits; a
group of 32 bits is called a **word**, a natural unit of access in a computer.

### Arithmetic operands: `add` and `sub`

A MIPS instruction operates on two source operands and places the result in one
destination operand. The `add` instruction adds the contents of two registers
and places the result into another one.

```
add $s0, $s1, $s2 # s0 = s1 + s2
sub $s0, $s1, $s2 # s0 = s1 - s2
```

**Example:** Given the C statement `f = (g + h) – (i + j);`. The variables f, g,
h, i, and j are assigned to the registers $s0, $s1, $s2, $s3, and $s4,
respectively. What is the compiled MIPS code?

1. The first MIPS instruction calculates `g + h`. The result must be placed
somewhere, so the compiler creates a temporary variable `$t0` and places it
there.

2. Next we calculate `i + j` by placing the sum into a temporary variable `$t1`:

3. Finally, the `sub` instruction substracts the second sum from the first and
places the difference in the register `$s0` (i.e., variable `f`):

Thus:

```
add $t0, $s1, $s2 # t0 = s1 + s2
add $t1, $s3, $s4 # t1 = s3 + s4
sub $s0, $t0, $t1 # s0 = t0 - t1
```

are the MIPS instructions for `f = (g + h) – (i + j);`

### Memory operands: `lw` and `sw`

Arithmetic operations occur only on registers in MIPS instructions; thus, MIPS
must include instructions that transfer data between memory and registers. These
instructions are called **data transfer instructions**. To access a word in
memory, the instruction must supply the memory address. Memory is just a large,
single-dimensional array, with the address acting as the index to that array,
starting at 0. For example, the address of the third data element is `2`, and
the value of ``Memory [2]` is `C`, as shown below:

```
Address | 0 | 1 | 2 | 3 | ... 
Data    | A | B | C | 4 | ...
```

---

**lw** (stands for *load word*) - This MIPS instruction copies data from memory
to a register, and is traditionally called **load**. The format of the load
instruction is the name of the operation followed by the register to be loaded
with the memory's data, then a constant (i.e., **offset**) and register used to
access memory (i.e., **base register**). The sum of the constant portion of the
instruction and the contents of the second register forms the memory address.
 
```
lw $s1, 0($s0)  # loads the memory address of s0 into s1
lw $s1, 16($s0) # loads the memory address of s0 + 4 into s1
```

All offsets are divisible by 4 and increase the memory address by 1 (for each 4
bits).

**Example**: Let’s assume that A is an array of 100 words and that the compiler
has associated the variables g and h with the registers $s1 and $s2 as before.
Let’s also assume that the starting address, or base address, of the array is in
$s3. Compile the C assignment statement `g = h + A[8];`.

1. We must first transfer `A[8]` into a register. The address of this array
element is the sum of the base of the array A, found in register $s3, plus the
number to select element 8. The data should be placed in a temporary register
for use in the next instruction.

2. Next we add `h` to `A[8]` (already in register `$t0`) and place the sum into
the register corresponding to `g`.


Thus we get:

```
lw $t0, 32($s3)   # t0 = A[8]
add $s1, $s2, $t0 # g = h + A[8]
```

Notice that the proper offset added to the base register `$s3` is `4 x 8 = 32`,
and not simply `8`. If we used `8($s3)`, we would select `A[8/4]` (remember
offsets are divisible by 4) and not the appropriate `A[8]`.

---

**sw** (stands for *store word*) - Traditionally called **store**, this
instruction copies data from a register to memory. The format of a store is
similar to that of a load.

**Example:** Assume variable h is associated with register $s2 and the base
address of the array A is in $s3. What is the MIPS assembly code for the C
assignment statement `A[12] = h + A[8];`?

1. Transfer `A[8]` into a register (we use `$t0`).
2. Add `h` and `A[8]` into a register (we reuse `$t0`).
3. Store the sum into `A[12]`.

Thus we've:

```
lw $t0, 32($s3)   # t0 = A[8]
add $t0, $s2, $t0 # t0 = h + A[8]
sw $t0, 48($s3)   # A[12] = t0
```

---

Load word and store word are the instructions that copy words between memory and
registers in the MIPS architecture. Other brands of computers use other
instructions along with load and store to transfer data. An architecture with


### Constant/Immediate operands: `addi`

Many times a program will use constants in an operation (e.g., incrementing an
index to point to the next element of an array). In such cases we could use the
arithmetic instructions we've seen so far but we would need to load the
constants from memory, which can be avoided by using versions of the arithmetic
instructions in which an operand is a constant.

**Example:** To add the constant 4 to register $s3, we could use the code 

```
lw $t0, AddrConstant4($s1)  # t0 = constant 4
add $s3,$s3,$t0             # s3 = s3 + t0
```

assuming that `$s1 + AddrConstant4` is the memory address of the constant 4.

The same operation can be achieved by using `addi` (*add immediate*) instead:

```
add $s3, $s3, 4 # $s3 = $s3 + 4
```

By including constants inside arithmetic instructions, operations are much
faster and use less energy than if constants were loaded from memory. The
constant zero has another role, which is to simplify the instruction set by
offering useful variations. For example, the move operation is just an add
instruction where one operand is zero. Hence, MIPS dedicates a register $\$zero$
to be hard-wired to the value zero. (As you might expect, it is register number
0.)

**NOTE:** Since MIPS supports negative constants, there is no need for subtract
immediate (or `subi` had there been one) in MIPS.

### Big endian vs little endian

Computers are divided either into those that use the address of:

* the **leftmost** or “big end” byte as the word address. Thus **big endian**
  refers to reading the memory address from left to right. Big endian has what's
  referred to as **big end**, i.e., the 2 leftmost bits.

* the **rightmost** or “little end” byte as the word address. Thus **little
  endian** refers to reading the memory address from right to left. Little
  endian has what's referred to as **little end**, i.e., the 2 rightmost bits.

MIPS is in the big-endian camp. Since the order matters only if you access the
identical data both as a word and as four bytes, few need to be aware of the
endianess. Byte orderig is only a problem when exchanging data among computers
with different orderings.

## Logical operations / Bit Twiddling

## Branching

## Loop

## Signed and unsigned numbers

## Instruction representation
