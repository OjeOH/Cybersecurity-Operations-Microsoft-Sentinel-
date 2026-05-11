# Incident Detection with Microsoft Sentinel

**Cybersecurity Operations Management**   
[← Back to course overview](../README.md)

---

## Overview

A full end-to-end Microsoft Sentinel lab covering workspace setup, data connector configuration, custom analytics rules, real incident triage, and threat hunting using MITRE ATT&CK techniques. This lab simulates the daily workflow of a Security Operations Centre (SOC) analyst.

---

## Module 1 — Sentinel Setup & Playbooks

### Part 1 — Azure Account & Sentinel Instance

**Steps:**
1. Created an Azure student account with Conestoga email
2. Navigated to Microsoft Sentinel in the Azure portal
3. Created a Log Analytics workspace named **`sentinel-training-lab-ws`**
4. Added Sentinel to the workspace

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image2.png) | ![](screenshots/image3.png) |
| *Figure 1 — Azure account created* | *Figure 2 — Microsoft Sentinel accessed* |
| ![](screenshots/image4.png) | ![](screenshots/image5.png) |
| *Figure 3 — Workspace created* | *Figure 4 — Sentinel added to workspace* |

---

### Part 2 — Deploy Sentinel Training Lab Solution

**Steps:**
1. Opened **Content Hub** in Sentinel
2. Searched for and deployed the **Microsoft Sentinel Training Lab Solution**
3. Verified events were ingested into the workspace

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image6.png) | ![](screenshots/image7.png) |
| *Figure 5 — Sentinel Training Solution deploying* | *Figure 6 — Solution deployed successfully* |
| ![](screenshots/image8.png) | |
| *Figure 7 — Events ingested into workspace* | |

---

### Part 3 — Add a Sentinel Playbook

**Steps:**
1. Navigated to **Automation → Playbooks**
2. Found the playbook API connection
3. Authorized the API connection to allow Sentinel to trigger the playbook

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image9.png) | ![](screenshots/image10.png) |
| *Figure 8 — Navigating to playbook API* | *Figure 9 — Authorizing API connection* |
| ![](screenshots/image11.png) | |
| *Figure 10 — Authorization successful* | |

**Module 1 Summary:**  
Set up a fully functional Sentinel instance with a training dataset and a connected playbook ready for automated response. The playbook API connection enables Sentinel to trigger automated workflows (e.g., blocking an IP, notifying a team) when an incident fires.

---

## Module 2 — Data Connectors

Configure three data connectors to ingest telemetry from different Microsoft security services into Sentinel.

### Part 1 — Azure Activity Connector

Ingests Azure control-plane events (resource deployments, role changes, subscription-level actions).

**Steps:** Content Hub → Search "Azure Activity" → Install → Configure data connector

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image12.png) | ![](screenshots/image13.png) |
| *Figure 11 — Installing Azure Activity* | *Figure 12 — Azure Activity installed* |
| ![](screenshots/image14.png) | |
| *Figure 13 — Azure Activity data connector active* | |

---

### Part 2 — Microsoft Defender for Cloud Connector

Ingests security alerts and recommendations from Defender for Cloud.

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image15.png) | ![](screenshots/image16.png) |
| *Figure 14 — Installing Defender for Cloud* | *Figure 15 — Connector installed* |
| ![](screenshots/image17.png) | |
| *Figure 16 — Data connector connected* | |

---

### Part 3 — Microsoft Defender Threat Intelligence Connector

Ingests Microsoft's threat intelligence feed — known malicious IPs, domains, and file hashes — for automatic indicator matching against ingested logs.

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image18.png) | ![](screenshots/image19.png) |
| *Figure 17 — Threat Intelligence installed* | *Figure 18 — Data connector connected* |

**Module 2 Summary:** Three data connectors active — Azure Activity (cloud operations), Defender for Cloud (workload security alerts), and Threat Intelligence (IOC feed). Each connector populates different Sentinel tables that analytics rules can query.

---

## Module 3 — Analytics Rules

Create detection rules that generate incidents when suspicious patterns are found in ingested data.

### Part 1 — Azure Activity Rule (Scheduled)

**Steps:**
1. Opened **Analytics** tab in Sentinel
2. Filtered rule templates by **Azure Activity**
3. Selected template: **Suspicious Resource Deployment**
4. Created a **scheduled analytics rule** from the template
5. Rule validated and created successfully

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image20.png) | ![](screenshots/image21.png) |
| *Figure 19 — Azure Activity rule template selected* | *Figure 20 — Rule passes validation* |
| ![](screenshots/image22.png) | |
| *Figure 21 — Rule created successfully* | |

---

### Part 2 — Microsoft Incident Creation Rule (Defender for Cloud)

Created a **Microsoft incident creation rule** that automatically turns Defender for Cloud alerts into Sentinel incidents.

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image23.png) | ![](screenshots/image24.png) |
| *Figure 22 — Defender for Cloud rule summary* | *Figure 23 — Rule enabled* |

---

### Part 3 — Review Fusion Rule

Reviewed Sentinel's built-in **Fusion rule** — an AI-powered correlation engine that detects multi-stage attacks by combining signals from multiple sources that individually might not trigger an alert.

![](screenshots/image25.png)  
*Figure 24 — Fusion rules filtered and reviewed*

---

### Part 4 — Custom Scheduled Analytics Rule (KQL)

**Steps:**
1. Wrote a **KQL query** to scan email logs for suspicious inbox rules
2. Created a new **Scheduled Analytics Rule** — "Malicious Inbox Rule Detection"
3. Rule validated and created successfully 

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image26.png) | ![](screenshots/image27.png) |
| *Figure 25 — KQL query scanning logs* | *Figure 26 — Validation passed* |
| ![](screenshots/image28.png) | |
| *Figure 27 — Malicious inbox rule scheduled rule created* | |

---

### Part 5 — Review a Security Incident

**Findings:** A rule triggered and generated an incident.

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image29.png) | ![](screenshots/image30.png) |
| *Figure 28 — Incident detected in Sentinel* | *Figure 29 — Full incident details* |
| ![](screenshots/image31.png) | |
| *Figure 30 — Rule that triggered the incident* | |

**Module 3 Summary:** Built a detection stack with three rule types — scheduled rules (time-based KQL queries), incident creation rules (alert forwarding), and Fusion rules (AI correlation). Writing custom KQL to detect suspicious inbox rules demonstrates the ability to create detections beyond out-of-the-box templates.

---

## Module 4 — Incident Response

### Part 1 — Sentinel Incident Management Tools

**Steps:**
1. Opened the **Incidents** view in Sentinel
2. Assigned the incident to myself (simulating SOC analyst assignment)
3. Changed incident status to **Active**

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image32.png) | ![](screenshots/image33.png) |
| *Figure 31 — Assigning incident to analyst* | *Figure 32 — Incident activated and assigned* |

---

### Part 2 — Handle "Sign-ins from IPs Attempting Disabled Accounts" Incident

**Steps:**
1. Ran playbook against incident → tag added automatically
2. Used **Workbook** to investigate the source IP further
3. Reviewed activity details for the associated user
4. **Determined: False Positive** — incident was triggered by a scheduled red team exercise

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image34.png) | ![](screenshots/image35.png) |
| *Figure 33 — Tag added after playbook ran* | *Figure 34 — Workbook investigation on IP* |
| ![](screenshots/image36.png) | ![](screenshots/image37.png) |
| *Figure 35 — User activity detail* | *Figure 36 — Incident closed as false positive (red team exercise)* |

>  **Real-world skill:** Correctly identifying false positives is as important as finding real threats. Documenting the reason (red team exercise) prevents alert fatigue and helps tune future detection rules.

---

### Part 3 — Handle "Solorigate Network Beacon" Incident

**Steps:**
1. Assigned incident to myself
2. Used **Hunting** to run the **Solorigate Inventory Check** query — identified affected hosts
3. Added the suspicious IP to **Threat Intelligence** to alert on future occurrences

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image38.png) | ![](screenshots/image39.png) |
| *Figure 37 — Solorigate incident assigned* | *Figure 38 — Solorigate inventory check query running* |
| ![](screenshots/image40.png) | ![](screenshots/image41.png) |
| *Figure 39 — Affected hosts identified* | *Figure 40 — Suspicious IP added to Threat Intelligence* |

**Module 4 Summary:** Hands-on experience with the full incident lifecycle — assignment, investigation via workbooks, playbook automation, false positive classification, and threat intelligence enrichment. Recognizing a red team exercise as a false positive and adding an attacker IP to threat intel are both critical SOC analyst skills.

---

## Module 5 — Threat Hunting

### Part 1 — Hunt by MITRE ATT&CK Technique

**Steps:**
1. Opened **Hunting** in Sentinel
2. Filtered queries by MITRE technique: **Account Manipulation (T1098)**
3. Ran the hunting query
4. Reviewed results — identified suspicious apps
5. Created a **Watchlist** to track criticality of discovered apps

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image42.png) | ![](screenshots/image43.png) |
| *Figure 41 — Hunting queries filtered by Account Manipulation* | *Figure 42 — Query results* |
| ![](screenshots/image44.png) | |
| *Figure 43 — Watchlist created for discovered apps* | |

---

### Part 2 — Bookmark a Hunting Result

- Bookmarked a suspicious result from the hunting query for further investigation

![](screenshots/image45.png)  
*Figure 44 — Bookmark created from hunting result*

---

### Part 3 — Promote Bookmark to Incident

**Steps:**
1. Reviewed the **Investigation Graph** — visualizes connections between entities (users, IPs, hosts)
2. Promoted the bookmark to a new incident for formal tracking

**Screenshots:**

| | |
|--|--|
| ![](screenshots/image46.png) | ![](screenshots/image47.png) |
| *Figure 45 — Investigation graph for bookmarked entity* | *Figure 46 — New incident created from bookmark* |

**Module 5 Summary:** Proactive threat hunting using MITRE ATT&CK techniques enables detection of attacks that haven't fired a rule yet. The bookmark-to-incident workflow creates a formal track for suspicious findings discovered during hunting, ensuring nothing falls through the cracks.

---

## Full Lab Summary

| Module | Focus | Key Outcome |
|--------|-------|-------------|
| 1 — Setup | Sentinel workspace, training data, playbooks | Live Sentinel instance ready for detection |
| 2 — Connectors | Azure Activity, Defender for Cloud, Threat Intel | Multi-source data ingestion active |
| 3 — Analytics | Scheduled rules, incident rules, Fusion, custom KQL | Full detection stack operational |
| 4 — Response | Incident triage, workbooks, false positive handling | Solorigate and sign-in incidents investigated |
| 5 — Hunting | MITRE filtering, bookmarks, investigation graph | Threat hunted and promoted to incident |

---

## Skills Covered

- Provisioning Microsoft Sentinel in Azure
- Configuring data connectors for multi-source ingestion
- Writing and deploying scheduled analytics rules with KQL
- Understanding Fusion (AI-based multi-stage attack correlation)
- Triaging, investigating, and closing security incidents
- Identifying and documenting false positives
- Threat hunting using MITRE ATT&CK technique mapping
- Adding indicators of compromise to Threat Intelligence
- Promoting hunting bookmarks to formal incidents
