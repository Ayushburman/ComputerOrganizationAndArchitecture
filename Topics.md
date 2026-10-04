# COA Cheat Sheet (GATE 2027)

## 1. Performance

- CPU time = IC × CPI × T = IC × CPI / f
- MIPS = f / (CPI × 10⁶)
- Amdahl: S = 1 / [(1−F) + F/s]
- Weighted CPI = Σ(fraction × CPI)

## 2. Number Representation

- n-bit 2's complement: −2ⁿ⁻¹ to 2ⁿ⁻¹−1
- 1's complement and sign-magnitude: −(2ⁿ⁻¹−1) to +(2ⁿ⁻¹−1), with two zeros
- Signed overflow: carry into MSB ≠ carry out of MSB
- Unsigned overflow: carry out of MSB = 1
- Sign extension preserves the value in 2's complement
- **IEEE 754**
  - Single: 1 | 8 | 23, bias 127, value = (−1)ˢ × 1.M × 2^(E−127)
  - Double: 1 | 11 | 52, bias 1023
  - E=0, M=0: zero
  - E=0, M≠0: denormal, 0.M × 2^(−126)
  - E=all 1s, M=0: ∞
  - E=all 1s, M≠0: NaN
  - Single range: about ±3.4×10³⁸, max E = 254

## 3. ALU and Arithmetic

- Ripple carry delay = n × gate delay
- Carry-lookahead: Gᵢ = AᵢBᵢ, Pᵢ = Aᵢ⊕Bᵢ, Cᵢ₊₁ = Gᵢ + PᵢCᵢ
- Full adder: Sum = A⊕B⊕C, Carry = AB + BC + CA
- Booth: handles signed multiplication, and runs of 1s are skipped
- Radix-4 (modified) Booth gives n/2 partial products
- Restoring division: restores the remainder after a negative result
- Non-restoring division: no restore step, so it is faster

## 4. Instruction Set

- **Address formats:** 0-address (stack), 1-address (accumulator), 2-address, 3-address (RISC)
- **Expanding opcode:** count the unused codes at each level, and multiply by 2^(extra bits) for the next level
- **Addressing modes** (effective address):
  - Immediate: operand is in the instruction
  - Direct: EA = A
  - Indirect: EA = M[A]
  - Register: operand is in the register
  - Register indirect: EA = R
  - Indexed: EA = A + Index
  - Base register: EA = A + Base
  - Relative: EA = PC + A (PC is already incremented)
  - Auto-inc/dec: used for array walks
- **RISC:** fixed length, load/store only, many registers, hardwired control, pipeline friendly
- **CISC:** variable length, memory operands, microprogrammed control

## 5. Control Unit

- **Hardwired:** fast, rigid, used in RISC
- **Microprogrammed:** flexible, slower, used in CISC
- **Horizontal:** wide microinstruction, little encoding, more parallelism, faster, large control store
- **Vertical:** narrow, encoded fields, needs a decoder, slower, small control store
- Control word bits = number of control signals (horizontal) or Σ log₂(group size) (vertical)
- Control store size = number of microinstructions × width
- Nanoprogramming: a two-level control store

## 6. Pipelining

- k stages, n instructions: time = (k + n − 1) × t
- Speedup = n·k / (k + n − 1), which tends to k as n grows
- Non-pipelined time uses the sum of all stage delays; the pipeline clock uses the **max stage delay + latch delay**
- Ideal CPI = 1, and CPI = 1 + stalls per instruction
- **Hazards:**
  - Structural: resource conflict
  - Data: RAW (true), WAR and WAW (only in out-of-order execution)
  - Control: branches
- Fix for data hazards: forwarding. A load-use stall of 1 cycle remains even with forwarding.
- Branch penalty = number of stages before the branch is resolved
- Branch handling: stall, predict not taken, delayed branch, dynamic prediction

## 7. Memory Hierarchy
- Locality: temporal and spatial
- **Average access time (hierarchical / sequential):** T = H·T₁ + (1−H)(T₁ + T₂)
- **Simultaneous:** T = H·T₁ + (1−H)·T₂
- Memory stall cycles = accesses per instruction × miss rate × miss penalty
- **Cache address split:**
  - Offset = log₂(block size)
  - Index = log₂(number of sets)
  - Tag = address bits − index − offset
- Number of lines = cache size / block size
- Number of sets = lines / k for k-way, and 1 for fully associative
- Direct mapped: k = 1, so each block maps to one line
- Fully associative: no index bits, tag comparators = number of lines
- Tag memory = lines × (tag + valid + dirty + state bits)
- **Misses:** compulsory, capacity, conflict
- **Write hit:** write-through (with write buffer) or write-back (dirty bit)
- **Write miss:** write-allocate or no-write-allocate
- **Replacement:** LRU, FIFO, random
- Multilevel cache: AMAT = T₁ + m₁(T₂ + m₂·T₃)
- **Main memory interleaving:**
  - Low-order: consecutive addresses go to different modules, and sequential access is fast
  - High-order: used for expansion
- Virtual memory EMAT with a TLB: h·(t_tlb + t_m) + (1−h)·(t_tlb + 2·t_m) for single-level paging
- Page table entry bits: frame number + valid/dirty/protection bits

## 8. Secondary Storage (Disk)
- Capacity = surfaces × tracks × sectors × bytes per sector
- Access time = seek + rotational latency + transfer
- Average rotational latency = ½ rotation time = 0.5 / RPS
- Transfer time = data size / (track size × RPS)
- Cylinder-wise: the same track number on all surfaces

## 9. I/O
- **Programmed I/O:** CPU polls, so the CPU is busy
- **Interrupt-driven:** CPU is free until an interrupt arrives. Context save and vector lookup cost overhead.
- **DMA:**
  - Burst: DMA holds the bus until the block is done
  - Cycle stealing: DMA takes one cycle at a time
  - Transparent: DMA uses idle bus cycles only
- DMA steps: CPU sets address, count, and control, then the DMA controller transfers, then it raises an interrupt
- CPU time stolen % = DMA cycles / total cycles
- Interrupt priority: daisy chain or a priority encoder
- Vectored interrupt: the device supplies the ISR address
- Maskable (INTR) vs non-maskable (NMI)
- Synchronous bus: common clock. Asynchronous bus: handshake.

## 10. Common GATE Traps
- Check whether the clock is in MHz or GHz and whether time is in ns, µs, or ms
- Memory size in bits vs bytes, and word-addressable vs byte-addressable
- Relative addressing: PC already points to the next instruction
- Pipeline clock = max stage delay + register delay
- Count tag bits only if the question asks for them, and add valid and dirty bits for total cache size
- Hit ratio vs miss ratio, and sequential vs simultaneous access
- IEEE denormal: no implicit leading 1
- 2's complement −(2ⁿ⁻¹) has no positive counterpart
- Stalls: count cycles per instruction, then CPI = 1 + stalls

If you want, I can add a PYQ-pattern section or numericals for any one topic.
