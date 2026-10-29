# SheetCut Pro acceptance report

Verified deliverable: the standalone `index.html` in this repository, opened directly with the `file://` protocol. No server or application runtime dependencies were used.

## Executed tests

| Boundary | Result | Evidence |
|---|---|---|
| JavaScript syntax | Pass | Both embedded script blocks compiled by Node's JavaScript parser |
| Geometry engine | 26/26 pass | `sheetcut-qa/engine-results.json` |
| Chrome workflows, responsive layout and Edge startup | 29/29 pass | `sheetcut-qa/browser-results.json` |
| Simple/Pro workflow refinement | 14/14 pass | `sheetcut-qa/simple-results.json` |
| Offline operation | Pass | No HTTP(S) requests during the Chrome acceptance run |
| Browser console | Pass | No uncaught page errors or error-level console messages |
| Final print corrections | 4/4 pass | `sheetcut-qa/print-results.json`; white paper margins, light print scheme, accurate alternative-layout label, native PDF |
| Firefox runtime | Unverified | Playwright Firefox executable absent; not installed during this task |
| Physical machine cutting | Unverified / outside deliverable | Outputs are finished design geometry, not verified CAM toolpaths |

Engine coverage includes: a single exact fit; four 500 mm squares on a 1000 mm square; the same with nonzero kerf; exact fit with nonzero gap but no fictitious boundary gap; rotation required/forbidden; repeated quantities and multiple sheets; multiple stock sizes and finite inventory; material and thickness isolation; oversized parts; tight margins; floating-point dimensions; utilization and perimeter arithmetic; common-line eligibility, partial edge union and time estimates; grain alignment; guillotine variants; intentional collision/bounds/orientation/inventory/quantity corruption; invalid input and duplicate IDs; reserved JavaScript words as stock IDs; missing cost handling; disjoint offcut geometry; 12 deterministic randomized jobs across 8 strategies each; and a 1,000-rectangle benchmark.

Browser coverage includes: real Blob worker execution; five viewports (1920×1080, 1440×900, 1280×800, 768×1024, 390×844); overview/detail and sheet navigation; zoom/fit/grid/offcuts; selection and inspector; part and stock CRUD; invalid inline dimensions; undo/redo; unit conversion preserving physical dimensions; light theme; quoted/multiline CSV; autosave/reload and named Save/New/Open; alternate candidate selection; actual JSON/CSV/SVG downloads and round-trip parsing; Print button invocation and Chromium PDF creation; invalid imports; valid and invalid manual edits; 1,000 parts with an active UI heartbeat; cancellation; worker-disabled fallback; keyboard focus continuity; accessible dialog and toggle states; mobile recovery actions; and Edge direct-file startup/optimization.

## Performance measurements

Deterministic benchmark: 40 part types × 25 copies = 1,000 rectangles; 2500×1250 mm unlimited stock; 0.2 mm kerf; 1 mm spacing; 10 mm margins; Balanced effort.

| Environment | Attempts | Engine duration | Result |
|---|---:|---:|---|
| Chrome worker | 24 | 292.2 ms | 1,000 placed; 19 sheets; 89.3617% gross utilization |
| Chrome complete UI action | 24 | 344 ms wall time | UI heartbeat advanced 16 times during optimization |
| Node VM engine harness | 24 | 1,718.8 ms | Same placement totals and utilization |

VM and browser timings are separate measurements and should not be compared as a runtime guarantee. Different jobs, hardware, browser policies and search efforts change performance. The Simple/Pro regression rerun measured 274.8 ms in the Chrome worker (322 ms wall time, 15 UI heartbeats), and 1,593.9 ms in the Node VM, with the same 1,000-part results.

## Simple/Pro refinement verification

The default Simple workspace was inspected at 1920×1080, 1440×900, 1280×800, 768×1024 and 390×844. It exposes inline panel/cut dimensions and quantities, a compact efficiency summary and the cutting diagram. Save/Open, material fields, laser metrics and advanced reports are hidden; all remain available in Pro.

Fourteen focused browser checks passed: default visibility; responsive overflow; preserving complete inputs and geometry across both mode switches; Reset with Undo recovery; keyboard panel/cut entry and a 100% exact fit; rejected invalid sizes; physical-unit preservation; kerf plus finite inventory with explicit unplaced pieces; simplified overview/selection; actual SVG and native PDF export without material metadata; retained Pro constraints and persisted mode; mobile input-to-layout and reset flow; Pro responsive behavior; and absence of runtime errors. Screenshots and sample exports use the `simple-` prefix in `sheetcut-qa/`.

The existing 26 engine tests and 29 Pro browser checks were rerun after integration and passed. A targeted final print check also passed. The optimizer's geometry model was not changed by the interface refinement.

## Visual and accessibility review

Desktop and mobile screenshots are retained in `sheetcut-qa/`. The mechanical interface detector returned no findings. Independent Impeccable review identified four material issues, all corrected and reverified:

| Finding | Final verdict | Verification |
|---|---|---|
| Keyboard focus lost after rerenders | Resolved | Repeated keyboard zoom and Tab/Shift+Tab/Enter assertions |
| Selected inspector remained offscreen | Resolved | Focused heading, panel visibility assertion and mobile screenshot |
| Mobile Undo/Redo hidden | Resolved | Both buttons visible at mobile widths |
| Toggle states and dialog name missing | Resolved | Synchronized `aria-pressed`; dialog `aria-labelledby` |

**Final independent interface review disposition: PASS.** The reviewer checked source, screenshots and recorded browser assertions; they did not independently rerun the browser suite.

The generated PDF was opened with a local PDF renderer and the summary and first sheet pages were visually inspected. A dark screen color-scheme leak into page margins was corrected; final PDF pages use white paper and the selected alternative candidate is labeled accurately. `print-page-1.png` and `print-page-2.png` retain this evidence.

## Known limits

- Heuristic search is not a proof of optimality and can miss a feasible or better packing.
- Material and thickness are strictly separated; no automatic compatibility aliases.
- Common-line length requires zero kerf and spacing. Nonzero-kerf compensation is a CAM responsibility.
- Nominal perimeter, piercing and duration are estimates; no machine-operation or rapid-distance claims.
- Kerf loss and reusable offcuts are estimates, not exact waste classification.
- Saved projects restore inputs/settings and require reoptimization. Manual layouts are session-only; retain them through SVG/CSV/PDF exports.
- Browser local storage is profile/path-dependent. JSON is the portable backup.
- 5,000-part / 200-stock-type limits; eight candidate layouts retained for switching.
- Firefox, screen-reader speech output, physical printer output and real machine/controller execution were not tested.
