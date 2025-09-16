<div align="center" style="display:inline-block;">
<img width="1139" height="147" alt="Image" src="https://github.com/user-attachments/assets/0ed460db-ec96-49c0-826c-9cc47dd0a502" /><br>
  Room Link: <a href="https://tryhackme.com/room/investigatingwindows">Investigating Windows</a>
</div><br>



---

# Objective

This room is all about investigate a compromised Windows machine by analyzing tasks, logs, and system files to uncover signs of attacker activity and gather forensic evidence.

---



## Whats the version and year of the windows machine?
<img width="563" height="97" alt="Image" src="https://github.com/user-attachments/assets/e25731f7-de02-4987-b587-a5eff90ce6da" />

Answer: `Windows Server 2016` 

---



## Which user logged in last?

<img width="756" height="176" alt="Image" src="https://github.com/user-attachments/assets/203e7c2c-4887-4db3-8bbd-b9fef7174b3d" />

Answer: `Administrator` 

---



## When did John log onto the system last?

<img width="432" height="444" alt="Image" src="https://github.com/user-attachments/assets/9acc2fc8-c7b0-4127-83ff-6c213d6523a8" />

Answer: `03/02/2019 5:48:32 PM`

---



## What IP does the system connect to when it first starts?

- The entry in Startup Programs `c:\tmp\p.exe` is configured to run automatically when the system starts.
since its command includes `-s \\10.34.2.3`, it connects to that IP right at boot and runs `'net user'`, likely to enumerate user accounts remotely

<img width="1094" height="358" alt="Image" src="https://github.com/user-attachments/assets/311c7183-8027-4c49-82bb-fa77df1b1231" />

Answer: `10.34.2.3`

---



## What two accounts had administrative privileges (other than the Administrator user)?

<img width="749" height="208" alt="Image" src="https://github.com/user-attachments/assets/a2ad10ac-318b-4bf3-98da-f6ba47a17836" />

Answer: `Guest, Jenny`

---




## Whats the name of the scheduled task that is malicous.

Scheduled tasks:

<img width="1548" height="281" alt="Image" src="https://github.com/user-attachments/assets/4e48956d-7ceb-4f61-82f4-7b62dae054b1" />

Among the scheduled tasks,`Clean file system`, `flashupdate22`, `GameOver`, are stands out as suspecious based on its actions


<h3> - Clean file System </h3>
<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/9c9010e6-a30d-4c43-99fd-fd5d620b0aee" />
  
- `Clean file system` runs a script from `C:\TMP\nc.ps1`, which is not a standard system location. The command includes a listener flag (-l 1348), often used to wait for remote connections something attackers use to open backdoors.
      
<h3> - flashupdate22 </h3>
<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/4383e929-841f-48dd-82eb-7e20cbc02309" />

- `flashupdate22` runs a hidden PowerShell command:`-WindowStyle Hidden -nop -c ""`, Although the command `-c ""` appears empty, it can be injected with payloads, and the task name mimics a real update but behaves unlike one.


<h3> - GameOVer </h3>
<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/48d3841f-13fe-4b4a-b877-808c4a224347" />

- `GameOver` runs `C:\TMP\mim.exe sekurlsa::LogonPasswords > C:\TMP\o.txt`, a credential-dumping command typically Mimikatz. It extracts login passwords and saves them to a file.

*TryHackMe flags "Clean file system" as the malicious task because it sets up a listener for remote access, indicating active backdoor behavior.*

Answer: `Clean file system`

---



## What file was the task trying to run daily?
<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/19abeb40-4294-4d74-a308-7d0ce1f0ba5f" />

Answer: `nc.ps1`

---



## What port did this file listen locally for?

<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/2020f6b8-0e4d-466b-947d-d608ceb143be" />

Answer: `1348`

---



## When did Jenny last logon?

<img width="524" height="556" alt="Image" src="https://github.com/user-attachments/assets/3565ed7e-19a4-4e3b-b377-de3e6dc481ea" />

Answer: `Never`

---



## At what date did the compromise take place?

<img width="1443" height="255" alt="Image" src="https://github.com/user-attachments/assets/e8d88978-c2fc-4b99-a957-c564bc64805c" />

Answer: `03/02/2019`

---

## During the compromise, at what time did Windows first assign special privileges to a new logon?

- To find this, filter the Security logs by the date and time of the compromise and Event ID 4672, which indicates a Special Logon- a unique event triggered when privileged access is assigned to a new session.

<img width="1483" height="544" alt="Image" src="https://github.com/user-attachments/assets/22cf916c-aad7-4c13-8e3c-307f201473e3" />

Answer: `03/02/2019 4:04:49 PM`

---


## What tool was used to get Windows passwords?

<img width="1539" height="484" alt="Image" src="https://github.com/user-attachments/assets/0a6b7e55-c813-4a9b-9675-9f95b422a5b9" />

The scheduled task `GameOver` runs `mim.exe` to dump credentials. Navigating to that path,

<img width="1119" height="728" alt="Image" src="https://github.com/user-attachments/assets/4555c737-db56-4a14-a61b-9440eb69bec1" />

We see several files including `mim.exe`. Opening mim-out shows the tool name "*Mimikatz*", confirming it was used for password dumping.

Answer: `Mimikatz`

---

## What was the attackers external control and command servers IP?

<img width="1655" height="726" alt="Image" src="https://github.com/user-attachments/assets/f0dd23b8-c879-4d1b-b6ad-437c7305005f" />

The hosts file shows redirection of trusted domains like google.com to `76.32.97.132`, which is not a legitimate Google IP. This is a classic sign of host file tampering, used to reroute traffic to an attacker-controlled C2 server.

Answer: `76.32.97.132`

---

## What was the extension name of the shell uploaded via the servers website?

<img width="1047" height="191" alt="Image" src="https://github.com/user-attachments/assets/75a3d866-f052-4f4e-8275-edd0255e6a4c" />

Navigating to `C:\inetpub\wwwroot` – the location of web server log files for Microsoft Internet Information Services, which is the common web server for Windows.

Answer: `.jsp`

---

## What was the last port the attacker opened?

<img width="1402" height="186" alt="Image" src="https://github.com/user-attachments/assets/53e87d3c-fe75-474a-8502-ee67cd11e4aa" />

We checked Windows Firewall Inbound Rules and filtered by Rules without a Group to isolate manually added entries. A suspicious rule named "Allow outside connections for development" revealed the last opened port 1337.

Answer: `1337`

---


## Check for DNS poisoning, what site was targeted?

<img width="790" height="545" alt="Image" src="https://github.com/user-attachments/assets/6d3e65ff-79f9-480a-97d6-0bf7d6c03922" />

We checked the hosts file and found that google.com was redirected to a suspicious IP. This shows DNS poisoning, where the attacker targeted google.com to reroute traffic to their own server.

Answer: `google.com`








