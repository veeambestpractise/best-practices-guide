# Active Directory

Veeam Backup and Replication natively supports backup of Microsoft Active Directory controllers and allows for image level and granular AD items restore.

## Preparation

For Microsoft Active Directory, check the tombstone lifetime settings, as described in Veeam Explorers User Guide at Veeam Help Center (https://helpcenter.veeam.com/backup/explorers/vead_recommendations.html).

## Job configuration

For backup and restore of domain controllers to work properly application aware image processing opption has to be enabled in the job properties. For more details refer to the [corresponding section](https://helpcenter.veeam.com/docs/backup/vsphere/backup_job_vss_vm.html?ver=95) of the User Guide.

## Recovery verification

There are two Domain Controller roles available in application group configuration - for authoritative and non-authoritative restore. When testing recovery of one domain controller only choosing role with authoritative restore will speed up verification process.
