# 1.0 - HACK THE BOX: Irked

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/777ebaaf-4cdb-481d-ab9e-7e2c262ae40d)

**Vulnerability Explanation**: The machine was running a vulnerable version of IRC and when exploited it allowed an attacker to gain remote access to the system. Furthermore, it was possible to switch users to 'djmardov' due to a backup file which contained a password that could be used for accessing another password file hidden using steganography. It was then possible to elevate privileges to root by exploiting a tool which is under development, called 'viewuser'. 

**Vulnerability Fix**: Update IRC to the latest release and restrict access to djmardov home directory to only themselves. It is also recommended to remove the password contained in the .backup file, change the password and store in a password manager. Steganography should also not be used to store sensitive information, such as passwords. 

**Severity**: Critical

**Steps to reproduce the attack**: Enumeration was performed using nmap to first discover the open ports. After connecting to one of the IRC ports, it was then possible to enumerate the software version (3.2.8.1) to search for available exploits. Using metasploit, a payload was configured and the exploit run to first gain access as the 'ircd' user. As this user, the .backup file was discovered within djmardov's home directory which exposed a password that was used to extract another password hidden within the irked.jpg file located on the main webpage. This password was then used to ssh to djmardov's account. A binary called viewuser (under development) was discovered and by creating a file called listusers in the /tmp/ directory and inserting code into it a reverse shell connection was made with root privileges.

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.117 | TCP: 80, 111, 22, 35202, 8067, 6697, 65534 |

#### **2.1 - NMAP**

Using the two following nmap scans the open ports were dsicovered:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/293dbfa8-8246-4479-b09d-848b7aba8a4e)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/51015b81-c8de-4de5-bf0f-5dce7ee0e811)

#### **2.2 - IRC** 

Connecting to the IRC port, it was possible to find out what version of IRC was running, which is Unreal 3.2.8.1. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/5d9945cd-58d5-4cc4-8c75-c89a38d57bb4)

Knowing this, a lookup on Searchsploit provided a backdoor command execution exploit on Metasploit. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/52ff5100-09dd-4efe-b893-d8f1bb43bbec)

## 3.0 - EXPLOITATION

#### **3.1 - unreal ircd 3281 backdoor**

A quick search for the exploit provided only one option named 'unreal_ircd_3281_backdoor':

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/da781067-1bd2-4d20-b238-11779d2d4180)

The following options were configured and a payload specified:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e0f0a9e5-b1ba-4fef-8dba-8de84a34f258)

Upon execution of the exploit it was possible to obtain a reverse shell on the machine as the user 'ircd':

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e2d352fa-9a18-4e92-8eef-f51b929312ed)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/1bda863c-2d62-43b6-9364-c83592f568f1)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
