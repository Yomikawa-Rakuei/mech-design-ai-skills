# 01 · text-to-cad — grow a 3D model straight from a requirement

| | |
|---|---|
| Repository | https://github.com/earthtojake/text-to-cad |
| Author / licence | Thompson Labs LLC · MIT |
| Form | PyPI package `cad` + skills + MCP server + CAD Viewer |
| In one line | Turns "I need a part shaped like this" into an executable Python modelling script and builds a real CAD model from it |

## What problem it solves

Going from a requirement to a 3D model always involved a hand-off: a human opens CAD, sketches, extrudes. Change one dimension and you walk the whole path again. text-to-cad makes the model **code**: a requirement goes in, a [build123d](https://build123d.readthedocs.io/) Python script comes out, and the dimensions are variables.

## Input → output

- **Input**: a natural-language requirement plus the critical dimensions (or Python modelling code directly)
- **Output**: a build123d script → a built solid (viewable in CAD Viewer) → exportable geometry

## How it works (from the README and repository layout)

1. **A model is a script.** `models/` is a corpus of self-contained, flat projects: run a project's own script and you reproduce the model. That answers the classic complaint that AI-generated geometry cannot be reproduced.
2. **Server-side build plus preview.** The script is handed to its own MCP server to build and to the Viewer to inspect, so the agent receives a *built model* rather than *code that looks like it would run*.
3. **One-line install.** `install.sh` on macOS/Linux, `install.ps1` on Windows; it wires the skills/plugin into every agent app it finds, registers the `cad` command and upgrades the MCP server.

## Environment

- Python (the installer brings `uv`)
- **No commercial CAD licence required** — this is the big difference from the other four: a fully open stack that runs once installed.

## When to use it / when not to

- ✅ Concept models from scratch, parametric standard parts, 2D profiles, part families you want generated in batch
- ✅ Iteration where "change one parameter, rebuild" is the normal move
- ❌ Complex free-form surfaces, large assemblies, production drawings that need a strict feature tree (build123d is script modelling, without SolidWorks-style feature history)
- ❌ Direct use as a manufacturing drawing — it still goes into SolidWorks for engineering treatment

## My assessment

**Strength**: it makes reproducibility the default. The model is a script, the script lives in git, and a diff shows exactly what changed — the part of AI-generated design that is easiest to skip and most damaging to credibility.

**Concern**: script modelling is not the mental model of feature-based modelling; a mechanical engineer has to re-learn one habit. Quality depends heavily on how precisely the prompt describes geometric constraints.

## Where it sits in the chain

The entry point: it turns an idea into geometry worth editing. [02 solidworks-automation](02-solidworks-automation.md) takes it from there.
