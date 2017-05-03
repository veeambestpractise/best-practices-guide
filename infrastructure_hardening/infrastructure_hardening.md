# Infrastructure Hardening
Running your Veeam Backup & Replication infrastructure in a secure configuration is a daunting task even for security professionals. This chapter provides practical advice to help administrators to harden their infrastructure following security best practices so that they can confidently deploy their Veeam services and lower their chances of being compromised.

Security is all about a pre-care balance between protection and usability. While hardening is about securing the availability infrastructure against attacks, detecting when an attack is or has taken place and making sure logs and traces are saved for law-enforcement and security specialists when needed.

When an attack is detected while in progress, it is vital to react as quick as possible by securing the backup data so it stays integer and available in need of restoring to a safe state.

## Protect
Protecting is understanding what and whom you are protecting, your availability infrastructure, against. Hardening is the process of securing a system by reducing its attack surface and thus eliminating as many risks as possible. This is typically done by removing all non-essential software programs and utilities from the deployed Veeam components.

While these programs may offer useful features to the administrator, if they provide ‘back-door’ access to the system, they must be removed during the hardening process.


**Simplicity is security**, overly complex designs become harder for the IT team to manage and overlook and it makes it easier for an attacker to exploit and stay in the shadows. Simpler designs that can be easily overviewed are more secure.




## Detect

## React



More specifically, Veeam Backup & Replication administrators focus on these aspects of infrastructure security:

* Access Control
* Managing access to backup data and components
* Encryption
* Password management
* Managing ports and other feature of the availability infrastructure

## Access Control

Monitor your accounts for suspicious activity


### Service Accounts

Enforcing the use of strong passwords across your infrastructure is a valuable control. It’s more challenging for attackers to guess passwords/crack hashes to gain unauthorized access to critical systems. Selecting passwords of 10 characters with a mixture of upper and lowercase letters, numbers and special characters is a good start.

Supporting a strong password policy with a robust account lockout policy, which locks accounts after a small number of incorrect attempts, can stop password guessing attacks dead in their tracks.


By setting your server/domain to lock a Veeam Service account for a period of time after a number of incorrect attempts to login are made, you will help prevent hackers from using automated password guessing tools from gaining access to your system. But also will give you visibility into suspicious behavior towards your data.

Principle of least privilege. Provide the minimal privilege needed for some operation to occur. If a process or system is exploited, you don't want to allow an attacker to gain any more access than is minimally required.




## Removing Unused Components
Enterprise Manager


### vPower NFS Service
You can switch off the Veeam vPower NFS Service if you do not plan on using the following Veeam features: SureBackup, Instant Recovery, or Other-OS FLR operations.

## Encryption
If encryption is enabled for configuration backup the passwords are also stored in the configuration backup files.







## Password management


## Ports


Try not to use obscure ports and other tricks to try and hide Veeam ports and protocols in use, while they may look like a good choice. In practice this often makes the infrastrutcure harder to manage which opens other possibilities for attackers. Obscurity is not security!




You can check which ports are in use by which service on a Windows system by using:

`netstat -bona > portlist.txt` you can open the text file with for instance `notepad portlist.txt`


Note: All software can be exploited. All software has flaws that allow an attacker with enough motivation to exploit it. By hardening you will make it much harder for an attacker to get far fast.
