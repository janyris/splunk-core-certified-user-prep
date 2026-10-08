# 3 — Using Fields in Searches (20%) 🔥

---

## 3.1 What fields are

- **Fields** = searchable **name/value pairs** in event data (`host=www2`, `status=200`). Fields are **knowledge objects**.
- **Searching with fields is more precise and more efficient** than keywords alone. `status=503` beats a bare `503`, which also matches `area_code=503`.

### Index-time vs search-time fields

| | **Index-time (default/indexed) fields** | **Search-time fields** |
|---|---|---|
| When | Created as data is **indexed** (on the indexer) | Extracted when you **search** (on the search head) |
| Examples | **host, source, sourcetype, index**, `_time`, `_raw`, linecount, punct, splunk_server | `action`, `status`, `clientip`, `product_name`… |
| How | Automatic for every event | Automatic `key=value` discovery + sourcetype rules (`access_combined` knows the first IP is `clientip`) + custom extractions |

- **Internal fields start with `_`**: `_time`, `_raw` (the full original event). Hidden from the sidebar.
- `host` = machine the event came from · `source` = file/path/port it was read from · `sourcetype` = data format · `index` = where it's stored.
- **The search mode decides how many search-time fields you get:** Fast = only default fields + fields named in the search; Verbose = all of them. See [02-Basic-Searching](02-Basic-Searching.md).

## 3.2 The fields sidebar

| Section | What's in it |
|---|---|
| **Selected Fields** | Shown **under every event**. Defaults: **host, source, sourcetype** |
| **Interesting Fields** | Extracted fields present in **at least 20% of the events** |
| **All Fields** link | Opens a window with every field (including non-interesting ones) to select or deselect |

- The symbol left of a field: **`a` (α) = alphanumeric/string values**, **`#` = numeric values**.
- The number right of a field = **count of unique values**.
- Click an interesting field → *Selected: Yes* to move it into Selected Fields.
- **Click a field name → Field Window:**
  - top 10 values with count and %
  - **reports** that add a transforming command for you: *Top values, Top values by time, Rare values, Events with this field*
  - numeric fields also get *Average/Max/Min over time*
- Click a value → adds `field=value` to the search.
- **`index`** always appears as a field. **If no index is specified, Splunk searches all indexes your role can access by default.** Best practice: **always specify the index.**

## 3.3 Using fields in searches

### Case sensitivity (the most-tested fact in this topic)

> [!IMPORTANT]
> ****Field NAMES are case-sensitive. Field VALUES are not.****
> `Source=/var/log/syslog` ≠ `source=/var/log/syslog`, but `source=/VaR/lOg/sYsLoG` = `source=/var/log/syslog`.

### Quotes

- `product_name="Dream Crusher"` matches the **exact phrase**. Without quotes it means `Dream` AND `Crusher` anywhere in the event.
- Use quotes for values with **spaces or special characters**.

### Wildcards: best practices

| ✅ / ❌ | Pattern | Why |
|---|---|---|
| ✅ Best | `fail*` | Wildcard at the **end** of a term is the most efficient |
| ❌ | `*fail` | **Prefix/leading wildcard**: Splunk has to check every string. Slow |
| ❌ | `f*il` | **Middle** wildcard: inconsistent results with punctuation |
| ❌ | `*fail*` | Worst of both |
| ❌ | `uri_path="/cart*"` | Don't wildcard **punctuation**. List the exact values: `uri_path="/cart.do" OR uri_path="/cart/error.do"` |

- Searching for a specific term is **always** better than a wildcard: `"access denied"` > `access*`.

### Comparison operators

`=` `!=` `<` `<=` `>` `>=`, e.g. `status>=500`, `bytes>1000`. Field-value searches can also use wildcards: `status=4*`.

### `!=` vs `NOT` (classic exam trap)

| Search | Returns |
|---|---|
| `src_ip!="211.166.11.101"` | Events that **have** a `src_ip` whose value is not that IP. **Events with no `src_ip` are excluded** |
| `NOT src_ip="211.166.11.101"` | **Every** event except ones with that value, **including events that have no `src_ip` field at all** |

Both are inefficient: **inclusion is better than exclusion.**

### Displaying fields: `table` and `rename` (preview; full syntax in [04-Search-Language-Fundamentals](04-Search-Language-Fundamentals.md))

```
index=web sourcetype=access_combined action=purchase
| table clientip product_name price sale_price
| rename clientip AS "Client IP", product_name AS "Product"
```

- **`|` = pipe**: passes the output of one command into the next.
- `rename` changes the display only, **for the life of that search**. The indexed data never changes. **After renaming, use the new name** later in the search, in quotes if it has spaces.
- Table formatting: the **paintbrush** icon on a column → color by value, number format.

---

## ⏱️ 60-second self-check

1. Three default selected fields?
2. Interesting fields appear in what % of events?
3. `a` vs `#` next to a field name?
4. `status!=200` vs `NOT status=200`: which also returns events with no status field?
5. Most efficient wildcard placement?
6. Index-time vs search-time fields: name 3 index-time fields.
7. No index in the search: what gets searched?
8. After `rename`, how long does the new name exist?

<details><summary>Answers</summary>

1. host, source, sourcetype
2. At least 20%
3. `a` = alphanumeric (string) values; `#` = numeric values
4. `NOT status=200`
5. At the end of the term (`fail*`)
6. Created at indexing vs extracted when searching. Examples: host, source, sourcetype (also index, _time)
7. All indexes your role searches by default. Best practice: always specify the index.
8. Only for that search. Nothing changes in the index.

</details>
