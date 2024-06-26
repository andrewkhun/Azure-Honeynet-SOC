# Azure Honeynet and SOC with Real-World Cyber Attacks
![Cloud Honeynet / SOC](https://imgur.com/bZI88hg.jpg)

## Introduction and Objective

A miniature honeynet was constructed within the Azure platform. Log data originating from diverse sources were aggregated into a Log Analytics workspace. Subsequently, Microsoft Sentinel leveraged this data to formulate attack visualizations, initiate alert notifications, and generate incident reports.

The experiment involved the assessment of security metrics within an initially vulnerable environment over a 24-hour duration. Following this baseline measurement, security protocols were implemented to fortify the environment. Another 24-hour period was then dedicated to the reassessment of metrics under the enhanced security posture. The ensuing section presents the findings, detailing the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Technologies, Azure Components, and Regulations Employed
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance


## Architecture Before Hardening and Security Controls
![Architecture Diagram](https://i.imgur.com/lFKkzuM.jpg)

<b>Before Hardening Measures and Security Controls:</b>

- In the "BEFORE" stage, all resources were initially deployed with public exposure to the internet. This setup was intentionally insecure to attract potential cyber attackers and observe their tactics. The Virtual Machines had both their Network Security Groups (NSGs) and built-in firewalls wide open, allowing unrestricted access from any source. Additionally, all other resources, such as storage accounts and databases, were deployed with public endpoints visible to the internet, without utilizing any Private Endpoints for added security.

## Architecture After Hardening and Security Controls
![Architecture Diagram](https://i.imgur.com/QXL0t70.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/pIIIRkR.jpeg)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/0vQIELg.jpeg)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/YoGzHpz.jpeg)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-05-04 20:29:32
Stop Time 2024-05-15 20:29:32

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 16140
| Syslog                   | 1832
| SecurityAlert            | 4
| SecurityIncident         | 301
| AzureNetworkAnalytics_CL | 11139

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-05-07 19:18:11
Stop Time	2024-05-08 19:18:11

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 122
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion


In this project, a miniature honeynet was established within Microsoft Azure, and log sources were seamlessly integrated into a Log Analytics workspace. Microsoft Sentinel played a pivotal role in alert triggering and incident management based on the ingested logs. Furthermore, security metrics were initially gauged in the insecure environment, followed by a reassessment post-implementation of security measures. Notably, the application of these measures led to a significant reduction in both security events and incidents, underscoring their efficacy.

It's important to acknowledge that if the network's resources were heavily utilized by regular users, it's conceivable that a greater volume of security events and alerts might have been generated within the 24-hour period subsequent to the implementation of security controls.
