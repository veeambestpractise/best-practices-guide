# Microsoft SQL Server

In addition to the image level backup of a VM that will include full backup of the SQL databases Veeam Backup and Replication can perform additional backup of transaction logs. This process is described in the [corresponding section](https://helpcenter.veeam.com/docs/backup/vsphere/sql_backup_hiw.html?ver=95) of the User Guide in details.

## Preparation

Transaction logs are processed periodically and stored in temporary folder inside of the VM before shipping to repository/shipping server. Default location of the temporary folder is %allusersprofile%\Veeam\Backup. To change temporary folder use SqlTempLogPath (STRING) registry value as described at [How It Works: SQL Server and Transaction Log Backup](https://helpcenter.veeam.com/docs/backup/vsphere/sql_backup_hiw.html?ver=95):

-   Path: `HKEY_LOCAL_MACHINE\SOFTWARE\Veeam\Veeam Backup and Replication`
-   Key: `SqlTempLogPath`
-   Type: REG_SZ
-   Default value: `undefined`

For the list of all registry keys responsible to fine-tuning MS SQL server backup (for example excluding certain databases from processing) refer to [KB2182](https://www.veeam.com/kb2182).

As restore is integral part of SQL Server protection, special attention should be paid to planning Veeam Explorer for SQL configuration, specifically network connectivity between mount server and staging servers in restricted environments. Ports used for communication between them are listed in the [Used Ports](https://helpcenter.veeam.com/docs/backup/vsphere/used_ports.html?ver=95#explorers) section of the User Guide.

## Job configuration

When backing up AlwaysOn availability group make sure all cluster nodes are processed by the same backup job for transaction logs processing and restores to work properly. Consider increasing cluster timeouts in case failover occurs during the backup, similar to Exchange DAG as per [KB1744](https://www.veeam.com/kb1744).

## Granular item restore

In certain scenarios use of staging server is necessary (see https://helpcenter.veeam.com/docs/backup/explorers/vesql_staging_server.html?ver=95). When staging server is used transaction logs from the backup are transported to staging server and replayed there. For that to work ensure that staging server has enough disk space in ADMIN$ share to store all log files.

If you have special features/enhancements/configuration settings on the production Microsoft SQL and/or Microsoft SharePoint server to be protected with Veeam, these custom settings should be implemented on the staging SQL Server, too.

One special case of custom settings that must be configured on staging server is encryption. When performing restore/export of encrypted database please refer to [KB2006](https://www.veeam.com/kb2006) for details on configuring the staging server.
