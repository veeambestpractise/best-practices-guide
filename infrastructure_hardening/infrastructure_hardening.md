# Infrastructure Hardening

Hardening is the process of securing a system by reducing its attack surface and thus eliminating as many risks as possible. This is typically done by removing all non-essential software programs and utilities from the Veeam components deployed. While these programs may offer useful features to the administrator, if they provide ‘back-door’ access to the system, they must be removed during the hardening process.

More specifically, Veeam Backup & Replication administrators focus on these aspects of infrastructure security:

* Access Control
* Managing access to backup data and components
* Encryption
* Password management
* Managing ports and other feature of the availability infrastructure

## Access Control

### Service Accounts







## Removing Unused Components
Enterprise Manager


### vPower NFS Service
You can switch off the Veeam vPower NFS Service if you do not plan on using the following Veeam features: SureBackup, Instant Recovery, or Other-OS FLR operations.

## Encryption
If encryption is enabled for configuration backup the passwords are also stored in the configuration backup files.







## Password management


## Ports
You can check which ports are in use by which service on a Windows system by using:

`netstat -bona > portlist.txt` you can open the text file with for instance `notepad portlist.txt`
