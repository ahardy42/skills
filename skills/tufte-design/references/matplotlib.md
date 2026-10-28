# Tufte in matplotlib / seaborn / plotly

## Base style (apply first, every time)

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

TUFTE = {
    # erase the box: keep only left + bottom spines
    "axes.spines.top": False,
    "axes.spines.right": False,
    "axes.spines.left": True,
    "axes.spines.bottom": True,
    "axes.edgecolor": "#999999",
    "axes.linewidth": 0.8,
    # no grid by default; enable per-chart only if lookup matters
    "axes.grid": False,
    "grid.color": "#e6e6e6",
    "grid.linewidth": 0.5,
    # muted structure, readable text
    "text.color": "#333333",
    "axes.labelcolor": "#555555",
    "xtick.color": "#555555",
    "ytick.color": "#555555",
    "xtick.direction": "out",
    "ytick.direction": "out",
    "xtick.major.size": 3,
    "ytick.major.size": 3,
    "font.size": 10,
    "axes.titlesize": 12,
    "axes.titlelocation": "left",   # findings read like sentences: left-align
    "axes.titleweight": "bold",
    "figure.facecolor": "white",
    "axes.facecolor": "white",
    "legend.frameon": False,
    "figure.dpi": 150,
}
mpl.rcParams.update(TUFTE)
```

Palette: structure `#999999`/`#cccccc`, default data `#555555` or a single muted blue `#4477aa`, accent (the series that matters) `#cc3311`. Context series in multi-line charts: `#bbbbbb`.

## Direct labeling instead of legends

```python
for name, series in data.items():
    color = ACCENT if name == key_series else "#bbbbbb"
    ax.plot(series.index, series.values, color=color,
            lw=2 if name == key_series else 1)
    ax.annotate(name, xy=(series.index[-1], series.values[-1]),
                xytext=(6, 0), textcoords="offset points",
                va="center", color=color, fontsize=9)
ax.set_xlim(right=series.index[-1] + label_padding)  # room for labels
```

## Range-frame axes (spines span only the data)

```python
ax.spines["left"].set_bounds(ymin, ymax)      # data min/max, not axis limits
ax.spines["bottom"].set_bounds(xmin, xmax)
ax.set_yticks([ymin, ymax])                   # optionally: only extremes
```

The axis itself now conveys the data range — ink doing double duty.

## Dot plot (default replacement for most bar charts)

```python
order = df.sort_values("value")
ax.scatter(order["value"], order["name"], color="#4477aa", s=30, zorder=3)
ax.hlines(order["name"], 0, order["value"], color="#dddddd", lw=0.8, zorder=1)
for _, r in order.iterrows():
    ax.annotate(f'{r["value"]:,.0f}', (r["value"], r["name"]),
                xytext=(6, 0), textcoords="offset points",
                va="center", fontsize=8, color="#555555")
ax.xaxis.set_visible(False)  # values are labeled; the axis is redundant
```

Sort by value, never alphabetically, unless the category order itself is meaningful (time, ordinal scales).

## Bars, when you must

- `ax.bar(..., color="#4477aa", width=0.6)` — one color, no edge, no hatch
- Baseline at zero, no exceptions; label values on/above bars and delete the y-axis
- Horizontal bars for category names >8 chars

## Small multiples

```python
fig, axes = plt.subplots(nrows, ncols, figsize=(ncols*2.2, nrows*1.6),
                         sharex=True, sharey=True)
for ax, (name, g) in zip(axes.flat, df.groupby("category")):
    ax.plot(g["x"], g["y"], color="#4477aa", lw=1.2)
    ax.set_title(name, fontsize=9, loc="left", color="#333333", weight="normal")
    ax.tick_params(labelsize=7)
for ax in axes.flat[len(groups):]:
    ax.set_visible(False)
fig.suptitle(finding_headline, x=0.02, ha="left", fontsize=12, weight="bold")
```

Shared scales are the point — never let each panel autoscale.

## Sparklines

```python
def sparkline(ax, values, color="#555555"):
    ax.plot(values, color=color, lw=1)
    ax.scatter(len(values)-1, values[-1], color="#cc3311", s=8, zorder=3)
    ax.axis("off")
    ax.margins(y=0.3)
# in a table context: figsize=(1.2, 0.25) per cell, print last value as text beside it
```

## Annotate the finding in the plot

```python
ax.annotate("Price change", xy=(event_x, event_y),
            xytext=(event_x, event_y*1.15),
            fontsize=9, color="#333333",
            arrowprops=dict(arrowstyle="-", color="#999999", lw=0.8))
```

Reference lines: `ax.axhline(target, color="#999999", lw=0.8, ls=(0, (4, 3)))` with a small text label at the right edge — never an unexplained line.

## Seaborn / plotly notes

- seaborn: call `sns.despine()` after plotting; pass `palette=["#4477aa"]`; avoid `sns.set_theme()` defaults (heavy grid).
- plotly: `template="simple_white"`, then `fig.update_layout(showlegend=False)` and add annotations for labels; kill `hovermode` clutter with `hovermode="x unified"`.
