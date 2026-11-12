# 04 · autocad-dwg-redraw — redraw an old drawing and prove it

| | |
|---|---|
| Repository | https://github.com/pengxiaoan/autocad-dwg-redraw-skill |
| Author / licence | pengxiaoan · **no LICENSE file in the repository** (confirm with the author before use) |
| Form | Two Codex skills: `autocad-dwg-redraw`, `autocad-image-redraw` |
| In one line | Rebuild DWG from a source DWG, a PDF or an image, then validate the result at entity level |

## What problem it solves

One of the most common jobs in a workshop or a course: all you have is a scan or a PDF, and you need an editable DWG. A human traces it. The mistake an AI makes is **tracing the appearance** — the line weights look right, nothing is measurable. The core value of this skill is that it closes that door.

## Three input modes

1. **Source DWG mode** — when the original DWG exists, profile it, generate a drawing-specific standardised prompt, then reproduce it exactly.
2. **PDF-derived DWG mode** — convert the PDF into an auditable intermediate DWG first, then follow the source-DWG flow; it explicitly forbids claiming exact fidelity to the original DWG, because the PDF already lost information.
3. **Image + metrics mode** — normalise the reference image and the authoritative dimensions into a parameter specification; here the supplied dimensions, units, counts and layout constraints are authoritative, and the image only fixes shape.

## Design choices

- **Core rule**: with a source DWG available, do not infer a complex drawing from screenshots or visual style — extract the entities first.
- **Delivery priority**: prefer exact entity copy through AutoCAD COM; AutoLISP/Python reconstruction is the fallback.
- **Auditable intermediate**: the PDF route deliberately keeps an intermediate DWG so every step can be traced back.
- **Second skill in the same repo**, `autocad-image-redraw`, does pure image → DWG/DXF with input preflight and evidence awareness (image quality determines what is possible).

## Environment

- Windows + a local AutoCAD (COM)
- Image mode requires you to supply authoritative dimensions — that is the mandatory human step

## When to use it / when not to

- ✅ Refreshing old drawings, converting PDFs and scans into editable DWG, standardising an archive
- ✅ Batch cleanup where changes must be verifiable
- ❌ A photo with no dimensional reference (schematic only, never a production drawing)
- ❌ Resolve the licence question first — the repository has no LICENSE

## My assessment

**Strength**: it turns "evidence first" into a hard rule, especially *"the PDF route must not claim fidelity to the source DWG"* — stating a limit openly is more convincing than claiming capability. It is also the only one of the five that says plainly that a human must supply the authoritative dimensions.

**Concern**: the missing licence is a real blocker for commercial delivery, and AutoCAD COM brings the same version-compatibility traps.

## Where it sits in the chain

Takes the geometry finalised in [02](02-solidworks-automation.md)/[03](03-cae-agent-hub.md) and produces archivable, deliverable 2D drawings; once the drawings are final, [05](05-totally-integrated-claude.md) checks mechanical–electrical consistency.
