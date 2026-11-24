Effective security monitoring relies heavily on an operating system's ability to record and process system events. Windows event logging captures critical actions—such as user logins, process executions, and service installations—within binary .evtx files stored in the system directory. Proper log management and analysis serve as the foundation for Security Operations Center (SOC) workflows, providing the necessary visibility for incident response, threat hunting, and automated alerting. This project explores the anatomy of Windows event logs, key security event IDs, and practical methods for parsing and analyzing log data to identify malicious activity and reinforce system security.
In this project i will be analysiing a window logs for malacious activities. To start with, i open the event veiwer on my window using the **window + r** shortcut.

<img width="452" height="313" alt="Image" src="https://github.com/user-attachments/assets/d79dd5be-c44a-4ace-a770-9c1d37dffe0b" />

**TASK 1: Open the "Practice-Security.evtx" file on the VM's Desktop.
Which IP performed a brute force of the THM-PC?**

**Event ID 4264** mean succesfull logged-in while **Event ID 4625** mean attempted logged-in.
Brute force  mean random attempt to log into a system with predicted password which result to series of **event id 4625**. I checked on the event detail and the ip address that perform the brute force is **10.10.53.248**

<img width="480" height="368" alt="Image" src="https://github.com/user-attachments/assets/12babff4-c61e-49a1-8763-b61af4264c08" />

**TASK 2: Which user has been breached as a result of the attack?**

Further analysis reveal that the targeted  user in the brute force attack  is the adminstrator 

<img width="485" height="379" alt="Image" src="https://github.com/user-attachments/assets/273f1f12-d39a-4aba-ba10-79acbfaf67d6" />

TASK 3: What was the Logon ID of the malicious RDP login?

In remote desktop analyse the login is sucesfull remotely which  mean the event ID will be 4264, but since it remote acces there wont be a Network Level Authentication ([ NLA ([https://superops.com/rmm/what-is-network-level-authentication] which mean the logo type will be 10

<img width="484" height="399" alt="Image" src="https://github.com/user-attachments/assets/dfa27bf4-d8bd-46ea-b408-1a96ed44e855" />


**### USER MANAGEMENT**

Unlike logon event, user managemnet has to do with user activities suche as account creation,account deletion, user password change or addition por removal of users into the security group.

<img width="541" height="275" alt="Image" src="https://github.com/user-attachments/assets/59b34772-521d-41c4-be49-abc5aa04dded" />

User Managemnet can be   structured into:
**Subject**: Who did the action i.e which account did the action, (which ccount initiated the attack.)
**Object:** Who is the targer i.e which account is the target (can be a new or exixting user)
**Detail:**What was the actions. what changes are made.

**TASK 1: Which user was created by the attacker soon after the RDP login?**

Since this has to do with  account creation, i have to look for **event ID 4720**  and analyse the detail, which reveal the account name to be  **_svc_sysrestore_**  as shown in the screenshot below:
<img width="479" height="359" alt="Image" src="https://github.com/user-attachments/assets/8a7fdda6-860d-46ed-b723-d050691843e7" />

**TASK 2:Which two privileged groups was the backdoor user added to?**

(Answer in alphabetical order, e.g. "Administrators, Power Users")
This tadk has to do with user added to a priveledge group such as the security group, i will be analsying the **_event id 4732_**  after the  creation of the user   **_svc_sysrestore_** . as shown in the screenshots below. The user wa addes to 2 groups which are  the **_Backup Operato_**r and the **_Remote desktop users_** group.

<img width="479" height="365" alt="Image" src="https://github.com/user-attachments/assets/ac0a3663-a9fc-480a-99e7-75909bfb9a39" />

<img width="474" height="367" alt="Image" src="https://github.com/user-attachments/assets/5ffa0b61-1cb7-46f5-91a3-8afa47510523" />

**PROCESS  MONITORING**
This is the process of monitoring how an event happen, even if you know who is breached, you often need to  know how .

**Event ID 4688- (Security Log: Process Creation)**:  Log an event every time a new process is launched, including its command line and parent process details.
**EVENT ID 1 (Sysmon: Process Creation):** Replace 4688 event code and provide more advanced fields like process hash and its signature.
In the task below i will be making use of TRYHACK ME extracted logs .

**Open the "Practice-Sysmon.evtx" file on the VM's Desktop.
Which web browser does Sarah use to browse the web?**

I filter the log to look for event id 1, this reveal the bowser use to be  **Google chrome** as shown in the screen shot below:

<img width="488" height="308" alt="window log task 1" src="https://github.com/user-attachments/assets/2f0db73f-3ecf-41d5-a4d8-ce9f89ae53ed" />

<img width="485" height="332" alt="window log task 1b" src="https://github.com/user-attachments/assets/45ec427c-62df-422b-b91f-bf2b8bf6f731" />

<img width="448" height="322" alt="windo log task 1c " src="https://github.com/user-attachments/assets/9d0d7adc-3e43-4364-925c-e0b551b70974" />

**Which file did Sarah download from the browser?**

As shown after analysis of the log i found the file downloaded to be  **_C:\Users\sarah.miller\Downloads\ckjg.exe_** 

<img width="478" height="362" alt="window log task 2" src="https://github.com/user-attachments/assets/d844ee81-7a3a-4b04-9f6b-8695d1bf352b" />

**Which URL was the file downloaded from?
Note: Use other Sysmon events to find out!**

I unfiltered the event and analyse the log one after the other, since i am dealing with file downloaded, i consider **_EVENT ID 15_**,  where the url was reveal as **http://gettsveriff.com/bgj3/ckjg.exe** as shown in the screenshit below:

<img width="490" height="373" alt="window log task 3" src="https://github.com/user-attachments/assets/46bdf0cd-9fc6-45ce-9c16-30a7f9e23079" />


**FILES AND NETWORK**


**EVENT ID 11 / 13(File Create / Registry Value Set)** : 4656 for file changes and 4657 for registry changes, both disabled by default  Detect files dropped by malware or its changes to the registry (e.g. for persistence)

**EVENT ID 3 / 22(Network Connection / DNS Query)** : No direct alternative, requires additional firewall and DNS configuration Detect traffic from untrusted processes or to known malicious destinations.

**TASK 1 : Continue with the "Practice-Sysmon.evtx" file on the VM's Desktop.
Which file was created by the downloaded malware to persist on the host?**

I analyse event id 11 to have a look at the **targetevent id**  which is the file created by the malware to ensure persistence

<img width="451" height="340" alt="WINDOW LOG TASK 4" src="https://github.com/user-attachments/assets/4c7ebd86-cdc4-45ec-b232-58d098216503" />

**What is the Command & Control server malware connected to?**

In this task i analyse **_event id 3_** that dela with  network connection that reveal the destination ip address and port number that is use for the command and control.  as shown below: **_193.46.217.4:7777_**

<img width="482" height="389" alt="window log task 5" src="https://github.com/user-attachments/assets/f19ebfdd-b22a-4cf6-a631-41618d44d9d2" />


**which domain does the malicious IP correspond to?**

To find the domain corespondendt to the  malicious Ip address, i :analyse the event id 22 that log the dns query to reveal the dns query to be **hkfasfsafg.click** as shwon in the screenshot below

<img width="443" height="326" alt="window log task 6" src="https://github.com/user-attachments/assets/621dd834-da7a-4d6e-8b63-29ce990728ef" />

**### POWERSHELL COMMAND** 

Powershell is an important  tool in the window and at the same could be use  by an attacker  as  it cannot be easilly monitor  using the event id  1 on symon.
For every command run on powershell, sysmon or event veiwer will  not  log a new process creation  events.  we will see a single event ID 1 stating that powershell.exe was launched, with no information about the executed commands.

 On  powershell.exe, you can run hundreds of different commands within the same terminal session without creating new processes for each action. This is why Sysmon is not very helpful here, and you'll need to find an alternative logging approach.However powershell has ist internal way of monitoring every event that comes on in powershell.

To get this i open the powershell, and navigate to the powershell directory using the following command:

_**cd C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\**_ 

**Review the Administrator's PS history on the attached VM.
Which PowerShell command was executed first?**

Using the command, **cd C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\**_ ,
 list all file using the **_ls_**  comnand,  the use the  **_cat .\ConsoleHost_history.txt_**.
a shown in the screenshot below:

<img width="494" height="373" alt="poweshell task 1" src="https://github.com/user-attachments/assets/bdeb8547-140c-41a0-9df4-15bef1261f67" />

The  first command command ran is the **_Get-ComputerInfo_**

**When did the Administrator run the first PS command? (Format: April 18, 2025)
Note: You might need to right-click the history file and open "Properties" to get the answer!**

In this task i  make of the command  **_Get-Item "ConsoleHost_history.txt" | Select-Object Name, CreationTime_** to  analyse the  detail of the command 

<img width="481" height="86" alt="powershell task 2" src="https://github.com/user-attachments/assets/15a124f9-f66d-4423-bd33-8d6f861eb4fe" />

**Can you find the flag stored in the PowerShell history? (Format: THM{...})
Note: You might want to check the PS history of other local users!**

i stand by navigating to the file using the command  cd **_C:\Users\thm.alex\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\_** then  i list the directory using **_ls_**   the  read the file ConsoleHost_history.txt using the  **_cat .\ConsoleHost_history.txt_** as shown in the screenshot belown:

<img width="483" height="371" alt="powershell task 3" src="https://github.com/user-attachments/assets/3479a4f2-44a7-4c5f-be71-19fdd3950597" />

TAKE AWAYS
 I undrstand  indows Logon Events, and how to efficiently read and interpret Event IDs 4624 (successful logon) and 4625 (failed logon)—they are foundational to SOC analysis.

I learn how to trace the Attack Chain: Group your logs by Logon ID and Process ID to quickly connect isolated events into a clear timeline.

 I deploy and Learn Sysmon: use  Sysmon  for deep visibility needed to understand what is truly happening on a host.

Monitor PowerShell Activity: Ensure PowerShell logging is active, as adversaries continue to rely on it heavily every year.
