# 7 — Creating and Using Lookups (6%)

---

## 7.1 What a lookup is

- A lookup **enriches events with fields from an external table** (data that isn't in the index). Example: `productId=WC-SH-G04` → add `product_name`, `price`.
- It matches on a **shared field**: at least one field that exists **both in the events and in the lookup table**.
- Applied at **search time**. **Raw indexed events are never changed.**

### Four lookup types

| Type | Data source | Notes |
|---|---|---|
| **CSV** (file-based) | A `.csv` file | **"Static lookup."** Best for **small, relatively static** data |
| **External** | Python script or binary (e.g. a DNS lookup) | **"Scripted lookup."** ⚠️ **Can't be written to with `outputlookup`** |
| **KV Store** | A KV Store collection | **Large** tables or tables that are **updated often** |
| **Geospatial** | **KMZ / KML** file | Maps coordinates to regions (country, state) → **choropleth maps** |

- **All** lookup types need a **lookup definition**. Only **CSV and geospatial** need an **uploaded table file**.
- One table file can be used by **several** lookup definitions.

### CSV file rules

- At least **2 columns**, one of which matches a field in your events.
- Plain ASCII / **UTF-8** only. No `\r` (old Mac) line endings. Header row ≤ 4096 characters.
- **Downside of CSV lookups:** they're static and slow down when very large → use KV Store for big or frequently changing data.

## 7.2 Creating a lookup (Settings → Lookups): 3 steps

1. **Lookup table files → + Add new**: choose the destination app (e.g. *search*), upload the CSV, set the destination filename (`http_status.csv`). Your role needs the **`upload_lookup_files`** capability.
   → Then **Permissions**: e.g. *This app only*, Everyone: Read.
2. **Lookup definitions → + Add new**: app, **name** (`http_status`), **Type: File-based**, pick the lookup file. (Optional advanced settings: matching rules, case sensitivity, min/max matches.)
3. *(Optional)* **Automatic lookups → + Add new**: name, lookup table (definition), **apply to** a host / source / **sourcetype** (e.g. `access_combined`), **input fields** (`status = status`), **output fields** (`status_description`, `status_type`).

> [!TIP]
> **Order matters: **file → definition → (automatic)**. The definition is what the `lookup` command uses.**

## 7.3 Lookup commands

```
| inputlookup http_status.csv                         ← READ / view the lookup table itself
| inputlookup products.csv | where price - sale_price > 5
index=web | lookup http_status status OUTPUT status_description, status_type
index=web | lookup http_status status AS http_code OUTPUTNEW status_description
... | outputlookup my_table.csv                        ← WRITE search results to a lookup
```

| Command | Purpose |
|---|---|
| **`lookup`** | **Add fields** from the lookup to your **events** (enrich). Uses the **definition** name |
| **`inputlookup`** | **Search/view the contents** of a lookup table (CSV or KV Store). Starts with a pipe |
| **`outputlookup`** | **Write** search results **to** a CSV lookup or KV Store. ⚠️ Not for external lookups |

- **`OUTPUT`** overwrites existing field values. **`OUTPUTNEW`** only fills fields that don't already exist.
- Use **`AS`** when the event field name differs from the lookup column name.
- Lookup output fields appear in the fields sidebar like any other field.

## 7.4 Automatic lookups

- Applied **to every search** (for the chosen sourcetype/source/host) **without typing the `lookup` command**.
- Configured under **Settings → Lookups → Automatic lookups**.

---

## ⏱️ 60-second self-check

1. What does a lookup do, and does it change indexed data?
2. Four lookup types? Which is "static", which is "scripted"?
3. Which command views a lookup table's contents? Which writes to one? Which enriches events?
4. The 3 setup steps in Settings → Lookups?
5. What does an automatic lookup give you?
6. Which lookup type can't be used with `outputlookup`?
7. OUTPUT vs OUTPUTNEW?
8. Downside of CSV lookups / when to use KV Store?

<details><summary>Answers</summary>

1. Adds fields from an external table to events at search time. No, the index is never changed.
2. CSV, External, KV Store, Geospatial. CSV = static; External = scripted.
3. `inputlookup`; `outputlookup`; `lookup`
4. Upload the lookup table file → create the lookup definition → (optional) create an automatic lookup
5. The lookup runs on every matching search without typing the `lookup` command
6. External
7. OUTPUT overwrites existing values; OUTPUTNEW only adds fields that don't exist yet
8. Static and best for small data → use KV Store for large or frequently updated tables

</details>
