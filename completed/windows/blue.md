# 1.0 - HACK THE BOX: BLUE

**Vulnerability Explanation**: The Windows machine is vulnerable to MS17-010 (EternalBlue) which allows an attacker to take advantage of a flaw in the file-sharing protocol (SMBv1). By sending maliciously-crafted packets, the attacker can remotely install malware on a target, most commonly ransomware. 

**Vulnerability Fix**: Install the MS17-010 security patch, see https://learn.microsoft.com/en-us/security-updates/securitybulletins/2017/ms17-010. It is also possible to disable SMBv1 for a short-term fix, but it is not recommended as a solution to the vulnerability and the system should be patched. 

**Severity**: Critical

**Steps to reproduce the attack**: First, open ports were enumerated using NMAP and then using the same tool a vulnerability script was run which identified CVE-2017-0143. Using the Metasploit Framework to exploit port 445 a payload was configured and when executed created a reverse shell connection to the machine with nt authority\system privileges.  

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.40 | TCP: 135, 139, 445, 49152, 49153, 49154, 49155, 49156, 49157 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
