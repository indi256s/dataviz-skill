# Chart Selection Rules

Hard rules, not guidelines.

## Approved Chart Types

| Chart Type | When to Use | Notes |
|-----------|-------------|-------|
| **Bar (horizontal preferred)** | Comparison across categories | The workhorse. Default when uncertain |
| **Line** | Trends over time | Only for continuous data with meaningful sequence |
| **Scatter** | Correlation between two continuous variables | Add trend line when relationship exists |
| **Small multiples** | Same metric across many groups | Dramatically underused. Tufte's favorite |
| **Slope / Slopegraph** | Before/after or two-point comparison | Great for "what changed" stories |
| **Table / Heatmap** | When exact values matter more than patterns | Tables are not inferior to charts |
| **Sparklines** | Inline trend in tables or KPI cards | Word-sized, data-intense |
| **Stacked area** | Part-to-whole over time where total matters | Only when total is meaningful |
| **Bullet** | Actual vs. target | Replaces gauges in every case |
| **Treemap** | Hierarchical part-to-whole, many categories | Proportions in nested data |
| **Box plot / Violin** | Distribution comparison across groups | Analyst audience only |
| **Waterfall** | Value builds up or breaks down | Sequential additive/subtractive steps |
| **Sankey** | Flow between stages/categories | Pipelines, user journey funnels |
| **Sunburst** | Hierarchical with drill-down | Alternative to treemap for deeper hierarchies |
| **Parallel coordinates** | Multi-dimensional comparison | Analyst audience, many variables |
| **Calendar heatmap** | Daily patterns over months/years | Activity tracking, incident frequency |
| **Gauge** | Single KPI with target context | Use sparingly — bullet usually better |

## Banned Chart Types

| Chart Type | Why | Use Instead |
|-----------|-----|-------------|
| **Pie chart** | Eyes can't compare angles | Horizontal bar. Exception: 2 slices, dramatic majority (95/5) |
| **Donut chart** | Pie with less data-ink | Horizontal bar |
| **3D anything** | Zero information, distorts perception | 2D version |
| **Dual-axis** | Misleading — axis relationship is arbitrary | Small multiples or indexed lines |
| **Radar/spider** | Can't read accurately, arbitrary axis order | Parallel coordinates or small multiples |
| **Word cloud** | Not visualization, it's decoration | Ranked bar chart |
| **Stacked bar (>4 categories)** | Only bottom category is comparable | Small multiples |

## Decision Tree

```
What are you showing?
├─ Comparison across categories → Bar chart (horizontal)
├─ Change over time → Line chart
├─ Part-to-whole
│  ├─ Over time → Stacked area
│  ├─ Point in time, few categories → Horizontal bar (% of total)
│  └─ Hierarchical → Treemap or sunburst
├─ Distribution → Histogram, box plot, or violin
├─ Correlation → Scatter plot
├─ Flow/connection → Sankey or graph
├─ Geographic → Choropleth or bubble map
└─ Single KPI → Big number + sparkline + delta
```
