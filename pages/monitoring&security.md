* Activity Log
    - 90 days retention
    - subsription level

* ARM Resources
    - Metric
        - Out of box
        - Numeric
        - 90 day retention
        - < 60 seconds 
    - Logs 
        - Configurable
        - Numeric and text

* OS
    - Diagnotic extention
    - Agents

* App Insights

- Logs and metrics can be sent to
    - Storage 
        - Cheap 
        - Longterm retention
    - Event Hub  
        - 3rd party SEIM
    - Log Analytics
        - 2 hear max retention 
        - u pay for ingestion and storage
        - tables can be exported to storage(hourly) or Event Hub (real time)
        - Use KQL

- All metric can be added into
    - Dashboard
        - RBAC
    - Workbook
        - RBAC not supported
        - more as document
        - can be pinned to dashboard
        - interactive

        
* Alert Rules
    - All logs metrics can be alerted
    - Action Groups (what to do)
    - Action Rules (filtering)
        - Can trigger or suppress action group

* Azure Security Center
    - Free
    - Can be inproved by adding Defender

* Sentinel
    - Lives on top of Log Analytics
    - Has a wide range of connectors (OS, syslogs, devices, network)
