# 1.0 - HACK THE BOX: VALENTINE

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/9011e492-9af8-42a6-b9d2-98cd2a63eb1a)

**Vulnerability Explanation**: The machine is vulnerable to a security flaw which exploits the OpenSSL cryptographic software library, known as Heartbleed (CVE-2014-0346). This can allow remote attackers access to sensitive information by sending malformed packets to a vulnerable server to return random blocks of memory from its process space. Whilst random, the attacker may get 'lucky' or obtain enough information from continuous attacks to gather sensitive information such as usernames, passwords or cryptographic keys. 

Furthermore, the tmux service was running as the root user, but it had the 'hype' user group assigned to it with r+w permissions. This allows anyone within the 'hype' group to make changes and hijack sessions running with root privileges. 

**Vulnerability Fix**: Update OpenSSL to the latest version and remove the 'hype' group from the tmux service. 

**Severity**: Critical

**Steps to reproduce the attack**: The first step was to enumerate open services running on the machine using NMAP, additionally a vulnerability scan was performed. Then, using dirbuster to brute force web directories on port 80 and 443 the 'dev' directory was discovered along with the SSH key (password protected) and notes.txt files. Identified by the vulnerability scan, the heartbleed exploit was then used to find the password for the SSH key. It was then possible to log into the machine via SSH as the 'hype' user. With a foothold on the machine, it was then possible to enumerate running services and bash history to see a tmux session running with root privileges. Since tmux had 'hype' group permissions, and the user 'hype' is part of that group with r+w permissions, it was possible to connect to the running tmux instance, with root privileges allowing full take over of the machine. 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.11.13 | TCP: 22, 80, 443 |

The above ports were discovered using NMAP. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/75d09006-57e2-48b0-8129-e877c885f0d2)

Furthermore, a vulnerability scan was undertaken, which indicated that the website was vulnerable to the SSL-Heartbleed exploit:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/fe46d330-d7fd-4c63-8ce2-0442f8f536bb)

#### **2.1 - dirbuster **

Following discovery of port 80 and 443 a manual enumeration of the website just revealed a basic webpage with little information. Using dirbuster, a brute force of web directories was undertaken, which exposed an SSH key called 'hype_key' and a notes.txt file. It was assumed 'hype' was the username for the key. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/953d447c-f913-466a-ad5c-4ab6b15284a6)

The notes.txt file did not contain much information of use, but the hype_key was in a hex format:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/29378279-5484-4e58-938b-2d69ca960c0d)

By converting the hex to ASCII it revealed the SSH private key:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/8e39f6e7-97e7-4e44-91a3-af9f42c293b0)

An attempt was made to log into the SSH service running on the machine but the private key was protected with a password. 

## 3.0 - EXPLOITATION

#### **3.1 - Heartbleed**

A search for 'Heartbleed' on searchsploit displayed multiple downloadable exploits, in which the following was downloaded:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/27ccdf88-b91e-44c4-984c-c497b9adabd4)

By simply running the python script and directing it to the website, after a few attempts we find a string of text which looks like it may have been encoded with base64. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/efac99be-bd6c-4cdb-966f-f4b3d4f41a25)

Decoding the string, we are presented with the following string 'heartbleedbelievethehype':

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/3928b4b2-f858-45f0-a947-63e06d07e431)

It was surmised that this string was the password for the SSH key previously discovered. After attempting to sign in with the key a 'no mutual signature supported' error was displayed but by using the -o PubkeyAcceptedKeyTypes=+ssh-rsa option access to the machine was possible as the 'hype' user:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/d6168054-5c4c-4319-954e-850652aa5074)

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - Improper Group Permissions - Tmux**

Following manual enumeration of the machine and running the 'history' command, it was evident that the hype user was executing commands for a tmux process. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/2ce1b841-995b-45dd-8171-9ceab2fc4595)

This was confirmed by running the 'ps aux' command and by reviewing the file permissions we can see that this application has read + write access for the 'hype' group. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/c849507e-a615-495d-831b-edc7ddd25440)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/70f25deb-940c-4dba-b162-47a35bdae715)

Because tmux is a terminal multiplexer it allows multiple terminals to be created and in this case it was running in the background with the privilege level of the owner, which was root. By using the command 'tmux -S /.devs/dev_sess' we can connect to the session with root privileges:

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/6ff65bce-49b6-4062-af6b-74fba9707b64)
![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/31911f25-4b0a-4a41-947b-a3a6f703012c)

## 5.0 - POST-EXPLOITATION 
