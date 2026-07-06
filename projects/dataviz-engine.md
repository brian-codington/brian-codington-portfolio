# DataViz Engine — Open Source Charting Library

**Repository:** [View Source Code](https://github.com/brian-codington/dataviz-engine)

**Live Demo:** [View Storybook](https://storybook.dataviz-engine.dev)

**Status:** Production Ready | Actively Maintained

**npm:** `npm install dataviz-engine`

**Tech Stack:** TypeScript, D3.js, React, Rollup, Storybook

---

## Project Overview

A composable, accessible charting library for React applications built on top of D3.js. Designed for developers who need more flexibility than Chart.js but less complexity than building raw D3 visualizations from scratch.

**The Problem:** Existing React charting libraries either lock you into rigid styling, lack accessibility support, or expose too little of the underlying D3 API for custom use cases.

**The Solution:** A thin, composable layer over D3 that:
- Ships fully typed TypeScript components
- Is WCAG 2.1 AA accessible out of the box
- Supports custom themes and full style overrides
- Renders server-side (Next.js compatible)
- Weighs in at under 40KB gzipped

---

## Key Features

- **Composable API** — Mix and match chart primitives to build custom visualizations
- **TypeScript First** — Full type safety and IntelliSense support
- **Accessible** — ARIA labels, keyboard navigation, screen reader support
- **Responsive** — Auto-resizes to container width via ResizeObserver
- **SSR Compatible** — Works with Next.js and other SSR frameworks
- **Themeable** — CSS custom properties for easy white-labeling

---

## Chart Types

`LineChart` · `BarChart` · `AreaChart` · `ScatterPlot` · `PieChart` · `DonutChart` · `HeatMap` · `TreeMap` · `Histogram` · `Sparkline`

---

## Performance Metrics

| Metric | Result |
|--------|--------|
| Bundle Size | 38KB gzipped |
| GitHub Stars | 500+ |
| Weekly npm Downloads | 12,000+ |
| Lighthouse Accessibility Score | 98/100 |

---

## Code Sample

### Basic Usage

```tsx
import { LineChart, XAxis, YAxis, Tooltip, Legend } from 'dataviz-engine';

const SalesChart = ({ data }) => (
  <LineChart
    data={data}
    width="100%"
    height={300}
    margin={{ top: 20, right: 30, bottom: 40, left: 50 }}
    theme="dark"
    accessible
    ariaLabel="Monthly sales data for 2024"
  >
    <XAxis dataKey="month" label="Month" />
    <YAxis dataKey="revenue" label="Revenue ($)" format="currency" />
    <Tooltip formatter={(val) => `$${val.toLocaleString()}`} />
    <Legend />
  </LineChart>
);
```

### Custom Theme

```tsx
import { DataVizProvider, LineChart } from 'dataviz-engine';

const theme = {
  colors: ['#6366f1', '#8b5cf6', '#a78bfa'],
  fontFamily: 'Inter, sans-serif',
  fontSize: 12,
  grid: { stroke: '#374151', strokeDasharray: '4 4' },
  tooltip: { background: '#1f2937', color: '#f9fafb' }
};

const App = () => (
  <DataVizProvider theme={theme}>
    <LineChart data={data} />
  </DataVizProvider>
);
```

---

## Screenshots

![DataViz Engine Storybook showing multiple chart types with dark theme](../images/dataviz-storybook.png)

---

[← Back to Main Portfolio](../README.md)
