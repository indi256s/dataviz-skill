# ECharts Guide

Apache ECharts is the preferred charting library. This guide covers setup, patterns, and
where to find examples.

## Setup

```bash
npm install echarts
```

### Register the Linear Dark Theme

```typescript
import * as echarts from 'echarts';
import linearDarkTheme from './echarts-theme-linear.json';

// Register once at app startup
echarts.registerTheme('linear-dark', linearDarkTheme);

// Every chart uses the theme
const chart = echarts.init(container, 'linear-dark');
chart.setOption(option);
```

### React Integration

```tsx
import { useRef, useEffect } from 'react';
import * as echarts from 'echarts';

export function EChart({ option, style }: { option: echarts.EChartsOption; style?: React.CSSProperties }) {
  const ref = useRef<HTMLDivElement>(null);
  const chartRef = useRef<echarts.ECharts>();

  useEffect(() => {
    if (!ref.current) return;
    chartRef.current = echarts.init(ref.current, 'linear-dark');
    const resizeObserver = new ResizeObserver(() => chartRef.current?.resize());
    resizeObserver.observe(ref.current);
    return () => {
      resizeObserver.disconnect();
      chartRef.current?.dispose();
    };
  }, []);

  useEffect(() => {
    chartRef.current?.setOption(option, true);
  }, [option]);

  return <div ref={ref} style={{ width: '100%', height: 400, ...style }} />;
}
```

### Next.js (App Router)

ECharts requires browser APIs, so wrap in `'use client'`:

```tsx
'use client';
import dynamic from 'next/dynamic';

const EChart = dynamic(() => import('./EChart').then(m => m.EChart), { ssr: false });
export { EChart };
```

## Examples Gallery

Browse interactive examples with full option configs:
**https://echarts.apache.org/examples/en/index.html**

### Chart Types Available

| Category | Examples |
|----------|---------|
| **Line** | Basic, smooth, area, stacked, step, gradient |
| **Bar** | Basic, horizontal, stacked, waterfall, negative, sorted |
| **Pie** | Basic, doughnut, rose, nested |
| **Scatter** | Basic, bubble, effect (animated), large-scale |
| **Candlestick** | Basic, OHLC, with MA lines |
| **Radar** | Basic, filled, multiple |
| **Boxplot** | Basic, horizontal, multiple groups |
| **Heatmap** | Cartesian, calendar, large-scale |
| **Graph/Network** | Force layout, circular, les miserables |
| **Tree** | Basic, radial, polyline |
| **Treemap** | Basic, nested, disk usage |
| **Sunburst** | Basic, monochrome, drink flavors |
| **Parallel** | Basic, AQI, nutrients |
| **Sankey** | Basic, vertical, energy flow |
| **Funnel** | Basic, comparison, ascending |
| **Gauge** | Basic, multi-ring, progress |
| **ThemeRiver** | Basic, themed |
| **Calendar** | Basic, heatmap, activity |
| **Custom** | Error bar, gantt, vector field, profit |
| **Dataset** | Connect, transform, encode |
| **Rich Text** | Styled labels, formatted tooltips |
| **Map/Geo** | China, world, USA, heat |
| **3D** | Bar3D, scatter3D, globe (analyst/demo use only) |

Use context7 MCP (`/apache/echarts-doc`) for up-to-date API reference.

## Key Patterns

### Responsive Resize

Always handle container resize:

```javascript
window.addEventListener('resize', () => chart.resize());
// Or use ResizeObserver (preferred):
new ResizeObserver(() => chart.resize()).observe(container);
```

### Large Datasets

For >10k data points, use progressive rendering:

```javascript
option = {
  series: [{
    type: 'scatter',
    large: true,
    largeThreshold: 2000,
    progressive: 400,
    data: bigDataArray
  }]
};
```

### Animation Best Practices

```javascript
option = {
  animation: true,
  animationDuration: 300,           // fast — not slow reveals
  animationEasing: 'cubicOut',      // ease-out, not bouncy
  animationDurationUpdate: 200,     // state transitions even faster
};
```

### Tooltip Formatting

```javascript
tooltip: {
  trigger: 'axis',
  formatter: (params) => {
    const header = `<strong>${params[0].axisValueLabel}</strong>`;
    const rows = params.map(p =>
      `${p.marker} ${p.seriesName}: <strong>${p.value.toLocaleString()}</strong>`
    ).join('<br/>');
    return `${header}<br/>${rows}`;
  }
}
```

### Direct Labels (Preferred Over Legends)

```javascript
series: [{
  type: 'line',
  data: [...],
  endLabel: {                        // ← label at line end
    show: true,
    formatter: '{a}',               // series name
    color: '#e6edf3'
  }
}]
```

### Semantic Colors for Status

```javascript
// Consistent with Linear design taste
const STATUS_COLORS = {
  success:  '#3fb950',  // On track
  warning:  '#d29922',  // At risk
  danger:   '#f85149',  // Off track
  info:     '#58a6ff',  // Neutral highlight
  muted:    '#484f58',  // Background/inactive
};
```

### Dark Mode Anti-Patterns

Avoid these common mistakes in dark mode charts:
- Pure white (#fff) text — use #e6edf3 instead
- Pure black (#000) backgrounds — use #0d1117
- High-saturation neon colors — use muted, desaturated palette
- Thick borders or heavy gridlines — keep subtle
- White tooltips — match the dark theme
