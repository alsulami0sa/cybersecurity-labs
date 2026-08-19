# Enhanced Incident Detection with Threat Intelligence — Lab Walkthrough

> **Environment:** Authorized academic lab  
> **Focus:** SOC / Threat Intelligence / SIEM  
> **Purpose:** Enriching security telemetry with Indicators of Compromise (IoCs) and external threat-intelligence data

This walkthrough documents the main successful activities shown in my recorded lab session. Sensitive authentication material such as API keys and credentials is intentionally excluded from the public documentation.

---

# Exercise 1 — Integrating IoCs into ELK Stack

The first exercise built an **ELK Stack** workflow for collecting Windows events and enriching them with malware Indicators of Compromise.

## 1. Preparing the ELK Environment

I prepared the Elasticsearch, Logstash, and Kibana components used in the lab environment.

![ELK lab files preparation](images/elk-lab-files-preparation.png)

Supporting service-management tooling was also prepared for running the ELK components as Windows services.

![ELK components and service tools](images/elk-components-and-service-tools.png)

## 2. Configuring Elasticsearch

Elasticsearch was configured as a Windows service so it could run continuously as the storage and search layer of the monitoring stack.

![Elasticsearch service configuration](images/elasticsearch-service-configuration.png)

## 3. Configuring Logstash

I created and configured the Logstash pipeline used to receive Windows events and process them before forwarding them to Elasticsearch.

![Logstash pipeline configuration](images/logstash-pipeline-configuration.png)

The Logstash service was then verified as running.

![Logstash service running](images/logstash-service-running.png)

## 4. Configuring Kibana

Kibana was prepared as the visualization and investigation interface for the ELK Stack.

![Kibana service and configuration](images/kibana-service-and-configuration.png)

### SOC relevance

Together, these components provide:

- Centralized log collection
- Event processing and enrichment
- Search and investigation
- A visual interface for reviewing security telemetry

---

# Configuring Windows Event Forwarding with Winlogbeat

The Windows endpoint was prepared with **Winlogbeat** to forward event data into the ELK pipeline.

![Winlogbeat endpoint preparation](images/winlogbeat-endpoint-preparation.png)

## Kibana Endpoint Configuration

The Winlogbeat configuration was updated with the Kibana endpoint used by the lab.

![Winlogbeat Kibana endpoint](images/winlogbeat-kibana-endpoint.png)

## Logstash Output Configuration

Winlogbeat was configured to send collected events to the Logstash service.

![Winlogbeat Logstash output](images/winlogbeat-logstash-output.png)

## Validating the ELK Services

The ELK-related Windows services were checked to confirm the monitoring stack was running.

![ELK services validation](images/elk-services-validation.png)

Winlogbeat setup was then used to prepare its index/template information for Elasticsearch and Kibana.

![Winlogbeat setup and index template](images/winlogbeat-setup-and-index-template.png)

---

# Adding Malware IoCs to the Detection Pipeline

The lab included a malware IoC data file that was added to the Logstash workflow.

![Malware IoC dictionary placement](images/malware-ioc-dictionary-placement.png)

The ELK services were then running together as the event-processing pipeline.

![ELK stack services running](images/elk-stack-services-running.png)

### SOC relevance

This type of enrichment allows raw endpoint events to be compared with known malicious indicators so that matching activity can be labeled for analyst review.

---

# Investigating Enriched Events in Kibana

## Creating the Winlogbeat Index Pattern

I created the Winlogbeat index pattern in Kibana so Windows event data could be searched and investigated.

![Kibana index pattern creation](images/kibana-index-pattern-creation.png)

The resulting index included the fields collected and enriched by the pipeline.

![Kibana Winlogbeat index fields](images/kibana-winlogbeat-index-fields.png)

A dedicated **Malware** field was visible in the indexed data, showing that the enrichment logic had been incorporated into the event pipeline.

![Kibana malware field visible](images/kibana-malware-field-visible.png)

## Detecting a Malicious IoC

I searched Kibana for the lab's malicious process indicator and identified a matching event.

![Kibana malicious IoC match](images/kibana-malicious-ioc-match.png)

The event included threat-intelligence enrichment indicating that the activity matched the malware IoC data.

![Kibana IoC enriched event](images/kibana-ioc-enriched-event.png)

### SOC Perspective

This demonstrates a useful threat-intelligence workflow:

1. Collect endpoint events
2. Process events through Logstash
3. Compare telemetry with known IoCs
4. Add an enrichment field when a match occurs
5. Search the enriched events in Kibana
6. Prioritize matching activity for investigation

---

# Exercise 2 — Integrating OTX Threat Data in OSSIM

The second exercise focused on integrating **AlienVault Open Threat Exchange (OTX)** with **AlienVault OSSIM**.

## AlienVault OTX

I accessed AlienVault OTX, an open threat-intelligence community used to share threat indicators and threat-data collections known as pulses.

![AlienVault OTX portal](images/alienvault-otx-portal.png)

## AlienVault OSSIM

The lab used an AlienVault OSSIM instance as the security monitoring platform.

![AlienVault OSSIM console](images/alienvault-ossim-console.png)

After the OSSIM environment was initialized, I reviewed the main dashboard and its threat-intelligence integration area.

![OSSIM dashboard before OTX integration](images/ossim-dashboard-before-otx-integration.png)

## Connecting OTX Threat Intelligence

The OTX integration was configured using the training account's OTX authentication key. The key itself is intentionally omitted from this repository.

After the connection was established, OSSIM displayed the subscribed OTX threat-intelligence pulses.

![OSSIM OTX subscriptions connected](images/ossim-otx-subscriptions-connected.png)

### SOC Perspective

Integrating OTX with OSSIM allows a SOC platform to consume external threat intelligence and use it to add context to monitored activity.

Examples of information provided by threat-intelligence feeds include:

- Malicious IP addresses
- Domains
- URLs
- File hashes
- Malware families
- Campaign information
- Threat tags and classifications

This helps analysts move from simply observing an event to understanding whether the event is associated with known malicious infrastructure or campaigns.

---

# Key Takeaways

This module demonstrated two complementary threat-intelligence approaches.

## Local IoC Enrichment with ELK

I practiced:

- Configuring an ELK event pipeline
- Forwarding Windows events with Winlogbeat
- Adding malware IoC enrichment
- Creating Kibana index patterns
- Searching for known malicious indicators
- Identifying threat-intelligence-enriched events

## External Threat Intelligence with OTX and OSSIM

I practiced:

- Working with AlienVault OTX
- Accessing OSSIM
- Connecting an external threat-intelligence source
- Reviewing subscribed OTX pulses
- Understanding how threat feeds can improve incident detection and prioritization
