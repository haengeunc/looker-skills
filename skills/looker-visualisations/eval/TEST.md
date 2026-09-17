# Looker Visualisations Skill — Agent E2E Test Plan

## Prerequisites

**Read `skills/looker-visualisations/SKILL.md` and sub-skills first** to understand 2026 design standards, 72-col granular layout geometry (`width: 36`, `height: 14`), mandatory numeric compaction, explicit value labels, decluttered y-axes (`y_axes: [{showValues: false}]`), null suppression, and declarative formatters.

---

## Test 1: Modern Framework & Executive Overview Dashboard

**Prompt:** "Create a modern 2026 LookML dashboard definition for executive sales overview under `dashboards/sales_overview.dashboard.lookml`. It needs an executive summary banner, a navigation button linking to `/dashboards/deep_dive`, global filters for year, region, and category, and a net revenue column chart with value labels and active filter bindings."

**Verify:**
- Dashboard specifies `preferred_viewer: dashboards-next`, `style: modern`, `layout: newspaper`, and `layout_granularity: granular` (72-column grid).
- Dashboard defines global filters with `field_filter` and connects data tiles using explicit `listen:` blocks.
- Data tile specifies `modern2026: true`, default half-width `width: 36`, standard `height: 14`, `show_value_labels: true`, and declutters the y-axis with `y_axes: [{showValues: false}]`.
- Incorporates a clean HTML text banner and a native Looker navigation button (`type: button`).
- Adheres to maximum 5 tabs recommendation.

---

## Test 2: Sparkline KPI & Hierarchical Multi-Level Table

**Prompt:** "We need an executive KPI and performance breakdown section for our dashboard. Author LookML elements for a headline gross revenue KPI with a 30-day sparkline and dark-mode Liquid tooltip, followed by a multi-level performance table with collapsible row groups for department, category, and brand."

**Verify:**
- KPI element uses `type: single_value`, `sparkline: true`, and `show_single_value_title: true`.
- KPI embeds `global_tooltip_options` with custom dark-mode styling and Liquid template variables (`{{ ... }}`).
- Table element uses `type: looker_grid`, enables `row_groups: true` with `collapsible: true`, and includes `bars_in_table: true`.
- Both elements conform to default half-width `width: 36` and standard `height: 14`.

---

## Test 3: YoY Combo Chart & Status Waterfall

**Prompt:** "Author Cartesian visualization elements in LookML: 1) A YoY order count combo chart plotting current year and prior year bars with a difference line, ensuring 0 and null values are completely suppressed from plotting; 2) A revenue contribution waterfall chart broken down by fulfillment status."

**Verify:**
- Combo chart uses `type: looker_line` with `series_types` specifying columns for counts and a line for difference.
- Combo chart suppresses 0 and null values using `show_null_points: false` and `show_null_labels: false`.
- Waterfall chart uses `type: looker_waterfall` with fields `[order_items.status, order_items.total_sale_price]` and explicit `up_color`, `down_color`, and `total_color`.
- Visual elements enforce explicit value formatting and default half-width `width: 36` and `height: 14`.

---

## Test 4: Spatial Bullet Targets & Sankey Flows

**Prompt:** "Our operations team needs specialized visualizations: 1) A bullet gauge chart showing regional sales actuals against 80% and 100% quota targets; 2) A customer journey Sankey flow tracking progression from traffic source through device and conversion."

**Verify:**
- Bullet chart uses `type: looker_bullet` with `ranges` defining target bands and `marker_color` for quota thresholds.
- Sankey diagram specifies `type: looker_sankey`, enables `flow_gradient: true`, and caps queries with `limit: 50`.
- Both elements specify `modern2026: true` and conform to standard 72-col grid geometry.

---

## Test 5: Highcharts Declarative Formatters & Reference Line

**Prompt:** "Create a LookML column chart visualization for category revenue that dynamically highlights bars exceeding the average revenue in green and bars below average in grey, accompanied by an aligned horizontal mean reference line."

**Verify:**
- Element defines a `reference_lines` block with `reference_type: line` and `line_value: mean`.
- Element configures `advanced_chart_config` with declarative `formatters`.
- Formatter rule tests `condition: 'value >= mean'` and styles fill color and data labels.
- Cascading rules correctly order general statistical rules before specific threshold overrides.
