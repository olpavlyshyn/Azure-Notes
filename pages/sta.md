## Storage Account

* The name must be unique across all existing storage account names in Azure.
* Account kinds:
    - ***General-purpose v2 accounts***: Basic storage account type for blobs, files, queues, and tables. Recommended for most scenarios using Azure Storage. (Blobs (all types: Block, Append, Page), Data Lake Gen2, Files, Disks, Queues, Tables)
    - ***General-purpose v1 accounts***: Legacy account type for blobs, files, queues, and tables. Use general-purpose v2 accounts instead when possible.(Blobs (all types), Files, Disks, Queues, Tables)
    - ***BlockBlobStorage accounts***: Storage accounts with premium performance characteristics for block blobs and append blobs. Recommended for scenarios with high transactions rates, or scenarios that use smaller objects or require consistently low storage latency. BlockBlobStorage accounts don't currently support tiering to hot, cool, or archive access tiers. This type of storage account does not support page blobs, tables, or queues.
    -  ***FileStorage accounts***: Files-only storage accounts with premium performance characteristics. Recommended for enterprise or high performance scale applications. This storage account kind supports files but not block blobs, append blobs, page blobs, tables, or queues. FileStorage accounts offer unique performance dedicated characteristics such as IOPS bursting.
    - ***BlobStorage accounts***: Legacy Blob-only storage accounts. Use general-purpose v2 accounts instead when possible.
* Performance tiers: 
    - ***Standard*** storage accounts are backed by magnetic drives and provide the lowest cost per GB. They're best for applications that require bulk storage or where data is accessed infrequently. 
    - ***Premium*** storage accounts are backed by solid state drives and offer consistent, low-latency performance. They can only be used with Azure virtual machine disks, and are best for I/O-intensive applications, like databases. Additionally, virtual machines that use Premium storage for all disks qualify for a 99.9% SLA, even when running outside of an availability set. This setting can't be changed after the storage account is created.
* Access tiers(https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal):
    - ***Hot*** - Optimized for storing data that is accessed frequently.
    - ***Cool*** - Optimized for storing data that is infrequently accessed and stored for at least 30 days.
    - ***Archive*** - Optimized for storing data that is rarely accessed and stored for at least 180 days with flexible latency requirements (on the order of hours).
    - Only the hot and cool access tiers can be set at the account level. The archive access tier isn't available at the account level.
    - Hot, cool, and archive tiers can be set at the blob level during upload or after upload.
    - Object storage data tiering between hot, cool, and archive is only supported in Blob Storage and General Purpose v2 (GPv2) accounts.  
* Replication: (https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy)
    - Data in an Azure Storage account is always replicated three times in the *primary region*.
    - Azure Storage offers two options for how your data is replicated in the primary region:
        - ***Locally redundant storage (LRS)***  copies your data synchronously three times within a single physical location in the primary region.
            - server level redundancy
        - ***Zone-redundant storage (ZRS)*** copies your data synchronously across three Azure availability zones in the primary region.
            - availability zones level redundancy
    - Azure Storage offers two options for copying your data to a *secondary region*:
        - ***Geo-redundant storage (GRS)*** copies your data synchronously three times within a single physical location in the primary region using LRS, then it copies your data asynchronously three times using LRS in the secondary region.
            - region level redundancy
        - ***Geo-zone-redundant storage (GZRS)*** copies your data synchronously across three Azure availability zones in the primary region using ZRS, then it copies your data asynchronously three times using LRS in the secondary region.
            - region level redundancy 
        - With GRS or GZRS, the data in the secondary region isn't available for read or write access unless there is a failover to the secondary region. For read access to the secondary region, configure your storage account to use read-access geo-redundant storage (RA-GRS) or read-access geo-zone-redundant storage (RA-GZRS). If the primary region becomes unavailable, you can choose to fail over to the secondary region. After the failover has completed, the secondary region becomes the primary region, and you can again read and write data. Azure Files does not support read-access geo-redundant storage (RA-GRS) and read-access geo-zone-redundant storage (RA-GZRS).
        - Only general-purpose v2 storage accounts support GZRS and RA-GZRS. 
* Data Protection Options:
    - Recovery
    - Point-in-time restore for block blobs:
        - To enable point-in-time restore, you create a management policy for the storage account and specify a retention period. During the retention period, you can restore block blobs from the present state to a state at a previous point in time.
        - Point-in-time restore requires that the following Azure Storage features be enabled before you can enable point-in-time restore: Soft delete, Change feed, Blob versioning
    - Soft Delete for Blobs:
        - Whensoft  delete for blobs is enabled on a storage account, you can recover objects after they have been deleted, within the specified data retention period. This protection extends to any blobs (block blobs, append blobs, or page blobs) that are erased as the result of an overwrite.
    - Soft Delete for Containers(preview)
    - Soft Delete for File Shares
    - Tracking
    - Versioning for Blobs
    - Blob Change Feed:
        The purpose of the change feed is to provide transaction logs of all the changes that occur to the blobs and the blob metadata in your storage account. The change feed provides ordered, guaranteed, durable, immutable, read-only log of these changes
* ***Blob Types***:
    - Azure Storage supports three types of blobs:
        - ***Block blobs*** store text and binary data. Block blobs are made up of blocks of data that can be managed individually. Block blobs store up to about 4.75 TiB of data. Larger block blobs are available in preview, up to about 190.7 TiB
        - ***Append blobs*** are made up of blocks like block blobs, but are optimized for append operations. Append blobs are ideal for scenarios such as logging data from virtual machines.
        - ***Page blobs*** store random access files up to 8 TB in size. Page blobs store virtual hard drive (VHD) files and serve as disks for Azure virtual machines.
* ***Queues***:
    - Azure Queue Storage is a service for storing large numbers of messages. You access messages from anywhere in the world via authenticated calls using HTTP or HTTPS. A queue message can be up to 64 KB in size. A queue may contain millions of messages, up to the total capacity limit of a storage account. Before version 2017-07-29, the maximum time-to-live allowed is seven days. For version 2017-07-29 or later, the maximum time-to-live can be any positive number, or -1 indicating that the message doesn't expire. If this parameter is omitted, the default time-to-live is seven days. 
* ***Tables***:
    - Azure Table storage stores large amounts of structured data. The service is a NoSQL datastore which accepts authenticated calls from inside and outside the Azure cloud. Azure tables are ideal for storing structured, non-relational data. 
    - Entity: An entity is a set of properties, similar to a database row. An entity in Azure Storage can be up to 1MB in size. An entity in Azure Cosmos DB can be up to 2MB in size.
    - Properties: A property is a name-value pair. Each entity can include up to 252 properties to store data. Each entity also has three system properties that specify a partition key, a row key, and a timestamp. Entities with the same partition key can be queried more quickly, and inserted/updated in atomic operations. An entity's row key is its unique identifier within a partition.
* ***File Shares***:
    - Azure Files offers fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol or Network File System (NFS) protocol. Azure file shares can be mounted concurrently by cloud or on-premises deployments. Azure Files SMB file shares are accessible from Windows, Linux, and macOS clients. Azure Files NFS file shares are accessible from Linux or macOS clients. Additionally, Azure Files SMB file shares can be cached on Windows Servers with Azure File Sync for fast access near where the data is being used.
* ***CORS***:
    - CORS is an HTTP feature that enables a web application running under one domain to access resources in another domain. Web browsers implement a security restriction known as same-origin policy that prevents a web page from calling APIs in a different domain. CORS provides a secure way to allow one domain (the origin domain) to call APIs in another domain.
* ***A shared access signature (SAS)***:
    -  SAS is a URI that grants restricted access rights to Azure Storage resources. You can provide a shared access signature to clients who should not be trusted with your storage account key but to whom you wish to delegate access to certain storage account resources. By distributing a shared access signature URI to these clients, you can grant them access to a resource for a specified period of time, with a specified set of permissions.
    - Types:
        - Service SAS:
            - Provides access to a specific resource in one of the storage services 
        - Account SAS:
            - Provides access to one of more storage services
            - Specific operations can be granted fot the sevice and rights for service content
        - Both types have certain common parameters and signed by an account key
    - Forms of SAS:
        - Ad hoc SAS:
            - The parameters are specified as part of the SAS creation and are part of the SAS URI
        - SAS using stored access policy:
            - SAS is created from  a stored access policy that defines many of the constraits
            - Only service type


        

* ***Some links***:
    - https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction

## Data Lake Gen2:

* Azure Data Lake - a hyper-scale repository for big data analytic workloads
* ***Key Features***:
    - Hadoop compatible. Manage data same as Hadoop HDFS
    - POSIX permissions. Supports ACL  (POSIX-like file access permissions) (https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control)
    - Cost effective. Offers low-cost storage capacity
    - Includes enterprise-grade capabilities: security, manageability, scalability, reliability, and availability
* ACLs:
    - Unlike a file system, the POSIX-stype does not inherit permissions
    - Folders have ACLs for the folder and a default ACL that new files will use
    - Changing permissions an the default ACL of a folder will not change the permissions on existing files in the folder
    - There are three types of permissions:
        - Read(R, 4)
        - Write(W, 2)
        - Execute(X, 1) -Does not app;y to files
    - Can be combined, e.g RWX or numeric 7
    - There is also an owner and ownering group which can be modified

* ***Some links***:
    - https://www.bluegranite.com/blog/10-things-to-know-about-azure-data-lake-storage-gen2