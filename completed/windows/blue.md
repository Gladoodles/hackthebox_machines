# 1.0 - HACK THE BOX: BLUE

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/eef432d7-68ae-4f5f-849f-2de4760179ac)

**Vulnerability Explanation**: The Windows machine is vulnerable to MS17-010 (Eternal Blue) which allows an attacker to take advantage of a flaw in the file-sharing protocol (SMBv1). By sending maliciously-crafted packets, the attacker can remotely install malware on a target, most commonly ransomware. These malicious packets execute a buffer overflow attack on a vulnerable service such as spoolsv.exe allowing the attacker to create a shell gaining control of a Windows system as the NT Authority user. 

**Vulnerability Fix**: Install the MS17-010 security patch, see https://learn.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010. It is also possible to disable SMBv1 for a short-term fix, but it is not recommended as a long-term solution to the vulnerability and the system should be patched. It was also noted that message signing is disabled. To increase SMB integrity and authenticity this should be enabled to prevent certain types of attacks, like man-in-the-middle attacks. 

**Severity**: Critical

**Steps to reproduce the attack**: First, open ports were enumerated using NMAP and then using the same tool a vulnerability script was run which identified CVE-2017-0143. Using the Metasploit Framework to exploit port 445 a payload was configured and when executed created a reverse shell connection to the machine with nt authority\system privileges.  

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.40 | TCP: 135, 139, 445, 49152, 49153, 49154, 49155, 49156, 49157 |

#### **2.1 - NAMP Scans** 

Firstly, an NMAP scan of the IP address revealed nine open ports, as identified in section 2.0. The scan also indicated the system was running Windows 7 Professional 7601 Service Pack 1 and that message signing was disabled. With this information in mind, a quick search revealed that this version is susceptible to Eternal Blue. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e15ba4ca-09d3-4148-9127-38544d373a1b)

An additional scan was performed which highlighted the machine is indeed vulnerable to CVE-2017-0143 (MS17-010). 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/2a2a9a75-bc31-48a3-9772-0a294bbcbb65)

Note: the SMB shares were also investigated but nothing out of the ordinary was discovered. 

## 3.0 - EXPLOITATION

Exploitation was performed using Metasploit, specifically the exploit/windows/smb/ms17_010_psexecrgy/EternalChamption SMB Remote Windows Code Execution module (#1).

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d6563b00-0058-4a22-ad14-762d8e755b58)

#### **3.1 - EternalBlue**

After selecting the module, the following settings were applied:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/f138c331-6801-464e-9dc7-c8a9f8fdcab1)

Running the exploit, we can see a reverse shell was obtained with NT authority system privileges:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9f88f208-73e1-4d0a-b980-707294088374)

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/2499ffbc-1de2-4e50-8828-4ac950cc2cd1)

## 4.0 - PRIVILEGE ESCALATION 

Privilege escalation was not required as the exploit created a reverse shell with NT authority system privileges, this is the highest privilege level that can be run on a Windows machine. This allows unrestricted access to all aspects of the operating system. 

Malicious actors who gain control of a process with system privileges can potentially install malware, steal sensitive information, or perform other malicious activities.

## 5.0 - POST-EXPLOITATION 

Lessons learned:
- Attempted to exploit this without Metasploit but ran into multiple issues when trying to run the python scripts, so I intend to revisit this.
- UPDATE: Managed to get the exploit to work without Metasploit using https://github.com/3ndG4me/AutoBlue-MS17-010. 
