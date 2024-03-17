# 1.0 - HACK THE BOX: DEVEL

**Vulnerability Explanation**: The machine was hosting an IIS web server with a misconfigured FTP service. The FTP service allowed anonymous login with read/write capabilities, this can allow threat actors to upload malicious code and execute it by browsing to the file location. Furthermore, the machine is running an end-of-life operating system (Windows 7 Enterprise) and version (6.1.7600, build 7600) which is vulnerable to CVE-2011-1249. This CVE allows for local privilege escalation to the nt authority/system user allowing full take over of the system. 

**Vulnerability Fix**: 

**Severity**: 

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
