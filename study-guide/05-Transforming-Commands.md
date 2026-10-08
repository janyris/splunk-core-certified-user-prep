# 5 — Using Basic Transforming Commands (15%)

---

## 5.0 What "transforming" means

- A **transforming command** turns events into a **statistics table** (rows and columns of numbers).
- Transforming commands: **`stats`, `chart`, `timechart`, `top`, `rare`** (also `contingency`).
- Their results appear on the **Statistics** and **Visualization** tabs.
- **They're required to make charts** (column, bar, line, area, pie). No transforming command → no visualization.
- In Smart/Fast mode, a transforming search shows **no event list**.

## 5.1 `top`: most common values

```
| top status                       ← top 10 values, with count + percent columns
| top limit=5 status               ← same as: top 5 status
| top limit=0 status               ← ALL values
| top 3 clientip by host           ← top 3 IPs for each host
| top product_name countfield="Sold" percentfield="Share" showperc=false
| top limit=5 status useother=true otherstr="Everything else"
```

| Option | Effect |
|---|---|
| default | **10 rows**, adds **`count`** and **`percent`** columns |
| `limit=N` / `N` | Number of rows; **`limit=0` = all** |
| `by <field>` | Separate top list for each value of that field |
| `countfield=` / `percentfield=` | **Rename** the count / percent column |
| `showcount=false` / `showperc=false` | **Hide** the count / percent column |
| `useother=true` | Add an **OTHER** row for everything past the limit (default false) |
| `otherstr=` | Rename the OTHER row |

## 5.2 `rare`: least common values

**Same syntax and options as `top`**, but it returns the **least** frequent values (default 10). Good for spotting **anomalies**: unusual user agents, rare error codes, a rare login source.

## 5.3 `stats`: aggregate statistics

```
| stats count                                        ← ONE row: total events
| stats count(action)                                ← events that HAVE an action field
| stats count AS "Total Sales"                        ← rename with AS
| stats count, dc(clientip) AS "Unique IPs"          ← several functions, comma-separated
| stats count by host                                ← one row per host
| stats count by host, user                          ← one row per host+user combination
| stats sum(bytes) AS total by usage | sort -total
| stats avg(bytes) AS avg, min(bytes), max(bytes) by usage | eval avg=round(avg,2)
| stats values(clientip) list(action) by user
```

| Function | Returns |
|---|---|
| `count` / `count(field)` | Number of events / number of events **that contain** that field |
| **`dc(field)`** = `distinct_count` | Number of **unique** values |
| `sum`, `avg`, `min`, `max`, `median`, `mode`, `stdev`, `range` | Math. **Numeric fields only** (you can't sum host names) |
| **`values(field)`** | The **unique** values, **sorted** |
| **`list(field)`** | **All** values (duplicates included), **in event order** |
| `earliest`, `latest`, `first`, `last` | Time-based or order-based values |

- **No `by` clause → exactly one row.** **`by` clause → one row per unique value** (or combination of values).
- **`AS`** (2 letters) renames the output column.
- **Filter BEFORE `stats`**: `index=web action=purchase | stats count` is better than counting everything and then filtering.

## 5.4 `chart` and `timechart` (visualization makers, see [06-Reports-and-Dashboards](06-Reports-and-Dashboards.md))

```
| timechart count by host                 ← X-axis is ALWAYS _time
| timechart span=1h count                 ← bucket size
| chart count over product_name by host   ← X = product_name, one series per host
| chart avg(price) by categoryId
```

- **`timechart`: X-axis = `_time`, always.** Use it for "over time" or "trend".
- **`chart`: any field on the X-axis** (`over` = X-axis, `by` = split series).

## 5.5 Choosing the right command

| The question says… | Pick |
|---|---|
| "most common / most frequent / top N" | `top` |
| "least common / unusual / anomalies / rare" | `rare` |
| "count, total, sum, average, min/max, per X" | `stats … by X` |
| "number of unique / distinct users" | `stats dc(user)` |
| "list the unique values" | `stats values(field)` |
| "over time / trend / per hour" | `timechart` |
| "count of X split by Y as a chart" | `chart count over X by Y` |
| "count per IP, highest first" | `stats count by src_ip \| sort -count` |
| "just show these columns" (no math) | `table` |

---

## ⏱️ 60-second self-check

1. How many rows does `top` return by default? Which columns does it add?
2. Keyword to change how many rows `rare` returns? To hide the percent column?
3. `stats dc(clientip)` vs `stats count(clientip)`?
4. Why use a `by` clause with `stats`?
5. What 2-letter keyword renames a stats output?
6. `values()` vs `list()`?
7. Which command always puts `_time` on the X-axis?
8. Why must a search have a transforming command before you can chart it?

<details><summary>Answers</summary>

1. 10; `count` and `percent`
2. `limit=` (or just the number); `showperc=false`
3. Unique IPs vs events that have a clientip value
4. To get one row (one group) per value of that field instead of one overall row
5. `AS`
6. `values` = unique and sorted; `list` = all values in event order, duplicates included
7. `timechart`
8. Charts need a statistics table (rows/columns of numbers). Only transforming commands produce one.

</details>
