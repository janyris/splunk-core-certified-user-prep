# Reference — Defaults, Numbers & Glossary

Pure rote recall. Many exam questions are "what is the default…?"

---

## Every number and default

| Thing | Value |
|---|---|
| Search job lifetime (default) | **10 minutes** |
| Max via Edit Job Settings | **7 days** |
| Interesting field threshold | In **≥ 20%** of events |
| Default selected fields | **host, source, sourcetype** |
| `top` / `rare` rows | **10** |
| `top limit=0` | All values |
| `sort` default limit | **10,000**; `sort 0` = all |
| `head` / `tail` default | 10 |
| Triggered alert record expiration | **24 hours** |
| Default search mode | **Smart** |
| Default Boolean | **AND** |
| Wildcard | **`*`** (only one) |
| Default app | **Search & Reporting** |
| Default index | **main** (others: `_internal`, `_audit`) |
| Default Time Range Picker preset | **Last 24 hours** |
| Event order | **Reverse chronological** (newest first) |
| Default permission of new objects | **Private** |
| Splunk Web port | **8000** |
| Management port (splunkd) | **8089** |
| Forwarder → indexer receiving port | **9997** |
| Syslog | **514** |
| Certification Agreement acceptance | **3 minutes** at exam start |

## Lists to recite

| List | Items |
|---|---|
| 5 functions | Index · Search & Investigate · Add Knowledge · Monitor & Alert · Report & Analyze |
| 3 components | Forwarder → Indexer → Search Head |
| 3 deployments | Standalone · Distributed · Cloud |
| 3 roles | User · Power · Admin |
| 3 search modes | Fast · Smart · Verbose |
| 4 result tabs | Events · Patterns · Statistics · Visualization |
| 3 event display options | List · Raw · Table |
| Time Range Picker | Presets · Relative · Real-time · Date Range · Date & Time Range · Advanced |
| Export formats | Raw Events · CSV · XML · JSON |
| Job menu | Edit Job Settings · Send to Background · Inspect Job · Delete Job |
| Edit Job Settings | Read permissions · Lifetime · Shareable link |
| Lookup types | CSV · External · KV Store · Geospatial |
| Lookup setup | File → Definition → (Automatic) |
| Lookup commands | lookup · inputlookup · outputlookup |
| Alert types | Scheduled · Real-time |
| Real-time triggering | Per-result · Rolling window |
| Trigger conditions | Per-Result · # Results · # Hosts · # Sources · Custom |
| Permission levels | Private · This app · All apps |
| Get data in | Upload · Monitor · Forward |
| Index-time phases | Input → Parsing → Indexing |
| Transforming commands | stats · chart · timechart · top · rare |
| Search string parts | Search terms · Commands · Functions · Arguments · Clauses |
| Time units | s · m · h · d · w · mon · y |

## Glossary

| Term | Meaning |
|---|---|
| **Machine data** | Data generated automatically by machines, systems, devices |
| **Event** | A single timestamped record of data |
| **Index** | Repository where events are stored |
| **sourcetype** | Default field naming the data's format; drives parsing |
| **source** | File, path, or port the event came from |
| **host** | Machine the event came from |
| **_raw / _time** | Full original event text / event timestamp |
| **Field** | Searchable name=value pair |
| **Knowledge object** | User-defined thing that adds meaning: fields, tags, event types, lookups, reports, alerts, macros, data models |
| **SPL** | Search Processing Language |
| **Job** | The process created each time a search runs |
| **Bucket** | Time-based directory the indexer stores data in |
| **Splunkbase** | Splunk's app store (apps & add-ons) |
| **Transforming command** | Turns events into a statistics table (needed for charts) |
| **Lookup definition** | Named pointer to a lookup table (+ settings) that the `lookup` command uses |
| **Automatic lookup** | Lookup applied to every matching search without the `lookup` command |
| **Throttle** | Suppress repeated alert triggers for a period |
| **Snap to (`@`)** | Round a relative time down to the start of a unit |
