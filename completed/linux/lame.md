# HACK THE BOX: LAME 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e24cc444-abba-43ca-af87-98ad93bac192)

**Vulnerability Explination**: The SMB server is vulnerable to CVE-2007-2447 and when exploited allows RCE and a shell with root privileges. Guest login was also enabled on the /tmp and /IPC$ shares. The distccd service was also found to be vulnerable to CVE-2004-2687 allowing an unauthenticated user to create a shell as a low privilge user. The FTP server appeared to be vulnerable to CVE-2011-2523, however, the exploit did not execute properly but anonymous login was allowed. 

**Vilnerability Fix**: Update all services to the latest versions. Disable anonymous login on the FTP service. SMB should also be configured with credentials and guest enumeration should be removed. 

**Severity**: Critical

**Steps to reproduce the attack**: Using NMAP to enumerate the services running on the host, called Lame. Access to the system was gained by running exploits using the Metasploit Framework for the Samba and ddistccd services resulting in root privileges. 

## ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.3 | TCP: 21, 22, 139, 445, 3632 |

The following NMAP scan was executed which discovered a number of open ports. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/933e9f64-e0cf-4216-9eea-5b7a86fdacaa)



## EXPLOITATION

TO FINISH THIS REPORT!

## PRIVILEGE ESCALATION 

## POST-EXPLOITATION 





