# 1.0 - HACK THE BOX: BASHED

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/026729e2-83a0-443d-a421-c2c83d2bd744)

**Vulnerability Explanation**: The web server has unintentionly exposed a php script in the /dev/ web directory that allows an unauthenticated user to remotely execute bash commands. Furthermore, the www-data user can run commands as the 'scriptmanager' user without needing a password which created a path for privilege escalation to that user and then to the 'root' user via a missconfigured cron job. 

**Vulnerability Fix**: Restrict access to the /dev/ web directory and remove the phpbash.php script. Additionally, require a password when performing sudo commands as the www-data user. 

**Severity**: Critical.

**Steps to reproduce the attack**: Enumeration was undertaken using NMAP which discovered the web server running on port 80. Web diirectories were then scanned using dirbuster which exposed the /dev/ directory and the .php scripts; navigating to this on a browser results in a web shell. Privilege escalation was then performed by taking advantage of the NOPASSWD value set for sudo commands on the www-data user by moving to the scriptmanager user shell. Escalation to the root user was then performed by editing a script to copy the /bin/bash binary to a new location with the setuid bit set

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.3 | TCP: 21, 22, 139, 445, 3632 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
