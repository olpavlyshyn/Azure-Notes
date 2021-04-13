## ***Azure Stream Analytics***

* *Azure Stream Analytics* is a real-time analytics and complex event-processing engine that is designed to analyze and process high volumes of fast streaming data from multiple sources simultaneously.
* *Azure Stream Analytics Cluster* offers a single-tenant deployment for complex and demanding streaming scenarios. At full scale, Stream Analytics clusters can process more than 200 MB/second in real time. Stream Analytics jobs running on dedicated clusters can leverage all the features in the Standard offering and includes support for private link connectivity to your inputs and outputs.
* ***Data stream*** - is an ongoing sequence of events over time
![alt](../img/sadataflow.png) 

* ***Inputs***:
    - Stream Analytics has first-class integration with four kinds of resources as inputs:
        - Azure Event Hubs
        - Azure IoT Hub
        - Azure Blob storage
        - Azure Data Lake Storage Gen2
    - * ***Input Categories***:
        - ***Data stream input*** 
            - any data stream which you need to process in real-time and act upon
            -  Stream Analytics jobs must include at least one data stream input.
        - ***Reference data input***
            - Data which does not change or changes very slowly, such as metadata lookups
            - Storage Account, Azure SQL Database
    - Stream Analytics supports compression across all data stream input sources. Supported compression types are: None, GZip, and Deflate compression. Support for compression is not available for reference data.
* ***Outputs***:
    - Stream Analytics supports partitions for all outputs except for Power BI. 
    - Batching window properties:
        - *timeWindow* - The maximum wait time per batch. The default value is 1 minute and the allowed maximum is 2 hours. 
        - sizeWindow - The number of minimum rows per batch. For Parquet, every batch creates a new file. The current default value is 2,000 rows and the allowed maximum is 10,000 rows.
* ***User-defined functions***:
    - Azure Stream Analytics supports the following four function types:
        - JavaScript user-defined functions (UDF.<function name>)
        - JavaScript user-defined aggregates (UDA.<function name>)
        - C# user-defined functions (using Visual Studio) (UDF.<function name>)
            - not all regions
        - Azure Machine Learning (UDF.<function name>)
    - User-defined functions are stateless, and the return value can only be a scalar value. You cannot call out to external REST endpoints from these user-defined functions 
* ***Streaming Units (SUs)***:
    - Streaming Units (SUs) represents the computing resources that are allocated to execute a Stream Analytics job. The higher the number of SUs, the more CPU and memory resources are allocated for your job.
    - It's best to keep the SU metric below 80% to account for occasional spikes. 
* ***Windowing Functions*** (https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-window-functions):
    - There are five kinds of temporal windows to choose from: 
        - ***Tumbling*** 
            - Tumbling window functions are used to segment a data stream into distinct time segments and perform a function against them
            - The key differentiators of a Tumbling window are that they repeat, do not overlap, and an event cannot belong to more than one tumbling window.
            - Tell me the Count of tweets per time zone every 10 seconds
        - ***Hopping***
            - Hopping window functions hop forward in time by a fixed period. It may be easy to think of them as Tumbling windows that can overlap and be emitted more often than the window size. 
            - Events can belong to more than one Hopping window result set. 
            - Every 5 seconds gives me the count of Tweets over the last 10 seconds
        - ***Sliding***
            - Sliding windows, unlike Tumbling or Hopping windows, output events only for points in time when the content of the window actually changes. In other words, when an event enters or exits the window. So, every window has at least one event. 
            - Similar to Hopping windows, events can belong to more than one sliding window.
            - Give me the count of Tweets for all topics which are Tweeted more than 10 times in the last 10 seconds 
        - ***Session*** 
            - Session window functions group events that arrive at similar times, filtering out periods of time where there is no data. It has three main parameters: timeout, maximum duration, and partitioning key (optional).
            - Tell me the count of Tweets that occur within 5 minutes to each other 
        - ***Snapshot***
            - Snapshot windows groups events that have the same timestamp. Unlike other windowing types, which require a specific window function (such as SessionWindow(), you can apply a snapshot window by adding System.Timestamp() to the GROUP BY clause.
            - Give me the count  of tweets with the same topic type that occur at exactly the same time
    - You use the window functions in the GROUP BY clause of the query syntax in your Stream Analytics jobs.
* ***Error policy***:
    - ***Retry*** - When an error occurs, Azure Stream Analytics retries writing the event indefinitely until the write succeeds. There is no timeout for retries. Eventually all subsequent events are blocked from processing by the event that is retrying. This option is the default output error handling policy.
    - ***Drop*** - Azure Stream Analytics will drop any output event that results in a data conversion error. The dropped events cannot be recovered for reprocessing later. All transient errors (for example, network errors) are retried regardless of the output error handling policy configuration.
* ***Time Handling***:
    - Stream Analytics gives users two choices for picking event time:
        - ***Arrival time ***
            - Arrival time is assigned at the input source when the event reaches the source. You can access arrival time by using the EventEnqueuedUtcTime property for Event Hubs input, the IoTHub.EnqueuedTime property for IoT Hub input, and the BlobProperties.LastModified property for blob input.
            - default
        - ***Application time*** (also named Event Time)
            - Application time is assigned when the event is generated, and it's part of the event payload. To process events by application time, use the Timestamp by clause in the SELECT query. If Timestamp by is absent, events are processed by arrival time.