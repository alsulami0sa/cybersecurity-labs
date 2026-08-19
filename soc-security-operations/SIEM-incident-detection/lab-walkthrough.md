# Incident Detection with SIEM — Lab Walkthrough

> **Environment:** Authorized academic lab  
> **Focus:** SOC / Security Monitoring  
> **Purpose:** Building and validating practical SIEM detection use cases

This walkthrough documents the main successful activities shown in my recorded lab session. The emphasis is on detection logic, alert creation, and the telemetry a SOC analyst can use to identify suspicious behavior.

---

# Exercise 1 — Detecting Failed Login Attempts

The first exercise focused on detecting repeated Windows authentication failures in **Splunk**.

I searched Windows security events associated with successful and failed logons and used the resulting data to build a failed-login detection use case.

![Splunk failed-login search](images/splunk-failed-login-search.png)

I configured the search as a Splunk alert so repeated authentication failures could be surfaced automatically.

![Failed-login alert configuration](images/failed-login-alert-configuration.png)

Authorized authentication testing was generated from the Kali lab system to create realistic failed-login telemetry.

![Hydra authentication testing](images/hydra-authentication-testing.png)

Splunk statistics were then used to summarize attempts, failures, and successful authentications by account.

![Failed-login statistics](images/failed-login-statistics.png)

## SOC Perspective

Useful indicators include:

- Multiple failed logins within a short time window
- Repeated failures against the same account
- Multiple accounts targeted from one source
- A successful login following numerous failures
- Windows authentication event IDs such as 4624 and 4625

---

# Exercise 2 — Detecting SQL Injection Attempts

The second exercise focused on application-level detection of **SQL Injection** attempts.

Authorized SQL Injection activity was generated against the vulnerable lab application.

![SQL Injection activity](images/sql-injection-activity.png)

I searched IIS/web telemetry in Splunk for request patterns associated with SQL Injection.

![Splunk SQL Injection search](images/splunk-sql-injection-search.png)

The resulting search was used as a reusable SIEM detection use case.

![SQL Injection alert use case](images/sql-injection-alert-use-case.png)

## SOC Perspective

Common SQL Injection indicators include:

- SQL keywords embedded in query strings
- Encoded quote characters
- `UNION SELECT`-style patterns
- Repeated abnormal requests to the same endpoint
- Database-oriented payloads appearing in HTTP logs

---

# Exercise 3 — Detecting Cross-Site Scripting (XSS)

The third exercise created a Splunk detection use case for **XSS** attempts.

I searched web telemetry for script-related indicators and suspicious request content.

![Splunk XSS search](images/splunk-xss-search.png)

The detection was configured as an alert.

![XSS alert configuration](images/xss-alert-configuration.png)

Authorized XSS activity was then generated in the lab application.

![XSS execution result](images/xss-execution-result.png)

The corresponding web event was reviewed in Splunk.

![XSS event analysis](images/xss-event-analysis.png)

## SOC Perspective

Potential XSS indicators include:

- `<script>` or encoded equivalents
- JavaScript-related keywords in request parameters
- Suspicious HTML event handlers
- Repeated script payloads in user-controlled fields

---

# Exercise 4 — Detecting Network Scanning

The fourth exercise focused on **network-level incident detection**.

A Splunk alert was configured for network-scanning indicators.

![Network-scan alert configuration](images/network-scan-alert-configuration.png)

**Snort IDS** generated alerts as scan activity was observed.

![Snort network-scan alerts](images/snort-network-scan-alerts.png)

The Kali lab system was used to generate scan traffic, including FIN/XMAS-style scanning.

![Nmap FIN/XMAS scans](images/nmap-fin-xmas-scans.png)

The resulting scan detections were reviewed through the Splunk monitoring workflow.

![Splunk network-scan alerts](images/splunk-network-scan-alerts.png)

## SOC Perspective

Network-scan indicators may include:

- One source contacting many destination ports
- FIN/XMAS/TCP scan signatures
- IDS alerts from Snort
- Short-lived connection attempts
- Multiple scan patterns from the same source address

---

# Exercise 5 — Detecting Insecure Services

The next exercise monitored network connections using **Netstat** data forwarded into Splunk.

A monitoring script was placed in the Splunk Universal Forwarder workflow so network-state information could be collected regularly.

![Netstat monitoring script](images/netstat-monitor-script.png)

The forwarder input was configured to ingest the script output.

![Splunk Netstat input configuration](images/splunk-netstat-input-configuration.png)

A Splunk alert was created to detect an insecure service condition.

![Insecure-service alert saved](images/insecure-service-alert-saved.png)

The saved searches and security alerts were reviewed in Splunk.

![Splunk saved security alerts](images/splunk-saved-security-alerts.png)

Triggered-alert results showed detection of the monitored Telnet port condition.

![Telnet port triggered alerts](images/telnet-port-triggered-alerts.png)

## SOC Perspective

This type of use case can help identify:

- Insecure services exposed on endpoints
- Unexpected listening ports
- Legacy protocols such as Telnet
- Changes in network-service state
- Services that violate a security baseline

---

# Exercise 6 — Endpoint Telemetry with Sysmon and Winlogbeat

The final section extended host visibility using **Sysmon** and **Winlogbeat**.

## Sysmon Configuration

I prepared a Sysmon configuration intended to increase Windows endpoint visibility and collect richer process/network telemetry.

![Sysmon configuration](images/sysmon-configuration.png)

## Winlogbeat Preparation

Winlogbeat was prepared on the Windows host as the event-shipping component.

![Winlogbeat package preparation](images/winlogbeat-package-preparation.png)

The Winlogbeat YAML configuration was updated to include Windows event-log channels such as Security and Sysmon Operational telemetry.

![Winlogbeat event-log configuration](images/winlogbeat-event-log-configuration.png)

The configuration also included the destination used for Kibana-related setup.

![Winlogbeat Kibana output configuration](images/winlogbeat-kibana-output-configuration.png)

The Elasticsearch output destination was configured for event forwarding.

![Winlogbeat Elasticsearch output configuration](images/winlogbeat-elasticsearch-output-configuration.png)

## SOC Perspective

Sysmon + Winlogbeat can provide richer endpoint telemetry such as:

- Process creation
- Network connections
- File activity
- Hashes
- Parent/child process relationships
- Windows Security events
- Endpoint events forwarded to a centralized analytics platform

---

# Key Takeaways

This module gave me hands-on exposure to the core SIEM detection lifecycle:

1. Identify suspicious behavior
2. Determine the available log source
3. Build a search or detection query
4. Configure an alert
5. Generate authorized test activity
6. Review the resulting telemetry
7. Validate whether the detection logic identifies the expected behavior

The lab covered detections for:

- Failed logins / brute force
- SQL Injection
- XSS
- Network scanning
- Insecure/open services

It also introduced endpoint telemetry collection using:

- Sysmon
- Winlogbeat
- Elasticsearch / Kibana-oriented configuration
