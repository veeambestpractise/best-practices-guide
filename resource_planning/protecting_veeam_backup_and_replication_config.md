Protecting Veeam Backup & Replication Configuration
---------------------------------------------------

### Protecting the Veeam Backup Server

As recommended by best practice for disaster recovery you can place
Veeam Backup & Replication installation on a virtual machine but do not protect
it with backups or replicas, it can have issues with disconnections and corruption during the snapshot process so it is not recommended. Out-of-the box Veeam automatically creates a configuration backup on the default backup repository, the favored approach is point the configuration backup to a remote repository or if possible another machine designated as a managed server acting as a repository to only hold the config backup file.

These configuration backups contain all the information about Veeam Backup & Replication such as Backup Infrastructure components plus objects, Backup jobs (passwords are not stored by default), Sessions and Tape setup. The configuration backup can be used to automatically rebuild the Veeam Backup & Replication server with all objects, sessions and jobs.

To restore all jobs and their metadata (you will be asked for all required passwords during the restore process if they are not stored in the configuration backup encrypted). Please refer to the Veeam Backup & Replication User Guide for further details: <https://helpcenter.veeam.com/docs/backup/vsphere/vbr_config.html?ver=95>

**Tip:** If encryption is enabled for configuration backup the passwords
are also stored in the configuration backup files.

### Planning for Disaster Recovery of Veeam Backup Server
Having a solid disaster recovery strategy for your availability components, like the backup server, is key to a successful recovery. For all situations follow these basic guide lines:

1. Make sure the daily configuration backup is **not** placed in the default location on the backup server itself
2. Modify the backup configuration backup settings to point to a secure backup repository on a different location/site
3. Schedule the configuration backup to run when the backup server is least occupied;
4. Make sure the configuration backup is encrypted to protect the configuration details. Also all passwords are than stored in the configuration backup files
5. Check that you receive notifications about the status of the configuration backup job results
6. Think about placement of the backup server, configuration backup and database. This is highly depended on the overall infrastructure design and DR strategy of your organization

By default, Veeam Backup & Replication is configured to create a daily configuration backup. The resulting configuration backup file is stored in the `\VeeamConfigBackup\%BackupServer%` folder on the default backup repository. However, for security’s sake, it is recommended that you do **not** store configuration backups on the **default backup repository** or in any other folder on the backup server. In this case, if the backup server fails, its configuration data will remain, and you will be able to recover the failed backup server.

Also check the location of the configuration database, when the database is external ensure this server is replicated to the secondary site. If the server is rebuilt in the event of a disaster, you may start the secondary location Veeam Backup & Replication server and connect to the configuration backup. This will help to lower overall Recovery Time Objective (RTO).

**Tip** Use Veeam's `File Copy Job` to place a copy of the configuration backup at the DR site. You can configure another repository for that purpose.

**Note** All data required for a restore is directly placed within the backup file (which VMs are in the backup file as well as deduplication and encryption information), even in the event that configuration database is lost or damaged you can set up a new Veeam backup server and import the backup files there, or even use the stand-alone “Extract” utility (both a command line and a graphical version are provided). Then you will be able to restore VMs, files and application data without restoring the configuration database.

**Note:** Backup copy jobs do not process configuration backups. Remember that configuration backups are not processed with backup to tape jobs; if you want to store configuration backups on tape use file to tape jobs instead.



### Antivirus on Veeam Servers

Antivirus software monitors all 'write' operations on the operating system and this also extends to Veeam backup files. Data that is processed by a backup proxy and repository can overload the antivirus system so that it blocks the backup files, this can slow down the backup process or even lead to backup file corruption. To avoid this it is recommended to add the following items to the list of antivirus exclusions on all Veeam servers including Veeam backup server, proxy server, repository server, WAN accelerator server, tape server, and others.

#### Folders on the Veeam Server

-   *C:\Program Files\Veeam *

-   *C:\Program Files(x86)\Veeam *

-   *C:\Program Files\Common Files\Veeam *

-   *C:\Program Files(x86)\Common Files\Veeam *

-   *VBRCatalog* (\[`HKLM\SOFTWARE\Veeam\Veeam Backup Catalog\`]
    `CatalogPath` value)

-   *NFS* (Configured in each repository, stored in
    `[HKLM\SOFTWARE\Wow6432Node\Veeam\Veeam NFS\]`
    `RootFolder`value)

-   *C:\VeeamFLR\\**

-   *C:\Windows\Veeam *

#### Folder on Guest OS for VSS

-   *C:\Windows\VeeamVssSupport *

-   *C:\Windows\VeeamLogShipper *

#### Folder on VMware Backup Proxies and CIFS Repository Gateway

-   *C:\Program Files(x86)\Veeam *

-   *C:\Windows\Veeam *

#### Folders on Windows Repositories

-   *C:\Program Files(x86)\Veeam *

-   *C:\Windows\Veeam *

-   *All Veeam repository folders *

#### Folders on WAN accelerator

-   *C:\Program Files(x86)\Veeam *

-   *C:\Windows\Veeam *

-   *All WAN cache folders *


#### Files

-   *VeeamAgent.exe *

-   *VeeamAgent64.exe *

-   .vmdk  .vbk .vlb .vib .vrb .vbm .vbo


**Tip:** Due to the complex nature of antivirus software some additional exclusions may be needed. If the antivirus has a logging or history system you can review its logs to detect whether it has taken any actions that might affected Veeam Backup & Replication operations.

Consider that other services or process may be using ports configured for the Veeam vPower NFS Service. To avoid possible issues it is recommended to stop the Veeam vPower NFS Service if you do not plan to use it. Make sure that none of the NFS ports are used by other software (including antivirus systems). For more information please refer to this Veeam Knowledge Base article: <http://www.veeam.com/kb1055>.
