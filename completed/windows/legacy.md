# 1.0 - HACK THE BOX: LEGACY

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0dcdac0e-be06-4750-9bb9-fc15fe091d3b)

**Vulnerability Explanation**: The machine is susceptible to two critical CVE's; CVE-2017-0143 (MS17-010 - EternalBlue) and CVE-2008-4250 (MS08-067 Conficker). 

EternalBlue is an exploit that targets a vulnerability in Microsoft's implementation of the Server Message Block (SMB) protocol. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted packets to the target system's SMBv1 server. 

Conficker is an exploit that targets a vulnerability in the Windows Server service. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted RPC (Remote Procedure Call) requests to the target system's Server service.

After the initial foothold, both vulnerabilities allow an attacker to access the system as the highest privileged user (NT AUTHORITY/SYSTEM) without needing to escalate. 

**Vulnerability Fix**: Apply the relevant security patches from Microsoft, MS17-010 and MS08-067. 

**Severity**: Critical

**Steps to reproduce the attack**: NMAP was utilised to scan for open ports and to carry out a vulnerability scan. 

The MS08-067 vulnerability was first exploited by using a python script from the following GitHub Repository, https://github.com/jivoi/pentest/blob/master/exploit_win/ms08-067.py. A reverse shell payload was then crafted using msfvenom and the shellcode was inserted into the python script, once executed a shell was obtained on the machine. 

The MS17-010 vulnerability was exploited using the Metasploit Framework by selecting the appropriate module and configuring options specific for this machine. The exploit was run using the default meterpreter reverse shell payload, which returned an interactive shell. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.4 | TCP: 135, 139, 445 |

## 3.0 - EXPLOITATION

#### **3.1 - Conficker (MS08-067)**

#### **3.2 - EternalBlue (MS17-010)** 

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
