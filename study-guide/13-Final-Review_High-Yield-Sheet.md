# Final Review — High-Yield Sheet

The one file for the last 24 hours. If you can say every line here without looking, you're ready.

---

## 🔥 Basic Searching (22%)

- **Best optimization = narrow the time range.** Then: specify the index, use specific terms.
- `key=value`. **Field names case-sensitive, values not.** Search terms not case-sensitive.
- **AND implied.** Booleans **UPPERCASE**. Order: **( ) → NOT → OR → AND**.
- **`*`** is the only wildcard; quotes for phrases.
- **Time Range Picker**: Presets · Relative · Real-time · Date Range · Date & Time Range · Advanced.
- **Typed `earliest`/`latest` override the picker.** `@` **snaps down**. `m`=minutes, `mon`=months.
  - `earliest=-72h@h latest=@d` → 72 h ago (on the hour) up to **midnight today**
  - `-1d@d … @d` = yesterday · `-mon@mon … @mon` = last month · `-h@h` at 15:45 → 14:00
- Tabs: **Events · Patterns · Statistics · Visualization**. Events **newest first**; matching terms highlighted.
- Event views: **List · Raw · Table**.
- **Timeline:** select bars = **filter only**; **zoom = new search**. Peaks = spikes, gaps = downtime.
- **Modes:**
  - **Fast** = speed, no field discovery
  - **Smart** = **default**; events + fields for normal searches, stats only for transforming ones
  - **Verbose** = everything, slowest
- **Every search = a job.** Lifetime **10 min** → extend to **7 days** in **Edit Job Settings** (permissions, lifetime, link). Job menu: Edit settings · Send to background · Inspect · Delete.
- **Save As:** Report · Alert · Dashboard Panel. **Export:** Raw · CSV · XML · JSON.
- **Data Summary** = Hosts · Sources · Sourcetypes.

## 🔥 Using Fields (20%)

- Default **selected** fields: **host, source, sourcetype**. **Interesting** = in **≥20%** of events. **All Fields** = everything.
- **`a`** = string values, **`#`** = numeric; the number on the right = **unique value count**.
- **Index-time:** host, source, sourcetype, index, _time, _raw. **Search-time:** everything else.
- **`!=`** excludes events **missing** the field; **`NOT`** includes them. Inclusion > exclusion.
- Wildcard **at the end** (`fail*`). Never leading (`*fail`), middle, or on punctuation.
- No index given → all indexes your role can search. **Always specify the index.**
- `rename … AS "…"`: display only, lasts for the search; use the new name afterwards.

## Search Language (15%)

- **Pipe `|`**: output → input. Left → right. Filter early.
- 5 parts: **terms · commands · functions · arguments · clauses**.
- `fields` (no sign = **+**; early = faster; `_raw`/`_time` kept) · `table` (column order) · `rename` · `dedup [N] f` · `sort -desc +asc` (limit **10,000**, `0` = all).
- eval: `.` concatenates; `if()`, `case()`, `lower()`, `round()`; results are not written to the index.

## Transforming (15%)

- Transforming = **stats, chart, timechart, top, rare**. **Required for visualizations.**
- `top` / `rare`: **10 rows**, `count` + `percent`; `limit=N` (0 = all), `by`, `countfield`, `percentfield`, `showcount=false`, `showperc=false`, `useother`, `otherstr`.
- `stats`: no `by` → **1 row**; `by x` → **1 row per x**. `AS` renames. `dc` = unique count; `values` = unique sorted; `list` = all in order.
- `timechart` → **_time X-axis**; `chart count over x by y`.

## Reports & Dashboards (12%)

- **Report = saved search.** **Any** search can be one. 4 ways to create: Search · Pivot · Settings → New Report · convert an inline dashboard panel.
- Starts **Private**. Share with **App / All apps**. **Run As Owner (default) / User**. **Read** = view and run, **Write** = edit, **no box = can't see it**.
- Scheduled report: **up to 4 actions** (email, CSV lookup, webhook, log event). **Scheduling removes the time picker.** Cron or presets; Schedule Window.
- Only **scheduled** reports can be **embedded**.
- Dashboard = **panels**; create via **Save As → Dashboard Panel** (new/existing). **Classic = XML, Studio = JSON.**
- **Panel Powered By: Report** = linked, so report changes show up. **Inline Search** = a copy of the search and time picker. Changing the chart in the dashboard doesn't change the report.
- Add Panel: **New · New from Report · Clone from Dashboard · Add Prebuilt Panel**. Create New Dashboard needs a **Title** and **Classic or Studio**.
- Naming: **Group_Object_Description**. Export: **whole dashboard = PDF only**; **panel = PDF/CSV/XML/JSON**.
- Line = trend · Column/Bar = compare · Pie = parts of a whole · Single value = one number.

## Lookups (6%)

- Enrich events from an external table at **search time**; raw data unchanged.
- **CSV** (static, small) · **External** (scripted; **no outputlookup**) · **KV Store** (big or changing) · **Geospatial** (KMZ/KML).
- Setup: **file → definition → automatic (optional)**. Settings → Lookups.
- `lookup def f OUTPUT x` (enrich) · `| inputlookup` (view) · `| outputlookup` (write). **OUTPUTNEW** = only fill missing fields.
- **Automatic lookup** = no `lookup` command needed.

## Alerts & Scheduled Reports (5%)

- Scheduled **report**: action **every run**. Scheduled **alert**: action **only when triggered**.
- Alert types: **Scheduled** · **Real-time** (per-result or rolling window).
- Trigger: # results / hosts / sources / per-result / custom; **Once** or **For each result**.
- **Throttle** = suppress repeats. **Expires** = triggered-record lifetime, **24 h** default.
- Actions: **Add to Triggered Alerts** (+ Severity label), email, log event, lookup, webhook.
- **Activity → Triggered Alerts.**

## Basics (5%)

- 5 functions: **Index · Search & Investigate · Add Knowledge · Monitor & Alert · Report & Analyze**.
- **Forwarder → Indexer → Search Head.** UF = lightweight, no parsing; **HF = full instance, can filter and route**.
- Indexer = parse, timestamp, store (buckets). Search head = UI, distributes searches.
- **App** = UI/workspace; **add-on** = inputs/parsing. Splunkbase. **Built = by Splunk; Certified = reviewed by Splunk.**
- **Standalone · Distributed · Cloud.** Roles: **User · Power · Admin**.
- Ports: **8000** web · **8089** mgmt · **9997** receiving · **514** syslog. Set your **time zone**.
- Default app **Search & Reporting**; default index **main**. Data in: **Upload · Monitor · Forward**.
