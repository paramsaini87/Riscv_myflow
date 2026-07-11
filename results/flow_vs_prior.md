# Flow Evolution — Same Core, Measured Improvement

The **same RV32IMAC 8-stage CPU** was hardened by an earlier version of this flow
and by the current version. The current flow produces a **dramatically smaller
netlist** *and* adds a **complete formal correctness proof** the earlier version
did not have.

| Metric | Earlier flow | Current flow | Delta |
|---|---:|---:|---:|
| Mapped gates | 242,823 | **94,889** | **−60.9%** |
| Registers | — | 12,946 | — |
| Combinational equivalence (LEC) | — | 174 / 174 PASS | added |
| Complete sequential equivalence proof | none | **PROVEN EQUIVALENT** | added |
| DFT stuck-at coverage | — | 72.0% (565 patterns) | added |
| Target PDK | SKY130 sky130_fd_sc_hd | SKY130 sky130_fd_sc_hd | same |

### Why the gate count dropped so much
The current flow performs deeper correctness-preserving logic optimization and
a smarter mapping of logic to the standard-cell library — folding inversions
directly into complex library cells instead of emitting standalone inverters,
and collapsing redundant logic that the earlier flow left in place. Every one of
those transforms is verified equivalence-preserving, so the smaller netlist is
provably the *same circuit*, just cheaper to build.

### Why the proof matters
A smaller netlist is only valuable if it is still correct. The earlier flow
reported no formal equivalence result — correctness rested on simulation alone.
The current flow proves, over **all 12,946 registers and all reachable states**,
that the 94,889-gate netlist behaves identically to the RTL. Fewer gates **and**
a mathematical guarantee of correctness.

*Numbers are the measured outputs of each flow on the RV32IMAC 8-stage core.*
