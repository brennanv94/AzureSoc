# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/8131775f-15e4-4108-aa12-ec0a5690c3c0)



## Introduction

This project involves the creation of a Security Operations Center (SOC) and honeynet environment in Azure, designed to monitor and analyze live malicious traffic. Leveraging my skills in network systems and cybersecurity, this setup includes Windows and Linux virtual machines, integrated logging through Azure Log Analytics, and security orchestration with Microsoft Sentinel. The goal was to simulate real-world attack scenarios, analyze security events, and evaluate the effectiveness of hardened controls. Metrics such as Windows Event Logs, Linux Syslogs, security alerts, and malicious network traffic flows were monitored pre- and post-hardening to demonstrate measurable improvements in security posture.


## Architecture Before Hardening / Security Controls
![Architecture Before hardening](https://github.com/user-attachments/assets/ced174d6-603b-412a-8590-713acf03e2ce)

In the initial configuration, the environment was purposefully left vulnerable to simulate common security gaps and capture attack data. Virtual machines were deployed with unrestricted Network Security Groups (NSGs), leaving them open to all inbound and outbound traffic. Firewalls were disabled and public endpoints were exposed to the internet for key resources like Azure Key Vault and Azure Storage Account.

## Architecture After Hardening / Security Controls
![After Hardening](https://github.com/user-attachments/assets/e8cdcbb9-34e0-49e8-b19a-1f60dc64200a)

After implementing security controls, the environment was significantly hardened to reduce its attack surface. Network Security Groups (NSGs) were configured to block all traffic except from a designated admin workstation, and private endpoints were implemented for resources like Azure Key Vault and Storage Account. Built-in firewalls were properly configured on all virtual machines, ensuring only necessary ports were open. These measures effectively restricted unauthorized access, minimized exposure to malicious traffic, and enhanced overall security.


## **Technologies, Regulations, and Azure Components Employed:**

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel for (SIEM)
- Microsoft Defender for Cloud to protect cloud resources
- Windows RDP for remote access
- Command Line Interface (CLI) for system management
- PowerShell for automation and configuration management
- NIST SP 800-53 Revision 5 for security controls
- NIST SP  800-61 Revision 2 for Incident Handling Guidance



## Attack Maps Before Hardening / Security Controls
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/99d41a74-51c7-441a-bca0-883f52a81f04)
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/6529b4e3-3291-423c-a93f-3849c907ac13)
![windowss-rdp-auth-fail](https://github.com/user-attachments/assets/6a25ce65-883d-40cc-a76b-d2f7156f7c95)

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics Before Hardening / Security Controls
I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

  
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

This project demonstrates a comprehensive approach to building and hardening a SOC and honeynet in Azure. By initially deploying a vulnerable environment and assessing its security posture, I was able to implement robust security controls, significantly reducing attack surfaces and malicious activity. Key takeaways include understanding how to analyze and respond to live traffic, leveraging Microsoft Sentinel for monitoring and incident response, and using Azure’s built-in security features for proactive defense. This endeavor highlights my hands-on expertise in cloud security and my ability to apply practical solutions to real-world challenges.
