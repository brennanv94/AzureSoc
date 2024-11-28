# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/8131775f-15e4-4108-aa12-ec0a5690c3c0)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before hardening](https://github.com/user-attachments/assets/8e907a03-9193-4ff4-b752-758aa53d002d)




## Architecture After Hardening / Security Controls
![After Hardening](https://github.com/user-attachments/assets/e8cdcbb9-34e0-49e8-b19a-1f60dc64200a)




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
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/99d41a74-51c7-441a-bca0-883f52a81f04)
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/6529b4e3-3291-423c-a93f-3849c907ac13)
![windowss-rdp-auth-fail](https://github.com/user-attachments/assets/6a25ce65-883d-40cc-a76b-d2f7156f7c95)


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
