---
name: SheetCut Pro
description: A compact CAD workspace for rectangular sheet cutting jobs.
colors:
  bg: "#111820"
  panel: "#19232e"
  panel2: "#202d3a"
  input: "#101a24"
  line: "#354554"
  text: "#e7edf3"
  muted: "#aab9c7"
  accent: "#70dfcc"
  accent-ink: "#102923"
  warn: "#ffd18b"
  bad: "#ffaba8"
  canvas: "#0d141d"
  focus: "#96caff"
  light-bg: "#e7edf1"
  light-panel: "#f8fafb"
  light-panel2: "#edf2f5"
  light-input: "#fff"
  light-line: "#bac7d1"
  light-text: "#1b2d3a"
  light-muted: "#4f6474"
  light-accent: "#157966"
  light-accent-ink: "#fff"
  light-warn: "#825000"
  light-bad: "#b32828"
  light-canvas: "#d5dfe6"
  light-focus: "#175caa"
typography:
  body:
    fontFamily: '"Segoe UI", system-ui, sans-serif'
    fontSize: "13px"
    lineHeight: 1.45
  title:
    fontSize: "14px"
    fontWeight: 650
  label:
    fontSize: "12px"
  table-heading:
    fontSize: "10px"
    fontWeight: 600
    letterSpacing: "0.6px"
  metric:
    fontSize: "42px"
    fontWeight: 600
    lineHeight: 1.2
    letterSpacing: "-1.5px"
rounded:
  control: "5px"
  input: "4px"
  badge: "3px"
  overlay: "6px"
  dialog: "9px"
spacing:
  field-gap: "5px"
  row-gap: "7px"
  stack-gap: "12px"
  overview-gap: "16px"
components:
  button-primary:
    backgroundColor: "{colors.accent}"
    textColor: "{colors.accent-ink}"
    rounded: "{rounded.control}"
    padding: "5px 11px"
  button-secondary:
    backgroundColor: "{colors.panel2}"
    textColor: "{colors.text}"
    rounded: "{rounded.control}"
    padding: "5px 11px"
  button-quiet:
    backgroundColor: "transparent"
    textColor: "{colors.text}"
    rounded: "{rounded.control}"
    padding: "5px 11px"
  button-danger:
    backgroundColor: "{colors.panel2}"
    textColor: "{colors.bad}"
    rounded: "{rounded.control}"
    padding: "5px 11px"
  button-small:
    backgroundColor: "{colors.panel2}"
    textColor: "{colors.text}"
    rounded: "{rounded.control}"
    padding: "3px 7px"
  input-field:
    backgroundColor: "{colors.input}"
    textColor: "{colors.text}"
    rounded: "{rounded.input}"
    padding: "6px 8px"
  navigation-active:
    backgroundColor: "{colors.line}"
    textColor: "{colors.accent}"
  status-badge:
    textColor: "{colors.accent}"
    rounded: "{rounded.badge}"
    padding: "3px 7px"
  sheet-card:
    backgroundColor: "{colors.panel}"
    textColor: "{colors.text}"
    rounded: "{rounded.control}"
    padding: "13px"
---

# Design System: SheetCut Pro

## Overview

**Creative North Star: "The CAD workspace"**

A slate instrument panel surrounds a pale measured sheet. Compact controls, precise dimensions and visible production assumptions support fabrication work; colored parts carry information. This is the user's pinned CAD direction, documented from the implementation rather than a new visual concept.

**Key Characteristics:**

- Dense, divided working panels with a dominant drawing surface.
- Local system typography and tabular measurement numerals.
- Teal actions and validation, with explicit warning and error text.
- Immediate state feedback and deliberate keyboard focus.

Sources: `index.html`, `PRODUCT.md`, `docs/DESIGN-BRIEF.md`, and the final desktop/mobile renders in `sheetcut-qa/`. Frontmatter records implemented values; the matching sidecar records behavior and component examples.

## Colors

Primary teal identifies Optimize layout, selected navigation, progress and validation. Amber communicates estimate limitations or unplaced demand; pale red identifies errors and destructive actions. Part colors distinguish geometry and remain separate from interface status colors.

Neutral graphite/navy surfaces separate the workspace, panels, fields and canvas. Bright text and a readable muted tier preserve hierarchy without making supporting measurements disappear. The optional Light workshop theme substitutes every semantic interface color through the `light-*` tokens; do not change only the background.

**The Contrast Rule.** Preserve the paired text, muted, accent-ink and focus colors in both themes; communicate status with words as well as color.

## Typography

Use Segoe UI with system fallbacks, entirely offline. Body and form text stay compact; section headings use the title role, table headings use uppercase labels, and only the main utilization metric grows large. The metric is 35px at intermediate desktop widths and 44px on mobile. Measurements use tabular numerals and numeric columns align right. Hide drawing labels that cannot fit rather than overlaying illegible text on small parts.

## Layout

The default Simple mode uses a 380px input column and flexible canvas. Inputs are editable Width / Height / Quantity rows under Panels you have and Cuts you need. A three-value efficiency strip replaces the right sidebar and bottom reports. Simple/Pro remains visible in the toolbar. Reset is available in both modes; Save/Open and full settings belong to Pro. Simple mobile uses Inputs and Layout panes, with the selected piece inspector inline above the drawing. Mode changes preserve data and geometry.

Desktop uses a toolbar, three working columns, and a bottom status bar. The standard columns are 370px inputs, a flexible canvas of at least 400px, and 270px results. Below 1251px but above 900px they become 325px / minmax(320px, 1fr) / 230px; from 1750px they become 400px / minmax(500px, 1fr) / 300px. Inputs and results scroll independently. Resizable report tables sit below the drawing.

At 900px and below, the toolbar wraps and Inputs / Layout / Results navigation exposes one working pane at a time. Undo and Redo remain visible in the mobile toolbar; disabled recovery actions keep their place. Reports and tabs scroll horizontally within their own regions. Mobile report height is 200px and vertical resizing is disabled. The canvas reserves space for its controls.

**The Working Pane Rule.** Preserve direct access to Inputs, Layout, Results and recovery actions on narrow screens.

## Elevation & Depth

One-pixel dividers and tonal surfaces define normal workspace depth. Sheet overview cards retain thin borders. Shadows are reserved for dialogs and toast feedback; the dialog backdrop dims the working surface. Avoid adding elevation to every section.

## Shapes

Use compact rectangular controls with modest corners. Inputs, buttons, badges and dialogs retain their distinct radii from frontmatter. Segmented view controls join flush with rounded outer corners; utilization bars use small corners. The stock boundary, reserved margins and part rectangles remain precise geometry.

## Components

- **Actions:** Optimize layout uses the primary button. Standard actions use a bordered secondary button; quiet, small and danger variants preserve the same compact language. Disabled buttons use 45% opacity and a not-allowed cursor.
- **Fields and tables:** Labels remain visible, numeric entry uses compact inputs, and tables use sticky uppercase headers with thin row dividers. Field and button focus uses a 2px theme-aware outline with 2px offset.
- **Navigation:** Active view, report and mobile buttons use the line surface and accent text. Preserve the implemented pressed-state semantics.
- **Measured drawing:** Deterministic part colors, stock boundary, margins and ruler ticks describe actual placement. Selecting a part thickens its outline and brightens it; Enter or Space selects focused geometry.
- **Inspector:** Selection deliberately moves focus to the selected part heading and scrolls that section into view. On mobile it also opens Results. Preserve this purposeful focus transfer, exact coordinate fields and move/rotate actions.
- **Results:** The large utilization figure, compact metric rows, validation badge and written estimate caveats remain visually distinct. Overview cards combine sheet geometry and metrics.
- **Dialogs and feedback:** Modal forms use a divided header/body/footer; toast and inline error messages remain readable in either theme. No decorative animation is implemented. Reduced-motion preference explicitly disables animation, transitions and smooth scrolling.

## Do's and Don'ts

- Do retain exact dimensions, tabular numerals and visible estimate qualifications.
- Do preserve keyboard focus, mobile recovery controls and both complete themes.
- Do keep geometry and report content inside their own scrollable regions.
- Don't add external fonts, decorative imagery or network-dependent assets.
- Don't replace the measured workspace with promotional cards or decorative motion.
- Don't use color alone to communicate validation, selection or errors.
