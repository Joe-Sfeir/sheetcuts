# sheetcuts

**SheetCut Pro — 2D Cutting Optimizer** is an offline rectangular sheet-cutting application for desktop fabrication workflows.

## Run

Double-click **[index.html](index.html)** and open it in Chrome or Edge. No installation, server, internet connection, build step, or runtime dependencies are required. All application HTML, CSS, JavaScript and worker code live in that one file.

The first launch opens **Simple mode** with a labeled editable example. Enter **Panels you have** (width, height, quantity) and **Cuts you need** (width, height, quantity), then select **Optimize**. The output focuses on efficiency, panels used, placed pieces and the visual cutting plan.

**Simple / Pro** changes the interface without changing your job or its cutting rules. Simple hides material fields, the project library, advanced analytics and strategy reports. Pro retains all of those controls. The selected mode is remembered. Any advanced constraints already configured in Pro remain active, with a visible notice in Simple.

Use **Reset** to clear the current job; **Undo** restores it if needed. Simple-mode reset starts with zero kerf, gap and margins, so set your blade/laser clearance under **Cutting options** before a real job. Autosave still protects the current workspace, with no Save/Open steps required in Simple. Simple exports a clean report or selected-panel SVG; Pro exposes the complete export menu.

## Features

- Multiple stock sizes, finite/unlimited inventory, materials, thickness, grain, prices and active stock selection.
- Editable parts with quantity, rotation, priority, notes, color, duplicate/delete, CSV import and tab-separated spreadsheet paste.
- MaxRects short-side, long-side, area, bottom-left and contact-point strategies; slicing-compatible guillotine strategies; alternate orders and deterministic randomized restarts.
- Fast, Balanced and Thorough search; five objectives; validated comparison results and eight retained candidate layouts.
- Separate geometry validation of bounds, clearance, stock usage, identities, quantities, materials, thickness and orientation.
- Precise SVG layouts, overview/detail, zoom/pan, grid, rulers, kerf display, offcut estimates and keyboard-accessible selection.
- Validated coordinate edits and rotation for unrestricted layouts, with undo/redo. Manual edits are disabled for guillotine layouts to preserve slicing structure.
- Material utilization and cost, independent/shared cutting lengths, contour and pierce estimates, and configurable cutting-time estimates.
- Autosave, named local projects, Save as, duplicate, rename, delete, and portable JSON import/export.
- Per-sheet annotated SVG or clean finished outlines; parts/placements/statistics CSV; native browser Print / Save as PDF.
- Dark/light themes, millimeters/centimeters/inches, decimal precision and saved defaults. Mobile uses dedicated Inputs / Layout / Results panes.

## Geometry and manufacturing assumptions

All dimensions are stored in **millimeters**. Coordinates start at the sheet's top-left; X increases right and Y increases down. Switching display units does not alter physical dimensions.

Parts describe **finished rectangles**. Nominal part-to-part clearance is **kerf + extra spacing**. The optimizer adds one clearance envelope on each part's right and bottom and enlarges the usable packing bin by the same clearance on those boundaries. This applies a single gap between adjacent parts and does not invent an extra gap at outer sheet edges. Margins independently reserve the sheet perimeter. Choose margins appropriate to your actual edge-cutting process.

Materials match by trimmed, case-insensitive name; thickness and geometry use an absolute 0.0000001 mm tolerance. There is no implicit cross-material compatibility. Grain-constrained parts require a known matching stock grain after rotation.

Every accepted candidate is geometrically checked. **Best found does not mean proven optimal.** These packing families follow established [MaxRects and Guillotine research](https://github.com/juj/RectangleBinPack).

The comparison first minimizes unplaced quantity, then unplaced area. Subsequent objectives are:

| Objective | Primary score after placement completeness |
|---|---|
| Sheet count | Allocated sheet count, then gross area |
| Utilization | Gross allocated area, then sheet count |
| Stock cost | Total supplied stock cost, then count and area |
| Cut length | Selected nominal cutting length, then count and area |
| Balanced | Gross area × (1 + 0.08 × sheet count) |

Priority influences insertion order. Cost optimization requires prices for every active stock type. Independent perimeter is invariant for the same set of placed parts, so a cut-length objective can improve arrangement-dependent length only when eligible shared edges exist.

**Common-line savings are available only with zero kerf and zero extra spacing**, and only for coincident finished edges. Nonzero-kerf common-line compensation belongs in CAM. One pierce per independent rectangle remains the estimate even with shared cutting lines.

Time = selected cutting length / speed + estimated pierces × delay + job setup. Lead-ins, acceleration and rapid travel are excluded. Rapid speed is stored for reference; no travel distance is invented. Guillotine partition splits are reported separately and are not claimed as exact machine operations.

Gross unused area includes reserved margins, cutting loss and remaining material. Kerf area is a capped perimeter-times-kerf estimate; overlapping bands and sheet-edge clipping are not resolved. Offcuts are conservative non-overlapping rectangles outside exclusion zones, not certified reusable inventory. No exact non-recoverable waste claim is made.

**SVG is finished design geometry, not a machine-ready toolpath.** Perform CAM compensation, ordering, lead-ins and controller validation before cutting. G-code and DXF are intentionally not offered.

## Saving and limits

Autosave and the project library use this browser's local storage. Keep the same file location and browser profile; moving the file or clearing browser data can make local saves inaccessible. Export **JSON** for portable backups.

Saved projects preserve inputs and settings. Layouts are deliberately recomputed after reopening. **Manual layout edits and strategy selections are session-only**; export SVG/CSV/PDF to retain a particular cutting plan. The app explains this limitation when edits are made and when a project is restored.

Limits: **5,000 physical parts and 200 stock types per job**. Fast/Balanced/Thorough use up to 8/24/60 attempts, with nominal 5/15/45-second budgets after allowing the first candidate to complete. Work runs in a Blob worker, with a cooperative main-thread fallback. Cancel keeps completed candidates. These are search bounds, not real-time guarantees.

## Verification

See [the QA report](docs/QA-REPORT.md) and recorded results in [sheetcut-qa](sheetcut-qa/).

- 26 executed engine tests passed, including exact fits, kerf, margins, rotation, grain, stock limits, arithmetic, validation corruption checks and seeded geometry fuzzing.
- 29 executed browser checks passed, including direct `file://` Chrome and Edge, persistence, real exports, PDF generation, fallback, cancellation and keyboard/mobile workflows.
- 14 additional Simple-mode checks passed: inline entry, Reset/Undo, data-preserving mode switching, units, finite stock, hidden metadata, exports and mobile navigation.
- Zero external network requests and zero browser console errors during the Chrome acceptance run.
- A 1,000-part Chrome benchmark placed all parts over 19 sheets at 89.36% utilization: 24 attempts in approximately **292 ms engine time / 344 ms interaction wall time** on the tested machine. Performance varies by machine and job.
- Firefox was not executed because its Playwright browser binary is not installed. No browser was installed automatically.

## Development tests (optional)

These files are for maintainers; they are **not required to use the app**.

```text
node sheetcut-tests.cjs
node sheetcut-browser-tests.cjs
node sheetcut-print-check.cjs
node sheetcut-simple-tests.cjs
```

The engine suite uses only Node built-ins and reads the engine directly from `index.html`. The browser suite requires an already available Playwright installation and Chrome. It checks a local `playwright` module first, then the existing Windows Playwright CLI installation; alternatively set `SHEETCUT_PLAYWRIGHT_MODULE` to an installed module path. It does not install anything.

Keyboard: Ctrl/Cmd+S saves; Ctrl/Cmd+Z undoes; Ctrl/Cmd+Y redoes; Tab/Shift+Tab/Enter moves between part cells; Enter/Space selects a focused part. Scroll the canvas to zoom and drag the background to pan.
