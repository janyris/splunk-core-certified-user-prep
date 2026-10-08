# Commonly Confused Pairs

This is where wrong answer choices come from. For each pair, say the **one-word difference** out loud.

---

| # | A | B | The difference |
|---|---|---|---|
| 1 | **Field name** | **Field value** | Names **are** case-sensitive; values **aren't** |
| 2 | **`!=`** | **`NOT`** | `!=` drops events **without** the field; `NOT` **keeps** them |
| 3 | **Fast** | **Verbose** | Speed (no field discovery) vs completeness (all fields + events). **Smart** = default middle |
| 4 | **Smart + transforming** | **Verbose + transforming** | Smart: stats only (no event list). Verbose: stats **and** events |
| 5 | **Selected fields** | **Interesting fields** | Shown under every event (host/source/sourcetype) vs present in ≥20% of events |
| 6 | **Index-time fields** | **Search-time fields** | Created at indexing (host, source, sourcetype) vs extracted when searching |
| 7 | **Select timeline bars** | **Zoom to selection / Zoom out** | Filters the current results vs **runs a new search** (course answer; current docs say Zoom out changes the Time Range Picker) |
| 8 | **Time Range Picker** | **`earliest`/`latest` in search** | The typed modifiers **override** the picker |
| 9 | **`m`** | **`mon`** | Minutes vs months |
| 10 | **`-7d`** | **`-7d@d`** | Exactly 168 h ago vs midnight 7 days ago |
| 11 | **`fields`** | **`table`** | Keeps/removes fields (performance) vs formats columns for display |
| 12 | **`fields +`** | **`fields -`** | Keep only these vs remove these. **No sign = `+`** |
| 13 | **`rename`** | **`eval`** | Changes a field's **name** vs calculates a field's **value** |
| 14 | **`dedup`** | **`stats dc()`** | Removes duplicate **events** vs **counts** unique values |
| 15 | **`top`** | **`rare`** | Most common vs least common (both 10 rows, count + percent) |
| 16 | **`stats count`** | **`stats count(field)`** | All events vs only events that have that field |
| 17 | **`count`** | **`dc`** | Total vs unique |
| 18 | **`values()`** | **`list()`** | Unique and sorted vs all, in order, with duplicates |
| 19 | **`stats … by x`** | **`stats` alone** | One row per x vs one row total |
| 20 | **`chart`** | **`timechart`** | Any field on the X-axis vs `_time` on the X-axis |
| 21 | **`sort -f`** | **`sort +f` / `sort f`** | Descending vs ascending |
| 22 | **`lookup`** | **`inputlookup`** | Enriches **events** vs **views the table itself** |
| 23 | **`inputlookup`** | **`outputlookup`** | Read a lookup vs write to one |
| 24 | **`OUTPUT`** | **`OUTPUTNEW`** | Overwrites existing values vs only fills missing fields |
| 25 | **Lookup table file** | **Lookup definition** | The uploaded CSV vs the named config that points to it (`lookup` uses the **definition**) |
| 26 | **Manual lookup** | **Automatic lookup** | Needs the `lookup` command vs applied to every search |
| 27 | **CSV lookup** | **KV Store lookup** | Small, static data vs large or frequently updated data |
| 28 | **External lookup** | **CSV lookup** | Scripted (Python/binary), can't use `outputlookup` vs static file |
| 29 | **Scheduled report** | **Scheduled alert** | Action runs **every time** vs only **when triggered** |
| 30 | **Scheduled alert** | **Real-time alert** | Runs on a schedule (lighter) vs runs continuously (heavier) |
| 31 | **Per-result** | **Rolling window** | Every matching event vs N events within a time window |
| 32 | **Trigger: Once** | **Trigger: For each result** | One action per run vs one action per result |
| 33 | **Throttle** | **Expires** | Suppress re-triggering vs how long triggered-alert records are kept (24 h) |
| 34 | **Job lifetime (10 min)** | **Alert expiration (24 h)** | Search results kept vs triggered-alert listing kept |
| 35 | **Inline panel** | **Report-based panel** | Search lives in the dashboard vs edit the report and all dashboards update |
| 36 | **Run As Owner** | **Run As User** | Owner's permissions vs viewer's permissions |
| 37 | **Private** | **This app / All apps** | Only you vs shared in the app vs everywhere |
| 38 | **Universal forwarder** | **Heavy forwarder** | Lightweight, no parsing vs full instance that can parse, filter, route |
| 39 | **Indexer** | **Search head** | Stores, parses, timestamps vs the UI that distributes searches |
| 40 | **App** | **Add-on** | Workspace/UI for a use case vs input/parsing component, usually no UI |
| 41 | **Splunk Built** | **Splunk Certified** | Made by Splunk vs a third-party app reviewed by Splunk |
| 42 | **Standalone** | **Distributed** | One instance does it all vs roles split across instances |
| 43 | **User role** | **Power role** | Private objects only vs can share objects and create alerts |
| 44 | **Upload** | **Monitor** | A one-time file vs continuous watching of files/ports |
| 45 | **Patterns tab** | **Statistics tab** | Groups similar events vs transforming-command output |
| 46 | **`*fail`** | **`fail*`** | Slow (leading wildcard) vs efficient (trailing wildcard) |
| 47 | **`"a b"`** | **`a b`** | Exact phrase vs a AND b anywhere |
