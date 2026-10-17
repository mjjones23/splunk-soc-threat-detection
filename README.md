# SOC Threat Detection & Alert Investigation Using Splunk

## Project Overview
This cybersecurity lab demonstrates how Splunk Enterprise can be used to detect suspicious authentication activity, configure security alerts, and investigate potential brute-force attacks using simulated Windows security logs.

## Tools and Technologies
- Splunk Enterprise
- Splunk Search Processing Language (SPL)
- Windows Security Event Logs
- SOC Alert Configuration
- Security Event Analysis
- Dashboard Visualization

## Project Objectives
- Identify repeated failed login attempts.
- Detect potential brute-force activity.
- Configure a scheduled SOC security alert.
- Analyze successful and failed authentication events.
- Build a five-panel security monitoring dashboard.

## Detection Rule
Created a Splunk detection search to identify five or more failed login attempts from the same source IP within a fixed 10-minute time bucket.

```spl
index=incident_response_lab EventCode=4625
| bin _time span=10m
| stats count as Failed_Attempts by _time Source_IP
| where Failed_Attempts>=5
| eval Severity="Medium"
| eval Detection="Possible Brute Force Attack"
| table _time Source_IP Failed_Attempts Severity Detection
```

## SOC Alert Configuration
- **Alert Name:** SOC - Multiple Failed Login Attempts
- **Alert Type:** Scheduled
- **Schedule:** Daily at 00:00
- **Trigger Condition:** Number of results greater than zero
- **Action:** Add to Triggered Alerts
- **Severity:** Medium

The alert was configured in a Splunk Enterprise trial environment. Continuous monitoring beyond the trial period was not validated.

## Dashboard Panels
1. SOC Threat Detection Alerts
2. Failed Login Attempts by Source IP
3. Authentication Activity Timeline
4. Threat Severity Overview
5. SOC Investigation Details

## Investigation Findings
Analysis of the simulated logs identified:

- Eight failed login attempts from `203.0.113.25`.
- A successful authentication from the same IP following those failures.
- Subsequent access to an application server.
- Two failed administrator login attempts from `198.51.100.44`.
- Ten failed and five successful authentication events overall.

These observations are consistent with possible password guessing followed by successful authentication, but do not independently prove unauthorized access.

## Skills Demonstrated
- SIEM monitoring and event investigation
- SPL search development
- Detection engineering fundamentals
- Security alert configuration
- Authentication log analysis
- SOC dashboard creation
- Incident documentation

## Dashboard Screenshots

### SOC Threat Detection Dashboard
![SOC Threat Detection Dashboard](SOC%20-1.png)

### Threat Severity and Investigation Details
![SOC Investigation Details](SOC%20-2.png)

## Lab Disclaimer
This project uses simulated security event logs and documentation IP addresses. No real-world attack or confirmed compromise is represented.
