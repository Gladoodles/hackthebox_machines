# 1.0 - HACK THE BOX: LAME 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/8bd8a2d3-63cb-48f9-9fec-03bf31325c74)

**Vulnerability Explanation**: The SMB server is vulnerable to CVE-2007-2447 and when exploited allows RCE and a shell with root privileges. Guest login was also enabled on the /tmp and /IPC$ shares. The distccd service was also found to be vulnerable to CVE-2004-2687 allowing an unauthenticated user to create a shell as a low privilege user. The FTP server appeared to be vulnerable to CVE-2011-2523, however, the exploit did not execute properly but anonymous login was allowed. Following the exploitation of the distccd service it was discovered that the suid bit was set on the nmap binary which allows a user to run commands as root when running nmap interactively. 

**Vulnerability Fix**: Update all services to the latest versions. Disable anonymous login on the FTP service. SMB should also be configured with credentials and guest enumeration should be removed. The SUID bit should be removed from the namp binary. 

**Severity**: Critical

**Steps to reproduce the attack**: Using NMAP to enumerate the services running on the host, called Lame. Access to the system was gained by running known exploits using the Metasploit Framework on the Samba and ddistccd services, resulting in a shell on the target system. We also took advantage of the nmap binary which had the suid bit set, allowing commands to be run with root privileges when using nmap interactively. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.3 | TCP: 21, 22, 139, 445, 3632 |

The following NMAP scan was executed which discovered a number of open ports. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/933e9f64-e0cf-4216-9eea-5b7a86fdacaa)

## 3.0 - EXPLOITATION

#### **3.1 - Samba 3.0.20**

The /tmp and /IPC$ shares were not password protected and some files were downloaded, however, they did not yield any credentials. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/4accf14b-ab42-492e-8c72-8d3afb407e29)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/88b34184-807c-4e68-bfe4-33b826cb5921)

Using the Metasploit Framework we used the Samba "username map script" command execution exploit to obtain a root shell. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/34efd60a-3904-46db-bb83-ddabbc8c87f4)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/30032405-5c69-41b7-89d5-cb497e58d3dc)

Running the command 'whoami' we can see that we have gained root privileges.

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9a52b04b-d393-476c-b827-d64a4d61419f)

#### **3.2 - distccd**

Using Metasploit module distcc_exec we can gain a shell as a low privileged user. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/97f4c016-a8f9-480f-9f70-ee2ab0a5e4b1)

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/ec85c72e-5e9e-4b22-9d1d-cca3004a081a)

### **3.3 - vsftpd 2.3.4**

Anonymous login is allowed on the FTP server, however, no files were available and we could not change directory. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9bf641e5-5b77-4edc-8b9b-df0ac9b5f33a)

The following exploits were run but they did not successfully execute, this exploit was not pursued any further. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/a8482228-a5d8-426e-a88d-9430bb4acbdf)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - Samba 3.0.20**

Following the Samba username map script exploit we can see that by running the command 'whoami' that we have gained root privileges without needing to escalate. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9a52b04b-d393-476c-b827-d64a4d61419f)

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e429ac76-8fdd-4421-9ad1-37c2bf9d4661)


#### **4.2 NMAP SUID bit**

It was found that the nmap binary has the suid bit set. This allows users who execute the binary to temporarily assume the privileges of the file's owner, and in this case it is root. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/83e948b9-363c-43b1-8926-264c6dcc1302)

Using nmap in interactive mode we can execute commands by using '!' followed by the instruction. The SUID bit can be removed by using the following command:

```text
chmod u-s /usr/bin/nmap
```

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/7183c0f1-931e-491a-a743-0787681e2872)

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/ce9ec62c-e223-4aa0-8e6d-ddc8e080f8f9)

## 5.0 - POST-EXPLOITATION 

Lessons learned:
- Don't forget to use whoami after gaining a shell to prove what user you are!
- Finish enumerating services first and then move onto exploitation, I missed the much easier Samba exploit first time around because I focused on the distccd service first. 





