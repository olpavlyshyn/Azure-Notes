* Containers
    - VMs virtualize the hardware
    - Containers virtualize the OS
    - Uses numerous OS capabilities like namespaces, cgroups, layers (union file system)
    - Typically a container runs a primary process, and they share a lifecycle
    - Created from an immutable image

* Container Support
    - Hyper-V Containers (isoleted kernal)
    - Azure Container Registry
        - Provides private repositories
        - Can be geo-replicated (with Premium SKU)
        - Place close to deployment to reduce latency, increase reliability
        - Can also run jobs to build tje container image

* Azure Container Instatnces 
    - Linux or Windows "Container as a Service"
    - Built from standard or custom images
    - Public or private (Linux)
    - Very useful for burst scenarios or very basic scenarios
    - Can integrate with AKS via Viryal Kubelet

* Azure Kubernetes Service 
    - We often need more than one, isolated container
    - Complete orchestration is required, Kubernetes
    - AKS provides a free, managed Kubernetes env for clusters
    - You only pay for worker nodes which ca auto-scale
    - Special features:
        - Multiple node pools
        - ACI for burst via virual kubelet
        - User node pools can use spot instances
        - Stop/Start AKS cluster
        - Auto-healing
        - Managed identity use

* App Service Plan
    - The "original" Paas 
    - Hosting of web apps (including API)
    - Wide  range of runtimes and languages supported
    - Includes Windows and Linux (includijg containerized)
    - Have a certain number of nodes with auto scale for standard and above in a plan
    - Multiple apps can br deployed to same plan (sharing resource)
    - Special Features
        - Can scale up and out
        - Deployment slots
        - Network control viaservice endpoint
        - VNET integration (in and out)
        - App Service enviroment dedicated deploument into your VNET

* Azure Functions
    - Serverless compute (but can run within App Service Plan resources)
    - Event driven such as HTTP, schedule, event, grid, blob creation
    - Bind additional inputs and outputs
    - Wide support of runtimes
    - 1 million executions free

* Logic App
    - Graphical based orchestration of business logic
    - Serverless an pay only when running
    - Integration Service Env available for dedicated and isolated enc
    - Initiated bia a trigger (such as some event)
    - Logica app has may connectors and templates to get started

