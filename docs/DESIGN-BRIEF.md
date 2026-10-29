# SheetCut Pro design brief

The user's pinned CAD workspace is the design authority. The requested no-clarification workflow replaces the optional design interview.

Mode: Operate. Fabricators compare stock, finished dimensions, placements and production estimates. A slate instrument panel surrounds a pale measured sheet; colored parts are the information, not decoration. Teal denotes the primary Optimize action and valid geometry; amber identifies unplaced demand and estimate limitations.

Use local Segoe UI/system fonts to satisfy offline delivery, tabular numerals for all measurements, compact square controls, one-pixel panel dividers and restrained modal elevation. No external assets or decorative imagery. Motion is limited to native scrolling and immediate state feedback; no motion skill or asset generation is needed.

Desktop: toolbar; 370px input panel; flexible measured canvas; 270px results panel; report tabs below the canvas. Mobile: toolbar wraps and a dedicated Inputs / Layout / Results navigation shows one working surface at a time. Reports remain horizontally scrollable tables.

Distinct details: exact stock boundary and reserved margins; deterministic part colors and dimensions; inventory consumed per stock; selected-copy coordinate inspector; physically computed production ledger.

Acceptance: visible input-to-optimize workflow; a real SVG computed from placements; readable selected part metadata; errors and empty states; no document overflow at 390px; all controls keyboard accessible; desktop and mobile browser inspection; no external network calls.

## Simple-mode refinement

User-requested default: two columns on desktop, with an inline panel/cut quantity editor and a large drawing. A compact efficiency / panels-used / pieces-placed strip replaces the analytics sidebar and reports. Material names, costs, named-project controls and advanced strategies are exposed only in Pro. Reset and undo remain visible. Mobile has Inputs / Layout navigation; selection appears beside the drawing, with no separate Results destination. The graphite palette and drawing language remain unchanged. Simple SVG and print exports omit material metadata and production analytics. Both modes preserve the same data, geometry and constraints.
