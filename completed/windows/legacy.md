# 1.0 - HACK THE BOX: LEGACY

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0dcdac0e-be06-4750-9bb9-fc15fe091d3b)

**Vulnerability Explanation**: The machine is susceptible to two critical CVE's; CVE-2017-0143 (MS17-010 - EternalBlue) and CVE-2008-4250 (MS08-067 Conficker). 

EternalBlue is an exploit that targets a vulnerability in Microsoft's implementation of the Server Message Block (SMB) protocol. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted packets to the target system's SMBv1 server. 

Conficker is an exploit that targets a vulnerability in the Windows Server service. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted RPC (Remote Procedure Call) requests to the target system's Server service.

After the initial foothold, both vulnerabilities allow an attacker to access the system as the highest privileged user (NT AUTHORITY/SYSTEM) without needing to escalate. 

**Vulnerability Fix**: Windows XP is no longer supported by Microsoft as of April 2014, it is recommended that this machine is upgraded to the latest version of Windows Server. In the meantime, apply the relevant security patches from Microsoft, MS17-010 and MS08-067. Additionally, message signing should be enabled. 

**Severity**: Critical

**Steps to reproduce the attack**: NMAP was utilised to scan for open ports and to carry out a vulnerability scan. 

The MS08-067 vulnerability was first exploited by using a python script from the following GitHub Repository, https://github.com/jivoi/pentest/blob/master/exploit_win/ms08-067.py. A reverse shell payload was then crafted using msfvenom and the shellcode was inserted into the python script, once executed a shell was obtained on the machine. 

The MS17-010 vulnerability was exploited using the Metasploit Framework by selecting the appropriate module and configuring options specific for this machine. The exploit was run using the default meterpreter reverse shell payload, which returned an interactive shell. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.4 | TCP: 135, 139, 445 |

#### **2.1 - NMAP Scans** 

Port enumeration and vulnerability scanning was performed using NMAP. The following scan identified open port numbers 135, 139, and 445. Furthermore, the scan also identified the operating system as Windows XP and that message signing is disabled. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/09e0dd8f-ed6d-4237-8450-dc3d5d6ac716)

A vulnerability scan identified that the machine is susceptible to MS08-067 and MS17-010. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/7f8455ee-d4c3-4ca4-9423-c09968d01ea0)

## 3.0 - EXPLOITATION

#### **3.1 - Conficker (MS08-067)**

The exploit used for MS08-067 was obtained from https://raw.githubusercontent.com/jivoi/pentest/master/exploit_win/ms08-067.py. A reverse shell was generated using msfvenom and the output was replaced with the shellcode inside the python script. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/1b107596-fa8b-41f0-bb53-13c84abade34)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0ff97e87-677c-472e-a8c9-e3c1f322f1aa)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/3f719d95-7ad3-413e-87f8-58e4aa3b830a)

With the shellcode in place, the python script was executed using the following command:
```text
python3 ms08-067.py 10.10.10.4 445
```

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/a48ea28e-4b34-4b4b-9309-fe5055836668)

This retuned a shell on the machine with elevated privileges. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/a1b48799-477c-47e2-8552-43a8cb8ce51f)

#### **3.2 - EternalBlue (MS17-010)** 

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
