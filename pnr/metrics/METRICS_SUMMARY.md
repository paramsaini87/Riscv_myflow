# RUN 193 — Full Signoff Metrics (RV32IMAC, SKY130, 56 MHz)

Design: 8-stage RV32IMAC RISC-V core (full privileged CSRs, M/A/C extensions,
branch prediction, PMP). Process: SkyWater SKY130 high-density (sky130_fd_sc_hd),
typical-typical 25 °C 1.80 V. Flow: single-binary RTL→GDSII EDA engine.

## Synthesis
| Metric | Value |
|--------|-------|
| Gates (post-synth) | 79,229 (from 201,072 RTL — 61% reduction) |
| Flip-flops | 12,946 live DFFs |
| Mapped critical arrival | ~11.55 ns (delay-true NPN mapper) |
| LEC (combinational equiv.) | **PASS — 174/174 key points** |
| SEC (sequential equiv.) | **EQUIVALENT** |
| DFT / stuck-at coverage | **83.0%** (131,707 / 158,686 faults, 3,134 patterns) |

## Timing (signoff @ 56 MHz, period 17.857 ns)
| Metric | Value |
|--------|-------|
| STA WNS (TT corner) | **0.000 ns → +0.089 ns — MEETS** |
| STA TNS | 0.000 ns |
| Hold (FF corner) | **0.000 ns — clean** |

## Physical signoff
| Check | Result |
|-------|--------|
| **DRC total** | **2,003 violations** |
| — breakdown | m1.2=1000, die.3=510, via3.2=343, m3.5=66, via.2=50, rest small, **die.1=0** |
| **LVS** | **PASS — 78,656 / 78,656 cells matched (clean)** |
| **Latchup** | 99% covered, 408 violations |
| **Antenna** | 139 violations |
| **ESD** | PASS — 0 violations |

## Layout
| Metric | Value |
|--------|-------|
| Core die | 1,185 × 1,185 µm |
| Utilization | 58.5% |
| Routed wires | 688,889 |
| Vias | 247,173 |
| Metal layers | 5 |
| Well-tap cells (pre-placed) | 39,403 |
| GDSII | 46.5 MB, hierarchical SREF (`pnr/gds/rv32i_cpu_final.gds`) |

_All numbers are from the engine's own signoff run (RUN 193, 2026-07-23)._
