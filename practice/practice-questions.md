# SPLK-1001 Practice Questions

**103 original practice questions**, grouped by the 8 official exam topics. Written from the Splunk Core User course topics and checked against Splunk's documentation. These are **not** real exam questions. Click **Show answer** to reveal each answer.

For an interactive version with scoring, download [`practice-quiz.html`](practice-quiz.html) and open it in a browser.

| Topic | Exam weight | Questions |
|---|---|---|
| [1. Splunk Basics](#1-splunk-basics) | 5% | 7 |
| [2. Basic Searching](#2-basic-searching) | 22% | 23 |
| [3. Using Fields in Searches](#3-using-fields-in-searches) | 20% | 16 |
| [4. Search Language Fundamentals](#4-search-language-fundamentals) | 15% | 14 |
| [5. Using Basic Transforming Commands](#5-using-basic-transforming-commands) | 15% | 14 |
| [6. Creating Reports and Dashboards](#6-creating-reports-and-dashboards) | 12% | 15 |
| [7. Creating and Using Lookups](#7-creating-and-using-lookups) | 6% | 6 |
| [8. Scheduled Reports and Alerts](#8-scheduled-reports-and-alerts) | 5% | 8 |

## 1. Splunk Basics

**1.1** A server team needs to install a lightweight agent on 300 web servers. Its only job is to collect the logs and send them to the indexers. Which component should they deploy?

- **A.** Universal forwarder
- **B.** Search head
- **C.** Deployment server
- **D.** Heavy forwarder

<details><summary>Show answer</summary>

**A. Universal forwarder**

A universal forwarder is the lightweight agent Splunk recommends for collecting and sending data. It doesn't parse. A heavy forwarder is a full Splunk instance, more than this job needs.

💡 **Exam cue:** Universal forwarder = lightweight collect-and-send. Heavy forwarder = full instance that can parse, filter and route.

</details>

**1.2** Credit card numbers must be masked in events BEFORE the data leaves the source network and reaches the indexers. Which component can do this?

- **A.** License master
- **B.** Heavy forwarder
- **C.** Search head
- **D.** Universal forwarder

<details><summary>Show answer</summary>

**B. Heavy forwarder**

A heavy forwarder is a full Splunk Enterprise instance, so it can parse, filter, mask and route data before forwarding it. A universal forwarder doesn't parse. Search heads only work at search time.

💡 **Exam cue:** Heavy forwarder: parse, filter, mask, route before indexing.

</details>

**1.3** Which component receives raw data, breaks it into events, timestamps it, and stores it in time-based buckets?

- **A.** Deployment server
- **B.** Universal forwarder
- **C.** Indexer
- **D.** Search head

<details><summary>Show answer</summary>

**C. Indexer**

Indexers parse incoming data into events, assign timestamps and metadata, and store it in indexes organized as time-based buckets.

💡 **Exam cue:** Indexer = parse, timestamp, store (buckets). Search head = UI.

</details>

**1.4** An analyst writes SPL in a browser, and the request is sent to several indexers whose results are merged into one result set. Which component does the sending and merging?

- **A.** Indexer
- **B.** Heavy forwarder
- **C.** Universal forwarder
- **D.** Search head

<details><summary>Show answer</summary>

**D. Search head**

The search head is the UI. It distributes searches to the indexers and merges (reduces) their partial results.

💡 **Exam cue:** Search head = UI, distributes the search, merges the results.

</details>

**1.5** A small lab runs ONE Splunk instance that handles inputs, indexing, searching and dashboards. What deployment type is this?

- **A.** Standalone
- **B.** Distributed
- **C.** Cloud
- **D.** Clustered

<details><summary>Show answer</summary>

**A. Standalone**

A standalone deployment is a single instance doing every function. Distributed splits roles across instances. Cloud is hosted and managed by a third party.

💡 **Exam cue:** Standalone = 1 instance. Distributed = roles split. Cloud = hosted.

</details>

**1.6** A user in New York sees every event timestamp 5 hours off from their local clock. What should they change?

- **A.** The Time Range Picker preset
- **B.** The time zone in their account Preferences
- **C.** The sourcetype of the data
- **D.** The search mode

<details><summary>Show answer</summary>

**B. The time zone in their account Preferences**

Each user sets a time zone under Preferences. Splunk shows timestamps in that zone, so timestamps line up with you when you investigate.

💡 **Exam cue:** Time zone lives in user Preferences.

</details>

**1.7** Which statement correctly describes apps and add-ons?

- **A.** Apps can only come from Splunk; add-ons only from third parties
- **B.** Add-ons always include dashboards; apps never do
- **C.** An app usually gives a UI/workspace for a use case; an add-on usually supplies inputs and parsing for a data source
- **D.** Apps and add-ons are both types of indexes

<details><summary>Show answer</summary>

**C. An app usually gives a UI/workspace for a use case; an add-on usually supplies inputs and parsing for a data source**

Apps are collections of config files, knowledge objects and UI for a use case (e.g. Search & Reporting). Add-ons are reusable components such as data inputs, parsing and field extractions, usually with no UI. Both come from Splunk or third parties via Splunkbase.

💡 **Exam cue:** App = workspace/UI. Add-on = inputs/parsing, usually no UI.

</details>


## 2. Basic Searching

**2.1** A search over 'All time' is very slow. You know the incident happened in the last hour. What is the easiest and most effective fix?

- **A.** Remove the index from the search
- **B.** Add a wildcard to every search term
- **C.** Switch to Verbose mode
- **D.** Set the time range to Last 60 minutes

<details><summary>Show answer</summary>

**D. Set the time range to Last 60 minutes**

Narrowing the time range is the easiest and most effective way to optimize a search, because Splunk skips buckets outside the range.

💡 **Exam cue:** Best optimization = narrow the time range.

</details>

**2.2** What does earliest=-1d@d latest=@d return?

- **A.** Yesterday, from midnight to midnight
- **B.** Today from midnight until now
- **C.** The last 24 hours up to now
- **D.** The last 2 days

<details><summary>Show answer</summary>

**A. Yesterday, from midnight to midnight**

-1d@d = go back 1 day, then snap down to midnight (start of yesterday). @d = midnight at the start of today. Together: all of yesterday.

💡 **Exam cue:** -1d@d to @d = yesterday. -24h to now = last 24 hours.

</details>

**2.3** It is 14:35. You run a search with earliest=-2h@h. When does the search start?

- **A.** 13:00
- **B.** 12:00
- **C.** 12:35
- **D.** 14:00

<details><summary>Show answer</summary>

**B. 12:00**

-2h from 14:35 = 12:35. @h snaps down to the start of that hour = 12:00.

💡 **Exam cue:** Offset first, then @ rounds DOWN: 12:35 → 12:00.

</details>

**2.4** The Time Range Picker is set to 'Last 7 days', but the search string includes earliest=-4h. Which events are returned?

- **A.** Events from the last 7 days plus 4 hours
- **B.** No events, because the ranges conflict
- **C.** Only events from the last 4 hours
- **D.** Events from the last 7 days

<details><summary>Show answer</summary>

**C. Only events from the last 4 hours**

Time modifiers typed in the search string override the Time Range Picker.

💡 **Exam cue:** Typed earliest/latest OVERRIDE the picker.

</details>

**2.5** In a time modifier, which unit means MONTHS?

- **A.** m
- **B.** mo
- **C.** M
- **D.** mon

<details><summary>Show answer</summary>

**D. mon**

m = minutes. mon = months. Other units: s, h, d, w, y.

💡 **Exam cue:** m = minutes, mon = months.

</details>

**2.6** You need events that contain the words failed AND password anywhere in the event, in any order. Which search is correct?

- **A.** index=security failed password
- **B.** index=security failed OR password
- **C.** index=security "failed password"
- **D.** index=security failed NOT password

<details><summary>Show answer</summary>

**A. index=security failed password**

AND is implied between search terms, so 'failed password' means failed AND password anywhere. Quotes would require the exact phrase in that order.

💡 **Exam cue:** Space = implied AND. Quotes = exact phrase.

</details>

**2.7** How does Splunk evaluate: index=web error OR warn critical

- **A.** Events with error OR warn OR critical
- **B.** Events with critical AND (error OR warn)
- **C.** Events with error, warn and critical
- **D.** Events with error OR (warn AND critical)

<details><summary>Show answer</summary>

**B. Events with critical AND (error OR warn)**

In the base search, OR is evaluated before AND. So error OR warn is grouped first, then ANDed with critical. Use parentheses to be explicit.

💡 **Exam cue:** Order is ( ) → NOT → OR → AND.

</details>

**2.8** A user types: index=web error or warn  (lowercase 'or'). What happens?

- **A.** Splunk ignores the word 'or'
- **B.** Splunk treats it as OR
- **C.** 'or' is treated as a search term, so events must contain error, or, and warn
- **D.** Splunk returns a syntax error

<details><summary>Show answer</summary>

**C. 'or' is treated as a search term, so events must contain error, or, and warn**

Boolean operators must be UPPERCASE. A lowercase 'or' is just another keyword, ANDed with the others.

💡 **Exam cue:** AND/OR/NOT must be uppercase.

</details>

**2.9** Which search mode is the DEFAULT?

- **A.** Fast
- **B.** Verbose
- **C.** Auto
- **D.** Smart

<details><summary>Show answer</summary>

**D. Smart**

Smart is the default. It acts like Verbose for event searches and like Fast for transforming searches.

💡 **Exam cue:** Default = Smart.

</details>

**2.10** You run index=web \| stats count by status in SMART mode. What do you get?

- **A.** A statistics table, with no event list or timeline
- **B.** An error, because Smart mode can't run stats
- **C.** Only the raw events
- **D.** The full event list plus a statistics table

<details><summary>Show answer</summary>

**A. A statistics table, with no event list or timeline**

For a transforming search, Smart mode works like Fast: it returns the statistics/visualization without building the event list. Verbose would also return the events.

💡 **Exam cue:** Smart + transforming = stats only. Verbose = stats + events.

</details>

**2.11** An analyst needs the statistics table AND the full event list from the same transforming search. Which mode?

- **A.** Real-time
- **B.** Verbose
- **C.** Smart
- **D.** Fast

<details><summary>Show answer</summary>

**B. Verbose**

Verbose returns all fields and the full event list even when the search has a transforming command. It's the slowest mode.

💡 **Exam cue:** Verbose = everything, slowest.

</details>

**2.12** Which search mode turns OFF field discovery to return results as fast as possible?

- **A.** Real-time
- **B.** Smart
- **C.** Fast
- **D.** Verbose

<details><summary>Show answer</summary>

**C. Fast**

Fast mode disables field discovery. It returns default/indexed fields plus fields named in the search.

💡 **Exam cue:** Fast = no field discovery.

</details>

**2.13** You click and drag across several bars in the timeline. What happens?

- **A.** The timeline switches to Log scale
- **B.** The selection is saved as a report
- **C.** A new search runs over the selected time
- **D.** The current results are filtered to that time span, without running a new search

<details><summary>Show answer</summary>

**D. The current results are filtered to that time span, without running a new search**

Selecting bars only filters the events already returned. Zoom to selection and Zoom out are what re-run the search.

💡 **Exam cue:** Select bars = filter only. Zoom = new search.

</details>

**2.14** Which timeline action changes the Time Range Picker and searches a wider time range?

- **A.** Zoom out
- **B.** Hovering over a bar
- **C.** Selecting bars
- **D.** Deselect

<details><summary>Show answer</summary>

**A. Zoom out**

Splunk's docs: zooming out "changes not only the timeline but the value in the Time Range Picker", so a wider range is searched. Selecting bars (and Deselect) only filter the existing results; hovering just shows the count.

💡 **Exam cue:** Zoom out = wider time range (new search). Selecting bars = filter only.

</details>

**2.15** A coworker opens the link to your ad hoc search job 2 hours after you ran it, and the job is gone. Why?

- **A.** Jobs expire after exactly 1 hour
- **B.** Search jobs are kept for 10 minutes by default
- **C.** Job links only work for Admin users
- **D.** Search jobs are deleted when you log out

<details><summary>Show answer</summary>

**B. Search jobs are kept for 10 minutes by default**

An ad hoc search job is kept for 10 minutes by default. Extend it, up to 7 days, with Job → Edit Job Settings.

💡 **Exam cue:** Job lifetime: 10 min default, up to 7 days.

</details>

**2.16** You want a teammate to see your search results tomorrow WITHOUT re-running the search. What should you do?

- **A.** Save the search as an alert
- **B.** Switch to Fast mode
- **C.** Job → Edit Job Settings: extend the lifetime and share the link
- **D.** Send the job to the background

<details><summary>Show answer</summary>

**C. Job → Edit Job Settings: extend the lifetime and share the link**

Edit Job Settings lets you change read permissions, extend the job lifetime (to 7 days), and copy a link to the job.

💡 **Exam cue:** Edit Job Settings = permissions, lifetime, shareable link.

</details>

**2.17** A search will take a long time, and you want to keep working on other searches while it finishes. Which Job menu option?

- **A.** Delete Job
- **B.** Edit Job Settings
- **C.** Inspect Job
- **D.** Send Job to Background

<details><summary>Show answer</summary>

**D. Send Job to Background**

Send Job to Background keeps the job running while you do other work.

💡 **Exam cue:** Background = keep running while you work.

</details>

**2.18** Which tool shows a search job's execution costs and properties, to figure out why it was slow?

- **A.** Search Job Inspector
- **B.** Data Summary
- **C.** Patterns tab
- **D.** Field window

<details><summary>Show answer</summary>

**A. Search Job Inspector**

Job → Inspect Job opens the Search Job Inspector, with execution costs and job properties.

💡 **Exam cue:** Why slow? → Search Job Inspector.

</details>

**2.19** You run an ad hoc search and click the Export button. Which format is NOT offered?

- **A.** JSON
- **B.** PDF
- **C.** XML
- **D.** CSV

<details><summary>Show answer</summary>

**B. PDF**

For ad hoc searches, Splunk's docs list Raw Events, CSV, XML and JSON. PDF export is only available for saved searches (reports) and dashboards.

💡 **Exam cue:** Ad hoc export: Raw Events, CSV, XML, JSON. PDF = reports/dashboards only.

</details>

**2.20** In what order are events listed after a search?

- **A.** By relevance score
- **B.** Alphabetical by host
- **C.** Reverse chronological (newest first)
- **D.** Chronological (oldest first)

<details><summary>Show answer</summary>

**C. Reverse chronological (newest first)**

Events are shown newest first.

💡 **Exam cue:** Reverse chronological.

</details>

**2.21** What is the quickest way to see which hosts, sources and sourcetypes exist in your Splunk deployment?

- **A.** Check the Jobs page
- **B.** Run index=\* and scroll
- **C.** Open Settings → Indexes
- **D.** Click Data Summary

<details><summary>Show answer</summary>

**D. Click Data Summary**

Data Summary shows three tabs, Hosts, Sources and Sourcetypes. It's the quickest overview of the data present.

💡 **Exam cue:** Data Summary = Hosts / Sources / Sourcetypes.

</details>

**2.22** Which results tab groups events that share a similar structure?

- **A.** Patterns
- **B.** Events
- **C.** Visualization
- **D.** Statistics

<details><summary>Show answer</summary>

**A. Patterns**

The Patterns tab shows the most common patterns among the returned events.

💡 **Exam cue:** Patterns = similar-structure groups.

</details>

**2.23** Which is NOT a display option in the Events viewer?

- **A.** Raw
- **B.** Chart
- **C.** List
- **D.** Table

<details><summary>Show answer</summary>

**B. Chart**

The Events viewer offers List (the default), Raw and Table. Charts belong on the Visualization tab.

💡 **Exam cue:** Events views: List, Raw, Table.

</details>


## 3. Using Fields in Searches

**3.1** Status=404 returns nothing, but status=404 returns thousands of events. Why?

- **A.** Capitalized searches only run in Verbose mode
- **B.** 404 must be in quotes
- **C.** Field names are case-sensitive
- **D.** Field values are case-sensitive

<details><summary>Show answer</summary>

**C. Field names are case-sensitive**

Field NAMES are case-sensitive, so Status is a different field from status. Field VALUES are not case-sensitive.

💡 **Exam cue:** Names case-sensitive, values not.

</details>

**3.2** host=WEB01 and host=web01 return…

- **A.** An error
- **B.** No events for the uppercase version
- **C.** Different events, because values are case-sensitive
- **D.** The same events

<details><summary>Show answer</summary>

**D. The same events**

Field values are not case-sensitive. Only field names are.

💡 **Exam cue:** Values: case doesn't matter.

</details>

**3.3** Which three fields are the default Selected Fields?

- **A.** host, source, sourcetype
- **B.** \_raw, \_time, host
- **C.** host, index, \_time
- **D.** source, sourcetype, index

<details><summary>Show answer</summary>

**A. host, source, sourcetype**

The default Selected Fields are host, source and sourcetype.

💡 **Exam cue:** Selected = host, source, sourcetype.

</details>

**3.4** A field appears in only 12% of your results and is NOT listed under Interesting Fields. How do you find it?

- **A.** It can't be found
- **B.** Click All Fields
- **C.** Switch to Fast mode
- **D.** Rename the field

<details><summary>Show answer</summary>

**B. Click All Fields**

Interesting Fields only shows fields in at least 20% of events. All Fields lists every field, including non-interesting ones, and lets you select it.

💡 **Exam cue:** <20% → All Fields.

</details>

**3.5** What qualifies a field as an 'Interesting Field'?

- **A.** It has more than 10 unique values
- **B.** It was extracted at index time
- **C.** It appears in at least 20% of the events
- **D.** It appears in every event

<details><summary>Show answer</summary>

**C. It appears in at least 20% of the events**

Interesting fields are extracted fields present in at least 20% of the resulting events.

💡 **Exam cue:** ≥ 20% of events.

</details>

**3.6** In the fields sidebar, what does # next to a field name mean?

- **A.** The field is selected
- **B.** The field count is unknown
- **C.** The field is indexed
- **D.** The field contains numeric values

<details><summary>Show answer</summary>

**D. The field contains numeric values**

# = numeric values. a = alphanumeric (string) values. The number to the RIGHT of a field = its count of unique values.

💡 **Exam cue:** # numeric, a string, number on the right = unique values.

</details>

**3.7** In the fields sidebar, what does the number to the RIGHT of a field name show?

- **A.** How many unique values the field has
- **B.** How many events contain the field
- **C.** The field's position in the table
- **D.** The percentage of events with the field

<details><summary>Show answer</summary>

**A. How many unique values the field has**

It's the count of distinct values for that field in the results.

💡 **Exam cue:** Right-side number = unique value count.

</details>

**3.8** Which field is created at INDEX time for every event?

- **A.** action
- **B.** sourcetype
- **C.** clientip
- **D.** status

<details><summary>Show answer</summary>

**B. sourcetype**

host, source, sourcetype and index (plus \_time and \_raw) are created at index time. action, clientip and status are extracted at search time.

💡 **Exam cue:** Index-time: host, source, sourcetype, index, \_time, \_raw.

</details>

**3.9** Which internal field holds the complete original text of an event?

- **A.** punct
- **B.** \_time
- **C.** \_raw
- **D.** source

<details><summary>Show answer</summary>

**C. \_raw**

\_raw is the full raw event. \_time is its timestamp.

💡 **Exam cue:** \_raw = original text.

</details>

**3.10** You want every event EXCEPT those where src is 10.1.1.1, including events that have no src field at all. Which search?

- **A.** src=!10.1.1.1
- **B.** src NOT 10.1.1.1
- **C.** src!=10.1.1.1
- **D.** NOT src=10.1.1.1

<details><summary>Show answer</summary>

**D. NOT src=10.1.1.1**

NOT src=x returns everything except matches, including events without a src field. src!=x only returns events that HAVE src with a different value.

💡 **Exam cue:** != drops events without the field; NOT keeps them.

</details>

**3.11** According to Splunk best practice, which wildcard search is MOST efficient?

- **A.** user=adm\*
- **B.** user=a\*m
- **C.** user=\*adm\*
- **D.** user=\*adm

<details><summary>Show answer</summary>

**A. user=adm\***

A trailing wildcard is most efficient. Leading wildcards force Splunk to check every value. Middle wildcards can give inconsistent results.

💡 **Exam cue:** Wildcard at the END.

</details>

**3.12** You need events for uri\_path values /cart.do and /cart/error.do. What does Splunk recommend?

- **A.** uri\_path=/cart?
- **B.** uri\_path="/cart.do" OR uri\_path="/cart/error.do"
- **C.** uri\_path=\*cart\*
- **D.** uri\_path="/cart\*"

<details><summary>Show answer</summary>

**B. uri\_path="/cart.do" OR uri\_path="/cart/error.do"**

Don't use wildcards to match punctuation. Specify the exact strings with their punctuation.

💡 **Exam cue:** Punctuation? List exact values; don't wildcard.

</details>

**3.13** Why is status=503 better than just searching 503?

- **A.** status=503 searches every index
- **B.** There is no difference
- **C.** It is more precise and efficient; a bare 503 can also match fields like area\_code=503
- **D.** Bare numbers aren't allowed in searches

<details><summary>Show answer</summary>

**C. It is more precise and efficient; a bare 503 can also match fields like area\_code=503**

Field/value searches are more precise and efficient than keywords. A bare value matches it in any field.

💡 **Exam cue:** Use field=value instead of bare values.

</details>

**3.14** Which search matches the product name Dream Crusher as an exact phrase?

- **A.** product\_name=Dream Crusher
- **B.** product\_name=Dream\*Crusher
- **C.** product\_name=(Dream Crusher)
- **D.** product\_name="Dream Crusher"

<details><summary>Show answer</summary>

**D. product\_name="Dream Crusher"**

Values with spaces need double quotes. Without them, Splunk searches product\_name=Dream AND the keyword Crusher anywhere.

💡 **Exam cue:** Quotes for values with spaces.

</details>

**3.15** You click a field name in the fields sidebar. Which report is offered there?

- **A.** Top values by time
- **B.** Events with rare value fields
- **C.** Pivot by sourcetype
- **D.** Scheduled alert

<details><summary>Show answer</summary>

**A. Top values by time**

The field window offers Top values, Top values by time, Rare values and Events with this field. Choosing one adds a transforming command to your search.

💡 **Exam cue:** Field window reports: Top values (by time), Rare values, Events with this field.

</details>

**3.16** In Fast mode, a field you saw earlier isn't in the sidebar anymore. Why?

- **A.** The field is over the 20% threshold
- **B.** Fast mode disables field discovery; only default fields and fields named in the search are shown
- **C.** The field was deleted from the index
- **D.** Fast mode only shows numeric fields

<details><summary>Show answer</summary>

**B. Fast mode disables field discovery; only default fields and fields named in the search are shown**

Fast mode skips field discovery. Name the field in the search, or use Smart/Verbose mode.

💡 **Exam cue:** Fast = no field discovery.

</details>


## 4. Search Language Fundamentals

**4.1** Which of these is NOT one of the search language syntax components (search terms, commands, functions, arguments, clauses)?

- **A.** Clauses
- **B.** Arguments
- **C.** Pipes
- **D.** Functions

<details><summary>Show answer</summary>

**C. Pipes**

Splunk breaks search syntax into search terms, commands, functions, arguments and clauses. The pipe separates commands but isn't listed as a component.

💡 **Exam cue:** Components: terms, commands, functions, arguments, clauses.

</details>

**4.2** What does the pipe character \| do in a search?

- **A.** Means OR
- **B.** Separates two independent searches
- **C.** Starts a comment
- **D.** Passes the results of the command on its left into the command on its right

<details><summary>Show answer</summary>

**D. Passes the results of the command on its left into the command on its right**

Each command's output becomes the next command's input. Splunk processes the pipeline left to right.

💡 **Exam cue:** \| = output → input.

</details>

**4.3** Where should \| fields + be placed to improve performance the most?

- **A.** Right after the base search
- **B.** At the very end
- **C.** Only inside a subsearch
- **D.** Before the index

<details><summary>Show answer</summary>

**A. Right after the base search**

Using fields + early limits the fields that later commands have to carry and extract, which makes the search faster.

💡 **Exam cue:** fields + early = faster.

</details>

**4.4** What does \| fields host status do (no + or -)?

- **A.** Removes host and status
- **B.** Keeps only host and status (plus internal \_raw and \_time)
- **C.** Renames host to status
- **D.** Shows host and status as a chart

<details><summary>Show answer</summary>

**B. Keeps only host and status (plus internal \_raw and \_time)**

With no sign, fields defaults to + (include). \_raw and \_time stay unless you remove them explicitly.

💡 **Exam cue:** No sign = +.

</details>

**4.5** You want columns shown in the exact order \_time, user, action. Which command?

- **A.** rename \_time user action
- **B.** fields \_time user action
- **C.** table \_time user action
- **D.** dedup \_time user action

<details><summary>Show answer</summary>

**C. table \_time user action**

table displays the listed fields as columns in the order given. fields controls which fields are kept but doesn't lay out a table.

💡 **Exam cue:** Column order → table.

</details>

**4.6** After \| rename clientip AS "Client IP", how must later commands refer to the field?

- **A.** Either name works
- **B.** Client\_IP
- **C.** clientip
- **D.** "Client IP"

<details><summary>Show answer</summary>

**D. "Client IP"**

After rename, use the new name, in quotes if it contains a space. The rename lasts only for this search.

💡 **Exam cue:** Use the NEW name afterwards.

</details>

**4.7** Which command keeps only ONE event per user?

- **A.** dedup user
- **B.** table user
- **C.** rename user
- **D.** sort user

<details><summary>Show answer</summary>

**A. dedup user**

dedup removes events with duplicate values in the listed fields and keeps the first one found (the most recent, for a normal search).

💡 **Exam cue:** One per value → dedup.

</details>

**4.8** Which command lists results with the highest bytes first?

- **A.** sort bytes
- **B.** sort -bytes
- **C.** sort desc=bytes
- **D.** sort +bytes

<details><summary>Show answer</summary>

**B. sort -bytes**

- = descending. + or no sign = ascending.

💡 **Exam cue:** sort -field = descending.

</details>

**4.9** By default, how many results does sort return?

- **A.** 10
- **B.** All results
- **C.** 10,000
- **D.** 100

<details><summary>Show answer</summary>

**C. 10,000**

sort's default limit is 10,000 results. sort 0 field returns all of them.

💡 **Exam cue:** sort default 10,000; sort 0 = all.

</details>

**4.10** When sorting on multiple fields, what goes between the field names?

- **A.** A semicolon
- **B.** The keyword AND
- **C.** A pipe
- **D.** A comma

<details><summary>Show answer</summary>

**D. A comma**

Separate multiple sort fields with commas, e.g. sort -count, +host.

💡 **Exam cue:** Comma between sort fields.

</details>

**4.11** Which eval expression creates a field label like 'IP-10.1.1.5' from clientip?

- **A.** eval label="IP-".clientip
- **B.** eval label="IP-"\|clientip
- **C.** eval label=IP-clientip
- **D.** eval label="IP-" AND clientip

<details><summary>Show answer</summary>

**A. eval label="IP-".clientip**

In eval, the period (.) concatenates strings. String literals go in double quotes.

💡 **Exam cue:** eval concatenation = .

</details>

**4.12** An eval command overwrites the existing field status. What happens to the indexed data?

- **A.** The original events are deleted
- **B.** Nothing; the change only exists in this search's results
- **C.** The sourcetype is changed
- **D.** The index is updated permanently

<details><summary>Show answer</summary>

**B. Nothing; the change only exists in this search's results**

eval (like rename) only changes the results of the current search. Indexed data is never modified.

💡 **Exam cue:** eval never changes the index.

</details>

**4.13** A search doesn't specify an index. Which data does it search?

- **A.** No data; an index is required
- **B.** Only the main index
- **C.** The indexes your role searches by default
- **D.** Every index in the deployment, regardless of role

<details><summary>Show answer</summary>

**C. The indexes your role searches by default**

With no index specified, Splunk searches the default indexes for your role. Best practice: always specify the index.

💡 **Exam cue:** No index → your role's default indexes. Always specify one.

</details>

**4.14** Which is a Splunk search best practice?

- **A.** Exclude data with NOT whenever possible
- **B.** Put wildcards at the start of terms
- **C.** Use index=\* to be thorough
- **D.** Filter as early as possible in the search

<details><summary>Show answer</summary>

**D. Filter as early as possible in the search**

Best practices: limit the time, specify the index, use specific terms, inclusion over exclusion, and filter early.

💡 **Exam cue:** Filter early; avoid index=\*, leading wildcards, NOT.

</details>


## 5. Using Basic Transforming Commands

**5.1** Which search returns the 5 most common user agents?

- **A.** \| top limit=5 useragent
- **B.** \| stats top(useragent) 5
- **C.** \| rare limit=5 useragent
- **D.** \| top useragent count=5

<details><summary>Show answer</summary>

**A. \| top limit=5 useragent**

top returns the most common values. limit=N sets how many (default 10). You can also write top 5 useragent.

💡 **Exam cue:** top limit=N field.

</details>

**5.2** You want the LEAST common source IPs to spot unusual activity. Which command?

- **A.** sort
- **B.** rare
- **C.** top
- **D.** dedup

<details><summary>Show answer</summary>

**B. rare**

rare finds the least frequent values. It has the same options as top.

💡 **Exam cue:** Least common → rare.

</details>

**5.3** \| top status returns which columns by default?

- **A.** status only
- **B.** status, percent, total
- **C.** status, count, percent
- **D.** status, count

<details><summary>Show answer</summary>

**C. status, count, percent**

top (and rare) add count and percent columns next to the field values, 10 rows by default.

💡 **Exam cue:** top adds count + percent; 10 rows.

</details>

**5.4** How do you remove the percent column from top's output?

- **A.** percent=off
- **B.** showpercent=f
- **C.** nopercent=true
- **D.** showperc=false

<details><summary>Show answer</summary>

**D. showperc=false**

showperc=false hides the percent column. showcount=false hides the count. countfield= / percentfield= rename them.

💡 **Exam cue:** showperc=false / showcount=false.

</details>

**5.5** \| top limit=0 product\_name returns…

- **A.** All values of product\_name
- **B.** The top 10 values
- **C.** No results
- **D.** Only the single most common value

<details><summary>Show answer</summary>

**A. All values of product\_name**

limit=0 returns every value.

💡 **Exam cue:** limit=0 = all.

</details>

**5.6** \| top 3 status by host returns…

- **A.** An error; the syntax must be limit=3
- **B.** The 3 most common status values for EACH host
- **C.** The top 3 hosts
- **D.** The 3 most common status values overall

<details><summary>Show answer</summary>

**B. The 3 most common status values for EACH host**

The by clause splits the results, giving a top 3 for each host. top 3 is valid shorthand for limit=3.

💡 **Exam cue:** by = per value of that field.

</details>

**5.7** How many rows does \| stats count by src\_ip return?

- **A.** Ten rows
- **B.** One row per event
- **C.** One row per unique src\_ip
- **D.** Exactly one row

<details><summary>Show answer</summary>

**C. One row per unique src\_ip**

With a by clause, stats returns one row per distinct value. Without by, it returns a single row.

💡 **Exam cue:** by → one row per value; no by → one row.

</details>

**5.8** Which function counts how many DIFFERENT users appear?

- **A.** count(user)
- **B.** values(user)
- **C.** sum(user)
- **D.** dc(user)

<details><summary>Show answer</summary>

**D. dc(user)**

dc() = distinct count. count(user) counts events that have a user field, duplicates included.

💡 **Exam cue:** Unique count → dc().

</details>

**5.9** Which function returns the list of UNIQUE values of a field?

- **A.** values()
- **B.** dc()
- **C.** count()
- **D.** list()

<details><summary>Show answer</summary>

**A. values()**

values() returns unique values, sorted. list() returns all values in order, duplicates included.

💡 **Exam cue:** values = unique; list = all.

</details>

**5.10** Which search returns average bytes per host in a column named avg\_bytes?

- **A.** \| eval avg(bytes) by host
- **B.** \| stats avg(bytes) AS avg\_bytes by host
- **C.** \| stats avg\_bytes by host
- **D.** \| top avg(bytes) by host

<details><summary>Show answer</summary>

**B. \| stats avg(bytes) AS avg\_bytes by host**

stats (field) AS  by  computes and renames the result per group.

💡 **Exam cue:** stats fn(field) AS name by group.

</details>

**5.11** \| stats count returns 5,000 but \| stats count(action) returns 4,200. Why?

- **A.** count(action) only counts unique values
- **B.** action was renamed
- **C.** 800 events have no action field
- **D.** count is rounded

<details><summary>Show answer</summary>

**C. 800 events have no action field**

count counts every event. count(field) counts only events where that field exists.

💡 **Exam cue:** count(field) = events that HAVE the field.

</details>

**5.12** Which command builds a chart with \_time ALWAYS on the x-axis?

- **A.** chart
- **B.** stats
- **C.** top
- **D.** timechart

<details><summary>Show answer</summary>

**D. timechart**

timechart always puts \_time on the x-axis. chart can use any field.

💡 **Exam cue:** timechart = time on x-axis.

</details>

**5.13** What must a search contain before Splunk can show it as a column or pie chart?

- **A.** A transforming command such as stats, chart, timechart, top or rare
- **B.** The table command and an index
- **C.** A lookup
- **D.** An eval command

<details><summary>Show answer</summary>

**A. A transforming command such as stats, chart, timechart, top or rare**

Visualizations need data transformed into a statistics table (one or more data series). Transforming commands do that.

💡 **Exam cue:** Charts need a transforming command.

</details>

**5.14** Which is a list of valid stats functions?

- **A.** top, rare, rename
- **B.** count, dc, sum, avg, values
- **C.** count, add, less, more
- **D.** sum, table, dedup

<details><summary>Show answer</summary>

**B. count, dc, sum, avg, values**

Valid stats functions include count, dc, sum, avg, min, max, median, stdev, values, list. table, dedup, top, rare and rename are commands, not functions.

💡 **Exam cue:** Functions vs commands.

</details>


## 6. Creating Reports and Dashboards

**6.1** Which searches can be saved as a report?

- **A.** Only searches with a transforming command
- **B.** Only scheduled searches
- **C.** Any search
- **D.** Only searches that produce a chart

<details><summary>Show answer</summary>

**C. Any search**

Any search (or pivot) can be saved as a report. Save As → Report.

💡 **Exam cue:** ANY search can be a report.

</details>

**6.2** Users without write permission need to re-run your report over different time ranges without editing it. What should you include when saving?

- **A.** A schedule
- **B.** An embed link
- **C.** Acceleration
- **D.** A Time Range Picker

<details><summary>Show answer</summary>

**D. A Time Range Picker**

Adding a Time Range Picker lets viewers change the time range without editing. Without it, the report always uses the original range.

💡 **Exam cue:** Time Range Picker = viewers can change the range.

</details>

**6.3** Which is NOT one of the ways to create a report in Splunk Web?

- **A.** Uploading a CSV lookup
- **B.** Saving a Pivot as a report
- **C.** Settings → Searches, reports, and alerts → New Report
- **D.** Save As → Report from Search

<details><summary>Show answer</summary>

**A. Uploading a CSV lookup**

The four ways: save a search, save a pivot, Settings → New Report, or convert an inline-search dashboard panel. Uploading a CSV creates a lookup table file.

💡 **Exam cue:** 4 ways: Search, Pivot, Settings, dashboard panel.

</details>

**6.4** What is Pivot?

- **A.** A dashboard input
- **B.** A drag-and-drop tool for building reports on a dataset without writing SPL
- **C.** A command that rotates table columns
- **D.** A type of lookup

<details><summary>Show answer</summary>

**B. A drag-and-drop tool for building reports on a dataset without writing SPL**

Pivot lets you report on a dataset (data model) with a drag-and-drop interface instead of SPL.

💡 **Exam cue:** Pivot = reports without SPL.

</details>

**6.5** A report is shared with the app. Viewers should see data that only the report's creator can normally search. Which setting?

- **A.** Run As: User
- **B.** Read: Everyone only
- **C.** Run As: Owner
- **D.** Display For: Owner

<details><summary>Show answer</summary>

**C. Run As: Owner**

Run As Owner runs the report with the creator's permissions and data access. It's also the default.

💡 **Exam cue:** Run As Owner = creator's access (default). User = viewer's access.

</details>

**6.6** In a report's Edit Permissions dialog, a role has neither Read nor Write checked. What can users with that role do?

- **A.** They can run it but not edit it
- **B.** They can view it but not run it
- **C.** They can edit it
- **D.** They can't see the report

<details><summary>Show answer</summary>

**D. They can't see the report**

Read lets a role see and run the report. Write lets them edit it. With neither, the role can't see the report.

💡 **Exam cue:** No box = can't see it.

</details>

**6.7** Which reports can be embedded in an external web page?

- **A.** Scheduled reports
- **B.** Pivot reports only
- **C.** Reports with a Time Range Picker
- **D.** Any report

<details><summary>Show answer</summary>

**A. Scheduled reports**

Only scheduled reports can be embedded.

💡 **Exam cue:** Embed = scheduled reports only.

</details>

**6.8** When adding a report to a dashboard, you want later changes to the report to show up on the dashboard automatically. Which 'Panel Powered By' option?

- **A.** Inline Search
- **B.** Report
- **C.** Clone
- **D.** Prebuilt panel

<details><summary>Show answer</summary>

**B. Report**

Powered by Report links the panel to the report. Inline Search copies the search string and time range into the dashboard, with no link.

💡 **Exam cue:** Report = linked. Inline = copy.

</details>

**6.9** You save a search directly as a dashboard panel without saving it as a report first. What kind of panel is created?

- **A.** A cloned panel
- **B.** A report panel
- **C.** An inline search panel
- **D.** A prebuilt panel

<details><summary>Show answer</summary>

**C. An inline search panel**

The search is stored inside the dashboard: an inline search panel.

💡 **Exam cue:** Straight to dashboard = inline panel.

</details>

**6.10** On a Classic dashboard, you click the Export button at the top right. Which format do you get?

- **A.** JSON
- **B.** CSV
- **C.** Any of PDF, CSV, XML, JSON
- **D.** PDF only

<details><summary>Show answer</summary>

**D. PDF only**

Per the course, exporting a whole Classic dashboard supports PDF only (Export PDF / Print). Exporting a single panel's data supports PDF, CSV, XML and JSON. (Dashboard Studio can also download PNG.)

💡 **Exam cue:** Dashboard export = PDF. Panel export = PDF/CSV/XML/JSON.

</details>

**6.11** What is Splunk's recommended naming convention for dashboards and other knowledge objects?

- **A.** Group\_Object\_Description
- **B.** Description\_Group\_Object
- **C.** Owner\_Date\_Object
- **D.** Object\_Group\_Description

<details><summary>Show answer</summary>

**A. Group\_Object\_Description**

Group\_Object\_Description, e.g. Sales\_Dashboard\_Daily\_performance.

💡 **Exam cue:** Group\_Object\_Description.

</details>

**6.12** Which statement about dashboard frameworks is correct?

- **A.** Dashboards are written in SPL only
- **B.** Classic dashboards use Simple XML; Dashboard Studio uses JSON
- **C.** Both use only JSON
- **D.** Classic uses JSON; Studio uses XML

<details><summary>Show answer</summary>

**B. Classic dashboards use Simple XML; Dashboard Studio uses JSON**

Classic dashboards are defined in Simple XML. Dashboard Studio uses JSON.

💡 **Exam cue:** Classic = XML, Studio = JSON.

</details>

**6.13** Which Add Panel option reuses a saved report as a dashboard panel?

- **A.** New
- **B.** Add Prebuilt Panel
- **C.** New from Report
- **D.** Clone from Dashboard

<details><summary>Show answer</summary>

**C. New from Report**

New from Report lets you pick a saved report, preview it, and add it to the dashboard.

💡 **Exam cue:** Saved report → New from Report.

</details>

**6.14** Which visualization best shows a trend over the last 7 days?

- **A.** Single value
- **B.** Pie chart
- **C.** Radial gauge
- **D.** Line chart

<details><summary>Show answer</summary>

**D. Line chart**

Line (or area) charts show trends over time, usually from timechart. Pies show parts of a whole. Single value and gauges show one number.

💡 **Exam cue:** Trend over time → line chart.

</details>

**6.15** Which is a FALSE statement about dashboards?

- **A.** A dashboard can't be created from search results without saving a report first
- **B.** Panels can be rearranged by dragging
- **C.** Panels can be powered by reports
- **D.** Dashboards are made up of panels

<details><summary>Show answer</summary>

**A. A dashboard can't be created from search results without saving a report first**

You CAN create a dashboard straight from search results with Save As → Dashboard Panel. Every other statement is true.

💡 **Exam cue:** Dashboards can come straight from a search.

</details>


## 7. Creating and Using Lookups

**7.1** Firewall events contain an employee ID. You want each event enriched with the employee's department from a CSV file. What should you use?

- **A.** The rename command
- **B.** A lookup
- **C.** A new index
- **D.** The eval command

<details><summary>Show answer</summary>

**B. A lookup**

Lookups add fields from an external table to events at search time, by matching a field that exists in both.

💡 **Exam cue:** Enrich from an external table → lookup.

</details>

**7.2** Which command shows the contents of the lookup table file products.csv?

- **A.** \| table products.csv
- **B.** \| outputlookup products.csv
- **C.** \| inputlookup products.csv
- **D.** \| lookup products.csv

<details><summary>Show answer</summary>

**C. \| inputlookup products.csv**

inputlookup reads and displays a lookup table. outputlookup WRITES results to one. lookup enriches events.

💡 **Exam cue:** inputlookup = read, outputlookup = write, lookup = enrich.

</details>

**7.3** What is the correct order to set up an automatic CSV lookup?

- **A.** Create the definition → upload the file → create the automatic lookup
- **B.** Create the automatic lookup → upload the file → create the definition
- **C.** Run inputlookup → upload the file → create the definition
- **D.** Upload the lookup table file → create the lookup definition → create the automatic lookup

<details><summary>Show answer</summary>

**D. Upload the lookup table file → create the lookup definition → create the automatic lookup**

Settings → Lookups: 1) Lookup table files (upload) 2) Lookup definitions 3) Automatic lookups (optional).

💡 **Exam cue:** File → Definition → Automatic.

</details>

**7.4** Your lookup table is very large and updated many times a day. Which lookup type does Splunk recommend?

- **A.** KV Store
- **B.** CSV
- **C.** External
- **D.** Geospatial

<details><summary>Show answer</summary>

**A. KV Store**

KV Store lookups suit large or frequently updated tables. CSV lookups are best for small, relatively static data.

💡 **Exam cue:** Big/changing → KV Store. Small/static → CSV.

</details>

**7.5** Which lookup type can NOT be written to with outputlookup?

- **A.** They can all be written to
- **B.** External (scripted)
- **C.** KV Store
- **D.** CSV

<details><summary>Show answer</summary>

**B. External (scripted)**

External lookups run a Python script or binary. outputlookup writes only to CSV lookup files or KV Store collections.

💡 **Exam cue:** No outputlookup for external lookups.

</details>

**7.6** In \| lookup http\_status status OUTPUTNEW status\_description, what does OUTPUTNEW do?

- **A.** Writes the results back to the CSV
- **B.** Always overwrites status\_description
- **C.** Only adds status\_description where the field doesn't already exist
- **D.** Creates a new lookup file

<details><summary>Show answer</summary>

**C. Only adds status\_description where the field doesn't already exist**

OUTPUT overwrites existing values. OUTPUTNEW only fills in fields that don't already exist in the event.

💡 **Exam cue:** OUTPUT overwrites; OUTPUTNEW fills missing.

</details>


## 8. Scheduled Reports and Alerts

**8.1** A manager wants a summary email every Monday at 8:00, whether or not anything unusual happened. What should you create?

- **A.** A scheduled alert with a 'number of results > 0' trigger
- **B.** A dashboard
- **C.** A real-time alert
- **D.** A scheduled report with an email action

<details><summary>Show answer</summary>

**D. A scheduled report with an email action**

A scheduled report runs its action every time it runs. An alert only acts when its trigger condition is met.

💡 **Exam cue:** Every time → scheduled report. Only on a condition → alert.

</details>

**8.2** Security wants a notification whenever there are more than 20 failed logins within any 5-minute window, monitored continuously. Which alert setup?

- **A.** A real-time alert with a rolling 5-minute window and Number of Results > 20
- **B.** A scheduled alert that runs daily
- **C.** A scheduled report with email
- **D.** A real-time alert with per-result triggering and no condition

<details><summary>Show answer</summary>

**A. A real-time alert with a rolling 5-minute window and Number of Results > 20**

A real-time alert with a rolling time window fires when the count condition is met within the window. Scheduled checks would be delayed.

💡 **Exam cue:** Count within a window, continuously → real-time rolling window.

</details>

**8.3** A failed-login alert sent 150 emails in one run. The team still wants an email, just fewer. What fixes it?

- **A.** Disable the alert
- **B.** Change the trigger from 'For each result' to 'Once'
- **C.** Run the alert more often
- **D.** Change the action to webhook

<details><summary>Show answer</summary>

**B. Change the trigger from 'For each result' to 'Once'**

'For each result' sends an action per result. 'Once' sends one per run. Throttling would also cut repeats.

💡 **Exam cue:** Too many emails → Trigger: Once (and/or throttle).

</details>

**8.4** What does throttling an alert do?

- **A.** Makes the search run faster
- **B.** Deletes old triggered alerts
- **C.** Suppresses repeat triggers for a set period
- **D.** Limits the number of results returned

<details><summary>Show answer</summary>

**C. Suppresses repeat triggers for a set period**

Throttling suppresses the alert from firing again for a set time, optionally per field value.

💡 **Exam cue:** Throttle = suppress repeats.

</details>

**8.5** By default, how long are triggered-alert records kept on the Triggered Alerts page?

- **A.** 7 days
- **B.** 30 days
- **C.** 10 minutes
- **D.** 24 hours

<details><summary>Show answer</summary>

**D. 24 hours**

Triggered alert records expire after 24 hours by default. You can change this per alert.

💡 **Exam cue:** Triggered alerts: 24 h.

</details>

**8.6** Which is NOT one of the four actions a scheduled report can trigger (per the Splunk Core User course)?

- **A.** Add to Triggered Alerts
- **B.** Send email
- **C.** Write results to a CSV lookup
- **D.** Webhook

<details><summary>Show answer</summary>

**A. Add to Triggered Alerts**

Scheduled reports can send email, write to a CSV lookup, call a webhook, or log events. Add to Triggered Alerts is an alert action.

💡 **Exam cue:** Scheduled report actions: email, CSV lookup, webhook, log event.

</details>

**8.7** What happens to a report's Time Range Picker when you schedule the report?

- **A.** It is required to schedule
- **B.** It is removed; the report shows its last scheduled results
- **C.** It becomes a cron expression
- **D.** It stays and controls the schedule

<details><summary>Show answer</summary>

**B. It is removed; the report shows its last scheduled results**

Scheduling a report removes its time picker. The report displays the results of its most recent scheduled run.

💡 **Exam cue:** Scheduling removes the time picker.

</details>

**8.8** Which role, by default, is the minimum needed to create alerts?

- **A.** Alerting
- **B.** Admin
- **C.** Power
- **D.** User

<details><summary>Show answer</summary>

**C. Power**

The Power role can create alerts and share knowledge objects. The User role can't. Admin can, but it isn't the minimum.

💡 **Exam cue:** Alerts need Power (minimum).

</details>
