# Scribe — Session Log

## 2026-04-14

- Created `config/configapa_template.css` from `configapa.css` — zero hardcoded colors outside `:root`, added semantic tokens for surfaces, callouts, badges, rings, table borders
- Created `config/configapa_UofA.css` from template — colors extracted from `assests/docs/Poster Template UA.pptx`, then narrowed to user's three specified colors: `#AB0520`, `#09234B`, `#A8AAB0`
- Set `--color-h2` to `#AB0520` (red) per user direction
- Created `assests/html/poster_UofA.html` — 36in x 24in poster matching PPTX slide dimensions, 3-column layout with Navy hero banner
- Populated poster with data from `RAGARMY_local/assets/output/autism_moon_presentation.html` (Autism Moon grant proposal): Introduction, Methods, Literature Review, Discussion, References
- Iterated on poster layout: 3-row structure (content columns, QR flow diagram, product sample)
- Added ArizonaLEND logo extracted from `assests/docs/azlend graphic.odt` → `assests/graphics/azlend_logo.png`
- Added QR-to-Survey Experience SVG (6-step flow from source data), colors swapped to `var()` references
- Added Grant Application table, Study Design vertical flow diagram, Product Sample two-page spread SVG
- Proportionally scaled Study Design SVG to 75% to align with Grant Application table
- Restructured poster to 2-row layout: row 1 (3 columns) + row 2 (full-width Product Sample)
- Column 1: Introduction (Problem, Purpose, Grant Application table) — text sizes +2px
- Column 2: "The Study" with split layout — Study Overview table (left) + Study Design SVG flow (right)
- Column 3: Sustainability (5 cards in 2-col grid with navy highlight card) + Participant QR-to-Survey SVG
- Replaced Product Sample SVG with full source SVG from presentation (paper bag illustration, braille strips, activity content), colors only changed
- Red h2 underline extended continuously from column 2 through column 3
- Removed all column divider lines and row borders for cleaner layout
- Updated author to "Sean Mapoles, M.A." and affiliation to "Arizona LEND"
- Row 1 height tuned to align with Grant Application table bottom (8.50in + 120px buffer)
- Extensive iterative sizing: SVG box heights, card dimensions, table row padding, spacing offsets
- Next: further layout adjustments, print testing, final visual polish

---

# Scribe — Session Preferences

## CSS Architecture

- **No hardcoded values in markup or rules.** Every color, size, and spacing value must flow through a CSS custom property. No fallback values in `var()` — define the variable explicitly in `:root` or a scoped selector.
- **One variable, one purpose.** When multiple elements should match (e.g., all chart text), unify them under a single token (`--size-chart-text`) rather than maintaining separate variables that happen to share a value.
- **Scope overrides to the component.** Use class-scoped custom property overrides (e.g., `.fig-dark { --color-body: #FFFBF0; }`) rather than writing per-element color/size rules. Let the base rules do the work.
- **Collapse duplicate classes.** If two classes (e.g., `.mean-line` and `.mean-line-int`) should render identically, merge them into one class (`.trendline`). Don't maintain parallel definitions.

## SVG Charts

- **Axis label buffers must be visually consistent.** Account for SVG text baseline positioning — coordinate offsets differ between horizontal text (y = baseline, not top) and anchored text (`text-anchor="end"` measures from right edge). Match the *visual* gap, not the coordinate gap.
- **Axis titles get their own class** (`.t-axis-title`), separate from tick labels (`.t-axis`), so they can be sized independently via CSS.
- **Both data series must render identically** unless explicitly differentiated. Same stroke, fill, weight, opacity, dash pattern.
- **Trendlines follow actual regression slope** through the data points — not flat horizontal mean lines.
- **Slope labels** use the format `b = <value>` positioned at the end of the trendline.
- **Canvas (viewBox) must accommodate all labels.** Expand the viewBox rather than cramming labels into tight margins.

## Spacing & Layout

- **Consistent buffers across all four axis edges.** The gap from axis line to tick labels, and from tick labels to axis titles, should be the same on x and y axes.
- **Full-bleed dark panels** use `calc(-50vw + 50%)` margin/padding technique to break out of the container edge to edge.
- **Title spacing is independent.** `figure-number` and `figure-title` can have different top/bottom margins — don't group them if one needs adjustment.

## Color Palette (Current)

- Deep forest: `#071A0B`
- Warm parchment: `#FFFBF0`
- Dark panel tracks: `rgba(255,251,240,0.2)`
- All colors referenced via `--color-body`, `--bg-hero`, `--bar-track` — never raw hex in element rules.

## Process

- Iterative visual refinement: small adjustments, check in browser, refine.
- When asked to match spacing, provide the current coordinate values and math before making changes.
- Don't change elements the user has already approved — scope fixes tightly.
