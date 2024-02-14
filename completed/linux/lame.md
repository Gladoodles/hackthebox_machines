# 1.0 - HACK THE BOX: LAME 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e24cc444-abba-43ca-af87-98ad93bac192)

**Vulnerability Explination**: The SMB server is vulnerable to CVE-2007-2447 and when exploited allows RCE and a shell with root privileges. Guest login was also enabled on the /tmp and /IPC$ shares. The distccd service was also found to be vulnerable to CVE-2004-2687 allowing an unauthenticated user to create a shell as a low privilge user. The FTP server appeared to be vulnerable to CVE-2011-2523, however, the exploit did not execute properly but anonymous login was allowed. 

**Vilnerability Fix**: Update all services to the latest versions. Disable anonymous login on the FTP service. SMB should also be configured with credentials and guest enumeration should be removed. 

**Severity**: Critical

**Steps to reproduce the attack**: Using NMAP to enumerate the services running on the host, called Lame. Access to the system was gained by running exploits using the Metasploit Framework for the Samba and ddistccd services resulting in root privileges. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.3 | TCP: 21, 22, 139, 445, 3632 |

The following NMAP scan was executed which discovered a number of open ports. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/933e9f64-e0cf-4216-9eea-5b7a86fdacaa)

## 3.0 - EXPLOITATION

#### **3.1 - Samba 3.0.20**

The /tmp and /IPC$ shares were not password protected and some files were downloaded, however, they did not yield any credentials. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/4accf14b-ab42-492e-8c72-8d3afb407e29)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/88b34184-807c-4e68-bfe4-33b826cb5921)

Using the Metasploit Framework we used the Samba "username map script" command execution exploit to obtain a root shell. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/34efd60a-3904-46db-bb83-ddabbc8c87f4)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/30032405-5c69-41b7-89d5-cb497e58d3dc)

Running the command 'whoami' we can see that we have gained root privileges.

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9a52b04b-d393-476c-b827-d64a4d61419f)

#### **3.2 - distccd**



TO FINISH THIS!!

## 4.0 - PRIVILEGE ESCALATION 

#### **Samba 3.0.20**

Following the Samba username map script exploit we can see that by running the command 'whoami' that we have gained root privileges without needing to escalate. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9a52b04b-d393-476c-b827-d64a4d61419f)



## 5.0 - POST-EXPLOITATION 





