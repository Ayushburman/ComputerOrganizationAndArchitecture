
# 🖥️ Computer Organization & Architecture — GATE CSE 2027

> **COA Master Cheat Sheet**  
> Concept → Formula → Trap → PYQ → Revision  
>
> 🎯 **Goal:** Fast revision + numerical accuracy + GATE PYQ mastery

---

# 📚 Table of Contents

1. [Performance](#1-performance)
2. [Number Representation](#2-number-representation)
3. [ALU & Arithmetic](#3-alu--arithmetic)
4. [Instruction Set Architecture](#4-instruction-set-architecture)
5. [Control Unit](#5-control-unit)
6. [Pipelining](#6-pipelining)
7. [Memory Hierarchy & Cache](#7-memory-hierarchy--cache)
8. [Secondary Storage — Disk](#8-secondary-storage--disk)
9. [I/O & DMA](#9-io--dma)
10. [Common GATE Traps](#10-common-gate-traps)
11. [⚡ Ultra-Fast Formula Sheet](#-ultra-fast-formula-sheet)
12. [📝 Mastery Checklist](#-mastery-checklist)
13. [❌ Mistake Log](#-mistake-log)

---



# 1. Performance

## CPU Execution Time

\[
\boxed{CPU\ Time = IC \times CPI \times T}
\]

Since:

\[
T=\frac{1}{f}
\]

Therefore:

\[
\boxed{CPU\ Time=\frac{IC\times CPI}{f}}
\]

Where:

- `IC` = Instruction Count
- `CPI` = Cycles Per Instruction
- `f` = Clock frequency
- `T` = Clock cycle time

---

## MIPS


\[
\boxed{MIPS=\frac{f}{CPI\times10^6}}
\]

> ⚠️ MIPS alone does **not** necessarily indicate better performance.

---

## Amdahl's Law


If fraction `F` is improved by factor `s`:

\[
\boxed{
S=\frac{1}{(1-F)+\frac{F}{s}}
}
\]

Maximum possible speedup:

\[
\boxed{S_{max}=\frac{1}{1-F}}
\]

---

## Weighted CPI

\[
\boxed{CPI_{avg}=\sum(fraction_i\times CPI_i)}
\]

Example:

```text
30% instructions → CPI = 2
70% instructions → CPI = 1

CPI = 0.3(2) + 0.7(1)
    = 1.3
```

---

# 2. Number Representation

# 2's Complement

For `n` bits:

\[
\boxed{-2^{n-1}\leq x\leq2^{n-1}-1}
\]

Example: 8-bit:

```text
Minimum = -128
Maximum = +127
```

> ⭐ `−2ⁿ⁻¹` has no positive counterpart.

---

# 1's Complement

Range:

\[
\boxed{-(2^{n-1}-1)\text{ to }+(2^{n-1}-1)}
\]

Has **two zeros**:

```text
+0
-0
```

---

# Sign-Magnitude

Range:

\[
\boxed{-(2^{n-1}-1)\text{ to }+(2^{n-1}-1)}
\]

Also has:

```text
+0
-0
```

---

# Signed Overflow

For 2's complement addition:

\[
\boxed{Overflow=C_{in}\oplus C_{out}}
\]

That is:

```text
Carry into MSB ≠ Carry out of MSB
                ↓
             Overflow
```

Another useful rule:

```text
Positive + Positive → Negative  ⇒ Overflow

Negative + Negative → Positive  ⇒ Overflow
```

---

# Unsigned Overflow

For unsigned addition:

\[
\boxed{Carry\ out\ of\ MSB=1}
\]

---

# Sign Extension


When increasing the number of bits in 2's complement:

```text
Copy the sign bit into the new higher-order bits.
```

Example:

```text
4-bit:  1011
8-bit:  11111011
```

Value remains unchanged.

---

# IEEE-754 Floating Point

## Single Precision

```text
| Sign | Exponent | Fraction |
|------|----------|----------|
|  1   |    8     |    23    |
```

Bias:

\[
\boxed{127}
\]

Normal value:

\[
\boxed{
(-1)^S\times1.M\times2^{E-127}
}
\]

Maximum finite exponent field:

```text
E = 254
```

Approximate maximum finite value:

\[
\boxed{\pm3.4\times10^{38}}
\]

---

## Double Precision

```text
| Sign | Exponent | Fraction |
|------|----------|----------|
|  1   |    11    |    52    |
```

Bias:

\[
\boxed{1023}
\]

---

## Special IEEE-754 Cases

| Exponent | Fraction | Meaning |
|---|---|---|
| `0` | `0` | Zero |
| `0` | ≠ `0` | Denormal/Subnormal |
| All 1s | `0` | ±∞ |
| All 1s | ≠ `0` | NaN |

### Denormal

No implicit leading `1`.

Single precision:

\[
\boxed{0.M\times2^{-126}}
\]

> ⚠️ Normalized numbers use `1.M`; denormals use `0.M`.

---

# 3. ALU & Arithmetic

## Ripple Carry Adder

For `n` bits:

\[
\boxed{Delay=n\times Gate\ Delay}
\]

Carry must propagate through each stage.

---

# Carry Look-Ahead Adder

Generate:

\[
\boxed{G_i=A_iB_i}
\]

Propagate:

\[
\boxed{P_i=A_i\oplus B_i}
\]

Carry:

\[
\boxed{C_{i+1}=G_i+P_iC_i}
\]

Memory:

```text
G → Generate
P → Propagate
```

---

# Full Adder

Sum:

\[
\boxed{Sum=A\oplus B\oplus C}
\]

Carry:

\[
\boxed{Carry=AB+BC+CA}
\]

---

# Booth Multiplication

Booth algorithm:

- Supports signed multiplication
- Efficiently handles consecutive 1s
- Reduces unnecessary additions/subtractions

## Radix-4 / Modified Booth

Processes approximately 2 multiplier bits per iteration.

For `n` bits:

\[
\boxed{\text{Partial Products}\approx\frac n2}
\]

---

# Division

## Restoring Division

If subtraction produces a negative remainder:

```text
Negative remainder
       ↓
Restore previous remainder
```

---

## Non-Restoring Division

Does not restore the remainder immediately.

```text
Negative remainder
       ↓
Compensate during next operation
```

> ⭐ Usually faster than restoring division because explicit restoration is avoided.

---

# 4. Instruction Set Architecture

# Instruction Address Formats

| Format | Main Idea |
|---|---|
| 0-address | Stack |
| 1-address | Accumulator |
| 2-address | Destination often also source |
| 3-address | Separate source/destination operands |

> ⭐ Three-address instructions are commonly associated with register-based RISC-style designs, but address count alone does **not** define RISC.

---

# Expanding Opcode

Used when different instructions have different opcode lengths.

General approach:

```text
Available / unused opcode codes
        ↓
Allocate them to the next level
        ↓
Repeat
```

For an additional `b` bits:

\[
\boxed{\text{New possibilities}=2^b}
\]

> ⚠️ Carefully count **unused codes** at every level.

---

# Addressing Modes

## Immediate

Operand is directly inside instruction.

```text
MOV R1, #10
```

No effective address calculation required for the operand.

---

## Direct

\[
\boxed{EA=A}
\]

---

## Indirect

\[
\boxed{EA=M[A]}
\]

Memory contains the effective address.

---

## Register

Operand is in a register.

```text
R1
```

---

## Register Indirect

\[
\boxed{EA=R}
\]

Register contains the memory address.

---

## Indexed

\[
\boxed{EA=A+Index}
\]

Useful for arrays.

---

## Base Register

\[
\boxed{EA=A+Base}
\]

Useful for relocation and structured memory access.

---

## Relative

\[
\boxed{EA=PC+A}
\]

> ⚠️ PC normally already points to the **next instruction** after fetch.

---

## Auto-Increment / Auto-Decrement

Useful when sequentially accessing:

```text
Arrays
Stacks
Buffers
```

---

# RISC vs CISC

| Feature | RISC | CISC |
|---|---|---|
| Instruction length | Fixed | Often variable |
| Memory operations | Load/Store | Memory operands possible |
| Registers | Many | Generally fewer historically |
| Control | Hardwired | Traditionally microprogrammed |
| Pipelining | Easier | More difficult |
| Instruction complexity | Simple | Complex |

Memory trick:

```text
RISC → Reduced + Regular + Load/Store

CISC → Complex + Variable + Microprogrammed
```

---

# 5. Control Unit

# Hardwired Control

```text
Fast
Rigid
Difficult to modify
Commonly associated with RISC
```

---

# Microprogrammed Control

```text
Flexible
Easier to modify
Slower
Commonly associated with CISC
```

---

# Horizontal Microprogramming

```text
Wide control word
↓
Little/no encoding
↓
More parallelism
↓
Fast
↓
Large control store
```

Characteristics:

- Wide microinstruction
- Many control signals directly represented
- Fast
- Large memory requirement

---

# Vertical Microprogramming

```text
Encoded control signals
↓
Narrow control word
↓
Decoder required
↓
Less parallelism
↓
Smaller control store
```

Characteristics:

- Narrow
- Encoded
- Decoder required
- Slower
- Smaller control store

---

# Control Word Width

### Horizontal

Approximately:

\[
\boxed{\text{Width}=\text{Number of control signals}}
\]

### Vertical

For control groups:

\[
\boxed{
Width\approx\sum\log_2(group\ size)
}
\]

> ⚠️ If a group includes a "no operation" option, the required bits may involve `group size + 1`. Follow the exact question wording.

---

# Control Store Size

\[
\boxed{
Storage=
Number\ of\ Microinstructions
\times
Microinstruction\ Width
}
\]

---

# Nanoprogramming

Two-level control store:

```text
Microprogram
     ↓
Nanoprogram
     ↓
Actual control signals
```

---

# 6. Pipelining

## Classic 5-Stage Pipeline

```text
IF → ID → EX → MEM → WB
```

| Stage | Meaning |
|---|---|
| IF | Instruction Fetch |
| ID | Instruction Decode |
| EX | Execute |
| MEM | Memory Access |
| WB | Write Back |

---

# Pipeline Execution Time

For:

- `k` stages
- `n` instructions
- clock period `t`

\[
\boxed{
T=(k+n-1)t
}
\]

---

# Pipeline Speedup

\[
\boxed{
Speedup=\frac{nk}{k+n-1}
}
\]

As `n → ∞`:

\[
\boxed{Speedup\rightarrow k}
\]

---

# Pipeline Clock

\[
\boxed{
T_{clock}=
\max(T_{stage})+T_{latch}
}
\]

> ⭐ The **slowest stage** determines the pipeline clock.

---

# Non-Pipelined Time

If each stage has delay `tᵢ`:

\[
\boxed{
T_{non-pipeline}
=
n\sum_{i=1}^{k}t_i
}
\]

---

# CPI

Ideal pipeline:

\[
\boxed{CPI\approx1}
\]

With stalls:

\[
\boxed{
CPI=1+\frac{Stall\ Cycles}{Instructions}
}
\]

---

# Pipeline Hazards

## Structural Hazard

Hardware/resource conflict.

Example:

```text
Two instructions need the same memory
at the same time.
```

---

## Data Hazard

Dependency between instructions.

### RAW

\[
\boxed{Read\ After\ Write}
\]

True dependency.

### WAR

\[
\boxed{Write\ After\ Read}
\]

### WAW

\[
\boxed{Write\ After\ Write}
\]

> ⚠️ WAR and WAW generally arise in pipelines allowing out-of-order execution; a simple in-order 5-stage pipeline primarily faces RAW.

---

# Forwarding

Used to reduce data-hazard stalls.

```text
Producer
   ↓
Forward result
   ↓
Consumer
```

---

# Load-Use Hazard

Example:

```asm
LW  R1, 0(R2)
ADD R3, R1, R4
```

Even with forwarding:

\[
\boxed{1\ cycle\ stall}
\]

for the classic 5-stage pipeline.

---

# Control Hazard

Usually caused by:

```text
Branch
Jump
```

Possible solutions:

- Stall
- Predict not taken
- Static prediction
- Dynamic prediction
- Delayed branch

### Branch Penalty

Depends on how many stages/instruction cycles are lost before the branch outcome is available and the target can be used.

> ⚠️ Do **not** blindly use "number of pipeline stages" as branch penalty; use the exact branch resolution timing given in the question.

---

# 7. Memory Hierarchy & Cache

# Locality

## Temporal Locality

Recently accessed data is likely to be accessed again.

```text
Loop variables
Instructions in loops
```

## Spatial Locality

Nearby addresses are likely to be accessed.

```text
Array elements
Sequential instructions
```

---

# Cache Address

```text
| TAG | INDEX | OFFSET |
```

---

## Offset Bits

For block size `B` bytes:

\[
\boxed{Offset=\log_2(B)}
\]

---

## Number of Cache Lines

\[
\boxed{
Lines=\frac{Cache\ Size}{Block\ Size}
}
\]

---

## Number of Sets

For `k`-way set associative cache:

\[
\boxed{
Sets=\frac{Lines}{k}
}
\]

For fully associative:

\[
\boxed{Sets=1}
\]

---

## Index Bits

\[
\boxed{
Index=\log_2(Number\ of\ Sets)
}
\]

---

## Tag Bits

For an `A`-bit address:

\[
\boxed{
Tag=A-Index-Offset
}
\]

---

# Cache Organizations

## Direct Mapped

\[
k=1
\]

Each block maps to exactly one cache line.

---

## k-Way Set Associative

Each set contains:

\[
\boxed{k\text{ lines}}
\]

---

## Fully Associative

```text
One set
No index bits
```

Tag comparators:

\[
\boxed{Number\ of\ lines}
\]

---

# Cache Storage

Data storage:

\[
\boxed{Cache\ Size}
\]

But total implementation storage may include:

```text
Data
+ Tag
+ Valid bit
+ Dirty bit
+ Replacement/state bits
```

Tag memory:

\[
\boxed{
Lines\times(Tag+Valid+Dirty+State)
}
\]

> ⚠️ Only include fields requested by the question.

---

# Average Access Time

## Sequential / Hierarchical Lookup

If hit ratio is `H`:

\[
\boxed{
T=HT_1+(1-H)(T_1+T_2)
}
\]

Equivalent:

\[
\boxed{
T=T_1+(1-H)T_2
}
\]

---

## Simultaneous Lookup

\[
\boxed{
T=HT_1+(1-H)T_2
}
\]

> ⚠️ The distinction depends on how the two memories are accessed and what timing the question defines. Draw the timing before calculating.

---

# Memory Stall Cycles

\[
\boxed{
Stall\ cycles/instruction
=
Memory\ accesses/instruction
\times
Miss\ rate
\times
Miss\ penalty
}
\]

---

# AMAT

For three levels:

\[
\boxed{
AMAT=T_1+m_1(T_2+m_2T_3)
}
\]

Where:

- `T₁` = L1 access time
- `m₁` = L1 miss rate
- `T₂` = L2 access time
- `m₂` = L2 local miss rate
- `T₃` = main-memory access time

---

# Cache Miss Types

## Compulsory

First access to a block.

```text
First time ever
```

## Capacity

Cache is too small.

## Conflict

Blocks compete for the same set/line.

Memory:

```text
Compulsory → First
Capacity   → Too Small
Conflict   → Collision
```

---

# Write Policies

## Write Hit

### Write-Through

Update:

```text
Cache + Main Memory
```

Often uses a write buffer.

### Write-Back

Update cache first.

Main memory updated when block is evicted.

Requires:

\[
\boxed{Dirty\ bit}
\]

---

# Write Miss

### Write Allocate

Bring block into cache, then write.

### No Write Allocate

Write directly to lower memory.

---

# Replacement Policies

Common:

- LRU
- FIFO
- Random

---

# Main Memory Interleaving

## Low-Order Interleaving

Consecutive addresses go to different memory modules.

```text
Address 0 → Module 0
Address 1 → Module 1
Address 2 → Module 2
Address 3 → Module 3
```

Good for sequential access.

---

## High-Order Interleaving

Higher-order address bits select the module.

Often useful for expansion and larger contiguous regions.

---

# Virtual Memory & TLB

For single-level paging with TLB hit ratio `h`:

\[
\boxed{
EMAT=
h(t_{TLB}+t_m)
+
(1-h)(t_{TLB}+2t_m)
}
\]

Where:

- `h` = TLB hit ratio
- `tTLB` = TLB lookup time
- `tm` = memory access time

---

# Page Table Entry

Typical PTE contains:

```text
Frame Number
Valid/Present bit
Dirty bit
Protection bits
Other status bits
```

---

# 8. Secondary Storage — Disk

# Disk Capacity

\[
\boxed{
Capacity=
Surfaces
\times
Tracks/Surface
\times
Sectors/Track
\times
Bytes/Sector
}
\]

---

# Disk Access Time

\[
\boxed{
T_{access}
=
T_{seek}
+
T_{rotation}
+
T_{transfer}
}
\]

---

# Rotational Latency

If disk rotates at `RPS`:

One rotation:

\[
\boxed{
T_{rotation}=\frac1{RPS}
}
\]

Average rotational latency:

\[
\boxed{
T_{rot(avg)}=\frac{1}{2RPS}
}
\]

or:

\[
\boxed{
T_{rot(avg)}=\frac{30}{RPM}\ seconds
}
\]

---

# Transfer Time

If:

- Data = `D` bytes
- Track size = `B` bytes
- Rotation speed = `RPS`

Then:

\[
\boxed{
T_{transfer}=
\frac{D}{B\times RPS}
}
\]

---

# Cylinder

A cylinder consists of:

```text
The same track number
across all disk surfaces.
```

---

# 9. I/O & DMA

# Programmed I/O

```text
CPU
 ↓
Request
 ↓
Poll repeatedly
 ↓
Device ready
```

CPU remains busy.

---

# Interrupt-Driven I/O

```text
CPU starts I/O
       ↓
CPU does other work
       ↓
Device completes
       ↓
Interrupt
       ↓
CPU handles ISR
```

CPU is free while waiting.

Overhead can include:

```text
Context save/restore
Interrupt handling
Vector lookup
```

---

# DMA

Direct Memory Access:

```text
I/O Device ↔ DMA Controller ↔ Main Memory
```

CPU initializes the DMA controller and does not transfer every word itself.

---

# DMA Steps

```text
1. CPU sets starting address
2. CPU sets transfer count
3. CPU sets control/mode
4. DMA transfers data
5. DMA interrupts CPU on completion
```

---

# DMA Modes

## Burst Mode

DMA takes control of the bus and transfers the whole block.

```text
DMA owns bus
      ↓
Entire block
      ↓
Bus released
```

---

## Cycle Stealing

DMA takes the bus for one cycle at a time.

```text
DMA → one cycle
CPU → next available cycle
DMA → one cycle
...
```

---

## Transparent DMA

DMA uses the bus only during CPU idle periods.

```text
CPU not using bus
       ↓
DMA uses bus
```

---

# CPU Time Stolen

\[
\boxed{
CPU\ Time\ Stolen\ Fraction
=
\frac{DMA\ Cycles}{Total\ Cycles}
}
\]

Percentage:

\[
\boxed{
\frac{DMA\ Cycles}{Total\ Cycles}\times100
}
\]

---

# Interrupt Priority

Common methods:

- Daisy chaining
- Priority encoder

---

# Vectored Interrupt

Device/controller supplies or identifies the ISR address/vector.

---

# Maskable vs Non-Maskable

### Maskable

Can be disabled/masked.

Example:

```text
INTR
```

### Non-Maskable

Cannot normally be disabled.

Example:

```text
NMI
```

---

# Synchronous vs Asynchronous Bus

## Synchronous

```text
Common clock
```

## Asynchronous

```text
Handshake
```

---

# 10. Common GATE Traps

## ⚠️ 1. MHz vs GHz

```text
1 GHz = 10⁹ Hz
1 MHz = 10⁶ Hz
```

---

## ⚠️ 2. Time Units

```text
1 ms = 10⁻³ s
1 µs = 10⁻⁶ s
1 ns = 10⁻⁹ s
```

---

## ⚠️ 3. Bits vs Bytes

\[
\boxed{1 Byte=8 bits}
\]

Always normalize bandwidth and memory units.

---

## ⚠️ 4. Word Addressable vs Byte Addressable

Read carefully whether:

```text
Each address → 1 byte
```

or:

```text
Each address → 1 word
```

---

## ⚠️ 5. Hit vs Miss Ratio

\[
\boxed{
Miss\ Rate=1-Hit\ Rate
}
\]

Example:

```text
Hit = 95%
Miss = 5%
```

---

## ⚠️ 6. Relative Addressing

\[
\boxed{EA=PC+A}
\]

Usually PC already refers to the next instruction.

---

## ⚠️ 7. Pipeline Clock

Wrong:

```text
Average stage delay
```

Correct:

\[
\boxed{
Max\ stage\ delay+Latch/Register\ delay
}
\]

---

## ⚠️ 8. Cache Metadata

If total implementation storage is asked, consider:

```text
Data
+ Tag
+ Valid
+ Dirty
+ Replacement/state bits
```

But don't add metadata if the question asks only for data capacity.

---

## ⚠️ 9. IEEE Denormal

Normal:

\[
1.M
\]

Denormal:

\[
0.M
\]

No implicit leading `1`.

---

## ⚠️ 10. 2's Complement

For `n` bits:

\[
\boxed{-2^{n-1}\text{ has no positive counterpart}}
\]

---

## ⚠️ 11. CPI

Ideal:

\[
CPI=1
\]

With stalls:

\[
\boxed{
CPI=1+\frac{stall\ cycles}{instructions}
}
\]

---

## ⚠️ 12. Branch Penalty

Don't automatically assume:

```text
Branch penalty = number of pipeline stages
```

Instead identify:

```text
Where branch is resolved
+
How many instructions entered incorrectly
+
Whether prediction is used
```

---

## ⚠️ 13. Serial vs Simultaneous Cache

Always identify whether:

```text
L2 is accessed after L1 miss
```

or:

```text
Both are accessed simultaneously
```

before applying the formula.

---

# ⚡ Ultra-Fast Formula Sheet

| Topic | Formula |
|---|---|
| CPU Time | `IC × CPI / f` |
| Clock Period | `1/f` |
| MIPS | `f/(CPI × 10⁶)` |
| Amdahl | `1/[(1−F)+F/s]` |
| Weighted CPI | `Σ fraction × CPI` |
| 2's Complement | `−2ⁿ⁻¹ … 2ⁿ⁻¹−1` |
| Signed Overflow | `Cin ⊕ Cout` |
| CLA | `Ci+1 = Gi + PiCi` |
| Full Adder Sum | `A⊕B⊕C` |
| Ripple Delay | `n × gate delay` |
| Booth Radix-4 | `≈ n/2 partial products` |
| Pipeline Time | `(k+n−1)t` |
| Pipeline Speedup | `nk/(k+n−1)` |
| Pipeline Clock | `max(stage)+latch` |
| CPI with stalls | `1 + stalls/instruction` |
| Cache Lines | `Cache size / Block size` |
| Cache Sets | `Lines / k` |
| Offset | `log₂(block size)` |
| Index | `log₂(sets)` |
| Tag | `Address − Index − Offset` |
| Serial Access | `HT₁+(1−H)(T₁+T₂)` |
| AMAT | `T₁+m₁(T₂+m₂T₃)` |
| Disk Access | `Seek + Rotation + Transfer` |
| Avg Rotation | `1/(2×RPS)` |
| Disk Capacity | `Surfaces × Tracks × Sectors × Bytes` |
| DMA stolen fraction | `DMA cycles / Total cycles` |
| TLB EMAT | `h(tTLB+tm)+(1−h)(tTLB+2tm)` |

---

# 🧠 GATE COA Problem-Solving Protocol

Before solving **any COA numerical**, do this:

### STEP 1 — Identify the topic

```text
Performance?
Number representation?
ALU?
ISA?
Control?
Pipeline?
Cache?
Virtual memory?
Disk?
I/O?
DMA?
```

### STEP 2 — Write the formula

Don't substitute numbers immediately.

### STEP 3 — Normalize units

```text
GHz → Hz
MHz → Hz
ns → s
bits → bytes
```

### STEP 4 — Draw the structure

Cache:

```text
TAG | INDEX | OFFSET
```

Pipeline:

```text
IF → ID → EX → MEM → WB
```

Disk:

```text
Seek → Rotation → Transfer
```

### STEP 5 — Look for the trap

Ask:

```text
✓ Hit or miss?
✓ Local or global miss rate?
✓ Serial or simultaneous?
✓ Bits or bytes?
✓ Word or byte addressable?
✓ PC already incremented?
✓ Forwarding available?
✓ Branch resolution stage?
✓ Metadata included?
✓ TLB hit/miss?
```

### STEP 6 — Solve

### STEP 7 — Sanity check

Ask:

```text
Does the answer magnitude make sense?
Are the units correct?
Did I use the correct rate?
```

---

# 📝 Mastery Checklist

## Level 1 — Basic

- [ ] CPU execution time
- [ ] CPI
- [ ] MIPS
- [ ] Amdahl's Law
- [ ] 2's complement
- [ ] 1's complement
- [ ] Sign magnitude
- [ ] IEEE-754
- [ ] Addressing modes
- [ ] Pipeline timing
- [ ] Cache address decomposition
- [ ] Disk access time
- [ ] DMA modes

---

## Level 2 — Medium

- [ ] Weighted CPI
- [ ] Signed/unsigned overflow
- [ ] Carry Look-Ahead
- [ ] Booth multiplication
- [ ] Division
- [ ] Expanding opcode
- [ ] Microprogramming
- [ ] Pipeline hazards
- [ ] Forwarding
- [ ] Branch prediction
- [ ] Cache mapping
- [ ] AMAT
- [ ] Write policies
- [ ] TLB
- [ ] Disk numericals
- [ ] DMA calculations

---

## Level 3 — GATE PYQs

- [ ] Performance PYQs
- [ ] Number representation PYQs
- [ ] IEEE-754 PYQs
- [ ] ALU PYQs
- [ ] Booth/Division PYQs
- [ ] Addressing mode PYQs
- [ ] Control unit PYQs
- [ ] Pipeline PYQs
- [ ] Cache PYQs
- [ ] Virtual memory/TLB PYQs
- [ ] Disk PYQs
- [ ] I/O/DMA PYQs

---

# ❌ Mistake Log

| Date | Topic | Question | My Mistake | Correct Concept |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

---

# 📊 COA Mastery Tracker

| Topic | Concept | Basic | Medium | PYQ | Revision |
|---|---:|---:|---:|---:|---:|
| Performance | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Number Representation | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| ALU & Arithmetic | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| ISA | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Control Unit | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Pipelining | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Cache | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Virtual Memory | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| Disk | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |
| I/O & DMA | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |

---

# 🔥 Final COA Revision Strategy

## First Pass

```text
Concept
  ↓
Understand WHY
  ↓
Formula
  ↓
1–2 Basic Questions
```

## Second Pass

```text
Formula
  ↓
Medium Numericals
  ↓
Mixed Questions
```

## Third Pass

```text
GATE PYQs
  ↓
Identify Pattern
  ↓
Record Trap
  ↓
Mistake Log
```

## Final Revision

Only revise:

```text
FORMULA
   +
TRAP
   +
PYQ PATTERN
   +
YOUR MISTAKES
```

---

# 🏆 Definition of COA Mastery

You have mastered a topic when you can:

```text
Explain the concept without notes
             +
Recall the important formulas
             +
Solve basic questions quickly
             +
Solve medium numericals
             +
Solve GATE PYQs
             +
Identify common traps
             +
Explain WHY your answer is correct
```

> **Don't move to the next topic merely because the lecture is finished.**
>
> **Move forward when questions start becoming easy.**

---

# 🚀 GATE CSE 2027

```text
CONCEPT
   ↓
FORMULA
   ↓
BASIC
   ↓
MEDIUM
   ↓
GATE PYQs
   ↓
MISTAKE LOG
   ↓
REVISION
   ↓
MASTERy
```

> ### 🧠 COA Rule
> **Don't memorize the architecture. Understand it.**
>
> Once the architecture is clear, the formulas, numerical patterns, and GATE traps become much easier to remember.

---

## 📌 Quick Final Checklist

Before the exam, make sure you can recall these without looking:

```text
☐ CPU Time
☐ MIPS
☐ Amdahl
☐ CPI
☐ 2's complement range
☐ Overflow
☐ IEEE-754
☐ CLA
☐ Booth
☐ Addressing modes
☐ RISC vs CISC
☐ Horizontal vs Vertical microprogramming
☐ Pipeline timing
☐ Pipeline hazards
☐ Forwarding
☐ Cache mapping
☐ Tag/Index/Offset
☐ AMAT
☐ Write policies
☐ TLB
☐ Disk timing
☐ DMA modes
☐ I/O
☐ GATE traps
```

**This is the COA sheet to revise repeatedly—not a replacement for solving PYQs.**
