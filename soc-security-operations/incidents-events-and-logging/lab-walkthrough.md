# Incidents, Events, and Logging — Lab Walkthrough

> **Environment:** Authorized academic lab  
> **Focus:** SOC / security monitoring  
> **Purpose:** Configuring, collecting, and analyzing security logs from multiple sources

This walkthrough documents the successful activities visible in my recorded lab session. The emphasis is on the security evidence produced by Windows, IIS, and Snort and how those logs can support SOC monitoring and investigation.

---

# Exercise 1 — Local Logging: Configuring, Monitoring, and Analyzing Windows Logs

The first exercise focused on Windows security logging and the use of **Event Viewer** to investigate authentication activity.

## Reviewing Windows Security Events

I opened the Windows Security log and reviewed recorded audit events.

![Windows Event Viewer Security log](images/windows-event-viewer-security-log.png)

I inspected individual event properties to understand the information available to an analyst, including timestamps, account information, event IDs, and audit details.

![Windows audit event properties](images/windows-audit-event-properties.png)

The lab included review of successful authentication activity.

![Windows successful logon event](images/windows-successful-logon-event.png)

I also reviewed **Event ID 4625**, which represents a failed account logon.

![Windows failed logon Event ID 4625](images/windows-failed-logon-event-4625.png)

## SOC Perspective

Windows Security logs can provide evidence such as:

- Successful and failed authentication attempts
- Account names
- Logon types
- Source information
- Audit categories
- Event timestamps
- Event IDs such as 4624 and 4625

These events are especially useful when investigating brute-force attempts, compromised accounts, and suspicious login behavior.

---

# Exercise 2 — Local Logging: Configuring, Monitoring, and Analyzing IIS Logs

The second exercise focused on **Internet Information Services (IIS)** logging.

IIS access logs provide a record of activity against a hosted web application and can help analysts reconstruct user and attacker behavior.

## IIS Logging Configuration

I reviewed the IIS logging configuration and the available logging options.

![IIS Manager logging configuration](images/iis-manager-logging-configuration.png)

The exercise also covered the fields included in IIS log records.

![IIS log field configuration](images/iis-log-field-configuration.png)

## Generating Web Activity

I interacted with the lab web application to produce HTTP activity that would be recorded by IIS.

![IIS web activity generation](images/iis-web-activity-generation.png)

## Locating IIS Log Files

The generated IIS logs were stored in the server's logging directory.

![IIS log files directory](images/iis-log-files-directory.png)

I opened an IIS access log and reviewed the recorded HTTP requests.

![IIS access log analysis](images/iis-access-log-analysis.png)

The log files were then collected for further analysis.

![IIS log file collection](images/iis-log-file-collection.png)

## SOC Perspective

IIS logs can expose useful investigation fields such as:

- Client IP address
- Server IP address
- HTTP method
- Requested URI
- Query strings
- HTTP status codes
- User agent
- Timestamp
- Referrer information

These fields can help identify web attacks, abnormal browsing behavior, scanning, and suspicious application requests.

---

# Exercise 3 — Local Logging: Configuring, Monitoring, and Analyzing Snort IDS Logs

The third exercise focused on **Snort IDS**, alert generation, and forwarding security data into Splunk.

## Snort Rule Deployment

I prepared the Snort installation and deployed the required rules into the Snort directory.

![Snort rules deployment](images/snort-rules-deployment.png)

## Snort Network Configuration

The `HOME_NET` value in `snort.conf` was configured for the protected Windows Server.

![Snort HOME_NET configuration](images/snort-home-net-configuration.png)

I also reviewed and configured Snort rule paths and related configuration values.

![Snort rule path configuration](images/snort-rule-path-configuration.png)

## Monitoring Snort Alerts

Snort was started in IDS mode and produced alert output as monitored traffic matched detection rules.

![Snort alert console output](images/snort-alert-console-output.png)

Network-scanning activity generated recognizable IDS alerts.

![Snort port-scan alerts](images/snort-port-scan-alerts.png)

The authorized Kali host was used to generate network activity for detection and analysis.

![Nmap traffic for IDS detection](images/nmap-traffic-for-ids-detection.png)

I reviewed the Snort alert output to inspect the generated detection records.

![Snort alert log review](images/snort-alert-log-review.png)

## SOC Perspective

Snort alerts can provide:

- Alert signature
- Classification
- Priority
- Source and destination IP addresses
- Source and destination ports
- Protocol
- Timestamp

This information allows an analyst to quickly identify suspicious network activity and correlate IDS alerts with other telemetry.

---

# Centralizing Logs with Splunk

The module also demonstrated forwarding security telemetry to a SIEM.

## Installing Splunk Universal Forwarder

I installed the **Splunk Universal Forwarder** on the Windows server.

![Splunk Universal Forwarder installation](images/splunk-universal-forwarder-installation.png)

The forwarder was configured as part of the log-collection workflow.

![Splunk Forwarder setup](images/splunk-forwarder-setup.png)

## Configuring Forwarder Inputs

I reviewed the Universal Forwarder's local configuration files.

![Splunk Forwarder local configuration](images/splunk-forwarder-local-configuration.png)

The `inputs.conf` configuration was used to define data that should be monitored and forwarded.

![Splunk inputs configuration](images/splunk-inputs-configuration.png)

## Configuring the Splunk Receiver

On the Splunk Enterprise instance, I opened the forwarding and receiving settings used to accept data from the forwarder.

![Splunk forwarding and receiving](images/splunk-forwarding-and-receiving.png)

---

# Generating and Correlating Security Events

Additional authorized attack activity was generated in the lab so that the monitoring stack had relevant events to collect.

![SQL Injection traffic generation](images/sql-injection-traffic-generation.png)

## Verifying Data in Splunk

Splunk's **Data Summary** displayed the hosts contributing data to the SIEM.

![Splunk Data Summary hosts](images/splunk-data-summary-hosts.png)

I then searched Windows security data for **EventCode 4625** to isolate failed-logon events.

![Splunk Windows EventCode 4625 search](images/splunk-windows-event-search-4625.png)

## SOC Perspective

This demonstrates a common SOC workflow:

1. Generate or observe suspicious activity
2. Record it in local security logs
3. Forward telemetry to a centralized SIEM
4. Search for specific indicators or event IDs
5. Correlate activity across multiple log sources
