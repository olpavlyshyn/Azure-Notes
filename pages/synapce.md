* Azure Synapse Analytics - is a limitless analytics service, that brings together enterprise data warehausing and Big Data analytics.
* Suits for:
    - Store large volumes of data
    - Consolidate disparate data into a single location
    - Shape, model, transform, and aggregate data
    - Batch/Micro-batch loads
    - Perfor query analysis across large datasets
    - Ad-hoc reporting across large data volumes.
    - All using simple SQL constructs.

* Parallelism:
    - SMP - Symmetric Muliprocessing
        - Multiple CPUs used to complete individual processes simultaneously
        - All CPUs share the same memory, disks, and network controllers (scale-up)
        - All SQL  Server implementrations up until now have been SMP
        - Mostly, the solution is housed on a shared SAN
    - MPP - Massive Parallel Processing
        - Uses many separate CPUs running in parallel to execute a single program
        - Shared Nothing: Each CPU has its own memory and disk (scale-out)
        - Segments communicate using high-speed network between nodes

* Distributions
    - Round-Robin 
        - Distributes table rows evenly across all distributions at random
    - Hash
        - Distributs table rows across the Compute nodes by using a deterministic hash function to assign each row to one distribution
    - Replicated 
        - Full copy of table accessible on each Compute node

* Partitions
    - Table partitions divide data into smaller groups
    - In most casese, partitions are created on date column
    - Supported on all table types
    - Range Right - used for time partitions
    - Range Left - used for number partitions
    - Used for improvement efficiency and performance of loading and querying by limiting the scope to subset of data
    - Offers significant query performance enhancements where filting on the partition key can eliminate unnecessary scans and eliminate IO
    - Each shard is partitioned with the same date partitions
    - A minimum of 1 millio rows pre distribution and partition is needed for optimal compression and performance of clustered Columnstore tables

* Common table distribution methods
    - Fact
        - Use hash-distribution with clustered columnstore index. Performance improves because hashing enables the platformto localize certain operations within the node itself during query execution.
    - Dimension 
        - Use replicated for smaller tables. If tables are too large to store on each Compute node, use hash-distributed
    - Staging
        - Use round-robin for the staging table. The load with CTAS is faster. Once the data is in the staging table, use INSERT...SELECT to move the data to production tables
    
* COPY command
    - Copies data from source to destination
    - Retrieves data fro all files from the folder and all its subfolders
    - Supports multiple locations from the same storage account, separated by comma
    - Supports ADLS Gen2 and azure Blob Storage
    - Supports CSV, PARQUET, ORC file format

* Result-set caching
    - Cache the results of a query in DW storage. This enables interactive response times for repetitive queries against tables with infrequent data changes.
    - The result-set cache persists even if a data warehouse is paused and resumed later.
    - Result cache is evicted regularly based on a time-aware least recebtly used algorithm(TLRU)
    * Benefits 
        - Enhanced performance when same result is requested repetitively 
        - Reduced load on server for repeated queries
        - offres monitoring of query execution with a sesult cache hit ar miss

* Resource classes
    - Pre-determined resource limits defined for a user or role
    - Govern the system memory assigned to each query.
    - Effectively used to control the number of concurrent queries that can run on a data warehouse
    - Types: 
        - Static Resources Classes
            - Allocate the same amount of memory independent of the current service-level objective (SLO)
            - Well-suited for fixed data sizes and loading jobs
        - Dynamic Resource Classes
            - Allocated a variable amount of memory depending on the current SLO.
            - Well-suited for growing or variable datasets
            - All users default to the smallrc dynamic resource class

* Concurrency slots
    - Queries running on DW compete for access to system resources (CPU, IO, and memory)
    - To guarantee access to resources, running queries are assigned a chunk of system memory (a concurrency slot) for processing the query. The amount given is determined by the resource class of the user executing the query. Higher DW SLos provide more memory and concurrency slots
    - Concurrent query limits
        - The limit on how many queries can run at the same time is governed by to properties:
            - The max. concurrent query count for the DW SLO
            - The total available memory (concurrency slots) for the DW SLO
        - Increase the concurrent query limit by:
            - Scaling up to a higher DW SLO (up to 128 concurrent queries)
            - Using lower resource classes that use less memory  per query

* Workload Management
    - It manges resources, ensures highly efficient resource utilization, and maximizes return on investment (ROI)
    - The three pillars:
        - Workload groups 
            - To assing a request to a workload group and setting importance levels.
        - Workload Importance 
            - To influence the order in which a request gets access to resources
        - Workload Isolation
            - To reserve resources for a workload group
    - Workload classification
        - Map queries to allocations of resources via pre-determined rules
        - Use with workload importance to effectively share resources across different workload types
        - If a query request is not matched to a classifier, it is  assingen to  default workload group (smallrc resource class)
        - Benefits
            - Map queries to both Resource Management and workload Isolation concepts
            - Manage groups of useers with only a few classifiers
    - Workload importance
        - Queries past the concurrency limit enter a FIFO queue 
        - By default, queries are released from the queue on a first-in, first-out basis as resources become available
        - Workload importance allows higher priority queries to receive resources immediately regardless of queue
    - Workloas Isolation
        - Allocate fixed resources to  workload group
        - Assign maximum and minimum usage for varying resources under load. These adjustment can be done live without having to SQL Analytics offline
        - Benefits
            - Reserve resources for a group of requests
            - Limit the emount of resources a group of requests can consime
            - Shared resources accessed based on importance level
            - Set Query timeout value. Get DBAs out of the bussiness of killing runaeay queries

* Dynamic Management Views (DMVs)
    - Are queries that return information about model objects, server operations, and server health

* Maintenance window
    - Choose a time window for your updates
    - Select a primary and secondary window within a 7-day period
    - Windows can be from 3 to 8 hours
    - 24-hour advance notificaton for maintenance event
    - Benefits
        - Ensure upfrades happen on your schedule
        - Predictable planning for long-runing jobs
        - Stay informed of stat and end of maintenance
    
* Automatic statistics management
    - Statistics are automatically created and maintained for SQL pool
    - Statistics are automatically updated as data modifications occur in underlying tables

* Snapshots and restores
    - Automatic copies of the data warehouse state
    - Taken throughout the day, or triggered manually
    - Available for up to 7days, even after dwh deletion
    - 8-hours RPO for restores from snapshot
    - Regioal restore in under 20 minutes, no matter data size
    - Snapshots and geo-backups allow cross-region restores
    - Automatic snapshots and geo-backups on by default
    - Benefits
        - Snapshot protect against data corruption and deletion
        - Restore to quickly create dev/test copies of data
        - Manual snapshots protect large modifications
        - Geo-backup copies ine of the automatic snapshots each day to RA-GRS storage. This can be use in an event of a disaster to recover yout SQL dwh to a new region. 24-hour RPO for geo-restore

* SQl On-Demand (Serverless)
    - An interactive query service that provides T-SQL queries over high scale data in Azure Storage
    - Data intergration with Databricks, HDInsight ?
    - Uses OPENROWSET function o access data
    - CSV, PARQUET, JSON (via csv format) 

    
* Spark

* Data Lake

* Spark Component Features
    - Spark SQL
        - Unified data access: Query structured data sets with SQL or DataFrame APIs
        - Fast, familiar language across all data
        - Use BI tools tp connect and query bia JDBC or ODBC drivers
    - Mllib/SparkML
        - Predictive and prescripive analytics
        - Smart application desing from pre-built, out-of-box statical and algprithmic models
    - Spark Streaming
        - Micro-batch event processing fore near-real time analytics
        - Spark's engine drives some action or outputs data in batches to various data stores
    - GraphX
        - Represent and analyze systems represented by graph nodes
        - Trace interconnections between graph nodes

* Security
    * Object-level security
        - GRAND contorls permissions on designated tables, views, sprocs and functions
        - Prevent unauthorized queries against certain tables
        - Simplifies desing and implementation of security at the database leve as opposed to application level
    * Row-level security (RLS)
        - Fine grained access contro; of specific rows in a database table
        - Help prevent unauthotized acces when multiple users share the same tables
        - Eliminates need to implement connection filtering in multi-tenant applications
        - Easily locate enforcement logic inside the database and shcema bound to the table
    * Column-level security
        - Control access of specific columns in a database table based on customer's qroup membership in a database table based on customer's group membership or eecution context
        - Simplifies the design and implementation of security by putting trestrction logic in database tier as opposed to application tier
        - Administer via GRAND tsql statement
        - Both AAD and SQL authentication
    * Dynamic Data Masking 
        - Prevent abuse of sensitive data by hiding it from users
        - Easy configuration in Azure Portal
        - Policy-driven at table and column level. for a defined set of users
        - Data masking applied in real-time to qiery results based on policy
        - Multiple masking fuctions available, such as full or patial, for various sensitive data categories
    * Types of data encryption
        - In Transit
            - Transport Layer Security (TLS) from the client to the server
            - TLS 1.2
        - At rest
            - Transparent Data Encryption (TDE)
                - Protects data on disk
                - User ir Service Managed key management is handled by Azurem which makes it easier to obtain compliance

Reference:
https://www.slideshare.net/jamserra/azure-synapse-analytics-overview-234122698