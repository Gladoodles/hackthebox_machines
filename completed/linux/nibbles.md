# 1.0 - HACK THE BOX: Nibbles

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/dbf56053-9b4f-4533-b8d2-477efeece094)

**Vulnerability Explanation**: The server is running a vulnerable version of Nibbleblog (4.0.3) which can be exploited to gain a reverse shell (CVE-2015-6967). An attacker would first need to log into the /admin.php web directory to run this exploit, as it relies on uploading a shell via the 'My Image' plugin page. This exploit is possible because file extensions are not checked when files are uploaded. Due to weak password implementation, it was possible to gain access to the admin page. Once a foothold was gained on the machine, it was then possible to elevate privileges to root by editing the monitor.sh script to create another reverse shell. Because the 'nibbler' user could run this script with sudo without needing a password, the reverse shell connected as root allowing for full control of the system. 

**Vulnerability Fix**: Update Nibbleblog to the latest release. Furthermore, remove the 'NOPASSWD:' option from the line in the sudoers file so that the user is prompted to enter a password to run the monitor.sh script. 

**Severity**: Critical

**Steps to reproduce the attack**: Enumeration was performed using NMAP to discover the web service running on port 80, then a manual review of the page source revealed the /nibbleblog directory. Knowing this, additional web directory brute forcing was done using dirbuster to reveal the admin.php log in page. After a few attempts of guessing the password, it was possible to log in and run the exploit necessary to obtain a reverse shell into the machine. 

Privilege escalation was then undertaken by exploiting the monitor.sh script by inserting a command to create another reverse shell connection, but this time with elevated privileges as the script could be run as sudo without requiring a password.

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.75 | TCP: 22, 80 |

#### **2.1 - NMAP**

An NMAP scan identified two open ports, 80 and 22.

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/59b1870c-4ab8-427e-81b9-57909c95529f)

#### **2.2 - port 80 & dirbuster**

Navigating to the web service running on port 80 we are greeted with a simple webpage displaying 'Hello world!" but when viewing the page source we can see a comment about another web directory named /nibbleblog/. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/671c20b7-8aa4-4f78-a3ba-85cea23733a6)

With this knowledge a quick web crawl scan was performed using dirbuster, which identified a number of directories. The main one of interest is the /admin.php directory and after a few manual username and password attempts the credentials admin:nibbles worked. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/38ba6028-0149-4a9f-9910-1c3ca6a0c7fe)

Once logged into the admin page we were able to identify the current version (4.0.3) and investigate some exploits. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d4003278-eff2-4f86-94aa-a1cdd6ec9d5a)

## 3.0 - EXPLOITATION

#### **3.1 - CVE-2015-6967**

After accessing the admin page and identifying nibbleblog was running on version 4.0.3, a search on Exploit-DB shows this version is suseptable to an exploit which allows a threat actor to upload a malicious file. 

The exploit was obtained from https://github.com/dix0nym/CVE-2015-6967 and then a reverse shell payload was crafted using msfvenom. A listener was set up and the url, username, password and payload information was parsed into the script and executed, which promptly provided a connection back into the system. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/33b2fd4b-d7fc-4a6a-bc83-c618a06aeff0)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9e7c0d9e-6b3c-4831-93f7-727f79f7974c)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/82548cd7-8ab2-4277-8664-64c3026363c8)


## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
