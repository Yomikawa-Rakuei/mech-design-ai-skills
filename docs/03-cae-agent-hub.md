# 03 · CAE-Agent-Hub — an FEA pipeline that leaves evidence

| | |
|---|---|
| Repository | https://github.com/Cai-aa/CAE-Agent-Hub |
| Author / licence | Thompson Labs · MIT |
| Form | MCP tool set + reusable skills + result viewer |
| In one line | Connect AI to real CAE and keep the evidence behind every result |

## What problem it solves

The hard part of simulation is rarely getting a number out — it is **proving the number is right**: who meshed it, were the boundary conditions correct, is this the result from the last run? CAE-Agent-Hub chains modelling, meshing, solving, post-processing and checking into one pipeline with a viewer for the output.

## Input → output

- **Input**: geometry + load case (loads, constraints, material) + the quantities you want
- **Output**: results + check plots + provenance; the README pins one verifiable case per software module

## Coverage

Under `MCP/`, each solver is a separate leaf. What I found:

**Abaqus** (including 2020), **Adams**, **ANSA**, **Ansys** (CFX and others), **CalculiX**, **COMSOL**, **CST**, **HyperWorks**, **LAMMPS**, **OVITO**, **Peridigm**, plus a `FEP-Agent-Hub` sub-project and subagent resources.

It covers both commercial solvers (Abaqus/Ansys/COMSOL/HyperWorks) and open ones (CalculiX/LAMMPS) — the latter matters a lot if you have no licence.

## How it works

1. **Checkability first**: a module only gets a representative case in the gallery if the result is verifiable; anything not reproducible stays out.
2. **Solver-per-MCP-leaf**: the agent mounts only the solver it needs rather than carrying all of them.
3. **Result viewer**: cloud plots and curves have a single entry point, so "the AI says it passes" is never the only artefact.
4. **Subagents and Agentic PD**: long multi-step simulations are delegated, keeping a single conversation from filling up.

## Environment

- One or more CAE solvers (commercial ones licensed; CalculiX/LAMMPS are free to install)
- Python environment + the relevant MCP server registered
- Compute: fine meshes are a local workload

## When to use it / when not to

- ✅ Routine static, modal and contact checks in batch, with archived results
- ✅ Comparing design options — much faster than clicking through the UI case by case
- ❌ Strongly nonlinear, explicit dynamics, heavy multiphysics: still needs an expert
- ❌ Signed simulation reports: AI output is a first pass, the responsible engineer signs

## My assessment

**Strength**: the widest coverage of the five, and "evidence chain" is a level above "it runs". Including open-source solvers makes it genuinely useful for students without a budget.

**Concern**: the MCP leaves differ in maturity and the environment cost is high (each solver brings its own licence, paths and version traps). Result quality still depends on getting the boundary conditions right, which AI helps with least.

## Where it sits in the chain

Takes the model from [02 solidworks-automation](02-solidworks-automation.md). A failing result goes back to step 02 — that is the first loop in the chain.
