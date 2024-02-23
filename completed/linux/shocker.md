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

The following NMAP scan was invoked which discovered two open ports. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/1c90605a-d62a-40ec-a5bc-cd7ad9748a1a)

SSH on port 2222 was running an outdated version of openSSH, however, due to the limited exploits available for this version it was not persued further. 

An exploit search for the Apache web server version 2.4.18 was performed but none were available. 

#### **2.1 Port 80**

Navigating to the webpage running on this port we are presented with a static page and a quick look at the source code shows very little information. The bug.jpg image was downloaded and checked for any hidden files but none was discovered. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/c7cf9dc0-5211-42bf-b683-271e866d7bfb)

A gobuster scan of the website discovered a number of hidden directories, namely the /cgi-bin/ directory. If a file is found in this directory it could be vulnerable to CVE-204-6271 (Shellshock).

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/780ed8bf-ab7c-45f8-b3b3-b5294e98a9f5)

A further gobuster scan for hidden files was done which discovered a file called 'user.sh', downloading this file showed it was a uptime test script. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e26926a5-7b42-4bb5-b9a8-b0233785ab48)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/89987e49-4ae6-44d3-b3c2-b7190eef75ea)

To further enumerate whether a Shellshock exploit could be performed an NMAP script was run (http-shellshock) which confirmed it was.

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/330179d8-7c03-4c9d-b21d-ca970ec82ca0)

## 3.0 - EXPLOITATION

#### **3.1 - Shellshock**

Exploitation of this vulnerability was executed by sending a curl request to the /cgi-bin/user.sh file discovered in the enumeration process. Prior to sending the curl request a listener was set up on the attacking machine. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/1ebb423b-c98c-4a8a-a144-7d791507305b)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e341bf2e-bf38-4d03-b9b9-b963d5c86178)

Upon executing the request we get a reverse shell as the user 'shelly'. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/6aacc506-9314-4c6f-9a9d-1abc152f9319)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/0cffe30d-1b2d-434f-95cd-9c0ab150b3bc)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - sudo -l**

Privilege escalation was simple, by running the command 'sudo -l' as the user shelly we can see that the /usr/bin/perl binary could be run as root without no password. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/eac23c6c-92f6-4f4c-a2be-6fb8437b8964)

A quick search on GTFO bins (https://gtfobins.github.io/gtfobins/perl/#sudo) provided the command we need to be run to allow for privilege escalation to root, as shown below:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/5a16e5a5-52f1-4b49-8782-f41866563ecd)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/6b12a4df-54d2-4654-bdf8-3c1d767a09fb)

## 5.0 - POST-EXPLOITATION 

- Use id rather than 'whoami' because the account might have elevated privileges.
- It's worthwhile searching whether each web directory discovered during the enumeration process has any exploits as I did not pick up on the /cgi-bin/ exploit for quite some time. 
