# Selene

RV64GCHVZb*ZfhZic* superscalar out-of-order 14-stage pipelined design.

4 instruction decode, 8 instruction issue width. 256 instruction window (reorder buffer).

**Cache Hierarchy**
- L1D: 64 kB 4-way 3-cycle latency
- L1I: 64 kB 4-way 3-cycle latency
- L2: 2 MB private 15-cycle latency
- L3: 64 MB shared 30-cycle latency

**Registers**
- Integer: 256 integer registers of 64 bits each, of which 32 are architectural (x0-x31)
- Vector: 256 vector registers of 1024 bits each, of which 32 are architectural (v0-v31)
- Rename Maps: 64 bytes

**Branch Prediction**

- BTB: 8K entries
- RAS: 32 entries
- Global History: 20-bit

**Memory**
- Load queue: 64
- Store queue: 32
- 4 on-die controllers

**Execution Units**
- 4 integer ALUs (2 simple, 2 complex)
- 2 LSUs, 6 vector LSUs
- 4 vector lanes (configurable width)
- 2 FPU

**Coherenty, Interconnects, Directories**
- MOESI
- Directory-based
- Non-inclusive L3
- 2D mesh
- 256-bit links
- 16k entry directory cache


## Key OSS

Key open source IP this pulls from
- [Hardfloat](https://github.com/ucb-bar/berkeley-hardfloat) for IEEE-754 compliance
- [RocketChip](https://github.com/chipsalliance/rocket-chip) for various RTL IP
- [XiangShan](https://github.com/OpenXiangShan/XiangShan/) for advanced frontend/backend IP, as well as HPC scaffolding

