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

During the "BEFORE" stage of the project, a virtual environment was deployed and exposed to the public for malicious actors to discover. The intent for this stage was to attract bad actors and observe their attack patterns. To achieve this, A Windows virtual machine hosting a SQL database was deployed and a Linux server both had their network security groups (NSGs) had Allow All configured. To entice these attackers even further, a storage account and key vault were deployed with public endpoints which were visible on the open internet. In this stage, the unsecured environment was monitored by Microsoft Sentinel using logs aggregated by the Log Analytics workspace

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

During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:

Network Security Groups (NSGs): NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

Built-in Firewalls: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

Private Endpoints: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.

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
