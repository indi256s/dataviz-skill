# ECharts Option Templates

Production-ready skeletons for every approved chart type. The Linear dark theme is assumed
(`echarts.init(dom, 'linear-dark')`). Replace placeholder comments with real data.

## Table of Contents
- [Bar Chart (Horizontal)](#bar-chart-horizontal)
- [Bar Chart (Vertical)](#bar-chart-vertical)
- [Line Chart (Trend)](#line-chart-trend)
- [Area Chart (Stacked)](#area-chart-stacked)
- [Scatter Plot](#scatter-plot)
- [KPI Sparkline](#kpi-sparkline)
- [Heatmap](#heatmap)
- [Treemap](#treemap)
- [Sankey](#sankey)
- [Waterfall](#waterfall)
- [Bullet Chart](#bullet-chart)
- [Box Plot](#box-plot)
- [Calendar Heatmap](#calendar-heatmap)
- [Gauge (Minimal)](#gauge-minimal)

---

## Bar Chart (Horizontal)

The workhorse. Use for any categorical comparison.

```javascript
option = {
  title: {
    text: 'Cycle time improved 22% in Q1',       // ← conclusion, not description
    subtext: 'Median days from start to done, by team · Q1 2026'
  },
  tooltip: { trigger: 'axis', axisPointer: { type: 'shadow' } },
  grid: { left: 120, right: 40, top: 60, bottom: 24 },
  xAxis: {
    type: 'value',
    name: 'Days',
    nameLocation: 'end',
    min: 0                                        // ← ALWAYS zero for bars
  },
  yAxis: {
    type: 'category',
    data: ['Team A', 'Team B', 'Team C', 'Team D', 'Team E'],
    inverse: true                                 // ← top-to-bottom ranking
  },
  series: [{
    type: 'bar',
    data: [12, 9, 7, 5, 3],
    label: {
      show: true,
      position: 'right',                          // ← direct labels > legend
      formatter: '{c} days'
    },
    itemStyle: {
      color: '#58a6ff'                             // ← single color for single series
    }
  }]
};
```

## Bar Chart (Vertical)

Use when categories are few and time-ordered (quarters, months).

```javascript
option = {
  title: {
    text: 'Sprint velocity stabilized at 42 SP',
    subtext: 'Story points completed per sprint · Team Alpha'
  },
  tooltip: { trigger: 'axis', axisPointer: { type: 'shadow' } },
  grid: { left: 48, right: 24, top: 60, bottom: 36 },
  xAxis: {
    type: 'category',
    data: ['S1', 'S2', 'S3', 'S4', 'S5', 'S6']
  },
  yAxis: {
    type: 'value',
    name: 'Story Points',
    min: 0                                         // ← zero baseline
  },
  series: [{
    type: 'bar',
    data: [34, 38, 41, 42, 43, 42],
    label: { show: true, position: 'top' },
    markLine: {
      data: [{ type: 'average', name: 'Avg' }],   // ← reference line for context
      label: { formatter: 'Avg: {c}' }
    }
  }]
};
```

## Line Chart (Trend)

For continuous time-series data.

```javascript
option = {
  title: {
    text: 'Completion rate trending up since Nov',
    subtext: 'Weekly completion rate · All teams · Last 12 weeks'
  },
  tooltip: { trigger: 'axis' },
  grid: { left: 48, right: 24, top: 60, bottom: 36 },
  xAxis: {
    type: 'category',
    data: ['W1', 'W2', 'W3', 'W4', 'W5', 'W6', 'W7', 'W8', 'W9', 'W10', 'W11', 'W12'],
    boundaryGap: false
  },
  yAxis: {
    type: 'value',
    name: '%',
    min: 60,                                       // ← OK for line charts to start non-zero
    axisLabel: { formatter: '{value}%' }
  },
  series: [{
    type: 'line',
    data: [68, 70, 69, 72, 71, 74, 73, 76, 78, 79, 81, 82],
    areaStyle: { opacity: 0.08 },                  // ← subtle fill for context
    markLine: {
      data: [{ yAxis: 80, name: 'Target' }],
      lineStyle: { color: '#3fb950', type: 'dashed' },
      label: { formatter: 'Target: 80%', position: 'insideEndTop' }
    }
  }]
};
```

## Area Chart (Stacked)

Part-to-whole over time. Max 4 categories.

```javascript
option = {
  title: {
    text: 'Bug sources shifted to production in Q1',
    subtext: 'Bug count by source · Monthly · All teams'
  },
  tooltip: { trigger: 'axis', axisPointer: { type: 'cross' } },
  legend: { top: 4, right: 0 },
  grid: { left: 48, right: 24, top: 56, bottom: 36 },
  xAxis: {
    type: 'category',
    data: ['Oct', 'Nov', 'Dec', 'Jan', 'Feb', 'Mar'],
    boundaryGap: false
  },
  yAxis: { type: 'value', name: 'Bugs' },
  series: [
    { name: 'Testing', type: 'line', stack: 'total', areaStyle: { opacity: 0.4 }, data: [40, 38, 35, 30, 28, 25] },
    { name: 'Staging', type: 'line', stack: 'total', areaStyle: { opacity: 0.4 }, data: [20, 22, 18, 15, 14, 12] },
    { name: 'Production', type: 'line', stack: 'total', areaStyle: { opacity: 0.4 }, data: [10, 12, 15, 20, 24, 28] }
  ]
};
```

## Scatter Plot

Correlation between two continuous variables.

```javascript
option = {
  title: {
    text: 'Larger teams ship slower — above 8 devs, cycle time doubles',
    subtext: 'Team size vs. median cycle time · Q1 2026'
  },
  tooltip: {
    formatter: (p) => `${p.data[2]}<br/>Size: ${p.data[0]}<br/>Cycle: ${p.data[1]}d`
  },
  grid: { left: 56, right: 24, top: 60, bottom: 48 },
  xAxis: { name: 'Team Size', nameLocation: 'center', nameGap: 32 },
  yAxis: { name: 'Cycle Time (days)', min: 0 },
  series: [{
    type: 'scatter',
    data: [
      // [team_size, cycle_time, team_name]
      [3, 4, 'Alpha'], [5, 5, 'Beta'], [7, 7, 'Gamma'],
      [9, 12, 'Delta'], [12, 15, 'Epsilon'], [15, 18, 'Zeta']
    ],
    symbolSize: 12,
    label: {
      show: true,
      formatter: (p) => p.data[2],                 // ← direct label with team name
      position: 'right',
      fontSize: 10,
      color: '#8b949e'
    }
  }]
};
```

## KPI Sparkline

Tiny inline chart for KPI cards. No axes, no labels — pure trend.

```javascript
option = {
  grid: { left: 0, right: 0, top: 0, bottom: 0 },
  xAxis: { type: 'category', show: false, data: [1,2,3,4,5,6,7,8,9,10,11,12] },
  yAxis: { type: 'value', show: false },
  series: [{
    type: 'line',
    data: [68, 70, 69, 72, 71, 74, 73, 76, 78, 79, 81, 82],
    smooth: true,
    symbol: 'none',
    lineStyle: { width: 1.5, color: '#3fb950' },   // ← green = trending up
    areaStyle: { opacity: 0.06, color: '#3fb950' }
  }]
};
// Render in a small container: width ~120px, height ~32px
```

## Heatmap

For density, correlation matrices, or time-pattern grids.

```javascript
option = {
  title: {
    text: 'Deploys cluster on Tuesday-Thursday afternoons',
    subtext: 'Deploy count by day × hour · Last 90 days'
  },
  tooltip: { formatter: (p) => `${p.data[2]} deploys` },
  grid: { left: 80, right: 40, top: 60, bottom: 36 },
  xAxis: {
    type: 'category',
    data: ['9am','10am','11am','12pm','1pm','2pm','3pm','4pm','5pm','6pm']
  },
  yAxis: {
    type: 'category',
    data: ['Mon','Tue','Wed','Thu','Fri']
  },
  visualMap: {
    min: 0, max: 20,
    calculable: true,
    orient: 'horizontal',
    left: 'center', bottom: 0,
    inRange: { color: ['#0d1117', '#58a6ff'] }
  },
  series: [{
    type: 'heatmap',
    data: [/* [hour_idx, day_idx, value] */],
    emphasis: { itemStyle: { shadowBlur: 8, shadowColor: 'rgba(0,0,0,0.3)' } }
  }]
};
```

## Treemap

Hierarchical part-to-whole.

```javascript
option = {
  title: {
    text: 'Mobile consumes 42% of total story points',
    subtext: 'SP allocation by stream → team · Q1 2026'
  },
  tooltip: { formatter: (p) => `${p.name}: ${p.value} SP` },
  series: [{
    type: 'treemap',
    roam: false,
    breadcrumb: { show: true, itemStyle: { color: '#161b22', textStyle: { color: '#8b949e' } } },
    levels: [
      { itemStyle: { borderColor: '#30363d', borderWidth: 2, gapWidth: 2 } },
      { itemStyle: { borderColor: '#21262d', borderWidth: 1, gapWidth: 1 },
        colorSaturation: [0.3, 0.6] }
    ],
    label: { show: true, color: '#e6edf3', fontSize: 12 },
    data: [
      { name: 'Mobile', value: 420, children: [
        { name: 'iOS', value: 240 }, { name: 'Android', value: 180 }
      ]},
      { name: 'Web', value: 380, children: [
        { name: 'Frontend', value: 220 }, { name: 'BFF', value: 160 }
      ]}
    ]
  }]
};
```

## Sankey

Flow between stages.

```javascript
option = {
  title: {
    text: '35% of issues stall in code review',
    subtext: 'Issue flow through stages · Sprint 24'
  },
  tooltip: { trigger: 'item' },
  series: [{
    type: 'sankey',
    layout: 'none',
    emphasis: { focus: 'adjacency' },
    lineStyle: { color: 'gradient', opacity: 0.3 },
    label: { color: '#e6edf3' },
    data: [
      { name: 'Backlog' }, { name: 'In Progress' },
      { name: 'Code Review' }, { name: 'Testing' }, { name: 'Done' }
    ],
    links: [
      { source: 'Backlog', target: 'In Progress', value: 80 },
      { source: 'In Progress', target: 'Code Review', value: 72 },
      { source: 'Code Review', target: 'Testing', value: 47 },
      { source: 'Code Review', target: 'In Progress', value: 25 },  // ← rework loop
      { source: 'Testing', target: 'Done', value: 42 }
    ]
  }]
};
```

## Waterfall

Sequential buildup or breakdown.

```javascript
option = {
  title: {
    text: 'Sprint scope grew 15% after planning',
    subtext: 'Story point changes · Sprint 24 · Team Alpha'
  },
  tooltip: { trigger: 'axis', axisPointer: { type: 'shadow' } },
  grid: { left: 48, right: 24, top: 60, bottom: 36 },
  xAxis: { type: 'category', data: ['Planned', 'Added', 'Removed', 'Carry-over', 'Final'] },
  yAxis: { type: 'value', name: 'SP', min: 0 },
  series: [
    { name: 'Invisible', type: 'bar', stack: 'wf', itemStyle: { color: 'transparent' }, emphasis: { itemStyle: { color: 'transparent' } }, data: [0, 40, 45, 37, 0] },
    { name: 'Increase', type: 'bar', stack: 'wf', itemStyle: { color: '#3fb950' }, data: [40, 8, 0, 9, 0] },
    { name: 'Decrease', type: 'bar', stack: 'wf', itemStyle: { color: '#f85149' }, data: [0, 0, 3, 0, 0] },
    { name: 'Total', type: 'bar', stack: 'wf', itemStyle: { color: '#58a6ff' }, data: [0, 0, 0, 0, 46] }
  ]
};
```

## Bullet Chart

Actual vs. target (replaces gauges).

```javascript
// ECharts doesn't have a native bullet type — build with stacked bar + markLine
option = {
  title: { text: 'Sprint velocity hit 95% of target' },
  grid: { left: 100, right: 40, top: 48, bottom: 24 },
  xAxis: { type: 'value', max: 60, axisLabel: { formatter: '{value} SP' } },
  yAxis: { type: 'category', data: ['Sprint 24'] },
  series: [
    // Background range bands
    { type: 'bar', barWidth: 28, z: 1, itemStyle: { color: '#21262d' }, data: [60] },
    { type: 'bar', barWidth: 28, z: 2, barGap: '-100%', itemStyle: { color: '#1c2128' }, data: [48] },
    // Actual value
    { type: 'bar', barWidth: 12, z: 3, barGap: '-100%', itemStyle: { color: '#58a6ff' }, data: [42],
      label: { show: true, position: 'right', formatter: '{c} SP' } },
    // Target marker
    { type: 'bar', barWidth: 28, z: 4, barGap: '-100%', itemStyle: { color: 'transparent' }, data: [0],
      markLine: { symbol: 'none', data: [{ xAxis: 44 }],
        lineStyle: { color: '#e6edf3', width: 2, type: 'solid' },
        label: { formatter: 'Target: 44', position: 'end' } } }
  ]
};
```

## Box Plot

Distribution comparison.

```javascript
option = {
  title: {
    text: 'Cycle time variance highest in Platform team',
    subtext: 'Distribution of issue cycle times · Q1 2026'
  },
  tooltip: { trigger: 'item' },
  grid: { left: 100, right: 24, top: 60, bottom: 36 },
  yAxis: { type: 'category', data: ['Mobile', 'Web', 'Platform', 'Infra'] },
  xAxis: { type: 'value', name: 'Days', min: 0 },
  series: [{
    type: 'boxplot',
    // data: [[min, Q1, median, Q3, max], ...]
    data: [[1, 3, 5, 8, 12], [2, 4, 6, 9, 14], [1, 5, 10, 18, 35], [2, 3, 4, 6, 10]],
    itemStyle: { color: '#1c2128', borderColor: '#58a6ff' }
  }]
};
```

## Calendar Heatmap

Daily patterns over time.

```javascript
option = {
  title: {
    text: 'Deploy frequency dropped in December',
    subtext: 'Deploys per day · 2025'
  },
  tooltip: { formatter: (p) => `${p.data[0]}: ${p.data[1]} deploys` },
  visualMap: {
    min: 0, max: 10,
    orient: 'horizontal', left: 'center', bottom: 0,
    inRange: { color: ['#0d1117', '#3fb950'] }
  },
  calendar: {
    range: '2025',
    cellSize: [14, 14],
    itemStyle: { borderColor: '#161b22', borderWidth: 2 },
    dayLabel: { color: '#8b949e' },
    monthLabel: { color: '#8b949e' },
    yearLabel: { color: '#484f58' },
    splitLine: { lineStyle: { color: '#30363d' } }
  },
  series: [{
    type: 'heatmap',
    coordinateSystem: 'calendar',
    data: [/* ['2025-01-01', 3], ['2025-01-02', 7], ... */]
  }]
};
```

## Gauge (Minimal)

Use sparingly. For single KPI with target context.

```javascript
option = {
  series: [{
    type: 'gauge',
    radius: '90%',
    min: 0, max: 100,
    splitNumber: 4,
    axisLine: {
      lineStyle: {
        width: 12,
        color: [[0.3, '#f85149'], [0.7, '#d29922'], [1, '#3fb950']]
      }
    },
    axisTick: { show: false },
    splitLine: { length: 8, lineStyle: { color: '#484f58', width: 1 } },
    axisLabel: { distance: 20, color: '#8b949e', fontSize: 11 },
    pointer: { width: 4, length: '60%', itemStyle: { color: '#e6edf3' } },
    anchor: { show: true, size: 8, itemStyle: { borderColor: '#58a6ff', borderWidth: 2 } },
    title: { offsetCenter: [0, '70%'], color: '#8b949e', fontSize: 13 },
    detail: {
      valueAnimation: true,
      formatter: '{value}%',
      offsetCenter: [0, '40%'],
      color: '#e6edf3',
      fontSize: 28,
      fontWeight: 600
    },
    data: [{ value: 82, name: 'Completion Rate' }]
  }]
};
```
