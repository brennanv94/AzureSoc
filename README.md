# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/809b985d-f7d8-451d-959c-341c0eb0df26)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before hardening](https://github.com/user-attachments/assets/3481cadb-8924-4cc2-acaa-bbca888c7a76)


## Architecture After Hardening / Security Controls
![After Hardening](https://github.com/user-attachments/assets/4318e343-8ec3-417f-ab27-f3cf9bdab33d)


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
[NSG Allowed Inbound Malicious Flows]![Screenshot 2024-09-29 182341](https://github.com/user-attachments/assets/b550ec12-509d-4e2c-88db-b1f618a5d3d9)
<br>
[Linux Syslog Auth Failures]![Screenshot 2024-09-29 182046](https://github.com/user-attachments/assets/cd198c41-020c-4932-84a3-8cf77013fef2)
<br>
[Windows RDP/SMB Auth Failures]![Screenshot 2024-09-29 182541](https://github.com/user-attachments/assets/9503d99b-1452-4008-93b6-4441dbe0517a)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-09-20 21:13:38
Stop Time 2024-09-21 21:13:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10562
| Syslog                   | 32360
| SecurityAlert            | 0
| SecurityIncident         | 294
| AzureNetworkAnalytics_CL | 1356

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-09-23 19:20:45 
Stop Time	2024-09-24 19:20:45

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2740
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
