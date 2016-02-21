# Encryption

### Overview 

The encryption technology in Veeam Backup & Replication allows you to
protect data both while it is in transfer between backup infrastructure
components and at rest, when it stored at its final destination. This
can be disk, tape or a cloud repository. Customers can use one of
encryption methods or a combination of both to protect against
unauthorized access to important data through all steps in the data
protection chain.

Veeam Backup Enterprise Manager additionally provides the password loss
protection option that allows authorized Veeam users to recover data
from the backup even if the password is lost. Note that you can recover
data only if the backup server on which you decrypt data is connected to
the same Veeam Backup Enterprise Manager server as the backup server on
which the backup file was encrypted.

The encryption algorithms used are industry standard in all cases,
leveraging AES-256 and public key encryption methods. The User Guide
provides detailed information on the encryption algorithms and standards
used by the product.

The following sections describe encryption options available in the
product, what they protect, when they should be used and best practices
for their use.

### Backup and Backup Copy Job Encryption 

#### What does it do?  {#what-does-it-do .pseudo4}

Backup and backup copy job encryption is designed to protect data at
rest. These settings protect data if an authorized user gets access to
backup files outside of the backup infrastructure. Authorized users of
the Veeam console do not need to know the password to restore data from
encrypted backups. Encryption does not prevent authorized Veeam users
from being able to access data stored in backups.

An example is the use of rotated drives for an offsite repository.
Because these drives are rotated offsite, they are at a higher risk of
falling into the hands of unauthorized users. Without encryption
enabled, these unauthorized users could install their own copy of Veeam
Backup & Replication and gain access to the stored backups easily.

On the other hand, if the backup files are encrypted, unauthorized users
cannot access any data in the backups or even learn any critical
information about the backup infrastructure as even backup metadata is
encrypted. Without the key used for encryption or access to the original
Veeam Backup & Replication console itself, the backup files remain
secure.

#### How does it work?  {#how-does-it-work .pseudo4}

When Veeam Backup & Replication reads a block from disk, it compresses
the block (unless compression is disabled at the job level), encrypts it
with a session key generated for that job session, and stores the block
into the backup file.

#### When to use it?  {#when-to-use-it .pseudo4}

Backup and backup copy job encryption should be used if backups are
transported offsite, or if unauthorized users may easily gain access to
backup files in another way than by using the Veeam console. Common
scenarios are:

-   Offsite backups to a repository using rotated drives

-   Offsite backups using unencrypted tapes

-   Offsite backups to a Cloud Connect provider

-   Regulatory or policy based requirements to store backups encrypted

#### Best Practices  {#best-practices .pseudo4}

-   Enable encryption if you plan to store backups in locations outside
    of the security domain.

-   While CPU usage for encryption is minimal for most modern
    processors, some amount of resources will still be consumed. If
    Veeam backup proxies are already highly loaded, take it into account
    prior to enabling job-level encryption.

-   Use strong passwords for job encryption and develop a policy for
    changing them regularly.

**Note:** Veeam Backup & Replication helps with this, as it tracks
passwords’ age.

-   Store passwords in a secure location.

-   Obtain Enterprise or a higher-level license for Veeam Backup &
    Replication, configure Veeam Backup Enterprise Manager and connect
    backup servers to it to enable data loss prevention.

-   Back up the Veeam Backup Enterprise Manager configuration database
    and create an image-level backup of the Veeam Backup Enterprise
    Manager server. If these backups are also encrypted, make sure that
    passwords are not lost as there will be no data loss prevention for
    these backups.

### Tape Job Encryption 

#### What does it do?  {#what-does-it-do-1 .pseudo4}

Similar to backup job encryption, tape job encryption is designed to
protect data at rest. These settings protect data if an unauthorized
user gains access to tape media outside of the backup infrastructure.
Authorized users do not need to know the password to restore data from
encrypted tape backups. Encryption does not prevent authorized Veeam
users from being able to access data stored in tape backups.\
\
Typical use case is to protect data on tapes when media is shipped to an
offsite location or to a 3rd party vendor. Without encryption enabled, a
lost tape could easily be accessed, and data stored on tapes could be
compromised.

#### How does it work?  {#how-does-it-work-1 .pseudo4}

Data is read from disk, and the session encryption key is used to
encrypt data blocks as they are written to tape. Tape encryption can
leverage either hardware tape encryption (if present and enabled) or
software-based encryption. If the tape drive supports hardware
encryption, the session key is sent to the tape device via SCSI commands
and the drive itself performs the encryption prior to writing data to
tape. This allows encryption to occur with no impact on performance or
CPU of the tape server. If the tape hardware does not support
encryption, Veeam falls back automatically to using software-based AES
256 data encryption prior to sending it to the tape device.

#### When to use it?  {#when-to-use-it-1 .pseudo4}

Tape job encryption should be used any time you want to protect the data
stored on tape from unauthorized access by a 3rd party. Tapes are
commonly transported offsite and thus have a higher chance of being lost
and turning up in unexpected places. Encrypting tapes can provide an
added layer of protection if the physical tapes are lost.\
\
If tape jobs are pushing already encrypted data to tape (for example,
Veeam data from backup jobs that already have encryption enabled), you
may find it acceptable to not use tape-level encryption. However, be
aware that a user who gets access to the tape will be able to restore
the backup files. Although this user will not be able to access the
backup data in those files, some valuable information, for example, job
names used for backup files, may leak.

#### Best Practices  {#best-practices-1 .pseudo4}

-   Enable encryption if you plan to store tapes in locations outside of
    еру security domain.

-   Consider the risks/rewards of enabling tape job encryption even if
    the source data is already encrypted and decide appropriately for
    level of risk.

-   Use strong passwords for tape job encryption and develop a policy
    for changing them regularly (you can use Veeam Backup & Replication
    password age tracking capability).

-   Store passwords in a secure location.

-   Obtain Enterprise or a higher-level license for Veeam Backup &
    Replication, configure Veeam Backup Enterprise Manager and connect
    backup servers to it to enable data loss prevention.

-   Back up the Veeam Backup Enterprise Manager configuration database
    and create an image-level backup of the Veeam Backup Enterprise
    Manager server. If these backups are also encrypted, make sure that
    passwords are not lost as there will be no data loss prevention for
    these backups.

### Network Transport Encryption 

#### What does it do?  {#what-does-it-do-2 .pseudo4}

Unlike the backup and tape job encryption features, the network
transport encryption feature is designed to protect data “in-flight”.
For example, when the proxy is sending data across the network to the
backup repository, the data can be encrypted between these two points
even if job-level encryption is not enabled. This is primarily useful
when the network between the source and target is not trusted, for
example, when sending data across the Internet.

#### How does it work?  {#how-does-it-work-2 .pseudo4}

Network encryption in Veeam Backup & Replication is controlled via the
global Network Traffic options.

Whenever two backup infrastructure components need to communicate with
each other over the IP network, a dynamic key is generated by the backup
server and communicated to each node over a secure channel. The two
components then establish an encrypted connection with each other using
this session key, and all communications between these two components
for that session are then encrypted with this key. The key is of
one-time use and is discarded once the session is complete.

#### When to use it?  {#when-to-use-it-2 .pseudo4}

Network transport encryption should be used if the network between two
backup infrastructure components is untrusted or if the user desires to
protect Veeam traffic across the network from potential eavesdropping.

By default, Veeam Backup & Replication automatically encrypts
communication between two nodes if either one or both has an interface
configured (if used or not) that is not within the RFC1918 private
address space (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16,
169.254.0.0/16). Veeam also automatically uses network-level encryption
for any connection to a Cloud Connect provider.

#### Best Practices  {#best-practices-2 .pseudo4}

-   Enable encryption if a possibility of network-level eavesdropping is
    a security concern.

-   Network-level encryption can use significant CPU resources,
    especially on the encrypting side (source) of the connection. Make
    sure that component nodes have enough resources.

-   Use network-level encryption only where required. If backup
    infrastructure components are running on a network that is using
    non-RFC1918 IP addresses but is still private and secure from
    eavesdropping, consider using the
    **DisablePublicIPTrafficEncryption=1** registry key under
    **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Veeam\\Veeam Backup and
    Replication** at the Backup & Replication (management) server to
    disable automatic network-layer encryption.