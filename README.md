WINDOWS-EVENT-LOG-ANALYSIS-PROJECT
Windows Event Log Analysis for Threat Detection & Incident Investigation

Table of Contents
Project Overview
Windows Logon Events Analysis
User Account Management
Process Monitoring with Sysmon
File & Network Activity Investigation
PowerShell Activity Monitoring
Tools & Technologies
Conclusion

1. Project Overview

This project documents a hands-on Windows Event Log investigation using Windows Event Viewer, Security Event Logs, and Sysmon. The lab explores how Windows records system activities, including user authentication, account management, process execution, file creation, registry modifications, and network connections.

The objective is to analyse extracted .evtx files, identify suspicious activities, extract Indicators of Compromise (IOCs), and reconstruct an attack chain from available endpoint logs.

2. Windows Logon Events Analysis
Windows Security logs provide visibility into authentication activities and help identify suspicious login attempts.

Event ID 4624: Successful account logon.
Event ID 4625: Failed account logon.
Logon Type 10: RemoteInteractive logon, commonly associated with Remote Desktop Protocol (RDP).

Investigation Findings:

Brute-force source IP: 10.10.53.248
Compromised account: Administrator
Malicious RDP logon identified through Logon Type 10.

3. User Account Management

Attackers may create unauthorised accounts or modify security group memberships to establish persistence and gain elevated privileges.

Important Windows Event IDs:

Event ID 4720: User account creation.
Event ID 4726: User account deletion.
Event ID 4732: Member added to a security-enabled local group.
Event ID 4733: Member removed from a security-enabled local group.

Investigation Findings:

Backdoor account created: svc_sysrestore
Privileged groups targeted:
Backup Operators
Remote Desktop Users

4. Process Monitoring with Sysmon

Process monitoring helps analysts understand how suspicious applications and commands are executed on a compromised endpoint.

Windows Event ID 4688: Process creation.
Sysmon Event ID 1: Process creation with additional details, including command-line arguments, parent process information, and process hashes where configured.


Investigation Findings:

Browser identified: Google Chrome.
Downloaded executable: ckjg.exe
Download location: C:\Users\sarah.miller\Downloads\ckjg.exe
Download URL: http://gettsveriff.com/bgj3/ckjg.exe


5. File & Network Activity Investigation
6. 
Sysmon provides valuable visibility into file operations, registry modifications, network connections, and DNS queries that may indicate malicious activity.

Important Sysmon Event IDs:

Event ID 11: File creation.
Event ID 13: Registry value set.
Event ID 3: Network connection.
Event ID 22: DNS query.

Investigation Findings:

Investigated malware-created files for possible persistence.
Suspected Command-and-Control (C2) destination: 193.46.217.4:7777
Associated DNS query: hkfasfsafg.click

6. PowerShell Activity Monitoring

PowerShell is a legitimate Windows administration tool that can also be abused by attackers to execute commands, perform reconnaissance, and maintain persistence.

Important PowerShell Logging Sources:

Event ID 4103: PowerShell Module Logging.
Event ID 4104: PowerShell Script Block Logging.

PSReadLine History: Records commands entered in an interactive PowerShell session.
The investigation involved examining PowerShell command history to identify suspicious commands and retrieve information stored by other local users.

Investigation Finding:

The first command recorded in the Administrator's PowerShell history was:

Get-ComputerInfo

7. 📑 Lab Walkthrough & Documentation

Due to the extensive nature of this practical lab, including Windows Security Event Log analysis, Sysmon investigations, PowerShell command history examination, and evidence screenshots, the full step-by-step documentation has been placed in a dedicated file.

👉 Read the Full Step-by-Step Windows Event Log Analysis Walkthrough


8. Tools & Technologies Used

Windows Event Viewer: Analysing Windows Security and Sysmon event logs.
Windows Security Logs: Investigating authentication and account management activities.
Sysmon: Monitoring process execution, file activity, registry changes, and network connections.
PowerShell: Examining command history and investigating system activities.
TryHackMe: Practical lab environment and extracted Windows event logs.

9. Conclusion

This project demonstrates how Windows Event Logs and Sysmon provide essential visibility into endpoint activities. By correlating authentication events, account modifications, process execution, file activity, and network connections, SOC analysts can identify suspicious behaviour and reconstruct an attack sequence.
