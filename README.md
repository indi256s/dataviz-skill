# dataviz

A Claude Code skill for data visualization and dashboard design. Turns Claude into an opinionated dataviz expert with the aesthetic sensibility of Edward Tufte and the design taste of Linear's product team.

## What's Inside

| File | Purpose |
|------|---------|
| `SKILL.md` | Core philosophy, design rules, anti-patterns, output standards |
| `references/echarts-theme-linear.json` | Complete ECharts theme — Linear dark mode aesthetic |
| `references/echarts-templates.md` | 14 production-ready chart option skeletons |
| `references/echarts-guide.md` | ECharts setup, React/Next.js integration, patterns |
| `references/chart-selection.md` | Decision tree for choosing the right chart type |
| `references/palettes-and-accessibility.md` | Color palettes, WCAG compliance, colorblind-safe alternatives |
| `references/dashboard-patterns.md` | Layout blueprints, KPI cards, responsive grid, CSS patterns |

## Design Taste

All output follows the **Linear/Notion dark-mode aesthetic**:
- Dark backgrounds (`#0d1117`), subtle borders (`#30363d`)
- Muted, professional data colors — not shouty
- Semantic status colors (green/yellow/red for on-track/at-risk/off-track)
- Inter font, tabular figures, clean hierarchy

## Key Feature: Bundled Theme + Templates

This isn't just philosophy — it ships executable artifacts:

1. **`echarts-theme-linear.json`** — Register once, every chart looks premium automatically
2. **`echarts-templates.md`** — Copy-paste skeletons for bar, line, scatter, sparkline, heatmap, treemap, sankey, waterfall, bullet, boxplot, calendar, and gauge charts

## Installation

### Claude Code

```bash
# Clone to your skills directory
git clone https://github.com/indi256s/dataviz-skill.git ~/.claude/skills/dataviz
```

The skill auto-triggers whenever you work with charts, dashboards, or data visualization.

## Philosophy

- **Data-ink ratio is sacred** — every element earns its place or dies
- **Chart titles are conclusions** — "Revenue grew 23% YoY" not "Revenue Over Time"
- **Gray is your best friend** — highlight 1-2 series, push the rest to background
- **No pie charts** — horizontal bar is always better
- **Colorblind-safe always** — never rely on red/green alone

## License

MIT
