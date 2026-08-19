# Incident Response — Lab Walkthrough

> **Environment:** Authorized academic lab  
> **Focus:** SOC / Incident Response  
> **Purpose:** Practicing the incident-response lifecycle from detection through recovery

This walkthrough documents the successful activities visible in my recorded lab session. The portfolio focuses on the meaningful incident-response workflow and omits routine setup details that do not add investigative value.

---

# 1. Preparing Assets for Security Monitoring

The first section used **AlienVault OSSIM** to manage monitored assets and prepare a Windows Server system for security-event correlation.

I reviewed the monitored asset inventory in OSSIM.

![OSSIM asset inventory](images/ossim-asset-inventory.png)

A Windows Server asset was configured with its identifying and operating-system information.

![OSSIM Windows asset configuration](images/ossim-windows-asset-configuration.png)

### Incident Response Relevance

Accurate asset context helps an analyst understand:

- Which system generated an event
- The system's role and operating system
- Whether an alert involves a critical server or endpoint
- Which host should be prioritized during containment

---

# 2. Building a Brute-Force Correlation Directive

The lab then used OSSIM **Correlation Directives** to identify a sequence of authentication events associated with a brute-force attempt.

![OSSIM correlation directives](images/ossim-correlation-directives.png)

Failed-logon event signatures were selected as part of the correlation logic.

![OSSIM failed-login event signatures](images/ossim-failed-login-event-signatures.png)

The directive also incorporated successful-authentication behavior so a sequence of failures followed by a successful login could receive higher investigative priority.

![OSSIM authentication success rule](images/ossim-authentication-success-rule.png)

Reliability values were configured to strengthen the correlation as additional conditions were matched.

![OSSIM correlation reliability](images/ossim-correlation-reliability.png)

### SOC Perspective

This is more useful than treating every failed login as an isolated event. Correlation can identify a meaningful sequence such as:

**Repeated failures → successful authentication → elevated/suspicious account activity**

---

# 3. Alarm Triage and Incident Ticketing

After the relevant activity was detected, OSSIM generated a **Brute Force Attempt** alarm.

![OSSIM brute-force alarm](images/ossim-bruteforce-alarm.png)

The alarm was then converted into an incident ticket for tracking and response.

![OSSIM incident ticket](images/ossim-incident-ticket.png)

### What I Practiced

- Reviewing an alarm's source and destination
- Understanding the attack pattern and risk information
- Escalating a detection into an incident ticket
- Moving from automated detection into a managed response workflow

---

# 4. Monitoring FTP Activity

The next section focused on activity involving an FTP service.

Splunk Universal Forwarder parsing/configuration was prepared so FTP-related telemetry could be collected for analysis.

![Splunk FTP source configuration](images/splunk-ftp-source-configuration.png)

From the Kali lab system, I confirmed the FTP service was exposed on the authorized target.

![FTP service enumeration](images/ftp-service-enumeration.png)

The Windows client then established an FTP session using FileZilla. The client explicitly identified the connection as insecure because the service did not use FTP over TLS.

![FileZilla insecure FTP session](images/filezilla-insecure-ftp-session.png)

### Incident Response Relevance

FTP activity can provide useful evidence such as:

- Source and destination addresses
- Authentication attempts
- File-transfer activity
- Session timestamps
- Use of insecure protocols

---

# 5. Containing the FTP Service

As part of the response workflow, the FTP service on the affected Windows Server was stopped.

![FTP service containment](images/ftp-service-containment.png)

### Containment Objective

Stopping the exposed service reduces the immediate attack surface and interrupts further access through that service while investigation and remediation continue.

---

# 6. Web-Application Hardening with UrlScan and IIS

The lab then moved to protective controls for the web application.

The UrlScan security files were added to the application environment.

![UrlScan security files](images/urlscan-security-files.png)

A custom **SQL Injection** rule set was configured in `UrlScan.ini`.

![UrlScan SQL Injection rule](images/urlscan-sql-injection-rule.png)

The UrlScan component was then integrated into the application through **IIS ISAPI Filters**.

![IIS UrlScan ISAPI filter](images/iis-urlscan-isapi-filter.png)

The web application was accessed again to validate the environment after the security configuration.

![Web application validation](images/web-application-validation.png)

Additional authorized web-attack test activity was performed against the application.

![XSS test activity](images/xss-test-activity.png)

The IIS application configuration was reviewed as part of the hardening workflow.

![IIS web application configuration](images/iis-web-application-configuration.png)

### Incident Response Relevance

Remediation after a web incident may include:

- Blocking known malicious request patterns
- Hardening web-server configuration
- Reducing the vulnerable attack surface
- Validating that application functionality remains available after security changes

---

# 7. Improving PowerShell Visibility

The endpoint-monitoring portion of the lab focused on improving visibility into PowerShell activity.

Windows Group Policy was used to access PowerShell logging controls.

![PowerShell logging policy](images/powershell-logging-policy.png)

**PowerShell Script Block Logging** was enabled to capture more detailed command/script activity.

![PowerShell Script Block Logging](images/powershell-script-block-logging.png)

### SOC Perspective

PowerShell logging is valuable because attackers and administrators both use PowerShell extensively. Script Block Logging gives analysts additional visibility into commands that may otherwise be difficult to reconstruct after an incident.

---

# 8. Enabling Remote-Response Capabilities

PowerShell remoting was configured using **WinRM / Enable-PSRemoting**.

![PowerShell remoting WinRM](images/powershell-remoting-winrm.png)

Remote PowerShell activity was then used in the controlled response scenario.

![Remote response PowerShell](images/remote-response-powershell.png)

Permissions on the evidence/data drive were also reviewed as part of the workflow.

![Evidence drive permissions](images/evidence-drive-permissions.png)

### Incident Response Relevance

Remote administration can help responders:

- Collect information
- Execute approved response actions
- Access affected systems without physical interaction
- Coordinate containment across endpoints

Strong authentication, authorization, and logging remain essential when remote administration is enabled.

---

# 9. Detecting Remote PowerShell Activity in Splunk

The enhanced PowerShell telemetry was visible in **Splunk**.

The recorded event shows a remote PowerShell `Remove-Item` command targeting a folder, along with command invocation and parameter details.

![Splunk remote PowerShell detection](images/splunk-remote-powershell-detection.png)

### SOC Perspective

This demonstrates why enhanced endpoint logging matters. An analyst can use the captured PowerShell telemetry to understand:

- Which command executed
- Which host executed it
- The command parameters
- The target path
- The context of the remote session

This type of evidence is useful when investigating destructive or unauthorized administrative activity.

---

# 10. Recovery of Deleted Data

The final portion of the recorded workflow included recovery of deleted data.

Recovered evidence/files were located after the recovery process.

![Recovered evidence files](images/recovered-evidence-files.png)

### Recovery Phase

Recovery is a core part of incident response. Depending on the incident, this can include:

- Restoring deleted or damaged data
- Returning systems to an operational state
- Validating recovered information
- Preserving relevant evidence for later investigation

---

# Key Takeaways

This module connected several stages of the incident-response lifecycle into one practical workflow:

## Detection
- Asset monitoring
- Event correlation
- Brute-force alarm generation

## Triage
- Alarm review
- Incident-ticket creation
- Investigating authentication and service activity

## Containment
- Stopping an exposed FTP service

## Remediation / Hardening
- Adding web-application filtering controls
- Reviewing IIS configuration

## Improved Visibility
- Enabling PowerShell Script Block Logging
- Capturing remote command activity in Splunk

## Recovery
- Recovering deleted data after the simulated incident activity

The strongest lesson from the module was that incident response is not only about detecting an alert; it requires **triage, containment, remediation, evidence collection, and recove
