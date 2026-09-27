# Cybersecurity Labs

A portfolio of **hands-on lab modules** completed in authorized, controlled environments. The work focuses on SOC monitoring and detection, incident response, threat intelligence, digital forensics, and the analysis of web and mobile security risks.

Each module has a short overview, a detailed walkthrough, and screenshots of the lab work. Start with a topic below, then open its `lab-walkthrough.md` for the steps, observations, and any limitations recorded during the exercise.

## SOC and Security Operations

| Lab | What it covers |
| --- | --- |
| [Cyber Threats, IoCs, and Attack Methodology](soc-security-operations/cyber-threats-iocs-attack-methodology/) | SQL injection, XSS, scanning, brute-force behavior, packet evidence, and threat-intelligence checks. |
| [Incidents, Events, and Logging](soc-security-operations/incidents-events-and-logging/) | Windows, IIS, and Snort logs; event generation, forwarding, and searching in Splunk. |
| [Incident Detection with SIEM](soc-security-operations/SIEM-incident-detection/) | Splunk detection use cases for failed logins, web attacks, scans, and insecure services; Sysmon and Winlogbeat telemetry. |
| [Enhanced Incident Detection with Threat Intelligence](soc-security-operations/threat-intelligence-incident-detection/) | IoC enrichment in an Elasticsearch/Logstash/Kibana pipeline and OTX integration with AlienVault OSSIM. |
| [Incident Response](soc-security-operations/incident-response/) | OSSIM correlation and triage, incident ticketing, FTP containment, IIS hardening, PowerShell visibility, and recovery. |

## Digital Forensics and Investigation

| Lab | What it covers |
| --- | --- |
| [Computer Forensics Investigation Process](digital-forensics/computer-forensics-investigation-process/) | Deleted-file recovery, hashing, integrity comparison, file inspection, evidence handling, and disk imaging. |
| [Data Acquisition and Duplication](digital-forensics/data-acquisition-and-duplication/) | Disk and memory acquisition, E01-to-dd conversion, image mounting, NTFS examination, and PyTSK. |
| [Windows Forensics](digital-forensics/windows-forensics/) | Live artifacts, memory, registry, browser history, processes and DLLs, and Windows event logs. |
| [Network Forensics](digital-forensics/network-forensics/) | FTP and SSH authentication evidence, TCP streams, SYN floods, ARP poisoning, and a PyShark capture exercise. |
| [Investigating Web Attacks](digital-forensics/investigating-web-attacks/) | Splunk and Python analysis of web logs for XSS, SQL injection, traversal, command injection, XXE, and brute-force activity. |

## Web and Mobile Security

| Lab | What it covers |
| --- | --- |
| [SQL Injection](web-security/sql-injection/) | Controlled SQLmap enumeration, OWASP ZAP findings, and AI-assisted testing. |
| [Web Server Security](web-security/web-server-security/) | Banner grabbing, Nmap enumeration, WAF detection, FTP assessment, and Log4j lab analysis. |
| [Android Security](mobile-security/android-security/) | Exposed ADB, device and package enumeration, malicious APK risks, and mobile threat detection. |

## Tools and Methods Demonstrated

- **Monitoring and detection:** Splunk Enterprise, Splunk Universal Forwarder, AlienVault OSSIM/OTX, Elasticsearch, Logstash, Kibana, Winlogbeat, Sysmon, and Snort.
- **Traffic and log analysis:** Wireshark, Nmap, PyShark, Python, Windows Event Viewer, IIS logs, and Linux command-line log filters.
- **Forensics:** FTK Imager, `dd`, xmount, Belkasoft RAM Capturer, Volatility, Redline, MemProcFS, DiskExplorer, and PyTSK.
- **Controlled security testing:** SQLmap, OWASP ZAP, Hydra, Netcat, Searchsploit, ADB, and PhoneSploit-Pro.
