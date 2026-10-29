# SheetCut Pro architecture and assumptions

Single delivered index.html contains styles, markup, an independent engine factory, worker adapter, project codec, renderer and UI controller. Development tests are separate and never needed to run the application.

All physical values are millimeters internally. Coordinates are top-left, x right, y down. The finished part is w by h. Packing uses w+g by h+g where g=kerf+spacing, inside a usable bin enlarged by g at its right and bottom. This adds exactly one gap between neighbors without requiring a fictitious extra gap at sheet edges. Margins reserve material outside the usable rectangle.

MaxRects splits every intersected free rectangle and prunes contained rectangles. Guillotine splits only the selected disjoint region and records the slicing tree boundaries; no free-region merging that could erase the slicing guarantee. Deterministic multi-start ordering and stock policies explore inventory choices. A separate validator checks bounds, pairwise gap, stock limits, identity, quantity, material, thickness and grain before accepting candidates.

Common-line length is the union of coincident finished edges and is available only at zero kerf and zero spacing. Nonzero kerf requires CAM compensation; exported finished outlines are never claimed as toolpaths. Independent perimeter estimates exclude lead-ins, rapid travel and controller behavior. Pierce estimate is one per independent contour even when common-line length is selected, deliberately conservative.

Gross unused area includes margin, kerf and remaining material. Kerf loss is explicitly an approximate perimeter-times-kerf calculation, capped at gross unused area; it is not exact waste classification. Offcut rectangles are a conservative disjoint partition outside the expanded part exclusion zones, not guaranteed recoverable stock.

Projects persist inputs and settings. Layouts are deliberately recomputed after reopening so stale or foreign coordinates cannot masquerade as validated output. Local storage errors remain visible; explicit JSON provides portable backups.
