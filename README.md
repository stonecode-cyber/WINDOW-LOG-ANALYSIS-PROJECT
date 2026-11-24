WINDOWS-EVENT-LOG-ANALYSIS-PROJECT

Windows Event Log Analysis for Threat Detection and Incident Investigation

. Project Overview
2. Windows Logon Events Analysis
3. User Account Management
4. Process Monitoring with Sysmon
5. File and Network Activity Investigation
6. PowerShell Activity Monitoring
7. Tools & Technologies
8. Key Takeaways
9. Conclusion
Windows Event Logs provide valuable visibility into system activities, including user authentication, process execution, account modifications, file creation, and network connections.

In this project, I analysed Windows Security and Sysmon event logs using Event Viewer and PowerShell to investigate suspicious activities, identify indicators of compromise (IOCs), and reconstruct an attack sequence.

The investigation covered brute-force attacks, RDP compromise, unauthorised account creation, privilege escalation, malicious file downloads, Command-and-Control (C2) communication, and PowerShell activity.
Investigated authentication activities to identify suspicious login attempts and successful access.

Key Event IDs:
- 4624: Successful logon.
- 4625: Failed logon.
Findings:
- Brute-force source IP: 10.10.53.248
- Compromised account: Administrator
- Malicious RDP logon type: 10 (RemoteInteractive)
Investigated account creation and security group modifications to identify possible persistence and privilege escalation.

Key Event IDs:
- 4720: User account created.
- 4726: User account deleted.
- 4732: Member added to a security-enabled local group.
- 4733: Member removed from a security-enabled local group.
Findings:
- Unauthorised account: svc_sysrestore
- Groups targeted: Backup Operators and Remote Desktop Users
Used process creation logs to investigate application execution and identify suspicious downloaded files.

Key Event IDs:
- 4688: Windows process creation.
- Sysmon Event ID 1: Process creation with additional process details.
Findings:
- Browser identified: Google Chrome.
- Downloaded file: C:\Users\sarah.miller\Downloads\ckjg.exe
- Download URL: http://gettsveriff.com/bgj3/ckjg.exe
Investigated file creation, registry modifications, network connections, and DNS queries to identify malware activity and possible persistence mechanisms.

Key Sysmon Event IDs:
- 11: File creation.
- 13: Registry value set.
- 3: Network connection.
- 22: DNS query.
Findings:
- Suspected C2 IP and port: 193.46.217.4:7777
- Associated DNS query: hkfasfsafg.click
Examined PowerShell command history to identify commands executed by users and understand how attackers may abuse legitimate administrative tools.

Key investigation areas:
- PSReadLine command history.
- PowerShell Script Block Logging (Event ID 4104).
- PowerShell Module Logging (Event ID 4103).
Finding:

The first command recorded in the Administrator's PowerShell history was:

Get-ComputerInfo
- Windows Event Viewer: Windows Security and Sysmon log analysis.
- Windows Security Logs: Authentication and account management investigation.
- Sysmon: Process, file, registry, and network activity monitoring.
- PowerShell: Command history and system investigation.
- TryHackMe: Practical lab environment and extracted event logs.
Through this project, I gained practical experience in:
1. Analysing Windows authentication events to identify brute-force attacks and suspicious logins.
2. Investigating unauthorised account creation and privilege escalation.
3. Using Sysmon to trace process execution, file activity, and network connections.
4. Identifying suspicious files, domains, and Command-and-Control infrastructure.
5. Investigating PowerShell command history and understanding the importance of PowerShell logging.
6. Correlating events across different log sources to reconstruct an attack timeline.
This project strengthened my understanding of Windows endpoint monitoring and the role of event log analysis in SOC operations. It demonstrated how seemingly isolated events can be connected to uncover malicious activities, trace attacker behaviour, and identify indicators of compromise.

The key lesson: Effective threat detection is not just about identifying individual suspicious events; it is about correlating evidence across multiple log sources to understand the complete attack chain.
