# 1.0 - HACK THE BOX: Nibbles

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/dbf56053-9b4f-4533-b8d2-477efeece094)

**Vulnerability Explanation**: The server is running a vulnderable version of Nibbleblog (4.0.3) which can be exploited to gain a reverse shell (CVE-2015-6967). An attacker would first need to gain access to the /admin.php web directory to run this exploit and due to weak password implementation this was possible. Once a foothold was gained on the machine it was then possible to elevate privileges to root by editing the monitor.sh script to create another reverse shell. Becaue the 'nibbler' user could run this script with sudo without needing a password the reverse shell connected as root allowing for full control of the system. 

**Vulnerability Fix**: 

**Severity**: 

**Steps to reproduce the attack**: 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.75 | TCP: 22, 80 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
