# Selene

RV64GCBKHVZfh superscalar out-of-order 14-stage pipelined design, intended to push the state of the art of the field, rather than playing catch-up with commercial designs. The very high standard set by modern RISC-V IP all but compels us to set our aims higher. IP blocks exist for the OSS community to pull from (Hardfloat, Rocketchip), and the RV64GCBKHV playbook has been validated (XiangShan Kunminghu) at Neoverse V2-tier performance, proving open-source Chisel designs can compete with commercial HPC designs.

It's insufficient to play catch-up, though. AMD and Intel race to win the CPU war not by aiming to beat the current-generation but to make their competitor's next generation obsolete. For open-source to be a viable alternative to COTC components, we must not only succeed at this at the RTL stage, but succeed at the tapeout and economics stage. Of course, the latter is harder, but the former is lofty enough. 

This is the objective of Selene.

## Key Milestones

We fork XiangShan's Kunminghu v3 to use it as a baseline. However, the main improvements we plan to do to it are:
- Drastically improving the readability of code by making it self-documenting, breaking massive monofiles/monoclasses/mono-objects into smaller ones, cleaning up the repo hierarchy and overhauling documentation
- Massively overhauling the frontend qualitatively (much more aggressiveness in speculation and branching), and the microarchitecture quantitatively (number go up, basically). This is a must-have to drastically push peak performance.
- Improve parameterization ability, especially to do with cores (optimal core range should be 8 to 128. No point in making a HPC core if it's unable to get to Threadripper levels even in theory, eh?)
- Add Zfh support by modifying the floating-point units' parameterization (invaluable for ML workloads)

The key aim is this: Kunminghu v3 is very performant and built very quickly, but is very hard for an outsider to read. RocketChip is extremely readable and modular, but basically has no modern cores. Make massive frontend improvements to Kunminghu, leaving the backend mostly as-is, and rewrite everything to have the readability/modularity of RocketChip. It should not only be very easy to read, thus, but also easy to strip parts for use in other cores or implementations, or to modify this one for future improvements.

Basically, just adhere to normal hardware best practices.

## Microarchitecture:

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
- 4 vector lanes (configurable width) with 2 VFPUs, 2VALUs, 1VMISC
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

