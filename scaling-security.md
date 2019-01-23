https://www.youtube.com/watch?v=Rmf91qNcN8w

Cloud Adoption Framework - [security perspective](https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)

## Basics - Getting Started

* New account - create first IAM user, MFA, stop use of root user
* Users and groups -least privileged access
* VPCs, security groups, appropriate restrictions - apply naming and tagging conventions
* Enable CloudTrail for all regions
* Use CloudWatch to understand your performance and application patterns, use to detect unusual activity

## MVP gaining traction

* Address failover, introduce fault tolerance, multiple AZs
* Elastic load balancing, autoscaling
* Protect data with KMS, snapshots, S3, RDS, etc
* Extends IAM model to support KMS roles, etc

## Now 100,000+ Customers

* Security Infrastructure as Code
    * Manage security code just like your business workload
    * Strong change management processes
* Define multiple multiple security infrastructure stacks to align with separation of duties, IAM stack, infrastructure stack, logging stack.
* Why as code?
    * Version and source control
    * Transparency
    * Assurance and visibility
    * Traceability and change management
    * Knowledge management

Security of the CI/CD pipeline

* Least privilege access
* Logging and monitoring of pipeline

Security in the Pipeline

* Security unit test
* Vulnerability management

AMI Lifecycle Management

* Automate via pipeline
* Config rule - approved AMI
* Inspector in the pipeline

More Attention - add WAF

New features with sensitive data - encrypt

## More than 10 Million Customers

Cognito - handler your user directories, sign up, authentication, etc.

Logs - aggregate, Kibana searches, etc

Run Security Incident Response Simulations

Automate how security practitioners answer these questions

* Who changed my config?
* Who accessed my data?