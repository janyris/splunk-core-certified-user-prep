# 6 — Creating Reports and Dashboards (12%)

---

## 6.1 Reports

- **A report is a saved search (or pivot).** **Any search can be saved as a report**: *Save As → Report*.
- **4 ways to create a report:**
  1. From **Search**: save a search as a report
  2. From **Pivot**: save a pivot as a report. *Pivot* = drag-and-drop reporting on a dataset **without SPL**
  3. **Settings → Searches, reports, and alerts → New Report**
  4. From a **dashboard**: convert an **inline-search** panel to a report
- Save As Report dialog:
  - **Title**: required, unique; only `a-z A-Z 0-9 _`
  - *Description*: optional
  - *Time Range Picker*: optional. It lets users **without write permission** re-run the report over a different time range without editing it. **No picker → the report always runs over the original time range.**
  - **Content**: for a chart search there are 3 options: **Column Chart and Statistics Table · Column Chart · Statistics Table**
- After saving, you can **View** it, **Add to Dashboard**, **Continue Editing**, or change the **additional settings: Permissions · Schedule · Acceleration · Embed**.
- A report **re-runs its search each time it's opened**, so results are up to date (unless it's scheduled; then you see the latest scheduled results).
- Find reports in the app's **Reports** tab (the Reports listing page).

### Editing a report (Reports tab → Edit, or Edit menu inside the report)

| Menu item | Does |
|---|---|
| Edit Description | Change the description |
| **Edit Permissions** | **Owner**, **Display For: Owner / App / All apps**, **Run As: Owner / User**, Read/Write per role |
| **Edit Schedule** | Make it a **scheduled report** (see [08-Scheduled-Reports-and-Alerts](08-Scheduled-Reports-and-Alerts.md)) |
| Edit Acceleration | Summary acceleration (Power User topic) |
| Clone / Embed / Delete | **Embed** is only available for **scheduled** reports |
| Open in Search | Edit the actual SPL |

### Permissions (applies to all knowledge objects)

| Level | Who sees it |
|---|---|
| **Private** | Only the owner (the default for new objects) |
| **This app (Shared in App)** | Users of that app |
| **All apps (Global)** | Everyone in every app |

- **Every report starts Private**. Every report is created in the context of a specific app.
- **App** shares it with users of that app; **All apps** shares it with everyone. Choosing App or All apps reveals the **Run As** and role permission settings.
- **Run As Owner** = runs with the owner's permissions and data access. **Run As User** = runs with the **viewer's** permissions. **All reports run as Owner by default**.
  - Why use it: Owner lets viewers see data they couldn't search themselves. User helps avoid hitting **your concurrent search limit** when many people run reports you own.
- **Read** = see and run the report. **Write** = run **and edit** it. **A role with neither box checked can't see the report at all**.
- You can open Edit Permissions: when you first create the report, from the Reports listing page, from the report view, or from Settings.
- The **User** role can only create **private** objects. **Power** and **Admin** can share them.

### Editing a report's definition

- Reports listing page → **Actions** column → **Open in Search** (or **Open in Pivot**, whichever tool created it). From the report itself: **Edit → Open in Search**.
- Change the search string, time range or formatting, **re-run**, and then the **Save** button turns on. Or save it as a **new** report.

### Scheduled reports (see [08-Scheduled-Reports-and-Alerts](08-Scheduled-Reports-and-Alerts.md))

- A scheduled report runs on an interval and **can trigger an action each time it runs**.
- **Up to 4 actions:** **send a summary by email · write results to a CSV lookup file · webhook** (e.g. a chat room) **· log and index searchable events**.
- **Edit → Edit Schedule → check *Schedule Report*.**
- **Scheduling removes the time picker from the report.**
- **Schedule:** a preset (e.g. *Run every hour*) or **Run on Cron Schedule** with a cron expression.
- **Time range:** the data each run collects. It defaults to the report's time range.
- **Schedule Window** (optional): how long the scheduler may **defer** this report so higher-priority reports run first.

## 6.2 Reports that show statistics vs visualizations

- **Statistics** (tables) come from transforming commands: `stats`, `top`, `rare`, `chart`, `timechart` (`table` also fills the Statistics tab).
- **Visualizations** need a **transforming command** that produces one or more **data series**. A series is a sequence of related data points; each line in a line chart is one series.
- **Default chart types: Pie, Bar, Column, Line, Area, Scatter, Bubble.** **Other visualizations: Single Value, Radial Gauge, Cluster Map** (plus filler/marker gauges and choropleth maps).
- The chart-type picker shows a short description and a **search fragment** for each chart. The **Format** button holds options like **Stack Mode**.
- `| addcoltotals` adds a totals row that sums the numeric columns (used in the course's statistics-table example).

| Want to show… | Visualization | Typical SPL |
|---|---|---|
| A trend over time | **Line / Area** | `timechart count by host` |
| Comparing categories | **Column / Bar** | `stats count by product_name` / `chart count over x by y` |
| Parts of a whole (one series) | **Pie** | `top status` / `stats count by status` |
| One key number | **Single Value** | `stats count` |
| Value against a range or goal | **Gauge** | `stats count` |
| Locations | **Cluster / Choropleth map** | `iplocation` + `geostats` (with a geospatial lookup) |

- **Format** menu: axis titles, legend position, stacking, data labels, colors. **Trellis** = split one chart into small multiples.

## 6.3 Dashboards

- **A dashboard = a view made of one or more panels.** Each panel is powered by a **search** (inline) or a **report**.
- **Two main ways to create a dashboard:**
  1. **Directly from Search & Reporting:** *Save As → Dashboard Panel* → **New** or **Existing** dashboard → title, ID, description, permissions (**Private** / **Shared in App**), panel title, panel content.
  2. **The Dashboard Editor:** *Dashboards* tab → **Create New Dashboard**.
     - Dialog fields: **Dashboard Title (required)** · Description (optional) · Permissions · **Classic Dashboards or Dashboard Studio (required)**
     - Then **+ Add Panel**.
- **A third route from a report:** open the report → **Add to Dashboard** → the *Save As Dashboard Panel* dialog (New or Existing dashboard), with **Panel Powered By: Report or Inline Search** (below).
- **Two frameworks:** **Classic dashboards** (Simple **XML**) and **Dashboard Studio** (**JSON**; more layout and design control). the course covers Classic only.
- **Naming convention: `Group_Object_Description`**.
  - **Group** = business unit (sales, operations)
  - **Object** = object type (report, alert, dashboard)
  - **Description** = what it is
  - Example: `Sales_Dashboard_Daily_performance`

### Add Panel options

| Option | What it does |
|---|---|
| **New** | Pick a chart type → enter a **Search String**, **Time Range**, **Content Title** → *Add to Dashboard* (an inline panel) |
| **New from Report** | Pick a saved report → preview → *Add to Dashboard* |
| **Clone from Dashboard** | Copy a panel from another dashboard |
| **Add Prebuilt Panel** | Reuse a prebuilt panel |

### Panel Powered By: Report vs Inline Search

- **Report** = creates a **link** between the report and the dashboard. **Any change to the report shows up on the dashboard.**
- **Inline Search** = **copies the report's search string and time picker into the dashboard**. No link afterwards.
- Changing a report-based panel's **chart visuals** in the dashboard **does not change the original report**.

### Viewing and exporting a dashboard

- **Dashboard view options:** Edit · Export · Clone · **Set as Home Dashboard** · Delete · Refresh · Inspect · Open in Search.
- **Exporting:**
  - **Export button (top right) = the whole dashboard, PDF only.**
  - **Hover over a panel → Export = that panel's data as PDF, CSV, XML or JSON.**

### Panel types

| Panel | Behavior |
|---|---|
| **Inline search** panel | The SPL is saved **inside the dashboard**. Edit it right in the panel |
| **Report-based** panel | Built from a saved **report**. **Change the report → every dashboard that uses it updates.** You can't edit its search from the dashboard; edit the report (or convert the panel to inline / clone it) |

### Editing a dashboard (Edit mode)

- What you can change:
  - **Layout:** add, remove, **drag** to rearrange, and **resize** panels
  - **Panel settings:** search query, time range, visualization type, display options (colors, labels, legends), **refresh interval**
  - **Drilldown and interactivity:** clickable elements that open detail views, cross-panel interaction
  - **Title and description**
  - **Permissions and sharing**
- **Add Panel** (New, New from Report, Clone from Dashboard, Add Prebuilt Panel), **Add Input** (**time picker, dropdown, radio, text, checkbox, multiselect** turn it into a *form*), **drag panels** to rearrange, **change the visualization type**, edit a panel's search or time range.
- *UI* view vs *Source* view (XML/JSON).
- Panels refresh when the dashboard loads. Each panel runs a **search job**.
- Recommended naming convention for knowledge objects: **Group_Object_Description** (e.g. `SEC_Report_FailedLogins`).

---

## ⏱️ 60-second self-check

1. What is a report, technically?
2. Three permission levels for a report or dashboard?
3. Run As Owner vs Run As User?
4. What does a search need before you can visualize it?
5. Inline panel vs report-based panel: which one updates every dashboard when its source changes?
6. Two ways to start a dashboard?
7. Which visualization for "trend over the last 7 days"? For "share of each status code"?
8. Which reports can be embedded?

<details><summary>Answers</summary>

1. A saved search
2. Private, This app (shared in app), All apps (global)
3. The owner's permissions vs the viewer's permissions
4. A transforming command (stats/chart/timechart/top/rare)
5. Report-based panel
6. Save As → Dashboard Panel from search results, or Dashboards → Create New Dashboard
7. Line chart (timechart); pie chart
8. Scheduled reports only

</details>
