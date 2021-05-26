* Virtual Network Basics
    - Software defined networking 
    - A Virtual network consists of one or more IP ranges
    - Typically from RFC 1918  but not exclusivley
    - A VN exists within:
        - a specific subscription
        - a specific region
        - It cannnot span subscription nor regions
    - The address space is broken up into subnets with the smallest subnet  possible  being /29 which will give 3 usable IP addresses
    - Subnets are regional and span Availability Zones 

* VM NIC (Network Interface)
    - IP always comes via Azure
    - IP can be reserved in ARM
    - VMS can be configured with multiple NICS
    - Each NIC can be in different virtual subnet in same virtual network or different subnets of same network
    - Multiple IP configurations per NIC 
    - IP configuration has private IP and optional public one

* Supported Types of Traffic
    - Standard IP based protocols (Layer 3) supported including:
        - TCP
        - UDP
        - ICMP
    - Multicast, broadcast, IP-in-IP encapsulated packets and Generic Routing Encapsulation (GRE) blocked
    - You cannot ping the Azure gateway or use tools like tracert
    - Traditional Layer 2 VLANs are not supported

* IPV6
    - Virutal Networks are dual stack  enabling IPv4 and IPv6 address ranges assigned
    - IPv6 support in NSG, UDR, LB, peering etc.
    - NIC cannot use IPv6 only
    - Can enable IPv6 for existing resources (may require reboot)
    - No Express Rout support (maybe in feature)
    - Public IPs can be IPv4  or IPv6

* External Access
    - There is no specific "DMZ" subnet where resources get a public IP
    - By default Azure provides outbound SNAT/PAT enabling  resources to access the Internet and receive responses
    - To provide services to the Internet   either
        - Give IP configuration an intance public IP  (not a good idea)
        - Place the instances behind an Azure load balancer, gateway or NVA which has a public IP in the front-end configuration
        - Use a network virtual appliance with a public IP
    - Care shout be taken to only  expose the required ports

* Connecting VNETs
    - If u wish to have multiple subscriptions and/or use multiple regions u will have multiple vnets
    - In the past we could connect vnets using S2s VPN or by  connecting to the same ExpressRoute circuit but both approches have problems
    - VNET peering enables VNets to be connected via Microsoft Backbone in the same or different region (global peering)
    - IP space cannot overlap
    - can span subscriptions and even AAD Tenants
    - Peers are not transitive, but there is some methanisms to do so

* Connecting to On-Premises
    - There are number of options to connect to VNETs
        - P2S VPN - Connects a specific device to VPN
        - S2S VPN - Connect a network to a VNET
            - S2S VPN enable multiple VPN connections to different networks if rout not policy based
        - ExpressRoute Private Peering - Connects a network to a VNET via ExpressRoute Gateway
            - ExpressRoute circuits enable multiple VNETS to be connected to a single circuit but vnet to vnet better via peering
    - Most enterprises will leverage ExpressRoute which has the benefit of not going over the Internet, consistent latency and also provides optional Microsoft peering via route filter

* Control of Traffic
    - By default traffic can freely flow within  a VNET ant to any connected network
    - To segment and control traffic within a VNET, between networks and/or external a number of approaches can be utilized
        - Azure Firewall or an NVA with traffic routed to it via UDR
        - Network Security Groups, Application Security Groups and Service Tags
    - NSGs can be applied at the subnet or NIC level but are always enforced at the NIC
    - NGS are made up of rules based on IP ranges/tags, ports and actions
    - ASGs are tags applied to NICs which can be used instead of IP ranges in rules which may be easier to utilize]

* Service Endpoints and Service Endpoint Policies
    - NSG are focused on traffic into and out of the VNET
    - Many Azure PaaS offerings have their own firewall capabilities to lock down access
    - It is often required to restrict a service to only specific subnets of specific VNETs
    - Service endpoints make a specific subnet known to a specific Azure service and add optimal path to service
    - The virual firewall on the service can then be configured to allow only that specific subnet
    - Service Endpoint Policies allow specific instances of services to br allowd from a VNET which is not possible with NSG sevice tags

* DNS
    - VNETS can use Azure DNS or custom DNS
    - Azure DNS can provide public and private zones
    - Provate Zone u pick name and full management
    - Vnetes can be linked to Private DNS Zones in addition to thr buil-in internal.cloundapp.net which is always there

* Azure Private Link
    - When an externally facing Azure PaaS service is accessed from a resource in a Vnet the traffic stays on the Azure network
    - The PaaS service still has an external facing endpoint that some companies do not want even with firewall/authentication lockdown
    - Azure Private Link enables PaaS services to have a private endpoint for a service instance created in a virtual network that is an avatar for that service instance
    - Can also project custom services that are behind a standard load balancer
    - Resources in the Vnet can interact via the private endpint directly to the service using the most efficient path
    - Because it is instance specific helps stop data exfiltration
    - Remoces the need to peer Vnet which can be important where Vnets may leave overlapping Ip ranges


* Network Security Groups
    - Filtering of incoming and outgoing traffic for Azure resources located in VNET
    - Filtering controlled by rules
    - Ability to have multiple inbound and outbound rules
    - Rules are created ny specifying
        - Source/Destination (IP addresses, service tags, application security groups)
        - Protocols (TCP, UDP, any)
        - Port
        - Direction (inbound or outbound)
        - Priority
* Application Security Groups
    - Feature that allows grouping of virual machines located in vNET
    - Designed to reduce the maintenance effort (assing ASG insted of the explicit IP address)

* User-defined Routes
    - Custom (user-defined, static) routes (UDRs)
    - Managed via Azure Route Table resource
    - Associated with a zero or more vNETS subnets

* Azure Firewall
    - Managed, cloud-based firewall service
    - Inbound & outbound traffic filtering rules
    - Support for FQDN(Fully Qualified Domain Name), ex. microsoft.com
    - Fully integrated with Azure monitor for logging and analytics