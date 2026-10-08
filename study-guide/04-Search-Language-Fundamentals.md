# 4 — Search Language Fundamentals (15%)

---

## 4.1 The search pipeline

```
index=web sourcetype=access_combined status=500      ← search terms (retrieve from disk)
| top user                                           ← command → intermediate results
| fields - percent                                   ← command → final results table
```

- A search = **search terms first**, then **commands separated by pipes `|`**.
- **Output of each command = input to the next.** Splunk processes **left → right**.
- The **first word after each pipe is the command**; the rest are its arguments.
- There's an implied `search` command at the start of every search.
- **Filter as early as possible.** Put the most restrictive terms (index, sourcetype, host, time) in the base search, **before the first pipe**.

### The 5 parts of a search string (a common question)

| Part | Example |
|---|---|
| **Search terms** | `index=web status=500 error` |
| **Commands** | `stats`, `table`, `sort` |
| **Functions** | `count()`, `avg()`, `dc()` |
| **Arguments** | `limit=5`, field names |
| **Clauses** | `by host`, `as total` |

### Syntax coloring and formatting (Search bar)

- **Commands: blue** · **command arguments: green** · **functions: pink/purple** · **Boolean operators and keywords like `AS`/`BY`: orange**.
- **Ctrl + `\`** (Windows) / **⌘ + `\`** (Mac) puts each pipe on its own line.

### General best practices

1. **Time** is the most efficient filter.
2. Specify **index**, then source / host / sourcetype.
3. Specific terms > wildcards; **inclusion > exclusion**.
4. Filter early; remove unneeded fields early with `fields`.

## 4.2 Basic SPL commands

### `fields`: include or exclude fields

```
| fields clientip status          ← keep only these (same as fields + …)
| fields - categoryId productId    ← remove these
| fields - _raw _time              ← remove internal fields   (| fields - _*  = all internal)
```

- No sign = **`+` (include)** by default. Separate fields with a **space or a comma**.
- `_raw` and `_time` are kept by default, even with `fields +`.
- **Performance:** `fields +` **right after the base search** cuts the data Splunk extracts (faster). Use `fields -` at the end to tidy up the output.
- ⚠️ Leave a **space after `+`/`-`**, and always list at least one field. Otherwise all fields are removed.

### `table`: show fields as columns

```
| table _time host status clientip
```

- Columns appear **in the order you list them**. Results show on the **Statistics** tab. Supports wildcards (`table host, s*`).
- `table` formats the output; `fields` controls which fields are carried through. See [11-Commonly-Confused-Pairs](11-Commonly-Confused-Pairs.md).

### `rename`: friendlier names

```
| rename clientip AS "Client IP", product_name AS Product
```

- Quotes are needed for spaces or special characters. Rename several fields in one command with commas.
- **Downstream commands must use the new name.** Display only; the index is untouched.

### `dedup`: remove duplicates

```
| dedup clientip              ← keep the first event for each clientip
| dedup 3 clientip            ← keep up to 3 events per value
| dedup host, source          ← unique combination of both
| dedup clientip sortby -clientip
```

- Keeps the **first** event found. For historical searches that's the **most recent** one.
- With `sortby` it becomes a dataset-processing command (it needs all results first).
- Avoid `dedup _raw` on large data (slow).

### `sort`: order results

```
| sort -count                 ← descending
| sort +price, -date          ← ascending price, then descending date (commas between fields)
| sort 20 -bytes              ← return at most 20 results
| sort 0 host                 ← return ALL results
```

- **`-` = descending**, **`+` or nothing = ascending**.
- **Default limit: 10,000 results.** `sort 0` = no limit.
- Strings sort **lexicographically** (uppercase before lowercase: `AND, Bee, Foo, ant, bar, cat`); numbers sort numerically.

### `eval`: calculate a field (taught in the course, lightly tested on 1001)

```
| eval total=price*quantity
| eval label="IP - ".clientip            ← "." concatenates
| eval status_msg=if(status==200,"OK","Problem")
| eval msg=case(status==200,"OK", status==404,"Not Found", status==500,"Internal Server Error")
| eval low=lower(categoryId), kb=round(bytes/1024,2)
```

- New field name → a **new field is added to the results** (not to the index). Existing name → **values overwritten for this search only**.
- Operators:
  - arithmetic `+ - * / %`
  - concatenation **`.`** (prefer it over `+`)
  - Boolean/comparison `AND OR NOT XOR < > <= >= != = == LIKE`
- **String values in "double quotes"**; field names with spaces in 'single quotes'. eval values **are case-sensitive**.
- Common functions: `if`, `case`, `lower`, `round`, `pow`, `tostring(x,"commas"|"duration"|"hex")`, `strftime`, `cidrmatch`.

## 4.3 Bonus commands that show up in answer choices

| Command | Purpose |
|---|---|
| `where` | Filter using eval-style expressions: `| where bytes > 5000` |
| `head` / `tail` | First / last N results (default **10**) |
| `rex` | Extract a field with a regex (SPLK-1002 territory) |
| `chart` / `timechart` | Transforming. See [05-Transforming-Commands](05-Transforming-Commands.md) |

---

## ⏱️ 60-second self-check

1. What character passes results from one command to the next?
2. `| fields host status` with no sign means…?
3. Where should `fields +` go for best performance?
4. Descending sort syntax? Default sort limit? How do you get all results?
5. `dedup` keeps which event?
6. After `rename clientip AS "Client IP"`, what name do later commands use?
7. What operator concatenates strings in eval?
8. Name the 5 parts of a search string.

<details><summary>Answers</summary>

1. The pipe `|`
2. `fields +` (keep only host and status, plus _raw/_time)
3. Right after the base search
4. `sort -field`; 10,000; `sort 0 field`
5. The first one found. For a normal historical search that's the most recent.
6. `"Client IP"`
7. The period `.`
8. Search terms, commands, functions, arguments, clauses

</details>
