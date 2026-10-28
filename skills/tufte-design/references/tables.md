# Tufte tables

Tables are the best graphic for small datasets where exact values matter. Tufte's table rules (aligned with Booktabs conventions):

## Rules

1. **No vertical rules, ever.** Whitespace and alignment do the separating.
2. **Three horizontal rules maximum:** above the header, below the header, below the last row. Optionally thin rules for grouped sections. No zebra striping unless rows are very wide.
3. **Right-align numbers**, align on the decimal point, use a consistent number of decimals per column. Left-align text. Never center data columns.
4. **Tabular (monospaced-digit) figures** so digits line up: `font-variant-numeric: tabular-nums` in CSS; most serif/sans fonts support it.
5. **Order rows by the data**, not alphabetically — sort by the column the reader cares about. Alphabetical order is only right when the table is for lookup.
6. **Units in the header**, not repeated in every cell (`Revenue ($M)`, not `$1.2M`, `$0.8M`...).
7. **Round mercilessly.** Two significant digits are usually enough for comparison; footnote where precision lives if someone needs it.
8. **Summary rows** (totals, means) set off by a thin rule, not bold-everything.
9. **Sparkline column** for per-row trends; a tiny inline bar for per-row magnitude.

## HTML/CSS template

```html
<style>
  table.tufte { border-collapse: collapse; font-variant-numeric: tabular-nums; }
  .tufte th { border-bottom: 1px solid #999; font-weight: 600;
              text-align: left; padding: 4px 12px 4px 0; color: #333; }
  .tufte th.num, .tufte td.num { text-align: right; }
  .tufte td { padding: 3px 12px 3px 0; color: #333; }
  .tufte tbody tr:last-child td { border-bottom: 1px solid #999; }
  .tufte tr.total td { border-top: 1px solid #ccc; }
</style>
```

## Markdown / plaintext

Markdown can't drop its pipes, but keep alignment honest (`---:` for numeric columns), sort by value, put units in headers, and keep decimals consistent. For plaintext, pad with spaces to align decimal points.

## LaTeX

Use `booktabs`: `\toprule`, `\midrule`, `\bottomrule`, `S` columns from `siunitx` for decimal alignment, no `\vline`, no `\hline` between body rows.
