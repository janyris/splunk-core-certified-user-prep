# Reference — SPL Commands

---

## Search-bar building blocks

| Syntax | Meaning |
|---|---|
| `index=web sourcetype=access_combined` | Always start with the index (+ sourcetype/host/source) |
| `field=value` | Key/value pair (**name case-sensitive, value not**) |
| `a b` | `a AND b` (implied AND) |
| `a OR b`, `NOT a`, `(a OR b) c` | Uppercase Booleans; order **( ) → NOT → OR → AND** |
| `"exact phrase"` | Exact phrase / values with spaces |
| `fail*` | Wildcard (put it at the **end**) |
| `status>=500`, `bytes<1000`, `status!=200` | Comparison operators |
| `earliest=-24h@h latest=now` | Time modifiers (override the picker) |
| `\|` | Pipe: output → input of the next command |

## Commands

| Command | Syntax | Key facts |
|---|---|---|
| **fields** | `fields + a b` / `fields - a b` | Default `+`. Use early for **performance**. `_raw`, `_time` kept unless removed (`fields - _*`) |
| **table** | `table _time host status` | Columns in the listed order; wildcards OK; Statistics tab |
| **rename** | `rename a AS "New A", b AS B` | Quotes for spaces; use the new name afterwards; display only |
| **dedup** | `dedup [N] f1, f2 [sortby -f]` | Keeps the first (most recent) event per value; `N` = keep N per value |
| **sort** | `sort [N] -f1, +f2` | `-` desc, `+`/none asc; default limit **10,000**; `sort 0` = all |
| **eval** | `eval new=expr, x=if(c,a,b)` | `.` concatenates; new or overwritten field for this search only |
| **where** | `where bytes > 5000` | eval-style filter after a pipe |
| **head / tail** | `head 20` | First / last N (default 10) |
| **top** | `top [limit=N] f [by g]` | Default **10** rows + `count`, `percent`; `limit=0` all; `countfield`, `percentfield`, `showcount`, `showperc`, `useother`, `otherstr` |
| **rare** | `rare [limit=N] f [by g]` | Same as top, least common |
| **stats** | `stats count, dc(f) AS x by g` | No `by` → 1 row; `by` → 1 row per value |
| **chart** | `chart count over x by y` | Any X-axis |
| **timechart** | `timechart span=1h count by host` | X-axis always `_time` |
| **lookup** | `lookup def field [AS evfield] OUTPUT f1, f2` | Enrich events; `OUTPUTNEW` = only fill missing fields |
| **inputlookup** | `\| inputlookup file.csv` | Read/view a lookup table (starts with a pipe) |
| **outputlookup** | `\| outputlookup file.csv` | Write results to a lookup (not external lookups) |

## stats functions

| Function | Returns |
|---|---|
| `count` / `count(f)` | All events / events that have f |
| `dc(f)` / `distinct_count(f)` | Number of unique values |
| `sum avg min max median mode stdev range` | Math on numeric fields |
| `values(f)` | Unique values, sorted |
| `list(f)` | All values, in order, with duplicates |
| `first last earliest latest` | By order / by time |

## Patterns worth typing 3× each

```
index=web sourcetype=access_combined status>=400 earliest=-24h
index=security (fail* OR invalid) NOT user=admin
index=web | stats count by status | sort -count
index=web action=purchase | stats count AS Sales, dc(clientip) AS "Unique Buyers" by product_name
index=web | top limit=5 product_name showperc=false
index=web | rare useragent
index=web | timechart span=1h count by host
index=web | table _time clientip action status | rename clientip AS "Client IP"
index=web | dedup clientip | table clientip
index=web | lookup http_status status OUTPUT status_description
| inputlookup products.csv
```
