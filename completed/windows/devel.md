# 1.0 - HACK THE BOX: DEVEL

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/ed9f0ff9-12c5-4566-88a4-9c9ac8e16f30)

**Vulnerability Explanation**: The machine was hosting an IIS web server with a misconfigured FTP service. The FTP service allowed anonymous login with read/write capabilities, this can allow threat actors to upload malicious code and execute it by browsing to the file location. 

Furthermore, the machine is running an end-of-life operating system (Windows 7 Enterprise) with version (6.1.7600, build 7600) which is vulnerable to CVE-2011-1249, as detailed in Microsoft Security Bulletin MS11-046. By crafting an application which exploits the Windows Ancillary Function Driver (AFD), it is possible for an attacker to elevate their privileges to the nt authority/system user, allowing full control of the machine. 

**Vulnerability Fix**: Windows 7 is no longer supported by Microsoft and will not receive additional security updates. It is recommended the OS is upgraded to the most current distribution. If it is not possible to update the OS then apply the most recent security patches available. Additionally, remove anonymous login to the FTP service and user access restricted to those who need it. Appropriate NTFS folder permissions should also be set for the FTP service. 

**Severity**: Critical.

**Steps to reproduce the attack**: Enumeration of open ports was undertaken using NMAP. Additional enumeration was performed using dirbuster to identify web directories. With anonymous login possible on port 21 a test.txt document was uploaded and then by navigating to the webpage we confirmed the location of the ftp folder. Knowing the web server was running an IIS based server from the NMAP scan and the default webpage display, a reverse shell .aspx file was uploaded to the ftp server. Creating a netcat listener and triggering the reverse shell by navigating to it on the IIS web server, we gained access to the system. 

By enumerating the system information, a search online identified a vulnerability on the machine (CVE-2011-1249). By compiling the exploit and then transferring it to the Windows machine, the exploit was run, and privilege escalation was achieved to the system user.  

## 2.0 - ENUMERATION

| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.5 | TCP: 21, 80 |

An NMAP scan was conducted which identified two ports, a ftp server on port 21 which allowed anonymous login and an IIS web server which was running on port 80. 

#### **2.1 - Dirbuster** 

A dirbuster scan was undertaken to brute-force web directories based on a list available from the webpage detailed below, this yielded limited results.
- https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/iis-internet-information-services

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/528814b7-2489-4702-858d-1d08e1f4add0)

Browsing to the webpage we get the default welcome page. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/bc24f3e2-b9f6-4ac1-846f-4986da1ae3a8)

#### **2.2 - FTP** 

Connecting to the FTP service using anonymous credentials we can see it contains iisstart.htm and welcome.png files. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/f1c9875a-623e-422f-8899-52ff73aceb55)

Based on the files available, we have can see the FTP folder location is shared with the IIS service but to confirm this a test.txt file was uploaded. Navigating to http://devel.htb/test.txt confirmed the upload location. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/bdfcaadc-fde7-4691-8488-05ef48339428)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/dfd3b7a1-8b82-41f7-9386-0ac3cd640280)

## 3.0 - EXPLOITATION

#### **3.1 - Reverse Shell** 

As discovered in the enumeration phase, we can see it is possible to upload a file to the FTP service and then navigate to it via the web service the machine is running. Knowing this, a shell.aspx reverse shell payload was generated using msfvenom, then logging back into the FTP service the payload was uploaded. Starting a netcat listener and then visit the web-directory http://devel.htb/shell.aspx/ we obtain a reverse shell. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e7059207-3404-4a50-93a6-56116768bfb3)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
