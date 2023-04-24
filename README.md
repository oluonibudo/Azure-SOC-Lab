# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://imgur.com/4aCwzSW.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imgur.com/19XJntc.jpg)
## Architecture After Hardening / Security Controls
![Architecture Diagram](https://imgur.com/OtUIeZ8.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://imgur.com/QvaLLQT.png)<br>
![Linux Syslog Auth Failures](https://imgur.com/mn9R2HG.png)<br>
![Windows RDP/SMB Auth Failures](https://imgur.com/1U3vWAm.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-04-10 10:39
Stop Time	2023-04-11 10:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8131
| Syslog                   | 68
| SecurityAlert            | 1
| SecurityIncident         | 407
| AzureNetworkAnalytics_CL | 419

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-04-11 11:40
Stop Time	2023-04-12 11:40

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3133
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was created in Microsoft Azure, and log sources from various endpoints were integrated into a Log Analytics workspace. Using Microsoft Sentinel, alerts and incidents were triggered based on the analyzed logs. Furthermore, metrics were gathered for a period of 48 hours to demonstrate the importance of configuring cloud assets with security in mind. By implementing a section of NIST SP 800-53 r4, a significant reduction in the number of security incidents and events was achieved, highlighting the effectiveness of security controls.

It is noteworthy that if network resources were heavily utilized by regular users, there could have been an increase in the number of security events and alerts generated within the first 24 hours of implementing security controls.

To summarize, it is prudent to assume that in an actual organizational scenario, attackers would have had numerous opportunities to compromise the confidentiality, availability, and integrity of the organization's assets.
