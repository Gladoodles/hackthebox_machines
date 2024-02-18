# 1.0 - HACK THE BOX: Shocker

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/ef35205b-e4c5-49af-ad35-82211133b7c8)

**Vulnerability Explination**: The Apache webserver is vulnerable to Shellshock (CVE-2014-6271). If environment variables are not properly sanitised before being executed remote attackers can execute arbitrary code by sending commands via HTTP requests. The pearl binary could be used to execute commands as the root user without providing a password (NOPASSWD).

**Vilnerability Fix**: It is recommended that the system be updated (https://ubuntu.com/security/notices/USN-2362-1) to fix the Shellshock vulnerability. To fix the Perl binary vulnerability, remove the NOPASSWD directive for the user in the sudoers file (/etc/sudoers), requiring users to enter their password before executing privileged commands with sudo.

**Severity**: Critical 

**Steps to reproduce the attack**: Following an NMAP scan to enumerate basic services, gobuster was used to brute force directories of the Apache webserver. Upon discovering the /cgi-bin/ directory gobuster was then used to brute force file extensions within that directory. A file named user.sh was discovered and a subsequent NMAP scan of the file revealed the vulnerability. A packet was then crafted using curl to inject code into the webserver via a HTTP request to create a reverse shell on the system. Privilege escalation was then achieved by exploiting the pearl binary which could be run as root without a password. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.56 | TCP: 80, 2222 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
