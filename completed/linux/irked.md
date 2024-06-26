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

Escalation to root does not seem possible as the ircd user, however, it was possible to gain root privileges with djmardov user account. 

#### **4.1 - djmardov**

Escalation to the djmardov user was possible because as the ircd user can access djmardov home directory which contained a .backup file which contained a password. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d610e5f1-228d-44bd-b743-017023917c1d)

This password could then be used to extract another password hidden using steganography on the irked.jpg image displayed on the website. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/8186dc9c-99ec-4214-8624-3eab7f71bf98)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/287de04e-19e6-4ce5-bdba-e7998c658159)

With this new password, it was possible to log into djmardov's account using SSH. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/db7884a8-f51d-425e-81e1-96d9cfab90c8)

#### **4.2 - root**

After some enumeration the binary named 'viewuser' was discovered which appeared to be a programme in development. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0c3e649a-23b7-49c1-a7e0-cb49e0c48aee)

(screenshots missing)

This binary, when executed looked for a directory named '/tmp/listusers' which was non-existent. So by creating a file called 'listusers' in the /tmp/ directory, we could then make this file executable with the 'chmod +x listusers' command and insert reverse shell code into it, or any other command for that matter. Since djmardov was part of the sudoers group, we could execute a reverse shell with root privileges. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/2d1dfab8-ef2a-4ad2-9c74-feb08454c132)

## 5.0 - POST-EXPLOITATION 

Lessons learned:
- Be more diligent with documenting steps when performing exploits, as I had not taken screenshots of the privilege escalation to root. 
