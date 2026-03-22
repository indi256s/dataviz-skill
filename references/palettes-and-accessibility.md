# Color Palettes & Accessibility

## Primary Palette (Linear Dark Theme)

These 10 colors are registered in the ECharts theme and cycle automatically:

```
#58a6ff  Blue (primary accent)
#3fb950  Green (success)
#d29922  Yellow (warning)
#f78166  Orange (warm accent)
#bc8cff  Purple
#f778ba  Pink
#56d4dd  Teal
#e6edf3  Light gray (text-weight data)
#8b949e  Mid gray (secondary)
#7ee787  Light green
```

### Usage Rules
- **Single series:** Use `#58a6ff` (blue) by default
- **Two series comparison:** `#58a6ff` + `#8b949e` (blue vs gray)
- **Good/bad dichotomy:** `#3fb950` + `#f85149` (green + red)
- **Highlight one, mute the rest:** Color one series, push all others to `#484f58`
- **Never use more than 7 distinct colors** in one chart. Beyond that, use small multiples

## Semantic Colors

| Meaning | Color | Hex |
|---------|-------|-----|
| Success / On track | Green | #3fb950 |
| Warning / At risk | Yellow | #d29922 |
| Danger / Off track | Red | #f85149 |
| Info / Neutral | Blue | #58a6ff |
| Inactive / Muted | Dark gray | #484f58 |

These meanings are consistent everywhere. If green means "on track" in one chart, it means
"on track" in every chart. Color is a language.

## Sequential Palettes

For continuous data (light → dark of one hue):

**Blue sequential** (5 steps):
```
#0d2137 → #153d6b → #1f5fa0 → #2a82d5 → #58a6ff
```

**Green sequential** (5 steps):
```
#0d2818 → #16492c → #1f6b40 → #2d8c54 → #3fb950
```

**Neutral sequential** (5 steps):
```
#161b22 → #21262d → #30363d → #484f58 → #8b949e
```

## Diverging Palette

For data with a meaningful midpoint (positive/negative, above/below target):

```
#f85149 → #d29922 → #8b949e → #56d4dd → #3fb950
 danger    warning    neutral     cool     success
```

## Colorblind-Safe Alternative (Okabe-Ito)

When maximum accessibility is needed (public dashboards, shared reports):

```
#E69F00  Orange
#56B4E9  Sky blue
#009E73  Bluish green
#F0E442  Yellow
#0072B2  Blue
#D55E00  Vermillion
#CC79A7  Reddish purple
#000000  Black (use #e6edf3 on dark bg)
```

## Accessibility Checklist

- [ ] Colorblind-safe palette used (test with Coblis or Sim Daltonism)
- [ ] Color is NOT the only differentiator (use pattern, shape, or direct labels too)
- [ ] Minimum contrast ratio 4.5:1 for text, 3:1 for large text (WCAG AA)
- [ ] Alt text / aria-labels on all chart elements for screen readers
- [ ] Font size >= 10px everywhere, >= 12px for primary content
- [ ] Tab-navigable for keyboard-only users
- [ ] Annotations and legends use text, not just color references

## Rules

1. **Never rely on red/green distinction alone** — always pair with shape, pattern, or text
2. **Gray is your best friend** — use it to push non-essential data to the background
3. **Test every palette** against the three most common color vision deficiencies:
   - Deuteranopia (red-green, ~6% of males)
   - Protanopia (red-green, ~2% of males)
   - Tritanopia (blue-yellow, rare)
4. **Brand colors are suggestions, not commands** — if the brand has 4 blues, use a proper
   sequential palette and accent with brand color
