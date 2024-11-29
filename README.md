# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/8131775f-15e4-4108-aa12-ec0a5690c3c0)



## Introduction

This project involves the creation of a Security Operations Center (SOC) and honeynet environment in Azure, designed to monitor and analyze live malicious traffic. Leveraging my skills in network systems and cybersecurity, this setup includes Windows and Linux virtual machines, integrated logging through Azure Log Analytics, and security orchestration with Microsoft Sentinel. The goal was to simulate real-world attack scenarios, analyze security events, and evaluate the effectiveness of hardened controls. Metrics such as Windows Event Logs, Linux Syslogs, security alerts, and malicious network traffic flows were monitored pre- and post-hardening to demonstrate measurable improvements in security posture.

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
  
## Architecture Before Hardening / Security Controls
![Architecture Before hardening](https://github.com/user-attachments/assets/ced174d6-603b-412a-8590-713acf03e2ce)

In the initial configuration, the environment was purposefully left vulnerable to simulate common security gaps and capture attack data. Virtual machines were deployed with unrestricted Network Security Groups (NSGs), leaving them open to all inbound and outbound traffic. Firewalls were disabled and public endpoints were exposed to the internet for key resources like Azure Key Vault and Azure Storage Account.

## Attack Maps Before Hardening / Security Controls
![linux-ssh-auth-fail](https://github.com/user-attachments/assets/99d41a74-51c7-441a-bca0-883f52a81f04)
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/6529b4e3-3291-423c-a93f-3849c907ac13)
![windowss-rdp-auth-fail](https://github.com/user-attachments/assets/6a25ce65-883d-40cc-a76b-d2f7156f7c95)

## Metrics Before Hardening / Security Controls
I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

  
The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-11-27 02:20:53
Stop Time 2024-11-28 02:20:53

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 18017
| Syslog                   | 7599
| SecurityAlert            | 1
| SecurityIncident         | 73
| AzureNetworkAnalytics_CL | 103

## Architecture After Hardening / Security Controls
![After Hardening](https://github.com/user-attachments/assets/e8cdcbb9-34e0-49e8-b19a-1f60dc64200a)

After implementing security controls, the environment was significantly hardened to reduce its attack surface. Network Security Groups (NSGs) were configured to block all traffic except from a designated admin workstation, and private endpoints were implemented for resources like Azure Key Vault and Storage Account. Built-in firewalls were properly configured on all virtual machines, ensuring only necessary ports were open. These measures effectively restricted unauthorized access, minimized exposure to malicious traffic, and enhanced overall security.

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-11-28 01:31:06
Stop Time	2024-11-29 01:31:06

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 757
| Syslog                   | 287
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0


## Simulated Attacks
The simulated attacks that were used below were to generate incidents that would need to be addressed in a real world scenario.


**AAD Brute Force Success**
Started off by generating multiple failed login attempts with one succesful generating a brute force success.

![Screenshot 2024-11-29 122216](https://github.com/user-attachments/assets/390ff88c-e521-43c5-8bb9-07101dcf50d1)


**MSSQL Brute Force Attempt**
I used powershell script to generate 30 failed login attempts.

![Screenshot 2024-11-29 124339](https://github.com/user-attachments/assets/d4d68480-7d1b-481c-8631-e6edd51e1beb)


**Malware outbreak**
I used a powershell script that contained a string that would trigger an alert for antimalware detection. 

![Screenshot 2024-11-29 125142](https://github.com/user-attachments/assets/57991fe5-a9f2-4ce0-b07c-64dbf405aad4)


**Trigger Possible Privilege Escalation**
whenever you click show secret in the key vault this will create a log in the diagnostic setting in key vault. Sentinel will then query LAW and will see the log in there and generate an incident.

![Screenshot 2024-11-29 125932](https://github.com/user-attachments/assets/6759eaa1-4341-44b4-81d4-32d5a269846f)

## NIST 800-61 Incident Management Lifecycle

**Step 1: Preparation**

Initiated this already by ingesting all of the logs into Log Analytics Workspace and Sentinel and configuring alert rules.

**Step 2: Detection & Analysis** 

-Set Severity, Status, Owner  
-View Full Details (New Experience)  
-Observe the Activity Log (for history of incident)  
-Observe Entities and Incident Timelines (are they doing anything else?)  
-“Investigate” the incident and continue trying to determine the scope  
-Inspect the entities and see if there are any related events  
-Determine legitimacy of the incident (True Positive, False Positive, etc.)  
-If True Positive, continue, if False positive, close it out  

**Step 3: Containment, Eradication, and Recovery**

**Step 4: Document Findings/Info and Close out the Incident in Sentinel**

![Screenshot 2024-11-29 160759](https://github.com/user-attachments/assets/60726cf1-3ab0-42bd-b5ca-25109f6af337)

![Screenshot 2024-11-29 164204](https://github.com/user-attachments/assets/5af9d93d-6c33-4ba4-9f1c-f7bb34d4709b)

## Conclusion

This project demonstrates a comprehensive approach to building and hardening a SOC and honeynet in Azure. By initially deploying a vulnerable environment and assessing its security posture, I was able to implement robust security controls, significantly reducing attack surfaces and malicious activity. Key takeaways include understanding how to analyze and respond to live traffic, leveraging Microsoft Sentinel for monitoring and incident response, and using Azure’s built-in security features for proactive defense. This endeavor highlights my hands-on expertise in cloud security and my ability to apply practical solutions to real-world challenges.
