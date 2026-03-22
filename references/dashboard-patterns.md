# Dashboard Patterns

## Dashboard Types

Design differently for each:

### 1. Strategic / Executive
- KPI cards + trends + traffic lights
- Minimal interaction. Glanceable in 10 seconds
- Weekly/monthly cadence
- "How are we doing?"

### 2. Analytical / Explorer
- Filters, drill-downs, cross-filtering
- Dense but organized
- For analysts who live in the data daily
- "Why is this happening?"

### 3. Operational / Monitoring
- Real-time or near-real-time
- Alerts, thresholds, status indicators
- Designed for wall-mounted or always-on screens
- "What needs attention right now?"

## Layout Blueprint

```
┌──────────────────────────────────────────────────┐
│  TITLE  |  Date range  |  Key filters            │
├──────────┬──────────┬──────────┬─────────────────┤
│  KPI 1   │  KPI 2   │  KPI 3   │  KPI 4         │
│  +12%↑   │  -3%↓    │  On tgt  │  Alert          │
├──────────┴──────────┴──────────┴─────────────────┤
│  Primary chart (largest, most important trend)    │
│  Takes 50-60% of visual weight                    │
├────────────────────┬─────────────────────────────┤
│  Supporting chart 1 │  Supporting chart 2         │
│  (comparison/detail)│  (breakdown/composition)    │
├────────────────────┴─────────────────────────────┤
│  Table / detailed data (optional, below fold)     │
└──────────────────────────────────────────────────┘
```

### Reading Pattern
- **F-pattern / Z-pattern.** Most important KPIs top-left
- Summary flows top-to-bottom, left-to-right
- Progressive disclosure: overview first, drill-down on interaction

## KPI Card Pattern

```
┌─────────────────────────────┐
│  Completion Rate        ℹ️   │
│  82%           ▲ +3.2pp     │
│  ▂▃▃▄▄▅▅▆▆▇▇█              │  ← sparkline
│  vs. 78.8% last period      │
└─────────────────────────────┘
```

Elements:
- **Label** — metric name (secondary text color)
- **Value** — big number, tabular figures, primary text
- **Delta** — change indicator with direction arrow and color
  - Green ▲ for improvement, Red ▼ for decline (regardless of up/down — what matters is good/bad)
- **Sparkline** — 8-12 data points, no axes, pure trend context
- **Comparison** — "vs. last period" or "target: 80%"

## Interactivity Rules

1. **Cross-filtering** — filters affect ALL charts on the page. Isolated filters confuse
2. **Hover tooltips** — show exact values + context. Chart must be readable WITHOUT hovering
3. **Click-to-drill** — summary to detail. Never navigate away
4. **Loading states** — skeleton screens, not spinners
5. **Empty states** — helpful guidance: "No data for this period. Try adjusting the date range."

## Spacing & Grid

- **12-column grid** with consistent gutters (16px or 24px)
- **Card padding:** 16-24px internal
- **Card gap:** 16px between cards
- **Section spacing:** 24-32px between major sections
- **White space is cognitive breathing room** — cramped dashboards are unreadable

## Responsive Breakpoints

| Breakpoint | Layout |
|-----------|--------|
| Desktop (>1200px) | Full layout, 3-4 column KPI row |
| Tablet (768-1200px) | 2-column KPI row, charts stack |
| Mobile (<768px) | Single column, KPIs as horizontal scroll |

## CSS Grid for Dashboard Layout

```css
.dashboard {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
  padding: 24px;
  background: #0d1117;
}

.kpi-row { grid-column: span 12; display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }
.chart-primary { grid-column: span 12; }
.chart-half { grid-column: span 6; }
.table-full { grid-column: span 12; }

@media (max-width: 1200px) {
  .kpi-row { grid-template-columns: repeat(2, 1fr); }
  .chart-half { grid-column: span 12; }
}
```

## Linear-Style Initiatives Table

Based on the Linear initiatives view — great for project/team health overview:

| Column | Content | Notes |
|--------|---------|-------|
| Name | Project name + icon + optional description | Hierarchy via indentation |
| Target | Date or quarter | Right-aligned |
| Health | Status badge (On track / At risk / Off track) | Semantic colors |
| Progress | Completed / Total | e.g. "23 / 102" |
| Active items | Colored dots (green · orange · gray) | Counts by status |
| Activity | Trend indicator (chevrons) | Activity direction |

Style: dark rows, no heavy borders, subtle hover state, tree-style connector lines for children.
