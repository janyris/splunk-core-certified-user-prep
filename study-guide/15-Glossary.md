# SPLK-1001 Glossary

106 terms grouped by exam topic. ⭐ = high-yield. Back to 00-README.

## 1 · Splunk Basics

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Machine data** | Data generated automatically by machines, systems and devices (server, network, application and security logs, sensor and IoT data). Often high-volume and hard for humans to read raw. | Splunk can index structured AND unstructured machine data. |
| **Splunk** | A platform that collects, indexes, searches, analyzes and visualizes machine data in real time. | 5 functions: Index · Search & Investigate · Add Knowledge · Monitor & Alert · Report & Analyze. |
| ⭐ **Forwarder** | Component that runs on the source machine, collects data and sends it to the indexers. | Pipeline: Forwarder → Indexer → Search Head. |
| ⭐ **Universal forwarder** | Lightweight agent that only collects and forwards data. It doesn't parse it. Splunk recommends it for sending data. | Lightweight, no parsing. |
| ⭐ **Heavy forwarder** | A full Splunk Enterprise instance that can parse, filter, mask and route data before forwarding it. | Filter/mask BEFORE indexing → heavy forwarder. |
| ⭐ **Indexer** | Component that receives data, breaks it into events, timestamps it, and stores it in indexes (time-based buckets). It also searches its own data. | Stores + parses + timestamps. License usage is metered here. |
| ⭐ **Search head** | The web interface where users write searches. It sends searches to the indexers, merges their results, and hosts reports, dashboards and alerts. | UI + distributes searches + merges (reduces) results. |
| **Bucket** | A time-based directory in which an indexer stores events and index files. | Narrow time range → fewer buckets searched → faster. |
| **Deployment server** | Splunk instance that pushes configurations and apps to forwarders in larger deployments. | Exists in big deployments; barely tested on SPLK-1001. |
| ⭐ **Standalone deployment** | A single Splunk instance that does every function: input, indexing, search and visualization. | 1 box = standalone. |
| ⭐ **Distributed deployment** | Multiple Splunk instances, each responsible for one or more roles (forwarding, indexing, searching). | Roles split across instances; scales. |
| **Splunk Cloud** | A deployment hosted and managed by a third party (Splunk on AWS/Azure) rather than your own infrastructure. | Like distributed, but hosted. |
| ⭐ **App** | A packaged collection of configuration files, knowledge objects and UI (dashboards, reports) for a use case. | Default app = Search & Reporting. Apps live in $SPLUNK_HOME/etc/apps. |
| ⭐ **Add-on** | A reusable component that supplies data inputs, parsing or field extractions for a specific source. Usually has no UI. | App = workspace/UI; add-on = inputs/parsing. |
| **Splunkbase** | Splunk's online store for downloading apps and add-ons (most free, some premium). | splunkbase.com, or 'Find More Apps' inside Splunk Web. |
| ⭐ **Search & Reporting** | The default Splunk app, where you search, create reports and build dashboards. | Apps > Search & Reporting. |
| ⭐ **Roles** | Collections of capabilities assigned to users that determine what they can do and which data they can access. The 3 main ones are User, Power and Admin. | Power = can share objects + create alerts. User = private only. |
| ⭐ **Port 8000** | Default port for Splunk Web (browser access). | 8089 = management (splunkd), 9997 = forwarder receiving, 514 = syslog. |

## 2 · Basic Searching

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Index** | A repository where Splunk stores ingested, parsed events. Defaults include main, _internal and _audit. | Always specify index= in searches. |
| ⭐ **Event** | A single timestamped record of data plus its metadata. | Shown newest first (reverse chronological). |
| ⭐ **Sourcetype** | Default field that identifies the format of the data, telling Splunk how to parse it (e.g. access_combined, syslog). | 'What Splunk uses to categorize the data being indexed.' |
| ⭐ **SPL** | Search Processing Language, Splunk's query language of search terms and piped commands. | Parts: search terms, commands, functions, arguments, clauses. |
| ⭐ **Time Range Picker** | UI control next to the search bar used to set a search's time range: Presets, Relative, Real-time, Date Range, Date & Time Range, Advanced. | Typed earliest/latest override it. |
| ⭐ **earliest / latest** | Time modifiers typed into a search that set the start and end of its time range, e.g. earliest=-24h latest=now. | If you give only earliest, latest = now. |
| ⭐ **Snap to (@)** | The @ symbol in a time modifier rounds the time DOWN to the start of a unit, e.g. -1d@d = midnight yesterday. | Offset first, then round down. |
| **Real-time search** | A search over a rolling time window that keeps updating as new events arrive. | Relative = fixed window in the past; real-time = rolling window. |
| ⭐ **Timeline** | Bar chart above the events showing event counts over time. Peaks are spikes in activity; gaps may be downtime. | Select bars = filter only; Zoom = new search. |
| **Zoom to selection** | Timeline action that narrows the time range to the selected bars and re-runs the search. | Zoom (in/out/to selection) re-executes; selecting doesn't. |
| ⭐ **Fast mode** | Search mode that prioritizes speed. It turns off field discovery and returns only default fields and fields named in the search. | Speed over completeness. |
| ⭐ **Smart mode** | The default search mode. Like Verbose for event searches (fields + events); like Fast for transforming searches (stats only). | DEFAULT mode. |
| ⭐ **Verbose mode** | Search mode that returns all fields and the full event list, even for transforming searches. The slowest mode. | Completeness over speed. |
| ⭐ **Search job** | The process created every time a search runs. It tracks owner, app, events returned and run time. Kept 10 minutes by default. | 10 min default → 7 days via Edit Job Settings. |
| ⭐ **Edit Job Settings** | Job menu dialog for changing a job's read permissions, extending its lifetime (up to 7 days), and getting a shareable link. | Permissions · Lifetime · Link. |
| **Search Job Inspector** | Window (Job → Inspect Job) showing a job's execution costs and properties. Use it to troubleshoot slow searches. | 'Why was it slow?' |
| **Jobs page** | Activity → Jobs: a list of recent search jobs. Admins can manage other users' jobs there. | Shows the same results as the original run. |
| ⭐ **Data Summary** | Button under 'How to Search' that shows the Hosts, Sources and Sourcetypes in your deployment. | Quickest way to see what data exists. |
| **Search Assistant** | Feature that suggests terms, fields and command syntax while you type in the search bar. | Enabled by default (compact). |
| **Patterns tab** | Results tab that groups returned events with a similar structure. | Tabs: Events · Patterns · Statistics · Visualization. |
| ⭐ **Implied AND** | Splunk puts AND between search terms when no Boolean operator is given. | Booleans must be UPPERCASE. Order: ( ) NOT OR AND. |
| ⭐ **Wildcard** | The asterisk (*), which matches any number of characters. Most efficient at the END of a term. | fail* good; *fail slow. |

## 3 · Using Fields

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Field** | A searchable name/value pair in event data, e.g. status=200. It's a knowledge object, and searching with one is more precise than a keyword. | Field searches are more precise than keywords. |
| ⭐ **Default fields** | Fields Splunk adds to every event at index time: host, source, sourcetype, index, plus internal _time and _raw. | host = machine, source = file/path/port, sourcetype = format. |
| **host** | Default field naming the machine (by name or IP) an event came from. | One of the 3 default Selected Fields. |
| **source** | Default field naming the file, path or network port an event was read from. | One of the 3 default Selected Fields. |
| ⭐ **_raw** | Internal field holding the full original text of an event. | Internal fields start with _ and are hidden from the sidebar. |
| ⭐ **_time** | Internal field holding an event's timestamp, assigned at index time. | timechart always uses _time on the x-axis. |
| **Field discovery** | Automatic extraction of fields at search time based on key=value patterns and sourcetype rules. | Off in Fast mode. |
| **Search-time field extraction** | Fields extracted when a search runs (on the search head), e.g. action, status, clientip. | Index-time = host/source/sourcetype; search-time = the rest. |
| ⭐ **Selected Fields** | Fields shown under every event in the results. Defaults: host, source, sourcetype. | Make an interesting field selected: click it → Selected: Yes. |
| ⭐ **Interesting Fields** | Extracted fields that appear in at least 20% of the returned events. | ≥ 20%. |
| ⭐ **All Fields** | Sidebar link that opens a window with every field, including ones below 20%, to select or deselect. | Missing field? → All Fields. |
| ⭐ **!= vs NOT** | field!=x returns only events that HAVE the field with another value; NOT field=x also returns events without the field. | NOT is broader. |

## 4 · Search Language

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Pipe** | The \| character. It passes the results of one command to the next command in the search. | Left → right. |
| **Search pipeline** | The chain of search terms and piped commands, where each command's output is the next one's input. | Filter as early as possible. |
| **Clause** | Part of a command such as BY (group results) or AS (rename an output). | stats count AS total BY host. |
| ⭐ **fields** | Command that keeps (+, the default) or removes (-) the listed fields from results. Used right after the base search, it improves performance. | No sign = +. _raw/_time kept unless removed. |
| ⭐ **table** | Command that shows the listed fields as columns, in the order given. | Format/order columns → table. |
| ⭐ **rename** | Command that gives a field a friendlier display name for the life of the search, e.g. … AS "Client IP". | Use the NEW name afterwards; quotes for spaces. |
| ⭐ **dedup** | Command that removes events with duplicate values in the given fields, keeping the first (most recent) one. | dedup 3 user keeps 3 per value. |
| ⭐ **sort** | Command that orders results by fields. - = descending, + or nothing = ascending. Default limit is 10,000 results. | sort 0 = all results. Commas between fields. |
| **eval** | Command that calculates an expression and puts the result in a new or existing field (in the results only). | Concatenate with a period (.). |
| **where** | Command that filters results with an eval-style expression after a pipe, e.g. … bytes > 5000. |  |

## 5 · Transforming

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Transforming command** | A command that turns events into a statistics table: stats, chart, timechart, top, rare. Required for visualizations. | Fills the Statistics and Visualization tabs. |
| ⭐ **top** | Command that returns the most common values of a field, with count and percent columns (10 rows by default). | limit=N, showperc=false, countfield=, by. |
| ⭐ **rare** | Command that returns the least common values of a field. Same options as top. | Unusual/anomalies → rare. |
| ⭐ **stats** | Transforming command that calculates aggregate statistics (count, dc, sum, avg, min, max, values, list…), optionally BY fields. | No BY = 1 row; BY = 1 row per value. |
| ⭐ **dc()** | stats function that counts distinct (unique) values of a field. | count = total; dc = unique. |
| ⭐ **values()** | stats function that returns each distinct value of a field once, sorted. | list() = all values, in order, with duplicates. |
| **list()** | stats function that returns all values of a field, duplicates included, in event order. |  |
| ⭐ **timechart** | Transforming command that charts statistics over time, with _time always on the x-axis. | span=1h sets bucket size. |
| **chart** | Transforming command that plots statistics with ANY field on the x-axis (… count over x by y). | over = x-axis; by = series. |
| **Data series** | A sequence of related data points plotted on a chart; each line in a line chart is one series. | Charts need a transforming search producing 1+ series. |

## 6 · Reports & Dashboards

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Report** | A saved search (or pivot) that can be re-run, shared, scheduled, and added to dashboards. Any search can be saved as one. | Starts Private. Run As Owner by default. |
| ⭐ **Pivot** | Drag-and-drop tool for building tables and charts from a dataset (data model) without writing SPL. | One of the 4 ways to create a report. |
| ⭐ **Run As** | Report permission setting: Owner = runs with the creator's permissions (default); User = runs with the viewer's permissions. | Owner (default) gives access to data the viewer might not have. |
| ⭐ **Report permissions** | Display For: Owner (private), App, or All apps; plus Read (see and run) and Write (edit) per role. A role with neither can't see the report. | Private · App · All apps. |
| **Embed** | Report option that puts a report on an external web page. Only available for scheduled reports. | Scheduled only. |
| **Acceleration** | Report option that summarizes data so slow, search-based reports run faster. | Edit → Edit Acceleration. |
| ⭐ **Dashboard** | A view made of panels that show searches and reports as tables and charts. | Create via Save As → Dashboard Panel, or Dashboards → Create New Dashboard. |
| ⭐ **Panel** | One element of a dashboard (chart, table, single value…) powered by an inline search or a report. | Add Panel: New · New from Report · Clone from Dashboard · Add Prebuilt Panel. |
| ⭐ **Inline search panel** | Dashboard panel whose search is stored inside the dashboard. It's created when you save a search directly as a dashboard panel. | No link to a report. |
| ⭐ **Report panel** | Dashboard panel powered by a saved report. Edits to the report show up on every dashboard that uses it. | Panel Powered By: Report = linked. |
| **Dashboard Studio** | The newer dashboard framework, defined in JSON. (Classic dashboards use Simple XML.) | Classic = XML; Studio = JSON. |
| **Simple XML** | The source format of Classic Splunk dashboards. |  |
| ⭐ **Group_Object_Description** | Splunk's recommended naming convention for dashboards and other knowledge objects (e.g. Sales_Dashboard_Daily_performance). | Group = business unit, Object = type, Description = purpose. |
| **Knowledge object** | A user-defined object that adds meaning to data: fields, tags, event types, lookups, reports, alerts, macros, data models. | Permissions: Private · This app · All apps. |

## 7 · Lookups

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Lookup** | A way to enrich events at search time with fields from an external table, matched on a shared field. | Raw indexed events are never changed. |
| ⭐ **Lookup table file** | The uploaded file (e.g. a CSV) that holds the lookup data. Only CSV and geospatial lookups need one. | Step 1 of setup. |
| ⭐ **Lookup definition** | The named configuration that points to a lookup table and its settings. The lookup command uses it. | Step 2. All lookup types need one. |
| ⭐ **Automatic lookup** | A lookup applied to every matching search (by sourcetype, source or host) without typing the lookup command. | Step 3 (optional). Settings → Lookups → Automatic lookups. |
| ⭐ **CSV lookup** | File-based (static) lookup that uses a CSV file. Best for small, relatively static data. | At least 2 columns; UTF-8. |
| ⭐ **External lookup** | Scripted lookup that uses a Python script or binary to get values from an external source (e.g. DNS). | Can't be written to with outputlookup. |
| ⭐ **KV Store lookup** | Lookup that uses a KV Store collection. Best for large or frequently updated tables. | Big/changing data → KV Store. |
| **Geospatial lookup** | Lookup that uses a KMZ/KML file to match coordinates to regions, for choropleth maps. |  |
| ⭐ **inputlookup** | Command that reads and displays the contents of a lookup table (CSV or KV Store). | Input = read/view/validate. |
| ⭐ **outputlookup** | Command that writes search results to a CSV lookup file or KV Store collection. | Output = write. |
| **OUTPUTNEW** | lookup option that only adds output fields that don't already exist in the event (OUTPUT overwrites). |  |

## 8 · Alerts

| Term | Definition | Exam tip |
|---|---|---|
| ⭐ **Alert** | A saved search that runs on a schedule or in real time and triggers actions when its results meet a condition. | Search → type → trigger condition → action. |
| ⭐ **Scheduled report** | A report that runs on an interval and triggers its actions every time it runs (email, CSV lookup, webhook, log event). | Every run → report. Only on a condition → alert. |
| ⭐ **Scheduled alert** | Alert that runs its search on a schedule and acts only when the trigger condition is met. | Lighter than real-time. |
| ⭐ **Real-time alert** | Alert that searches continuously and fires per result or when a condition is met within a rolling time window. | Immediate but resource-heavy. |
| **Rolling window** | Real-time alert triggering when a condition (e.g. > 20 results) is met within a time window (e.g. 5 minutes). | vs per-result = fires on every matching event. |
| ⭐ **Throttle** | Alert setting that suppresses repeat triggers for a set period, optionally per field value. | Stops alert floods. |
| ⭐ **Triggered Alerts** | Page under the Activity menu listing alerts that fired, for alerts using the matching action. Records are kept 24 hours by default. | Severity = a filter label. |
| **Cron schedule** | A custom expression (minute hour day month weekday) for when a report or alert runs, used instead of presets like 'Run every hour'. | Or use presets like Run every hour. |
| **Schedule Window** | Optional setting that lets the scheduler delay a scheduled report so higher-priority searches run first. |  |
