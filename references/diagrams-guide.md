# Diagrams & Flowcharts Guide

Three tiers of diagram capability, from simple structural to the centerpiece data-enriched
pattern. All templates use the Linear dark theme tokens from the main skill.

## Table of Contents

1. [D2 Lang Diagrams](#part-1-d2-lang-diagrams) — standalone `.d2` files → SVG/PNG
2. [CSS/HTML Diagrams](#part-2-csshtml-diagrams) — embedded in web apps
3. [Data-Enriched Diagrams](#part-3-data-enriched-diagrams) — the centerpiece: diagrams that carry live metrics

---

## Part 1: D2 Lang Diagrams

D2 compiles plain-text to SVG/PNG/PDF. Requires `d2` CLI (`brew install d2`).
Preview instantly at https://play.d2lang.com.

### Layout Engines

| Engine | Best For | Flag |
|--------|----------|------|
| `dagre` | Flowcharts, trees, sequences (default) | `--layout dagre` |
| `elk` | Dense graphs, ERDs, many nodes | `--layout elk` |
| `tala` | Architecture, system topology, nested containers | `--layout tala` |

### Linear Dark Theme Classes

Use these classes at the top of every D2 diagram to match the dataviz design system:

```d2
classes: {
  node: {
    style.fill: "#1c2128"
    style.stroke: "#30363d"
    style.font-color: "#e6edf3"
    style.border-radius: 8
  }
  accent: {
    style.fill: "#1c2128"
    style.stroke: "#58a6ff"
    style.font-color: "#e6edf3"
    style.border-radius: 8
  }
  success: {
    style.fill: "#1c2128"
    style.stroke: "#3fb950"
    style.font-color: "#e6edf3"
  }
  warning: {
    style.fill: "#1c2128"
    style.stroke: "#d29922"
    style.font-color: "#e6edf3"
  }
  danger: {
    style.fill: "#1c2128"
    style.stroke: "#f85149"
    style.font-color: "#e6edf3"
  }
  storage: {
    shape: cylinder
    style.fill: "#1c2128"
    style.stroke: "#bc8cff"
    style.font-color: "#e6edf3"
  }
}
```

### D2 Templates

#### Flowchart

```d2
direction: down

start: Start {shape: oval}
end: End {shape: oval}
check: Condition? {shape: diamond}
step_a: Process A
step_b: Process B
step_c: Process C

start -> step_a
step_a -> check
check -> step_b: Yes
check -> step_c: No {style.stroke-dash: 4}
step_b -> end
step_c -> end
```

#### Sequence Diagram

```d2
sequence_diagram

actor User
participant Frontend
participant API
participant DB

User -> Frontend: Action
Frontend -> API: POST /endpoint
API -> DB: Query
DB -> API: Result
API -> Frontend: 200 Response
Frontend -> User: Update UI
```

#### Architecture / C4

```d2
vars: {
  d2-config: {layout-engine: tala}
}
direction: right

user: User {shape: person}

app: Application {
  web: Web App
  api: API Server
  cache: Cache {shape: cylinder}
  db: Database {shape: cylinder}
  web -> api: REST
  api -> cache: Read
  api -> db: SQL
}

external: External {
  auth: Auth Provider {shape: cloud}
  cdn: CDN {shape: cloud}
}

user -> external.cdn -> app.web
app.api -> external.auth: OAuth
```

#### ERD

```d2
vars: {
  d2-config: {layout-engine: elk}
}

users: {
  shape: sql_table
  id: int {constraint: primary_key}
  email: varchar(255) {constraint: unique}
  name: varchar(100)
  created_at: timestamp
}

orders: {
  shape: sql_table
  id: int {constraint: primary_key}
  user_id: int {constraint: foreign_key}
  total: decimal(10,2)
  status: varchar(20)
}

users.id -> orders.user_id
```

#### Class Diagram

```d2
MenuItem: {
  shape: class
  name: string
  price: number
  isAvailable: boolean
  +toggle(): void
  +translate(locale: string): string
}

Dish: {
  shape: class
  categoryId: number
  dietaryTags: string[]
  +getDietaryLabels(): string[]
}

MenuItem <- Dish: extends
```

#### User Flow

```d2
direction: down

classes: {
  step: {shape: rectangle; style.fill: "#1c2128"; style.stroke: "#58a6ff"; style.font-color: "#e6edf3"}
  gate: {shape: diamond; style.fill: "#1c2128"; style.stroke: "#d29922"; style.font-color: "#e6edf3"}
  terminal: {shape: oval; style.fill: "#1c2128"; style.stroke: "#3fb950"; style.font-color: "#e6edf3"}
}

land: Scan QR Code {class: terminal}
done: Menu Browsing {class: terminal}
detect: Auto-detect Language {class: step}
view_menu: View Menu {class: step}
lang_ok: Language OK? {class: gate}

land -> detect
detect -> lang_ok
lang_ok -> view_menu: Yes
lang_ok -> view_menu: No - show globe {style.stroke-dash: 3}
view_menu -> done
```

#### Org Chart

```d2
direction: down

ceo: CEO
product: Product {
  pm: PM
  design: Designer
}
eng: Engineering {
  lead: Tech Lead
  fe: Frontend
  be: Backend
}

ceo -> product
ceo -> eng
```

#### Product Roadmap

```d2
vars: {
  d2-config: {layout-engine: elk}
}
direction: right

classes: {
  done: {style.fill: "#1c2128"; style.stroke: "#3fb950"; style.font-color: "#e6edf3"}
  active: {style.fill: "#1c2128"; style.stroke: "#58a6ff"; style.font-color: "#e6edf3"}
  planned: {style.fill: "#1c2128"; style.stroke: "#484f58"; style.font-color: "#8b949e"}
}

q1: Q1 — Foundation {
  landing: Landing Page {class: done}
  brand: Brand Identity {class: done}
}
q2: Q2 — Core Product {
  menu: Menu Viewer {class: active}
  panel: Waiter Panel {class: active}
  ai: AI Translation {class: planned}
}

q1 -> q2
```

### CLI Quick Reference

```bash
d2 diagram.d2 out.svg                    # SVG (default)
d2 --format png diagram.d2 out.png       # PNG
d2 --layout tala diagram.d2 out.svg      # Layout engine
d2 --theme 200 diagram.d2 out.svg        # Terminal dark theme
d2 --sketch diagram.d2 out.svg           # Hand-drawn style
d2 --watch diagram.d2 out.svg            # Live preview
```

---

## Part 2: CSS/HTML Diagrams

For diagrams embedded in web applications. No dependencies, fully responsive, themeable.

### Design Tokens

```css
:root {
  --dg-bg: #0d1117;
  --dg-surface: #1c2128;
  --dg-border: #30363d;
  --dg-text: #e6edf3;
  --dg-muted: #8b949e;
  --dg-dim: #484f58;
  --dg-accent: #58a6ff;
  --dg-success: #3fb950;
  --dg-warning: #d29922;
  --dg-danger: #f85149;
  --dg-purple: #bc8cff;
  --dg-radius: 8px;
  --dg-font: 'Inter', -apple-system, system-ui, sans-serif;
}
```

### Template: Vertical Process Flow

```html
<div class="flow-v">
  <div class="flow-node">
    <span class="flow-num">1</span>
    <div><div class="flow-title">Submit</div><div class="flow-desc">User fills form</div></div>
  </div>
  <div class="flow-arrow"></div>
  <div class="flow-node">
    <span class="flow-num">2</span>
    <div><div class="flow-title">Review</div><div class="flow-desc">Manager approves</div></div>
  </div>
  <div class="flow-arrow"></div>
  <div class="flow-node flow-node--ok">
    <span class="flow-num">✓</span>
    <div><div class="flow-title">Done</div><div class="flow-desc">Request processed</div></div>
  </div>
</div>
```

```css
.flow-v { display: flex; flex-direction: column; align-items: center; font-family: var(--dg-font); }
.flow-node {
  display: flex; align-items: center; gap: 12px;
  padding: 14px 18px; min-width: 260px;
  background: var(--dg-surface); border: 1px solid var(--dg-border); border-radius: var(--dg-radius);
  transition: border-color 150ms ease-out;
}
.flow-node:hover { border-color: var(--dg-accent); }
.flow-node--ok { border-color: var(--dg-success); }
.flow-num {
  width: 28px; height: 28px; border-radius: 50%;
  background: var(--dg-accent); color: #fff;
  display: grid; place-items: center; font-size: 13px; font-weight: 600; flex-shrink: 0;
}
.flow-node--ok .flow-num { background: var(--dg-success); }
.flow-title { color: var(--dg-text); font-size: 14px; font-weight: 500; }
.flow-desc { color: var(--dg-muted); font-size: 12px; margin-top: 2px; }
.flow-arrow {
  width: 2px; height: 20px; background: var(--dg-dim); position: relative;
}
.flow-arrow::after {
  content: ''; position: absolute; bottom: -4px; left: 50%; transform: translateX(-50%);
  border-left: 5px solid transparent; border-right: 5px solid transparent;
  border-top: 6px solid var(--dg-dim);
}
```

### Template: Horizontal Pipeline

```html
<div class="pipe">
  <div class="pipe-stage pipe-stage--done"><div class="pipe-dot"></div><span>Build</span></div>
  <div class="pipe-line pipe-line--done"></div>
  <div class="pipe-stage pipe-stage--done"><div class="pipe-dot"></div><span>Test</span></div>
  <div class="pipe-line pipe-line--active"></div>
  <div class="pipe-stage pipe-stage--active"><div class="pipe-dot"></div><span>Deploy</span></div>
  <div class="pipe-line"></div>
  <div class="pipe-stage"><div class="pipe-dot"></div><span>Monitor</span></div>
</div>
```

```css
.pipe { display: flex; align-items: center; font-family: var(--dg-font); }
.pipe-stage { display: flex; flex-direction: column; align-items: center; gap: 8px; }
.pipe-dot {
  width: 14px; height: 14px; border-radius: 50%;
  border: 2px solid var(--dg-border); background: var(--dg-surface);
}
.pipe-stage--done .pipe-dot { background: var(--dg-success); border-color: var(--dg-success); }
.pipe-stage--active .pipe-dot { border-color: var(--dg-accent); box-shadow: 0 0 0 4px rgba(88,166,255,.15); }
.pipe-stage span { font-size: 12px; color: var(--dg-muted); }
.pipe-stage--active span { color: var(--dg-accent); font-weight: 500; }
.pipe-stage--done span { color: var(--dg-success); }
.pipe-line { flex: 1; height: 2px; min-width: 36px; background: var(--dg-border); margin: 0 4px 28px; }
.pipe-line--done { background: var(--dg-success); }
.pipe-line--active { background: linear-gradient(90deg, var(--dg-success), var(--dg-accent)); }
```

### Template: Decision Flow (branching)

```html
<div class="decision">
  <div class="d-node d-node--start">Start</div>
  <div class="d-arrow"></div>
  <div class="d-node d-node--q">Meets criteria?</div>
  <div class="d-branch">
    <div class="d-path">
      <span class="d-label d-label--yes">Yes</span>
      <div class="d-arrow"></div>
      <div class="d-node">Approve</div>
      <div class="d-arrow"></div>
      <div class="d-node d-node--end d-node--ok">Done</div>
    </div>
    <div class="d-path">
      <span class="d-label d-label--no">No</span>
      <div class="d-arrow"></div>
      <div class="d-node">Request Changes</div>
      <div class="d-arrow"></div>
      <div class="d-node d-node--end d-node--warn">Revise</div>
    </div>
  </div>
</div>
```

```css
.decision { display: flex; flex-direction: column; align-items: center; font-family: var(--dg-font); }
.d-node {
  padding: 10px 20px; background: var(--dg-surface); border: 1px solid var(--dg-border);
  border-radius: var(--dg-radius); color: var(--dg-text); font-size: 13px; text-align: center;
}
.d-node--q { border: 2px solid var(--dg-accent); color: var(--dg-accent); font-weight: 500; border-radius: 4px; }
.d-node--start, .d-node--end { border-radius: 20px; font-size: 12px; padding: 8px 20px; }
.d-node--ok { border-color: var(--dg-success); color: var(--dg-success); }
.d-node--warn { border-color: var(--dg-warning); color: var(--dg-warning); }
.d-arrow { width: 2px; height: 18px; background: var(--dg-dim); }
.d-branch { display: flex; gap: 40px; position: relative; }
.d-branch::before {
  content: ''; position: absolute; top: 0; left: 25%; right: 25%;
  height: 2px; background: var(--dg-dim);
}
.d-path { display: flex; flex-direction: column; align-items: center; }
.d-label { font-size: 11px; font-weight: 600; padding: 2px 8px; border-radius: 4px; margin-bottom: 4px; }
.d-label--yes { color: var(--dg-success); background: rgba(63,185,80,.1); }
.d-label--no { color: var(--dg-warning); background: rgba(210,153,34,.1); }
```

---

## Part 3: Data-Enriched Diagrams

The centerpiece. Diagrams where every node carries quantitative data — turning structural
diagrams into dashboards. Uses CSS/HTML + inline ECharts sparklines.

The philosophy: when someone asks "show me the architecture," the follow-up question is
always "and how is each piece doing?" Data-enriched diagrams answer both at once.

### Template: Architecture Health Map

Each service node shows: name, tech stack, latency badge, error rate, and a request
volume sparkline. Border color shifts based on health status.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.jsdelivr.net/npm/echarts@5/dist/echarts.min.js"></script>
  <style>
    * { margin: 0; box-sizing: border-box; }
    body { background: #0d1117; font-family: 'Inter', -apple-system, system-ui, sans-serif; padding: 32px; }

    .arch-title { color: #e6edf3; font-size: 20px; font-weight: 600; margin-bottom: 4px; }
    .arch-subtitle { color: #8b949e; font-size: 13px; margin-bottom: 24px; }

    .arch-row { display: flex; align-items: flex-start; gap: 16px; }
    .arch-arrow { color: #484f58; font-size: 28px; align-self: center; margin-top: 12px; }

    .arch-group {
      background: rgba(22,27,34,.5); border: 1px dashed #30363d;
      border-radius: 12px; padding: 14px; min-width: 180px;
    }
    .arch-group-label {
      font-size: 10px; text-transform: uppercase; letter-spacing: .06em;
      color: #8b949e; font-weight: 600; margin-bottom: 10px;
    }
    .arch-cards { display: flex; flex-direction: column; gap: 8px; }

    .svc-card {
      background: #1c2128; border: 1px solid #30363d; border-radius: 8px;
      padding: 12px; transition: border-color 150ms ease-out;
    }
    .svc-card[data-health="healthy"] { border-left: 3px solid #3fb950; }
    .svc-card[data-health="degraded"] { border-left: 3px solid #d29922; }
    .svc-card[data-health="down"] { border-left: 3px solid #f85149; }

    .svc-header { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 6px; }
    .svc-name { color: #e6edf3; font-size: 13px; font-weight: 500; }
    .svc-tech { color: #484f58; font-size: 11px; }

    .svc-metrics { display: flex; gap: 12px; align-items: center; margin-bottom: 6px; }
    .svc-metric { display: flex; flex-direction: column; }
    .svc-metric-label { font-size: 10px; color: #484f58; text-transform: uppercase; letter-spacing: .04em; }
    .svc-metric-value { font-size: 14px; font-weight: 600; font-variant-numeric: tabular-nums; }
    .svc-metric-value.ok { color: #3fb950; }
    .svc-metric-value.warn { color: #d29922; }
    .svc-metric-value.bad { color: #f85149; }

    .svc-spark { width: 100%; height: 24px; }

    @media (max-width: 768px) {
      .arch-row { flex-direction: column; align-items: center; }
      .arch-arrow { transform: rotate(90deg); margin-top: 0; }
    }
  </style>
</head>
<body>
  <div class="arch-title">API latency spiked 3x on Worker after deploy</div>
  <div class="arch-subtitle">Production services — last 24h — March 22, 2026</div>

  <div class="arch-row">
    <div class="arch-group">
      <div class="arch-group-label">Frontend</div>
      <div class="arch-cards">
        <div class="svc-card" data-health="healthy">
          <div class="svc-header">
            <span class="svc-name">Web App</span>
            <span class="svc-tech">Next.js</span>
          </div>
          <div class="svc-metrics">
            <div class="svc-metric">
              <span class="svc-metric-label">P50</span>
              <span class="svc-metric-value ok">42ms</span>
            </div>
            <div class="svc-metric">
              <span class="svc-metric-label">Errors</span>
              <span class="svc-metric-value ok">0.1%</span>
            </div>
          </div>
          <div class="svc-spark" id="spark-web"></div>
        </div>
      </div>
    </div>

    <div class="arch-arrow">→</div>

    <div class="arch-group">
      <div class="arch-group-label">Backend</div>
      <div class="arch-cards">
        <div class="svc-card" data-health="degraded">
          <div class="svc-header">
            <span class="svc-name">API Worker</span>
            <span class="svc-tech">CF Workers</span>
          </div>
          <div class="svc-metrics">
            <div class="svc-metric">
              <span class="svc-metric-label">P50</span>
              <span class="svc-metric-value warn">380ms</span>
            </div>
            <div class="svc-metric">
              <span class="svc-metric-label">Errors</span>
              <span class="svc-metric-value warn">2.3%</span>
            </div>
          </div>
          <div class="svc-spark" id="spark-api"></div>
        </div>
      </div>
    </div>

    <div class="arch-arrow">→</div>

    <div class="arch-group">
      <div class="arch-group-label">Data</div>
      <div class="arch-cards">
        <div class="svc-card" data-health="healthy">
          <div class="svc-header">
            <span class="svc-name">D1 Database</span>
            <span class="svc-tech">SQLite</span>
          </div>
          <div class="svc-metrics">
            <div class="svc-metric">
              <span class="svc-metric-label">P50</span>
              <span class="svc-metric-value ok">8ms</span>
            </div>
            <div class="svc-metric">
              <span class="svc-metric-label">Queries</span>
              <span class="svc-metric-value ok">12.4k</span>
            </div>
          </div>
          <div class="svc-spark" id="spark-db"></div>
        </div>
        <div class="svc-card" data-health="healthy">
          <div class="svc-header">
            <span class="svc-name">KV Cache</span>
            <span class="svc-tech">CF KV</span>
          </div>
          <div class="svc-metrics">
            <div class="svc-metric">
              <span class="svc-metric-label">Hit Rate</span>
              <span class="svc-metric-value ok">94%</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    function spark(el, data, color) {
      const chart = echarts.init(document.getElementById(el), null, { renderer: 'svg' });
      chart.setOption({
        grid: { top: 2, right: 0, bottom: 2, left: 0 },
        xAxis: { show: false, data: data.map((_, i) => i) },
        yAxis: { show: false },
        series: [{
          type: 'line', data, smooth: true, symbol: 'none',
          lineStyle: { width: 1.5, color },
          areaStyle: { color: { type: 'linear', x: 0, y: 0, x2: 0, y2: 1,
            colorStops: [{ offset: 0, color: color + '33' }, { offset: 1, color: color + '05' }]
          }}
        }]
      });
    }
    spark('spark-web', [120,115,118,110,108,112,105,102,98,95,92,90], '#3fb950');
    spark('spark-api', [90,95,100,110,180,250,320,380,370,360,375,380], '#d29922');
    spark('spark-db', [8,9,8,7,8,9,8,8,7,8,9,8], '#3fb950');
  </script>
</body>
</html>
```

### Template: Pipeline Funnel with Metrics

Each stage shows conversion count, rate, and a comparison bar to the previous period.

```html
<div class="funnel">
  <div class="funnel-stage">
    <div class="funnel-bar" style="width: 100%"></div>
    <div class="funnel-info">
      <span class="funnel-name">Visitors</span>
      <span class="funnel-count">24,500</span>
    </div>
    <span class="funnel-rate">100%</span>
  </div>
  <div class="funnel-drop">↓ 38% drop-off</div>
  <div class="funnel-stage">
    <div class="funnel-bar" style="width: 62%"></div>
    <div class="funnel-info">
      <span class="funnel-name">Sign-ups</span>
      <span class="funnel-count">15,190</span>
    </div>
    <span class="funnel-rate">62%</span>
  </div>
  <div class="funnel-drop">↓ 44% drop-off</div>
  <div class="funnel-stage">
    <div class="funnel-bar funnel-bar--warn" style="width: 35%"></div>
    <div class="funnel-info">
      <span class="funnel-name">Activated</span>
      <span class="funnel-count">8,575</span>
    </div>
    <span class="funnel-rate funnel-rate--warn">35%</span>
  </div>
  <div class="funnel-drop">↓ 71% drop-off</div>
  <div class="funnel-stage">
    <div class="funnel-bar funnel-bar--danger" style="width: 10%"></div>
    <div class="funnel-info">
      <span class="funnel-name">Paid</span>
      <span class="funnel-count">2,450</span>
    </div>
    <span class="funnel-rate funnel-rate--danger">10%</span>
  </div>
</div>
```

```css
.funnel { font-family: var(--dg-font); max-width: 480px; }
.funnel-stage {
  display: grid; grid-template-columns: 1fr auto; gap: 8px;
  align-items: center; padding: 8px 0; position: relative;
}
.funnel-bar {
  height: 32px; background: rgba(88,166,255,.15); border-radius: 4px;
  border-left: 3px solid var(--dg-accent); grid-column: 1 / -1; grid-row: 1;
}
.funnel-bar--warn { background: rgba(210,153,34,.12); border-color: var(--dg-warning); }
.funnel-bar--danger { background: rgba(248,81,73,.10); border-color: var(--dg-danger); }
.funnel-info { grid-column: 1; grid-row: 1; padding-left: 12px; z-index: 1; }
.funnel-name { color: var(--dg-text); font-size: 13px; font-weight: 500; }
.funnel-count {
  color: var(--dg-muted); font-size: 12px; margin-left: 8px;
  font-variant-numeric: tabular-nums;
}
.funnel-rate {
  grid-column: 2; grid-row: 1; z-index: 1;
  color: var(--dg-accent); font-size: 14px; font-weight: 600;
  font-variant-numeric: tabular-nums; padding-right: 8px;
}
.funnel-rate--warn { color: var(--dg-warning); }
.funnel-rate--danger { color: var(--dg-danger); }
.funnel-drop {
  color: var(--dg-dim); font-size: 11px; text-align: center; padding: 2px 0;
}
```

### Template: Team Org with Velocity Sparklines

Each person card shows name, role, story points this sprint, and a velocity sparkline.

```html
<div class="org">
  <div class="org-card org-card--lead">
    <div class="org-name">Alex Kim</div>
    <div class="org-role">Tech Lead</div>
    <div class="org-stats">
      <span class="org-sp">34 SP</span>
      <span class="org-delta org-delta--up">▲ +12%</span>
    </div>
    <div class="org-spark" id="spark-alex"></div>
  </div>
  <div class="org-connector"></div>
  <div class="org-row">
    <div class="org-branch">
      <div class="org-card">
        <div class="org-name">Sam Lee</div>
        <div class="org-role">Frontend</div>
        <div class="org-stats">
          <span class="org-sp">21 SP</span>
          <span class="org-delta org-delta--up">▲ +8%</span>
        </div>
        <div class="org-spark" id="spark-sam"></div>
      </div>
    </div>
    <div class="org-branch">
      <div class="org-card org-card--warn">
        <div class="org-name">Jordan Patel</div>
        <div class="org-role">Backend</div>
        <div class="org-stats">
          <span class="org-sp">13 SP</span>
          <span class="org-delta org-delta--down">▼ -22%</span>
        </div>
        <div class="org-spark" id="spark-jordan"></div>
      </div>
    </div>
    <div class="org-branch">
      <div class="org-card">
        <div class="org-name">Riley Chen</div>
        <div class="org-role">Full Stack</div>
        <div class="org-stats">
          <span class="org-sp">26 SP</span>
          <span class="org-delta org-delta--up">▲ +4%</span>
        </div>
        <div class="org-spark" id="spark-riley"></div>
      </div>
    </div>
  </div>
</div>
```

```css
.org { display: flex; flex-direction: column; align-items: center; font-family: var(--dg-font); }
.org-card {
  background: var(--dg-surface); border: 1px solid var(--dg-border); border-radius: var(--dg-radius);
  padding: 12px 16px; min-width: 140px; text-align: center;
}
.org-card--lead { border-color: var(--dg-accent); }
.org-card--warn { border-color: var(--dg-warning); }
.org-name { color: var(--dg-text); font-size: 13px; font-weight: 500; }
.org-role { color: var(--dg-muted); font-size: 11px; margin-top: 2px; }
.org-stats { display: flex; justify-content: center; gap: 8px; margin-top: 6px; }
.org-sp { color: var(--dg-text); font-size: 14px; font-weight: 600; font-variant-numeric: tabular-nums; }
.org-delta { font-size: 11px; font-weight: 500; }
.org-delta--up { color: var(--dg-success); }
.org-delta--down { color: var(--dg-danger); }
.org-spark { width: 100%; height: 20px; margin-top: 6px; }
.org-connector { width: 2px; height: 20px; background: var(--dg-dim); }
.org-row {
  display: flex; gap: 16px; position: relative; padding-top: 20px;
}
.org-row::before {
  content: ''; position: absolute; top: 0; left: 15%; right: 15%;
  height: 2px; background: var(--dg-dim);
}
.org-branch {
  display: flex; flex-direction: column; align-items: center; position: relative;
}
.org-branch::before {
  content: ''; position: absolute; top: -20px; left: 50%;
  width: 2px; height: 20px; background: var(--dg-dim);
}
```

### Template: Deployment Pipeline with Status

Each environment node shows version, deploy time, and health status.

```html
<div class="deploy-pipe">
  <div class="deploy-env deploy-env--ok">
    <div class="deploy-status">●</div>
    <div class="deploy-name">Development</div>
    <div class="deploy-version">v2.4.1-rc.3</div>
    <div class="deploy-time">Deployed 2h ago</div>
    <div class="deploy-commits">+14 commits ahead</div>
  </div>
  <div class="deploy-arrow">→</div>
  <div class="deploy-env deploy-env--ok">
    <div class="deploy-status">●</div>
    <div class="deploy-name">Staging</div>
    <div class="deploy-version">v2.4.0</div>
    <div class="deploy-time">Deployed 1d ago</div>
    <div class="deploy-commits">+3 commits ahead</div>
  </div>
  <div class="deploy-arrow">→</div>
  <div class="deploy-env deploy-env--ok">
    <div class="deploy-status">●</div>
    <div class="deploy-name">Production</div>
    <div class="deploy-version">v2.3.8</div>
    <div class="deploy-time">Deployed 5d ago</div>
    <div class="deploy-commits">stable</div>
  </div>
</div>
```

```css
.deploy-pipe { display: flex; align-items: center; gap: 12px; font-family: var(--dg-font); }
.deploy-env {
  background: var(--dg-surface); border: 1px solid var(--dg-border); border-radius: var(--dg-radius);
  padding: 14px 18px; min-width: 160px; text-align: center;
}
.deploy-env--ok .deploy-status { color: var(--dg-success); }
.deploy-env--warn .deploy-status { color: var(--dg-warning); }
.deploy-env--down .deploy-status { color: var(--dg-danger); }
.deploy-status { font-size: 18px; margin-bottom: 4px; }
.deploy-name { color: var(--dg-text); font-size: 14px; font-weight: 600; }
.deploy-version { color: var(--dg-accent); font-size: 12px; font-family: monospace; margin-top: 4px; }
.deploy-time { color: var(--dg-muted); font-size: 11px; margin-top: 4px; }
.deploy-commits { color: var(--dg-dim); font-size: 11px; margin-top: 2px; }
.deploy-arrow { color: var(--dg-dim); font-size: 24px; }
@media (max-width: 640px) {
  .deploy-pipe { flex-direction: column; }
  .deploy-arrow { transform: rotate(90deg); }
}
```

---

## Diagram Anti-Patterns

| Mistake | Fix |
|---------|-----|
| Too many nodes (>20) | Split into sub-diagrams or use progressive disclosure |
| No reading direction | Set `direction: down` or `right` and stick with it |
| Rainbow node colors | Neutral for most nodes, color only encodes meaning |
| Crossed connections | Switch layout engine, reorder nodes, or split |
| Unlabeled arrows | Every connection should say what flows through it |
| Inconsistent shapes | Diamond = decision, oval = terminal, rectangle = process |
| Static when data exists | If nodes represent services/people/stages with metrics, enrich them |

## Code → Diagram Auto-Generation

Read source code and generate accurate diagrams instead of asking the user to describe things.

| Trigger | Read | Output |
|---------|------|--------|
| "diagram the database" | `**/*.schema.ts`, `**/schema.sql`, `**/migrations/*` | ERD (D2 sql_table) |
| "diagram the architecture" | `wrangler.jsonc`, `docker-compose.*`, `src/app/**` | C4 container |
| "diagram the API flow" | Grep `fetch(` / `app.(get\|post)` | Sequence diagram |
| "diagram the components" | `src/components/**/*.tsx`, parse imports | Hierarchy |

**Workflow:** Glob/Grep → Read → Map to node IDs → Generate diagram → Save to `docs/architecture/`
