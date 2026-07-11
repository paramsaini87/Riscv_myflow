# Riscv_myflow

**RV32IMAC 8-stage pipelined CPU, synthesized and formally proven correct with my own RTL-to-netlist flow — 94,889 gates on SKY130, mathematically proven equivalent to the RTL across all 12,946 registers.**

This repository contains the complete synthesis flow I actually use — the RTL, the
one-file flow script, the resulting gate-level netlist, and the verification
results. Everything here was produced by my own EDA engine (a single self-contained
program, zero third-party tools).

---

## Headline results

| | |
|---|---:|
| Design | RV32IMAC 8-stage CPU (82 instructions) |
| Target | SKY130 130nm · `sky130_fd_sc_hd` |
| Elaborated gates | 201,072 |
| **Final mapped gates** | **94,889**  (−53%) |
| Registers | 12,946 flip-flops |
| **Formal equivalence** | **PROVEN EQUIVALENT** (complete, all registers/all states) |
| Combinational equivalence (LEC) | 174 / 174 PASS |
| DFT stuck-at coverage | 72.0% (565 patterns) |

The netlist is **not** validated by a handful of test vectors — it is proven, by a
complete equivalence check, to compute identical outputs to the RTL for **every**
possible input sequence.

---

## Flow vs. my earlier flow (same core)

The same 8-stage RV32IMAC core hardened by an earlier version of my flow produced
**242,823 mapped cells with no formal proof**. The current flow produces
**94,889 gates (−61%) *and* a complete formal correctness proof**. See
[`results/flow_vs_prior.md`](results/flow_vs_prior.md).

---

## Repository layout

```
Riscv_myflow/
├── rtl/
│   └── rv32i_cpu.v            RV32IMAC 8-stage CPU RTL (3,230 lines)
├── flow/
│   └── synthesis_flow.sf      the exact flow script used
├── netlist/
│   └── rv32i_cpu_synth.v      94,889-gate mapped netlist (the result)
└── results/
    ├── synthesis_report.txt   gate counts, cell composition, DFT
    ├── formal_verification.txt LEC + complete sequential equivalence proof
    ├── flow_vs_prior.md        measured improvement over the earlier flow
    └── synthesis_state.json    (generated on run)
```

## What the flow does

`flow/synthesis_flow.sf` runs, in order:

1. **Front-end sign-off** — lint, functional simulation, clock-/reset-domain
   crossing checks, bounded model checking.
2. **Logic synthesis** — RTL is elaborated and optimized into a gate-level
   netlist, then mapped to the SKY130 standard-cell library. Optimization is
   correctness-preserving and aggressively folds logic into complex library
   cells.
3. **DFT** — scan/test-pattern generation with fault-coverage reporting.
4. **Correctness sign-off** — combinational equivalence points, then a
   **complete sequential equivalence proof** covering every register, plus
   clock-gating and property checks.
5. **Persist** — write the netlist, save a checkpoint, export the full state.

The design (RV32IMAC 8-stage: gshare + BTB + RAS branch prediction, iterative
mul/div, full M-mode CSR/PMP, RV32C expansion, RV32A atomics, debug) is a
realistic, verification-heavy core — the results above are on the complete design,
not a toy.

---

*RTL and results produced by my own flow. SKY130 is
an open PDK.*
