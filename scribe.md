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
