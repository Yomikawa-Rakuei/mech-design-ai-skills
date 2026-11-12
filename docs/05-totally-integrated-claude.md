# 05 · totally-integrated-claude — reading why a motor is allowed to start

| | |
|---|---|
| Repository | https://github.com/Czarnak/totally-integrated-claude |
| Author / licence | Łukasz Czarnacki · MIT |
| Form | A bundle of TIA Openness skills + API baselines + CI evidence |
| In one line | Read and write Siemens TIA Portal projects through Openness, including the start conditions and interlocks behind a motor |

## What problem it solves

Final drawings do not make a machine move. Motor start conditions, interlocks, E-stops and drive enables all live in the PLC program, and conventionally only an electrical engineer can walk them by hand in TIA Portal. This project makes TIA Portal something an agent can read and write, so mismatches between drawing and logic surface before delivery instead of on site.

## What is inside

- **`tia-openness-roadmap`** — a routing layer: it honours the implementation path you explicitly choose, then dispatches to the Python or the C# route.
- **Python TIA Scripting V1.4.3** — Siemens' official scripting wrapper, covering PLC data, general HMI access, devices and libraries.
- **C# Openness (eleven domain skills)** — one per domain assembly of the V21 Openness API.
- **`tia-mac-module-builder`** — generates modules along the audited MAC V21.0.5 lifecycle.
- **Add-In development** — authoring, compiling and debugging TIA Portal Add-Ins in VS Code.
- **`tia-doctor`** — environment diagnostics: verifies the V21 executable, the modular Openness core and licensing.
- **`plc-code-analysis`** — multi-perspective safety and quality analysis of **already exported** SCL/ST/LAD/FBD code (independent of the automation path; it never edits).

## How it works

1. **Version pinned.** The whole project targets the **TIA Portal V21** Openness API. `api-baselines/` stores assembly versions, DLL/XML SHA-256 hashes and documented member-surface hashes extracted from a real installation, and CI validates them — it does not fake "it ran", it proves the interface matches.
2. **Diagnose before acting.** `tia-doctor` is the gate: a wrong environment stops the workflow.
3. **Analysis separated from automation.** `plc-code-analysis` deliberately does not participate in engineering automation, so reviewing and editing never contaminate each other.

## Environment

- Windows + TIA Portal V21 (with the Openness component and licensing)
- .NET for the C# route, Python for the scripting route
- Project files and the corresponding access rights

## When to use it / when not to

- ✅ Reading legacy projects: turning motor start conditions, interlock chains and alarm logic into readable documentation
- ✅ Batch block edits, standardised module generation, Add-In development
- ✅ Pre-delivery mechanical–electrical consistency checks (walking the PLC against the drawing)
- ❌ No TIA licence or no project file — a hard gate
- ❌ Non-Siemens platforms (Mitsubishi, Omron and the like are out of scope)

## My assessment

**Strength**: the only one of the five that pushes "evidence" down to the binary level. It is not satisfied with "I connected through the API"; it freezes the API versions and hashes into the repository and re-validates them in CI on every release — the most pragmatic attitude for industrial software, where a version bump breaks everything.

**Concern**: the highest barrier (licence + version + project files) and hard-pinned to V21. The real value is in reading and batch editing; writing complex safety logic from scratch still needs an expert.

## Where it sits in the chain

The last step: once models and drawings are final, it checks the electrical logic and closes the mechanical–electrical loop. After this, what ships is something that both assembles and moves.
