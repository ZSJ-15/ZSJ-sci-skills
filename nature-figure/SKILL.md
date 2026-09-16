---
name: nature-figure
description: Generate publication-ready figures following Nature/Cell/NEJM visual standards.
---

# Nature-style Figure Standards

## Visual Rules
- Font: Arial or Helvetica, 8-12 pt (consistency across panels).
- Color: Colorblind-friendly palettes (avoid red/green alone).
- Line width: 1-1.5 pt for axes, 2-3 pt for data lines.
- Panel labels: A, B, C (bold, 12-14 pt) at top-left.
- Resolution: 300-600 dpi for raster; vector (PDF/SVG) preferred.

## Layout
- Max 4-5 panels per figure (Nature standard).
- White background; minimal gridlines.
- Error bars: SD or SEM clearly defined in legend.
- Statistical markers: *, **, *** with exact test noted.

## Workflow (Python/R)
1. Use `matplotlib` (Python) or `ggplot2` (R).
2. Set global font/theme (e.g., `theme_classic()`).
3. Plot raw data + summary (boxplot/violin + scatter).
4. Annotate stats with `statannot` or `ggsignif`.
5. Export: `plt.savefig("fig.pdf", dpi=300, bbox_inches="tight")`.

## Constraints
- No 3D effects unless essential.
- No excessive color saturation.
- Legend inside plot if space allows.
