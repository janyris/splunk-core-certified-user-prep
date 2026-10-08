# 8 — Creating Scheduled Reports and Alerts (5%)

---

## 8.1 Scheduled reports

- Report → **Edit → Edit Schedule** → *Schedule Report* ✔.
- Settings:
  - **Schedule:** run every hour / day / week / month, or a **cron** expression.
  - **Time range** each run covers.
  - **Schedule priority** and **schedule window** (lets Splunk delay the run to balance load).
- **Actions (up to 4):** **email** a summary · **write results to a CSV lookup** · **webhook** · **log and index searchable events**.
- **Scheduling a report removes its time range picker.** **Schedule Window** lets the scheduler delay this report so higher-priority reports run first.
- **A scheduled report runs its action EVERY time it runs.** **A scheduled alert runs its action ONLY when the trigger condition is met.**
- Only scheduled reports can be **embedded**.

## 8.2 Alerts: the workflow

**Search** (what to track) → **Alert type** (how often to check) → **Trigger condition + throttling** (when to fire) → **Alert action** (what happens).

Create one with: run the search → **Save As → Alert**.

### Two alert types

| | **Scheduled** | **Real-time** |
|---|---|---|
| Runs | On a schedule (e.g. daily at 23:00) | **Continuously** |
| Use when | Immediate response isn't needed; lower load | Immediate response matters |
| Cost | Light | **Heavier on resources** (always running) |
| Example | "Fewer than 800 sales today" → check daily at 23:00 | "HTTP 5xx errors on web servers" / "> 20 failed logins in 5 min" |

### Real-time trigger styles

- **Per-result:** fires on **every** matching event. **Throttle** it to avoid floods. Example: suppress the same `status` value for 60 minutes.
- **Rolling time window:** fires when a condition is met **within a window**. Example: *Number of Results > 20 in 5 minutes* (brute-force detection).

### Save As Alert dialog

| Setting | Notes |
|---|---|
| Title / Description | Use a naming convention (`SEC_Alert_20_failed_login`) |
| **Permissions** | **Private** or **Shared in App** |
| **Alert type** | **Scheduled** or **Real-time** |
| **Expires** | How long **triggered-alert records** stay on the Triggered Alerts page. **Default 24 hours** (can be set, e.g., to 7 days) |
| **Trigger condition** | **Per-Result · Number of Results · Number of Hosts · Number of Sources · Custom**; with *is greater than / less than / equal to…* and an optional *in X minutes* window |
| **Trigger** | **Once** (one action per run) or **For each result** |
| **Throttle** | **Suppress** re-triggering for a period, optionally per field value. Stops alert floods |
| **Trigger actions** | **Add to Triggered Alerts** (with **Severity**: Info / Low / Medium / High / Critical), **Send email**, **Log event**, **Output results to lookup**, **Webhook**, Run a script (legacy) |

- **Severity** is a **label** for filtering on the Triggered Alerts page. It doesn't change behavior.

## 8.3 Viewing triggered alerts

- **Activity → Triggered Alerts** lists every alert instance that has fired.
- An alert appears there only if:
  1. **Add to Triggered Alerts** is one of its actions
  2. it triggered recently
  3. its expiration hasn't passed (default **24 h**)
  4. nobody deleted the listing
- **View results** opens the triggering events in Search. That's your starting point for troubleshooting.
- **Alerts** tab (app nav) lists all alerts: edit, enable/disable, clone, delete, permissions.
- To stop an alert's listings: update its expiration, delete the listing, or **disable** the alert.

---

## ⏱️ 60-second self-check

1. The two alert types?
2. Scheduled report vs scheduled alert: when does each run its action?
3. Pro and con of a real-time alert?
4. Per-result vs rolling-window triggering?
5. When would you throttle an alert?
6. How long are triggered alerts kept by default? Where do you see them?
7. What does the Severity setting do?

<details><summary>Answers</summary>

1. Scheduled and real-time
2. Report: every time it completes. Alert: only when the trigger condition is met.
3. Pro: immediate detection. Con: heavy on resources (runs continuously).
4. Per-result fires on every matching event; rolling window fires when a count is reached within a time window
5. To stop repeated alerts flooding you for the same issue (e.g. a server spitting out 5xx errors)
6. 24 hours; Activity → Triggered Alerts
7. Adds a label for filtering and finding alerts on the Triggered Alerts page

</details>
