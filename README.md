# Splunk Cloud SOC Homelab

**Security Log Analysis | SOC Investigation | SPL Detection Engineering | Dashboarding**

## Project Overview

I built this hands-on Security Operations Center (SOC) homelab using **Splunk Cloud Platform** to develop practical skills in security monitoring, log analysis, threat detection, and incident investigation.

The project focuses on analyzing simulated Windows authentication logs to identify suspicious login attempts, investigate potentially malicious activity, and create security monitoring dashboards using Splunk's Search Processing Language (SPL).

Through this project, I gained practical experience in how SOC Analysts investigate security events, analyze authentication patterns, and develop detection rules to identify potential security threats.

> **Project Scope:** This homelab uses simulated Windows-style security logs for educational purposes. Real-time Windows log monitoring is planned for a future phase.

## Project Objectives

- Collect and analyze security logs using Splunk Cloud.
- Investigate successful and failed Windows authentication events.
- Identify suspicious source IP addresses and targeted user accounts.
- Develop SPL queries for security investigation and threat detection.
- Build an interactive SOC security monitoring dashboard.
- Configure scheduled security detection alerts.
- Document investigation findings and security monitoring results.

## Tools and Technologies

| Technology | Purpose |
|---|---|
| Splunk Cloud Platform | Security log management and analysis |
| Splunk SPL | Searching, filtering, and analyzing security events |
| Windows Authentication Logs | Simulated login activity for security investigation |
| Splunk Dashboards | Visualizing authentication activity |
| Splunk Scheduled Alerts | Configuring automated security detections |
| GitHub | Technical documentation and project portfolio |

## SOC Monitoring Dashboard

I developed a dashboard called **SOC Secure Monitoring** to visualize authentication activity and support security investigations.

The dashboard provides visibility into failed login attempts, source IP addresses, targeted accounts, and authentication trends.


## Security Investigation

### Investigation: Suspicious Authentication Attempts

During the investigation, I identified repeated failed login attempts originating from a single source IP address.

The following findings were observed:

| Source IP Address | Failed Attempts |
|---|---:|
| 203.0.113.45 | 22 |
| 192.0.2.18 | 4 |
| 192.0.2.12 | 1 |

The source IP `203.0.113.45` accounted for approximately **81.5% of all failed login attempts**, making it the primary focus of the investigation.

Further analysis identified authentication attempts targeting four accounts:

- admin — 6 failed attempts
- administrator — 6 failed attempts
- arif — 5 failed attempts
- helpdesk — 5 failed attempts

The attempts occurred repeatedly within approximately eight minutes.

The observed behavior was consistent with possible multi-account password guessing. However, additional authentication evidence would be required to confirm password spraying or another specific attack technique.

No successful login from the investigated source IP was identified in the available training dataset.



## SPL Detection Rule Development

I created a custom SPL detection query to identify repeated authentication failures targeting multiple user accounts.

The detection uses the following conditions:

- At least 10 failed login attempts.
- At least 3 distinct targeted user accounts.
- Authentication failures grouped into 10-minute time intervals.
- Events originating from the same source IP address.

**SPL Detection Query:**

```spl
index=main sourcetype=soc_training EventCode=4625
| bin _time span=10m
| stats count as failed_attempts dc(user) as targeted_users by _time src_ip
| where failed_attempts >= 10 AND targeted_users >= 3
| sort -failed_attempts
```

### Detection Result



The query successfully identified a simulated suspicious authentication pattern involving 22 failed login attempts against four accounts.

This demonstrates how SPL can be used to develop custom security detections based on authentication activity.

## Scheduled Security Alert

I configured a scheduled alert named **SOC - Multiple Failed Login Detection** in Splunk Cloud.

The alert is designed to identify source IP addresses exceeding the configured failed-login threshold.



**Alert Status:** Successfully configured and enabled.

At the time of documentation, no alert firing had been observed because the uploaded training logs were historical. End-to-end alert triggering will be validated using fresh security events in a future phase.

## Skills and Knowledge Gained

Through this project, I gained practical exposure to:

- Security Information and Event Management (SIEM)
- Splunk Search Processing Language (SPL)
- Windows Security Event ID analysis
- SOC monitoring and security investigation
- Authentication log correlation
- Suspicious login pattern identification
- Dashboard development and security visualization
- Detection rule configuration
- Technical documentation and incident reporting

The project strengthened my understanding of how SOC Analysts use log data to investigate suspicious activity and support security monitoring operations.

## Future Improvements

- [x] Upload simulated security logs to Splunk Cloud
- [x] Investigate failed authentication attempts
- [x] Analyze targeted user accounts
- [x] Create SOC monitoring dashboards
- [x] Develop custom SPL detection queries
- [x] Configure a scheduled security alert
- [ ] Test alert triggering using new security events
- [ ] Install Splunk Universal Forwarder on a Windows lab system
- [ ] Collect real Windows Security Event Logs
- [ ] Improve detections and investigate false positives

## Conclusion

This homelab provided hands-on experience with security event monitoring, authentication log analysis, and basic detection engineering using Splunk Cloud.

By developing SPL queries, investigating suspicious login activity, and creating a SOC dashboard, I strengthened my foundational cybersecurity skills and gained practical experience relevant to entry-level SOC Analyst and Security Operations roles.

I plan to expand this project by integrating real-time Windows event monitoring and conducting more advanced security investigations.

---

**Disclaimer:** All security logs and IP addresses used in this project are simulated or reserved for documentation. The findings do not represent an actual cyberattack.
