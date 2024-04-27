# 1.0 - HACK THE BOX: VALENTINE

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9011e492-9af8-42a6-b9d2-98cd2a63eb1a)

**Vulnerability Explanation**: The machine is vulnerable to a security flaw which exploits the OpenSSL cryptographic software library, known as Heartbleed (CVE-2014-0346). This can allow remote attackers access to sensitive information by sending malformed packets to a vulnerable server to return random blocks of memory from its process space. Whilst random, the attacker may get 'lucky' or obtain enough information from continuous attacks to gather sensitive information such as usernames, passwords or cryptographic keys. 

Furthermore, the tmux service was running as the root user, but it had the 'hype' user group assigned to it with r+w permissions. This allows anyone within the 'hype' group to make changes and hijack sessions running with root privileges. 

**Vulnerability Fix**: 

**Severity**: Critical

**Steps to reproduce the attack**: The first step was to enumerate open services running on the machine using NMAP, additionally a vulnerability scan was performed. Then, using dirbuster to brute force web directories on port 80 and 443 the 'dev' directory was discovered along with the SSH key (password protected) and notes.txt files. Identified by the vulnerability scan, the heartbleed exploit was then used to find the password for the SSH key. It was then possible to log into the machine via SSH as the 'hype' user. With a foothold on the machine, it was then possible to enumerate running services and bash history to see a tmux session running with root privileges. Since tmux had 'hype' group permissions, and the user 'hype' is part of that group with r+w permissions it was possible to connect to the running tmux instance, with root privileges allowing full take over of the machine. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.11.13 | TCP: 22, 80, 443 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
