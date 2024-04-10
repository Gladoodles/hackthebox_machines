# 1.0 - HACK THE BOX: Irked

![image](https://github.com/Gladoodles/hackthebox_machines/assets/96867367/777ebaaf-4cdb-481d-ab9e-7e2c262ae40d)

**Vulnerability Explanation**: The machine was running a vulnerable version of IRC and when exploited it allowed an attacker to gain remote access to the system. Furthermore, it was possible to switch users to 'djmardov' due to a backup file which contained a password that could be used for accessing another password file hidden using steganography. It was then possible to elevate privileges to root by exploiting a tool which is under development, called 'viewuser'. 

**Vulnerability Fix**: Update IRC to the latest release and restrict access to djmardov home directory to only themselves. It is also recommended to remove the password contained in the .backup file, change the password and store in a password manager. Steganography should also not be used to store sensitive information, such as passwords. 

**Severity**: Critical

**Steps to reproduce the attack**: 

## 2.0 - ENUMERATION
| **IP ADDRESS** | **OPEN PORTS** |
|----------|--------------------|
| 10.10.10.117 | TCP: 80, 111, 22, 35202, 8067, 6697, 65534 |

## 3.0 - EXPLOITATION

#### **3.1 - [exploit]**

## 4.0 - PRIVILEGE ESCALATION 

#### **4.1 - [exploit]**

## 5.0 - POST-EXPLOITATION 
