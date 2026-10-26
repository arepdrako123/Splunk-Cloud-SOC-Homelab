# Lab setup and workflow

## 1. Import the synthetic dataset

- Use **Splunk Cloud → Settings → Add Data → Upload**.
- Select `splunk_soc_training.log`.
- Use one event per line (line breaking) and inspect the timestamp in preview.
- Assign sourcetype `soc_training` and index `main` (or adjust the queries).

## 2. Validate imported data

```spl
index=main sourcetype=soc_training
| stats count as total_events count(eval(EventCode=4625)) as failures count(eval(EventCode=4624)) as successes
```

Expected: total_events=73, failures=27, successes=46.

## 3. Investigation

- Identify sources with the most failed logons.
- Investigate the most active source and usernames it targeted.
- Check successful logons from the same source; none were found for `203.0.113.45` in this training data.
- Use a 1-minute `timechart` to understand temporal patterns.

## 4. Dashboard

Create `SOC Secure Monitoring` with these panels:

| Panel | Query file | Visualization |
|---|---|---|
| Total Failed Login Attempts | `01-failed-login-total.spl` | Single Value |
| Unique Source IPs | `06-unique-source-ips.spl` | Single Value |
| Targeted User Accounts | `07-targeted-users-total.spl` | Single Value |
| Failed Login Timeline | `04-failed-login-timeline.spl` | Column Chart |
| Top Failed Login Source IPs | `02-source-ip-ranking.spl` | Horizontal Bar Chart |

Use **All time** when reviewing this static, historical dataset.

## 5. Scheduled alert

- Save `08-multi-account-detection.spl` as `SOC - Multiple Failed Login Detection`.
- A cron-based schedule and a triggered-alert action were configured in the UI, and the saved alert appeared as enabled.
- Its *firing behavior was not validated*, and the alert page displayed **no fired events**.
- With a 10-minute lookback, previously uploaded historical data should not re-trigger.
- For future live testing, use an appropriate schedule and time window, and consider `09-rolling-window-alert-option.spl` to avoid splitting a rolling 10-minute window across fixed 10-minute buckets.

## 6. Planned Phase 2

Use a personal lab machine or VM with Splunk Universal Forwarder, following Splunk Cloud's forwarding setup instructions. Do not use employer devices or upload real security logs without permission.
