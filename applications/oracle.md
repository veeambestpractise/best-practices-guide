# Oracle 
Veeam supports backup and restore of Oracle databases.

## Backup Options:
The following options are available to backup Oracle Databases
-	Virtual Image Application Aware Backup.
-	Veeam Agent based Backup for Physical & VMs with RDM
-	RMAN Dump to staging server

### Virtual Image Application Aware Backup.
Veeam natively supports backup of virtualized Oracle database servers.
Unsupported Configuration:
1.	32-bit Oracle running on 64-bit operating systems is not supported.
2.	Oracle XE on Linux is not supported.
3.	Oracle Real Application Clusters (RAC) are not supported.

## Veeam Agent based Backup for Physical & VMs with RDM

