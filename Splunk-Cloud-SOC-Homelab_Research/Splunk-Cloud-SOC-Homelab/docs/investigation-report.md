# SOC Investigation Report: Repeated Failed Authentication Attempts

**Investigation date:** 2026-10-26  
**Environment:** Splunk Cloud training homelab  
**Data:** Synthetic Windows-style authentication events  
**Status:** Investigated as a lab simulation; not a real-world incident

## Executive summary

The dataset contains 73 events, including 27 failed logons (EventCode 4625). Three distinct source IPs and six distinct targeted usernames appear in the failed-logon subset. One documentation-only IP, `203.0.113.45`, was associated with 22 failed logons against four usernames in the 12:00-12:10 10-minute bucket of the simulated timeline.

## Evidence

| Source IP | Failed logons |
|---|---:|
| 203.0.113.45 | 22 |
| 192.0.2.18 | 4 |
| 192.0.2.12 | 1 |

For `203.0.113.45`:

| Username | Failed logons |
|---|---:|
| admin | 6 |
| administrator | 6 |
| arif | 5 |
| helpdesk | 5 |

The 22 attempts are distributed across roughly 8 minutes in the synthetic data. Searches of EventCode 4624/4625 from the same source found no successful logon events originating from that IP within the dataset.

## Assessment

This resembles **multi-account password guessing** and could be investigated for potential password spraying. A specific attack technique cannot be confirmed from event counts alone. Password contents, affected account context, and authorization details are not available.

**Confidence:** High in the reported event counts; limited confidence in attribution or attack classification because this is synthetic training data.

## Recommended response in a real environment

1. Check account owners, login source context and expected user activity.
2. Review logon types, failure codes, historical baselines, and additional event sources.
3. Correlate failed attempts with successful logons and subsequent account activity.
4. Tune alert thresholds; validate false positives before escalation.
5. Follow the organization's incident response process if unauthorized activity is suspected.

## Alert validation note

A scheduled alert was configured and enabled but had **not fired** at the time its status was captured. The lab demonstrates alert configuration, **not** successful end-to-end triggering or response.
