# Proxy Server - Microsoft Hyper-V

In Microsoft Hyper-V environments VMs usually reside on local storage and CSV (Cluster Shared Volume). Veeam Backup & Replication leverages the VSS (Volume Shadow Copy) framework and proprietary Microsoft Hyper-V components to retrieve data and it acts as the VSS requestor. Interacting with the VSS framework, it obtains information about the infrastructure and identifies volumes where VM files are located and triggers the VSS coordinator to create the volume snapshots.

The backup process, in Microsoft Hyper-V environments, expects the VM to be quiesced before taking the snapshot of the volume to make sure that there are not incomplete transactions in a database or no open files.
Veeam Backup & Replication uses three method to prepare Hyper-V VMs for backup:
 - [Online backup](https://helpcenter.veeam.com/docs/backup/hyperv/online_backup.html?ver=95) - the native Microsoft Hyper-V method for creating application-consistent backup without any downtime.

 - [Offline backup](https://helpcenter.veeam.com/docs/backup/hyperv/offline_backup.html?ver=95) - an alternative native method to obtain consistent backup. It requires a little downtime: Hyper-V uses the OS hibernation to freeze the I/O coming to the VM.

 - [Crash-consistent backup](https://helpcenter.veeam.com/docs/backup/hyperv/crash_consistent_backup.html?ver=95) - Veeam's proprietary method that allows the creation of crash-consistent backup without hibernating nor suspending the VM (no downtime required).

 Whenever possible, Veeam Backup & Replication uses online backup to quiesce VMs. If online backup cannot be performed, Veeam Backup & Replication uses one of the other two methods to prepare a VM for backup.

**Note:** If online backup cannot be performed, Veeam Backup & Replication fails over to the crash-consistent backup.
If you do not want to produce a crash-consistent backup, you can instruct Veeam Backup & Replication to use the offline backup method.

### On-host backup









### Off-host backup
