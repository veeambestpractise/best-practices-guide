# Search Server and Indexing

### Indexing and Search Overview 

Veeam Backup & Replication performs backups at the image-level using
APIs available from the underlying hypervisor. Thus, it has no direct
visibility of the file structure after backup is finished. However, it
is possible to use the Veeam File Level Restore wizard to mount VMs from
within a backup file and access/restore VM guest files.

If a user wants to perform file restore from the central Enterprise
Manager, it is not possible within a acceptable timeframe to mount all
backup files and VMs in it to find a file that the Enterprise Manager
user wants to restore.

To support advanced file-level restore scenarios, Veeam also offers the
capability to index files on VMs being backed up. Indexing is available
for both Microsoft Windows and Linux VMs and allows users of Veeam
Backup Enterprise Manager to browse and search for the necessary files
and to perform one-click file restores. The sections below will outline
some specific use cases for indexing and describe best practices and
guidelines for sizing.

### How Veeam Indexing Works 

Veeam indexing creates a separate catalog file for each restore point.
These index files are then used by Veeam Enterprise Manager to support
file browsing and search without a need to mount the restore point to
the Veeam backup server. Users can quickly search for files across
multiple restore points and view required files history when looking for
a specific version of a document. They can also select a specific VM and
browse the file system to restore guest files.

Veeam Backup Enterprise Manager also allows for file-level restore
functions to be delegated to a subset of users by leveraging the
role-based access control.

During the VM backup job run, the following operations are performed:

1.  Veeam accesses the guest OS (using credentials specified in the
    job settings) and injects a small run-time process to collect the
    list of files.

    -   For Microsoft Windows-based VMs, the process gathers file
        metadata by reading the MFT data of the supported file system
        (NTFS and ReFS).

    -   For Linux-based VMs, the process leverages the existing “locate”
        database that is commonly installed on most Linux distributions.

These operations take place in parallel with the backup and do not
increase the duration of the process. For more details on the indexing
process, refer to the [Veeam Backup Enterprise Manager User
Guide](http://helpcenter.veeam.com/backup/80/em/index.html?indexing_hiw.html).

1.  Veeam Backup & Replication creates a catalog (index) of the VM guest
    OS files and stores index files on the Veeam backup server in the
    *C:/VBRCatalog/Index/Machines/{vm\_name}* folder. Creation of index
    is extremely fast and has minimal impact on network and
    VMware environment.

**Important!** To search within the index catalog, it is necessary to
deploy Veeam Backup Enterprise Manager, as this component is in charge
of catalog data replication and retention (see [this
section](http://helpcenter.veeam.com/backup/80/em/index.html?veeam_backup_catalog.html)
of the User Guide for more details). If you enable indexing without
configuring Enterprise Manager, the catalog files in the *VBRCatalog*
folder of the backup server will never be collected and deleted, and may
fill up the disk drive.

1.  Once the index is created and stored on Veeam backup servers, the
    indexing service on Veeam Backup Enterprise Manager performs index
    replication — it aggregates index data for all VM image backups from
    multiple backup servers. This consolidated index is stored on the
    Veeam Backup Enterprise Manager server in the
    *C:/VBRCatalog/Index/catalog* and is used for search queries.

### When to Use Indexing? 

File-level indexing should be enabled only if you plan to utilize
advanced file search and one-click file level restore capabilities of
Veeam Backup Enterprise Manager (including delegated restore). While
indexing is a job-level setting, you can use filters to index only a
subset of files; it is also possible to exclude specific VMs from
indexing, as described, for example, in [this
section](http://helpcenter.veeam.com/backup/80/em/index.html?em_indexing_options.html)
of the Veeam Backup Enterprise Manager User Guide.

### Sizing Veeam Catalog 

Estimated raw space of the final index file is approximately 2 MB per
1,000,000 files for a single VM restore point on the Enterprise Manager
server, in the backup files and temporary folders on the Veeam backup
server own catalog. During the indexing process, indexing information is
temporarily stored on the local VM guest, requiring additional free
space on the system drive; estimated free space is about 10 MB per
1,000,000 files.

The Veeam Catalog Service is responsible for maintaining index data.
When running on the Veeam backup server, this catalog service will
maintain index data for all jobs that run on that specific server as
long as the backup data remains on disk. When running on the Enterprise
Manager server, the service will replicate index data from all managed
Veeam backup servers into the local catalog, so it should be sized
appropriately to hold all data from the remote Veeam servers.

-   When using a *Standard* license, Enterprise Manager will only keep
    index data for restore points still in repositories.

-   For *Enterprise* and *Enterprise Plus* licenses, you can configure
    Enterprise Manager to keep indexes even longer, with the default
    being 3 months. This can significantly increase the amount of space
    required for the catalog.

#### Example

There are two backup jobs configured to process 2 VMs, with 10 000 000
files per each VM. Backup jobs run two times a day, producing 60 restore
points a month. The default Enterprise Manager setting is used for
catalog retention (3 months).

Space required on the first drive in the VM (*С:\\* drive) can be
calculated as follows:

-   For Windows VMs: 100 MB per one million files and directories of all
    saved restore points with indexing enabled.

**Note:** This was tested with one million randomly named
20-character-long filenames in one directory. Depending on the saved
metadata and folder structure of the files, the value can be lower or
higher.

-   For Linux VMs: 50 MB per one million files and directories of all
    saved restore points with indexing enabled. Linux indexes require
    round about 50% less space because **mlocate** does not index any
    metadata (such as timestamps and ownership information).

-   For Enterprise Manager: 20 MB per one million files \* 60 restore
    points per month \* 3 months (for default Enterprise
    Manager retention). A total of 3.5 GB per indexed VM with
    10,000,000 files.

### Recommended Settings

Follow these recommendations when setting up Veeam indexing:

-   Place the catalog on a dedicated volume of good performance disk. To
    change the default Veeam Catalog folder location, refer to this
    Veeam Knowledge Base article: <http://www.veeam.com/kb1453.>

-   Consider that by default, Veeam enables NTFS compression on the
    catalog folder. This can reduce the space requirements by well
    over 50%. However, for very large catalogs (with 100s of VMs and
    10's of millions of files) it can be more beneficial to use a
    Windows 2012 R2 volume with Data Deduplication enabled. This volume
    should be dedicated to index files and configured to run
    deduplication functions outside of the normal backup window.

-   It is recommended to enable indexing only on VMs where the advanced
    search capabilities are necessary. Use filters to exclude
    unnecessary files from indexing (Windows system folder, Program
    Files and other system directories are excluded by default). For the
    Linux systems to be indexed, make sure they have **mlocate** or
    another compatible **locate** package installed.

**Note:** If you have SUSE Linux Enterprise Server (SLES) 11, consider
that indexing will not work, due to no native **mlocate** package. It is
recommended upgrading to SLES 12, which *does* have native **mlocate**
package, however you may use this openSUSE package &gt;
http://software.opensuse.org/package/mlocate

-   Configure index retention in Veeam Backup Enterprise Manager to the
    minimum necessary to meet the IT policy requirements. Index
    retention setting is available in the Enterprise Manager web console
    under **Configuration &gt; Settings &gt; Guest File System
    Catalog**.

**Note**: Veeam Backup Enterprise Manager SQL database
(*VeeamBackupReportin*g) will not grow much while using indexing
functions, as this database will only store the corresponding metadata.

-   To enhance search performance, SSDs can be used. If you plan to
    index a very large number of VMs, it is recommended to limit the
    search scope at restore to a single VM before you click the search
    button – this will bring faster results.

### Using Veeam Backup Search (Optional Component) 

In its early versions, Veeam did not have its own indexing engine;
instead, it used a connector named Veeam Backup Search to connect to the
Microsoft Search Server 2010 that provided search capabilities. Since
then, Veeam’s own indexing engine was developed for this purpose.

Currently, Veeam Backup Search is not necessary, as Veeam indexing
engine is likely to perform better.

If you need, however, to use that Veeam Backup Search component (and,
respectively, Microsoft Search Server) for indexing, consider the
following:

-   Microsoft Search Server Express Edition can be used, as it has no
    limitations for the number of indexed files.

-   Other editions of Microsoft Search Server deliver higher
    scalability, because Search Server components can be separately
    installed on multiple servers. Actually, if you are using Veeam
    Backup Enterprise Manager, consider that it can spread the load
    between multiple Microsoft Search Servers Express automatically.

-   Microsoft Search Server functionality is used to scan content in the
    shared *VBRCatalog* folder on the Veeam Backup Enterprise Manager
    server and to create a content index on the Search Server; this
    content index is used to process search queries. For more details,
    refer to the [Veeam Backup
    Search](http://helpcenter.veeam.com/backup/80/em/index.html?installing_veeam_backup_search.html)
    section of the User Guide.

**Note**: Though using content index streamlines the search process, the
index itself can require significant space on disk in
*C:\\VBRCatalog\\Journal\\\[YYYY\_MM\]\\\[search-server\]*.

-   Search Server requires an SQL database for its operation. Consider
    that Microsoft SQL Server Express Edition leverages only one CPU,
    which limits the Search Server performance. Besides, the database
    size supported by this edition is also limited (in particular, 10 GB
    for Microsoft SQL Server 2008 R2 Express Edition or later).