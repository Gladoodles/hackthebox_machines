# 1.0 - HACK THE BOX: LEGACY

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0dcdac0e-be06-4750-9bb9-fc15fe091d3b)

**Vulnerability Explanation**: The machine is susceptible to two critical CVE's; CVE-2017-0143 (MS17-010 - EternalBlue) and CVE-2008-4250 (MS08-067 Conficker). 

EternalBlue is an exploit that targets a vulnerability in Microsoft's implementation of the Server Message Block (SMB) protocol. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted packets to the target system's SMBv1 server. 

Conficker is an exploit that targets a vulnerability in the Windows Server service. It allows remote attackers to execute arbitrary code on vulnerable systems by sending specially crafted RPC (Remote Procedure Call) requests to the target system's Server service.

After the initial foothold, both vulnerabilities allow an attacker to access the system as the highest privileged user (NT AUTHORITY/SYSTEM) without needing to escalate. 

**Vulnerability Fix**: Apply the relevant security patches from Microsoft, MS17-010 and MS08-067. 

**Severity**: Critical

**Steps to reproduce the attack**: 

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
