**Scenario**

Microsoft Defender for Endpoint (MDE) generates alerts when devices attempt to access URLs, domains, IP addresses, or files that have been configured as Indicators of Compromise (IOCs). In most cases, these indicators are already blocked by Microsoft Defender, making the resulting alerts informational and low risk. While each alert can be reviewed and closed manually, a large number of IOC-related alerts can quickly accumulate over time. Manually investigating and closing these incidents one by one becomes repetitive, time-consuming, and operationally inefficient. To address this challenge, an automated Microsoft Sentinel playbook was developed to perform initial incident triage and determine whether an incident requires analyst attention.

**Objective**

Reduce alert fatigue within the Security Operations team.
Automatically triage low-risk IOC incidents.
Close incidents that do not meet escalation criteria.
Leave potentially risky incidents open for analyst review.
Ensure consistent and repeatable incident handling.

**Design Logic**

IOC Incident Created
        ↓
Get Associated Hosts
        ↓
Check Exposure Level for Each Host
        ↓
Any Host = High?
      /      \
    Yes       No
     │         │
 Keep Open   Close Incident

** WorkFlow**

Step 2: Sentinel Incident Trigger

Purpose:

Starts the playbook when Microsoft Sentinel creates an incident.

What it does:

Receives the incident from Sentinel.
Passes the incident information to the workflow.
Initiates the automated triage process.

Workflow: 
Microsoft Sentinel
        ↓
Playbook Triggered

<img width="1073" height="362" alt="image" src="https://github.com/user-attachments/assets/9fc995f4-de5d-463c-923c-1af5af67d3df" />

Step 3: Get Incident

Purpose:

Retrieves detailed information about the incident.

What it does:

Gets incident title.
Gets severity.
Gets entities attached to the incident.
Makes incident details available to the remaining workflow.

Workflow:

Sentinel Incident
↓
Get Incident
<img width="1060" height="233" alt="image" src="https://github.com/user-attachments/assets/cdc92303-96d5-4888-ae50-82527eca78b1" />

Step 4: Incident Validation Condition

Purpose

Ensures the playbook only processes IOC incidents.

Condition:
Title = Connection to a custom network indicator

What it does

True → Continue processing.
False → Ignore incident.

Workflow
Get Incident
      ↓
Title Check

<img width="1249" height="178" alt="image" src="https://github.com/user-attachments/assets/8773aa14-b1f8-4f83-b0c9-1503608bc520" />





