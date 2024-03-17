# 1.0 - HACK THE BOX: DEVEL

**Vulnerability Explanation**: The machine was hosting an IIS web server with a misconfigured FTP service. The FTP service allowed anonymous login with read/write capabilities, this can allow threat actors to upload malicious code and execute it by browsing to the file location. 

Furthermore, the machine is running an end-of-life operating system (Windows 7 Enterprise) with version (6.1.7600, build 7600) which is vulnerable to CVE-2011-1249, as detailed in Microsoft Security Bulletin MS11-046. By crafting an application which exploits the Windows Ancillary Function Driver (AFD), it is possible for an attacker to elevate their privileges to the nt authority/system user, allowing full control of the machine. 

**Vulnerability Fix**: Windows 7 is no longer supported by Microsoft and will not receive additional security updates. It is recommended the OS is upgraded to the most current distribution. If it is not possible to update the OS then apply the most recent security patches available. Additionally, remove anonymous login to the FTP service and user access restricted to those who need it. Appropriate NTFS folder permissions should also be set within the FTP service. 

**Severity**: Critical.

**Steps to reproduce the attack**: 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.5 | TCP: 21, 80 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
