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

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
