# HACK THE BOX: LAME 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/e24cc444-abba-43ca-af87-98ad93bac192)

## ENUMERATION

I started by adding the ip address of the machine to my /etc/hosts file and giving it the hostname of lame.htb. Next I executed the following scan using NMAP:
```text
nmap lame.htb -sV -A -p- -T4 -Pn
```
Note: I did start a scan without the -Pn switch but yielded no results. 

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/933e9f64-e0cf-4216-9eea-5b7a86fdacaa)

- -sV: Probe open ports to determine service/version info
- -A: Enable OS detection, version detection, script scanning, and traceroute
- -Pn: Treat all hosts as online -- skip host discovery
- -p- scan all ports
- -T<0-5>: Set timing template (higher is faster)

From this scan we can see the following services:
- FTP on port 21 (vsftpd 2.3.4)
- SSH on port 22 (OpenSSH 4.7p1)
- Samba on port 139/445 (3.0.20-Debian)
- Distccd on port 3632 (v1 4.2.4)

## EXPLOITATION

TO FINISH THIS REPORT!

## PRIVILEGE ESCALATION 

## POST-EXPLOITATION 





