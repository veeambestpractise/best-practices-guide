# Oracle 

Veeam supports backup and restore of Oracle databases.

Unsupported Configuration:
-	32-bit Oracle running on 64-bit operating systems is not supported.
-	Oracle XE on Linux is not supported.
-	Oracle Real Application Clusters (RAC) are not supported.

## Backup Options:
The following options are available to backup Oracle Databases
-	Virtual Image Application Aware Backup.
-	Veeam Agent based Backup for Physical & VMs with RDM
-	RMAN Dump to staging server

### Virtual Image Application Aware Backup.
Veeam natively supports backup of virtualized Oracle database servers.
To backup Oracle Database Server with Virtual Image Applicate Aware Backup, simply enable [application aware processing]( https://helpcenter.veeam.com/docs/backup/vsphere/application_aware_processing.html?ver=95) on Oracle Database server. In addition to Image Level Backup Veeam can backup database archived logs created by the Oracle system if archive logging mode is turned on, archived logs backup improves the RPO and provides [point in time recovery]( https://helpcenter.veeam.com/docs/backup/explorers/veo_rest_scenario_2_pt_time.html?ver=95) 

To learn more about archived logs or change the archived logs modes, Please follow [Oracle KB]( https://docs.oracle.com/cd/B19306_01/server.102/b14231/archredo.htm#i1006184) 

**_Note:_**
> Oracle database can run in one of the two modes, by default, Oracle database is created in NOARCHIVELOG mode. You can query to see archievelog mode:
```
SQL> connect sys/xxx as sysdba;
connected.
SQL> archive log list;
SQL> select log_mode from v$database;
```
## Veeam Agent based Backup for Physical & VMs with RDM
Veeam supports agent level backup of Physical Oracle Server, please note the following:
-	Oracle running on Windows Platform is natively supported with VSS.
-	Oracle running on Linux Platform requires guest processing [Pre Freeze & Post Thaw Scripts]( https://helpcenter.veeam.com/docs/backup/vsphere/pre_post_scripts.html?ver=95) 
## RMAN Dump to Staging Server:
Another option available for non-supported Oracle configuration is to use native tool from Oracle *RMAN* to dump the Oracle backup to staging server and protect the staging server with Veeam.
