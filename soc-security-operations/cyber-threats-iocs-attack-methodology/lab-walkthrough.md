# Understanding Cyber Threats, IoCs, and Attack Methodology — Lab Walkthrough

> **Environment:** Authorized academic lab  
> **Focus:** SOC / defensive security  
> **Purpose:** Understanding how attacks generate indicators that defenders can detect and investigate

This walkthrough documents the successful lab activities visible in my recorded session. The focus is on the observable evidence produced by each attack and how that evidence can support SOC detection and investigation.

---

# Exercise 1 — Application-Level Threats: SQL Injection

The first exercise demonstrated how a SQL Injection attack can manipulate a vulnerable web application and expose database information.

I interacted with the vulnerable application and observed how crafted input changed the application's database query behavior.

![SQL Injection request](images/sql-injection-request.png)

The application returned database content that would not normally be exposed to the user.

![SQL Injection database results](images/sql-injection-database-results.png)

Additional records were exposed during the exercise, demonstrating the potential confidentiality impact of a successful SQL Injection attack.

![SQL Injection extracted records](images/sql-injection-extracted-records.png)

### SOC perspective

Useful indicators can include:

- Unusual SQL keywords in HTTP requests
- Encoded query strings
- Repeated abnormal requests to the same endpoint
- Unexpected database errors or responses
- Requests attempting to enumerate database tables or records

---

# Exercise 2 — Application-Level Threats: Cross-Site Scripting (XSS)

The second exercise demonstrated XSS behavior in the same web-application environment.

A script payload was submitted through an application input field.

![XSS payload submission](images/xss-payload-submission.png)

The browser executed the injected script and displayed an alert, confirming the effect of the XSS payload.

![XSS alert result](images/xss-alert-result.png)

### SOC perspective

Potential XSS indicators include:

- `<script>` or encoded script content in HTTP requests
- Suspicious JavaScript event handlers
- Unexpected browser-side execution
- Repeated requests containing HTML/JavaScript payloads

---

# Exercise 3 — Network-Level Threats: Network Scanning

The third exercise demonstrated multiple Nmap scanning techniques against the authorized target.

## SYN Scan

A TCP SYN scan was used to identify exposed ports and services.

![Nmap SYN scan](images/nmap-syn-scan.png)

## TCP Full-Connect Scan

A TCP connect scan was used to establish complete TCP connections to candidate ports.

![Nmap TCP connect scan](images/nmap-tcp-connect-scan.png)

## UDP Scan

A UDP scan was used to identify UDP services.

![Nmap UDP scan](images/nmap-udp-scan.png)

### SOC perspective

Network-scanning IoCs can include:

- One source contacting many destination ports
- Large numbers of short-lived connection attempts
- Repeated SYN packets
- Sequential or patterned port access
- UDP probes across multiple services

These patterns are useful for IDS, firewall, and SIEM detection rules.

---

# Exercise 4 — Host-Level Threats: Brute-Force Attacks

The fourth exercise demonstrated a controlled brute-force authentication attack using **Hydra**.

The tool attempted credential combinations against the authorized target service.

![Hydra brute-force attempt](images/hydra-bruteforce-attempt.png)

The exercise produced a successful authentication result in the controlled lab environment.

![Hydra successful login](images/hydra-successful-login.png)

### SOC perspective

Typical brute-force indicators include:

- Many failed authentication attempts from the same source
- Repeated attempts against one or more accounts
- A successful login immediately after a high volume of failures
- Abnormal authentication timing or source address behavior

This type of pattern can be detected through authentication logs and correlation rules.

---

# Exercise 5 — Detecting and Analyzing IoCs with Wireshark

The final exercise shifted fully to the defender's perspective and used **Wireshark** to analyze network evidence.

## Identifying SYN-Scan Traffic

The packet capture showed SYN-based scanning behavior.

![Wireshark SYN scan analysis](images/wireshark-syn-scan-analysis.png)

## Identifying TCP Connect Scanning

A different packet pattern was reviewed to understand full TCP connection attempts.

![Wireshark TCP connect analysis](images/wireshark-tcp-connect-analysis.png)

## Identifying UDP Scanning

UDP traffic was analyzed for repeated probes to multiple ports/services.

![Wireshark UDP scan analysis](images/wireshark-udp-scan-analysis.png)

## ICMP Analysis

ICMP traffic was reviewed as another source of network-discovery evidence.

![Wireshark ICMP analysis](images/wireshark-icmp-analysis.png)

## OS-Fingerprinting Indicators

The capture also contained traffic useful for identifying operating-system fingerprinting behavior.

![Wireshark OS fingerprinting analysis](images/wireshark-os-fingerprinting-analysis.png)

## Recognizing the Overall Scan Pattern

Viewing the traffic together made the repeated probing behavior easier to identify.

![Wireshark network scan pattern](images/wireshark-network-scan-pattern.png)

---

## Malware-Traffic Analysis

The exercise then moved to a malware-related PCAP.

HTTP traffic was filtered and reviewed for suspicious communication patterns.

![Wireshark malware HTTP traffic](images/wireshark-malware-http-traffic.png)

Suspicious HTTP POST requests were identified in the packet capture.

![Wireshark suspicious POST request](images/wireshark-suspicious-post-request.png)

I followed the TCP stream to inspect the application-layer communication and identify additional indicators such as host information and request metadata.

![Wireshark Follow TCP Stream](images/wireshark-follow-tcp-stream.png)

### SOC perspective

Useful malware-traffic IoCs can include:

- Suspicious destination domains or IP addresses
- Unusual HTTP POST requests
- Repeated beacon-like communication
- Suspicious URI paths
- User-agent anomalies
- Connections to infrastructure associated with malware

---

# Threat-Intelligence Validation

After identifying suspicious network indicators, the exercise used external analysis platforms to add context.

## VirusTotal

A suspicious host identified during packet analysis was checked with **VirusTotal**.

![VirusTotal suspicious host analysis](images/virustotal-suspicious-host-analysis.png)

This demonstrates a common SOC workflow: extract an indicator from telemetry and enrich it using a threat-intelligence source.

## PacketTotal / DynamiteLab

The PCAP was also reviewed with an online packet-analysis platform.

![PacketTotal PCAP upload](images/packettotal-pcap-upload.png)

The platform summarized the capture and exposed additional network relationships and indicators.

![PacketTotal PCAP analysis](images/packettotal-pcap-analysis.png)

The analysis view provided additional network-level context useful during an investigation.

![PacketTotal network indicators](images/packettotal-network-indicators.png)

---

# Key Takeaways

This module connected offensive behavior directly to defensive detection.

I practiced recognizing IoCs generated by:

- SQL Injection
- XSS
- SYN scanning
- TCP connect scanning
- UDP scanning
- Brute-force authentication
- Suspicious HTTP malware traffic

I also practiced:

- Packet filtering
- TCP stream reconstruction
- Network-traffic investigation
- IOC extraction
- Threat-intelligence enrichment
- Thinking about how attack behavior would appear in SIEM, IDS, firewall, and authentication logs
