# PrivateLink Deep Dive

* Allows services to be directly provisioned into a VPC
* Uses private address space
    * No public IP addresses
    * Endpoints have regional and zonal names
* Works with VPC security groups, IAM policies
    * Shows up as an ENI with a private IP address

* If a provider has targets and NLB in each zone, those zones will be available to customer
* Best latency by being in as many zones as possible, use at least two for availability
    * Avoid zonal coupling
* Cross-region setups come with availability and data-soverneignity risks

* Ideal for compartimentalizing microservices into their own networks and accounts

Provider VPC

* NLB for your service
    * Expose NLB via private link
* Lifecycle model and events available via SNS
* You can automate signups, leave, events
* Invoke lambda from SNS
* Create endpoint names as you would ELB names; use CNAMES or Amazon Route 53 alias
* Integrate with wildcard DNS and wildcard SSL certificates

Tenancy Models

* Single-tenant mpde - create a private link NLB for every client/customer
* Multi-tenant model: allow many customers to use the same PrivateLink NLB
* How do we tell endpoint traffic from different VPCs apart?
    * Method 1 - use traditional accounts/passwords/security tokens at the application level
    * Method 2 - use separate NLBs and different listener ports on the targets
    * Method 3 - enable the ProxyProtocolV2 preamble (preferred)

AWS HyperPlane

* Privatelink foundation
* Evolution of the s3 load balancer
* Distributed, transactional, connection-tracking machine
* Not a proxy, rewrites packages and can even insert new ones