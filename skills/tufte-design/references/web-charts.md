# Tufte on the web: Recharts, D3, raw SVG, CSS

## Shared design tokens

```css
--ink-data: #4477aa;      /* default data color */
--ink-accent: #cc3311;    /* the one series that matters */
--ink-context: #c4c4c4;   /* de-emphasized series */
--ink-structure: #999999; /* axes, ticks */
--ink-label: #555555;     /* axis text */
--ink-text: #333333;      /* titles, annotations */
--grid: #ececec;          /* only if lookup needed */
```

Typography: system font stack or a humanist sans, 11–13px labels, titles 14–16px semibold, left-aligned, phrased as the finding.

## Recharts

Strip the defaults — Recharts ships with chartjunk on:

```jsx
<LineChart data={data} margin={{ top: 8, right: 90, bottom: 8, left: 8 }}>
  {/* no <CartesianGrid /> by default; if needed: */}
  {/* <CartesianGrid stroke="#ececec" vertical={false} /> */}
  <XAxis dataKey="x" axisLine={{ stroke: "#999" }} tickLine={false}
         tick={{ fill: "#555", fontSize: 11 }} />
  <YAxis axisLine={false} tickLine={false}
         tick={{ fill: "#555", fontSize: 11 }} width={36} />
  {/* no <Legend />: direct-label via a custom label on the last point */}
  <Line dataKey="y" stroke="var(--ink-data)" strokeWidth={2}
        dot={false} isAnimationActive={false}
        label={<EndLabel name="Revenue" />} />
  <Tooltip cursor={{ stroke: "#ccc" }} />
</LineChart>
```

```jsx
const EndLabel = ({ name, x, y, index, data }) =>
  index === data.length - 1 ? (
    <text x={x + 8} y={y} dy={4} fill="currentColor" fontSize={12}>{name}</text>
  ) : null;
```

- Bars: `<Bar fill="var(--ink-data)" radius={0} />`, `domain={[0, 'auto']}` — never a truncated bar domain.
- Multi-series: color only the key series with `--ink-accent`, rest `--ink-context`; or render small multiples as a CSS grid of little `<ResponsiveContainer>`s with identical `domain` props (hard-code the shared domain — don't let each panel autoscale).
- Keep `Tooltip` (interactivity is honest data density); disable animations (motion is non-data-ink).

## D3 / raw SVG

- Axes: `d3.axisLeft(y).tickSize(3).tickPadding(6)`, then remove the domain path or set it to `#999`; drop tick count to `.ticks(4)`.
- Range frames: instead of the full domain line, draw a single line from `y(min)` to `y(max)`.
- Direct labels: `selection.append("text").attr("x", x(last.x) + 6).attr("y", y(last.y))` per series, colored to match.
- Sparklines: `<svg width="90" height="18">` with a single path, `vector-effect="non-scaling-stroke"`, final-value dot in `--ink-accent`, no axes. Set `aria-label` with the actual numbers for accessibility.

## HTML/CSS-only options

- Inline bar in a table cell: a `<div>` with `height: 8px; background: var(--ink-data); width: calc(var(--v) * 100%)` on a fixed-width track — honest lengths, near-zero ink.
- Never use CSS gradients, shadows, or rounded bars for data elements.

## Dashboards

- Default row-level trends to sparklines with the current value as text: `▁▂▄▇ 1,204` beats a 300px chart per row.
- One accent color across the whole dashboard, meaning one thing ("needs attention").
- Big numbers are fine (a number is the densest graphic), but pair each with its comparison: delta vs. last period, or a sparkline. A number without "compared to what?" is decoration.
- Resist card borders and background fills; whitespace separates better than boxes.
