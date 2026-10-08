# 2 — Basic Searching (22%) 🔥

---

## 2.1 Indexes and sourcetypes (how data is organized)

- **Index** = the repository where events are stored. At indexing, data is **broken into events, fields are parsed, timestamps and metadata are assigned**.
- Default indexes: **main** (the default destination), **_internal** (Splunk's own logs), **_audit** (audit trail). Admins create custom indexes and set retention per index.
- **sourcetype** = metadata field that identifies the **format** of the data, so Splunk knows how to parse it. Examples: `access_combined` (Apache web logs), `syslog`, `json`.
- An **event** = a single timestamped "line" (record) of data plus its metadata.

## 2.2 Running a search

- Search in the **Search & Reporting app** (the default app).
- Interface (top to bottom): **App bar** (Search, Analytics, Datasets, Reports, Alerts, Dashboards) → **search bar** → **Time Range Picker** (right of the bar) → **search mode menu** (Smart/Fast/Verbose) → *How to Search* (docs, tutorial, **Data Summary**) → *Search History*.
- **Data Summary** shows **Hosts, Sources, Sourcetypes**. It's the quickest way to see what data exists.
- **Search Assistant / autocomplete** suggests terms, fields and command syntax as you type.
- **Search History** lists your past searches so you can re-run them.

### Search syntax rules (asked a LOT)

| Rule | Example |
|---|---|
| Key/value pairs are linked with **`=`** | `action=purchase` |
| **Field names are case-sensitive; values are not** | `Source=` ≠ `source=` · `host=WWW1` = `host=www1` |
| Search **terms** (keywords) are **not** case-sensitive | `error` = `ERROR` |
| **AND is implied** between terms | `error webserver` = `error AND webserver` |
| **Boolean operators must be UPPERCASE** | `AND`, `OR`, `NOT` (lowercase "or" is a literal word) |
| Evaluation order: **( ) → NOT → OR → AND** | `a OR b c` = `(a OR b) AND c` |
| **`*` is the only wildcard**; it matches any number of characters | `fail*` → failed, failure |
| **Quotes** for phrases with spaces or special characters | `"access denied"`, `product_name="Dream Crusher"` |

> [!TIP]
> **Exam tell: "Which search matches events containing both 'error' and 'webserver'?"**
> → `index=security error webserver` (implied AND). Not the OR version, and not `"error web server"` (that's an exact phrase).

## 2.3 Time range: the #1 optimization

> [!IMPORTANT]
> **"The easiest and most effective way to optimize/speed up a search?" → **Limit (narrow) the time range.****
> Example: if the incident happened in the last hour, use **Last 60 minutes**, not the default *Last 24 hours*.

**Time Range Picker** sections: **Presets · Relative · Real-time · Date Range · Date & Time Range · Advanced**.

### Time modifiers typed into the search

- `earliest=` sets the **start**, `latest=` sets the **end**. `now` = current time.
- **Typed time modifiers override the Time Range Picker.** Picker on *Last 24 hours* + `earliest=-30m latest=now` in the search → you only get the last 30 minutes.
- If you give only `earliest`, `latest` defaults to **now**.
- **Units:** `s` sec · `m` min · `h` hour · `d` day · `w` week · `mon` month · `y` year. (`m` = **minutes**, `mon` = **months**!)
- **Relative:** `-60m` = 60 minutes ago, `-24h`, `-7d`. A `+` means the future.
- **Absolute:** `earliest=04/19/2023:00:00:00 latest=04/27/2023:00:00:00` (format `%m/%d/%Y:%H:%M:%S`).
- **Snap-to with `@`:** rounds **down** to the start of the unit.
  - `-h@h` at 15:45 → **14:00**.
  - `@d` → **midnight today**.
  - `-7d@d` → midnight 7 days ago.
  - `@w0` (or `@w7`) → Sunday, `@w1` → Monday.

| Modifier | Means |
|---|---|
| `earliest=-72h@h latest=@d` | From 72 h ago (snapped to the hour) **up to the start of today** (excludes today) |
| `earliest=-24h@h latest=@h` | Last 24 hours, whole hours only |
| `earliest=-mon@mon latest=@mon` | **The whole previous month** |
| `earliest=-1d@d latest=@d` | **Yesterday** (midnight to midnight) |
| `earliest=@d` | Today so far |
| `earliest=-30m latest=now` | Last 30 minutes |

> [!TIP]
> **Reading trick**
> Read the part **before** `@` as "go back this far", then the part **after** `@` as "then round down to the start of this unit".

## 2.4 Reading search results

**Four tabs:** **Events · Patterns · Statistics · Visualization**. The commands you use decide which tab fills.

- **Events** tab: **timeline**, display options, **fields sidebar**, **events viewer**. Events are listed **newest first (reverse chronological)**. **Matching search terms are highlighted.**
- Events viewer display options: **List** (default) · **Raw** · **Table**.
- **Patterns**: groups events with a similar structure.
- **Statistics / Visualization**: filled only by **transforming commands** (`stats`, `top`, `rare`, `chart`, `timechart`).
- Fields sidebar: **Selected fields** (default: **host, source, sourcetype**) and **Interesting fields** (in **≥ 20%** of events). Details in [03-Using-Fields](03-Using-Fields.md).

## 2.5 Refining searches

- Add more terms, use fields, comparison operators, and narrower time.
- **Drill down** from results:
  - Click **`>` / *i*** to expand an event.
  - Click a **timestamp** to search nearby events (± time).
  - Click a **field name** for top values, reports (adds a transforming command), and event counts.
  - Click a **field value** to *Add to search* / *Exclude from search* / *New search*.

### Search modes (asked a LOT)

| Mode | Priority | Field discovery | With a transforming command |
|---|---|---|---|
| **Fast** | **Speed** | **Off**: only default/indexed fields + fields you name | Statistics/Visualization only |
| **Smart** (**default**) | Balance | On for event searches | Behaves like Fast: stats/chart only, **no event list or timeline** |
| **Verbose** | **Completeness** (slowest) | On, **all fields** | Stats/chart **and** the full event list + timeline |

> [!WARNING]
> **⚠️ Common mix-up**
> Some course material says Smart mode "switches between Fast and Verbose depending on size and complexity". The accurate rule, per Splunk's docs: **Smart acts like Verbose for non-transforming searches** (events + field discovery) **and like Fast for transforming searches** (no event list).

## 2.6 The timeline

- A bar chart of **event counts over time**. Bar height = count. **Peaks = spikes in activity; valleys/gaps = possible downtime.**
- **Clicking or selecting bars filters the existing results. It does NOT run a new search.** *Deselect* undoes it.
- **Zoom in / Zoom out / Zoom to selection run a new search** with the new time range. That's the course answer and what practice quizzes expect.
  - Splunk's current docs say **Zoom out** changes the Time Range Picker (a wider search) but describe **Zoom to selection** as filtering. If an exam question hinges on it, go with the course answer.
- **Format Timeline:** Hidden / **Compact** / **Full**; **Linear** or **Log** scale. Full view: count on the Y-axis, time on the X-axis.

## 2.7 Search jobs

- **Every search creates a job** (ad hoc searches; reports, pivots and dashboard panels create jobs too).
- **Default job lifetime = 10 minutes.** It can be extended to **7 days** via **Job → Edit Job Settings**.
- **Edit Job Settings** controls **read permissions** (private/everyone), **lifetime**, and a **shareable link**.
- **Job menu:**
  - *Edit Job Settings*
  - *Send Job to Background* (keep working while it runs)
  - *Inspect Job* (Search Job Inspector: execution costs and properties)
  - *Delete Job* (you can still save the search afterwards)
- Toolbar controls: **Pause, Stop (finalize), Share, Print, Export**.
- **Activity → Jobs** = the **Jobs manager** (recent jobs; admins can manage other users' jobs).

## 2.8 Saving and exporting results

- **Save As → Report · Alert · Dashboard Panel** (also *Existing Dashboard*, *Event Type*).
- **Export** (download button): **Raw Events · CSV · XML · JSON** (raw events only for event results).

---

## ⏱️ 90-second self-check

1. Optimize a slow search: what's the first thing to do?
2. `earliest=-72h@h latest=@d`: what range is that?
3. You set *Last 24 hours* in the picker and typed `earliest=-30m`. Which wins?
4. `Status=404` vs `status=404`: same results?
5. Default search mode? Which mode gives an event list even with `stats`?
6. Selecting timeline bars vs Zoom to selection: which one re-runs the search?
7. Default job lifetime, and the maximum via Edit Job Settings?
8. Export formats?
9. What is an event? In what order are events listed?

<details><summary>Answers</summary>

1. Narrow the time range (then: specify the index, use specific terms or fields)
2. 72 hours ago, rounded down to the hour, up to midnight at the start of today
3. The typed `earliest=-30m`. Search-bar modifiers override the picker.
4. No. Field names are case-sensitive (`Status` ≠ `status`). Values aren't.
5. Smart; Verbose
6. Zoom to selection runs a new search; selecting bars only filters the current results
7. 10 minutes; 7 days
8. Raw Events, CSV, XML, JSON
9. A single timestamped record of data; reverse chronological (newest first)

</details>
