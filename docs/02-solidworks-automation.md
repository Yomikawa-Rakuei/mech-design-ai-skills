# 02 · solidworks-automation — letting the AI actually drive SolidWorks

| | |
|---|---|
| Repository | https://github.com/wzyn20051216/solidworks-automation-skill |
| Author / licence | SolidWorks Automation Skill Contributors · MIT · v2.1.1 |
| Form | AI skill + MCP stdio server + CAD Studio |
| In one line | Drives a local SolidWorks instance over COM to change dimensions and features, produce drawings and exports, then verify the delivery |

## What problem it solves

An AI can describe how to change a model but often cannot touch the software. This skill reaches into SolidWorks on Windows: edit dimensions, rebuild features, change assembly mates, generate drawings, export STEP/STL/PDF — and prove that the delivered file really was produced in this run.

## Input → output

- **Input**: a task description plus parameters (part and assembly templates via `config/runtime.example.json`)
- **Output**: SolidWorks documents + exports + **a verification record** (which capabilities are proven, which require human review)

## Design choices worth copying

1. **Capability tiers with mandatory review.** `capabilities.yaml` grades every capability; anything not yet verified **must go through human review** instead of silently counting as success.
2. **The principle stated at the top of the README**: *"Tool success is not treated as task success."*
3. **Delivery-freshness gate.** CAD Studio checks that each deliverable was created or modified in the current run; stale files are refused and go to pending review.
4. **Runtime-config whitelist.** Only whitelisted fields are accepted; an unknown field yields `BLOCKED`. The point is that a config file cannot quietly change versions, tolerances or skip a review.
5. **Golden workflows.** `golden-workflows.yaml` provides end-to-end cases (e.g. parallel-gripper retention travel) to pressure-test the chain on realistic work.

## Environment

- Windows + a licensed, running SolidWorks (COM)
- Python (`pyproject.toml`) + Node (installer `install.js`)
- The MCP server must be registered in the agent client

## When to use it / when not to

- ✅ Batch dimension/feature edits, drawing generation from templates, export packaging, normalising existing parts
- ✅ Workflows that need a traceable, reviewable step for every change
- ❌ Free-form concept modelling (faster by hand in the UI)
- ❌ Machines without a SolidWorks licence

## My assessment

**Strength**: the most production-minded of the five. It is not about whether you can call the API but about why anyone should believe the result — capability tiers, a delivery gate and a config whitelist turn a demo into something that can enter a real process. It is mounted in my local DSH environment, so I can call it directly.

**Concern**: very sensitive to SolidWorks version, templates and machine state. Complex features (loft, sweep, surfaces) still need a human; the payoff concentrates in repetitive edits and drawing output.

## Where it sits in the chain

Takes geometry from [01 text-to-cad](01-text-to-cad.md), makes it engineering-ready and produces drawings; the finished model goes to [03 cae-agent-hub](03-cae-agent-hub.md) for checking.
