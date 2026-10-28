---
name: tufte-design
description: Apply Edward Tufte's information design principles when creating or critiquing any chart, graph, table, dashboard, data visualization, or report. Use this skill whenever the user asks for a chart, plot, graph, dashboard, sparkline, small multiples, data table, infographic, or asks to "make this chart better", "clean up this visualization", "audit this dashboard", or mentions Tufte, data-ink, or chartjunk — even if they don't name Tufte explicitly. Also use when writing matplotlib, D3, Recharts, SVG, or spreadsheet chart code, and when reviewing existing visualizations for clarity.
---

# Tufte Design

Design and implement data graphics following Edward Tufte's principles: show the data, maximize data-ink, erase chartjunk, respect the viewer's intelligence, and never distort what the numbers say.

The goal is not minimalism for its own sake. The goal is that every drop of ink earns its place by carrying information. A dense, layered graphic that rewards close reading beats both a decorated one and an empty one.

## Core principles (apply to every graphic)

**1. Data-ink ratio.** Data-ink is the non-erasable core of the graphic — the ink that changes when the data changes. Maximize it, within reason. For every visual element ask: if I delete this, is information lost? If not, delete it or mute it. Erase in this order: backgrounds, borders/frames, gridlines, tick marks, redundant labels, legends (replace with direct labels).

**2. No chartjunk.** Never use: 3D effects, drop shadows, gradients on bars, decorative icons inside plots, moiré patterns/hatching, heavy gridlines, background colors, ornamental borders. These are the most common defaults in charting libraries — actively strip them.

**3. Graphical integrity (the lie factor).** The size of an effect shown in the graphic must equal the size of the effect in the data. Concretely:
- Bar charts start at zero. Always. If the interesting variation is far from zero, use a dot plot or line chart instead — don't truncate the bars.
- Line/dot charts may use a non-zero baseline, but say so if it could mislead.
- Use one dimension to show one-dimensional data (length, not area or volume).
- Plot real units, not indexed or smoothed values, unless clearly labeled.
- Label axes with actual values; don't let auto-scaling hide magnitude.

**4. Direct labeling over legends.** Label lines at their endpoints, label bars with their values when few, annotate notable points in place. A legend forces the eye to ping-pong; a label sits where the eye already is. Keep legends only when series count makes direct labels collide (>6–8 series — and then reconsider small multiples).

**5. Small multiples.** When comparing across categories, time slices, or conditions, prefer a grid of small, identical charts over one tangled multi-series chart. Same scales, same design, repeated — the eye learns the format once and then reads only data. Trigger this whenever a single chart would need >4 lines or a grouped/stacked bar chart would need >3 groups per cluster.

**6. Sparklines.** For trends inline with text or in table rows, use word-sized graphics: a bare line, no axes, optional min/max/last dots, current value as a number beside it. Dashboards and tables with per-row trends should default to sparklines, not full charts.

**7. Layering and separation.** Establish visual hierarchy with muted grays for structure (axes, labels, reference lines) and stronger color only for data — and the strongest color only for the data that matters most. One accent color highlighting the key series against gray context lines usually beats a rainbow.

**8. Data density and context.** Don't dumb graphics down. More data in the same space is usually better, if layered well. Show comparisons, show causality where known, show the data over time rather than a single snapshot. Every graphic should answer "compared to what?"

**9. Tables for small datasets.** If there are fewer than ~20 numbers and the reader needs the values, a well-set table beats a chart. See `references/tables.md`.

**10. Words and graphics belong together.** Integrate annotations, units, and explanatory text into the graphic itself. The title should state the finding ("Churn doubled after the March price change"), not the topic ("Churn over time").

## Workflow

**When creating a chart:**
1. Ask what comparison the graphic must support (the "compared to what?"). If unstated, infer it and say so.
2. Pick the form: table (<~20 values, exact values matter) → dot plot / line → small multiples (many series) → sparklines (inline/rows). Reach for bar charts less often than instinct suggests; dot plots carry the same information with less ink and no zero-baseline constraint.
3. Strip library defaults, then implement using the recipe for the target stack:
   - matplotlib/seaborn/plotly → read `references/matplotlib.md`
   - Recharts, D3, raw SVG, HTML/CSS → read `references/web-charts.md`
   - tables (any medium) → read `references/tables.md`
4. Direct-label, annotate the finding, mute everything that isn't data.
5. Self-audit against the checklist below before presenting.

**When critiquing an existing graphic:** walk the checklist, quantify where possible (e.g., "the y-axis starts at 40, inflating a 6% difference to look like 3×" — that's a lie factor of ~50), and produce a corrected version rather than only listing problems.

## Self-audit checklist

Before delivering any graphic, verify:
- [ ] Every element survives the "delete it — is information lost?" test
- [ ] Bars (if any) start at zero; effect sizes in ink match effect sizes in data
- [ ] No legend where direct labels would work
- [ ] Grays for structure, color reserved for data; ≤2 accent colors
- [ ] Gridlines absent or barely visible (and never darker than data)
- [ ] Title states the finding; units and source noted
- [ ] >4 series → converted to small multiples
- [ ] Axis labels horizontal and readable; no rotated 45° label walls (shorten, abbreviate, or flip the chart horizontal instead)

## When to bend the rules

Tufte is a strong prior, not a religion. Keep light gridlines when readers must look up values from the chart. Keep a legend on an interactive chart where hover-labels exist. Accept a stacked bar when part-to-whole *and* trend both matter and the audience knows the form. When a house style guide conflicts, follow the house style and note the tension. Never bend rule 3 (integrity) — distortion is the one non-negotiable.
