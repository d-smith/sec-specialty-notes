# study approach

Assemble Notes from Two Aspects

* Add notes and links using domain outline format
* Layer in service notes into the domains

Services:

* [Macie](https://docs.aws.amazon.com/macie/latest/userguide/what-is-macie.html)
* Inspector
* GuardDuty - see notes
* Trusted Advisor
* Systems Manager
* Config
* KMS
* CloudHSM
* VPC Endpoints
* IAM
* Organizations
* Artifact
* Athena
* CloudTrail
* Cognito
* AWS SSO

Scenarios:

* Cross account access
* Forensics
* Key rotation - automatic and manual
* DNS - using your own not Amazon's
* CloudTrail set up/troubleshooting
* Basic ports - ssh, sftp, smtp
* Recover data from a deleted CMK
* Policy - write once read many archiving
* 


## Domain Outline - Presentations

### Domain 1: Incident Response

1.1 Given an AWS abuse notice, evaluate the suspected compromised instance or exposed access keys.

SEC389 - Detecting Credential Compromise in AWS [video](https://youtu.be/pagHGaercLs)

[Preventing Credential Compromise](https://medium.com/netflix-techblog/netflix-information-security-preventing-credential-compromise-in-aws-41b112c15179)

1.2 Verify that the Incident Response plan includes relevant AWS services.

SEC327 - AWS Security in Your Sleep: Build End-to-End Automation for IR Workflows [ppt](https://www.slideshare.net/AmazonWebServices/aws-security-in-your-sleep-build-endtoend-automation-for-ir-workflows-sec327-aws-reinvent-2018)


Open Source IR Solutions

* [Cloud Custodian](https://github.com/cloud-custodian/cloud-custodian)
* [Fido](https://github.com/Netflix/Fido)
* [Security Monkey](https://github.com/Netflix/security_monkey)
* [Stream Alert](https://github.com/airbnb/streamalert)

1.3 Evaluate the configuration of automated alerting, and execute possible remediation of security-related
incidents and emerging issues.

SEC322 Using AWS Lambda as a Security Team [video](https://youtu.be/ecT4eyy0CyU)

SEC403 - Five New Security Automations Using AWS Security Services & Open Source [video](https://youtu.be/M5yQpegaYF8)

Why automation?

* More than reactive auto remediation
* Provide proactive support to reactive humans in time of crisis, including information gathering

Services used in security: lambda, step functions, cloud trail, cloud trail events, parameter store, more

* Not just guard duty, inspector, macie, secrets manager, etc.

Project: SignalHub

* Centralized message hub for alerts and other notification needs
* Runs as per account or per organization Lambda function
* Send notifications to multiple destinations

Project: LambdaCanary

Lambda-based canary to verify the existence of cross account IR functions

* Do validation of expected Lambda functions
* Validate expected code with checksum
* Integrate with alert function if incorrect code or errors occur

Project: [GuardDuty Multi-Account Manager](https://github.com/mozilla/guardduty-multi-account-manager)

* Cross account role manager and configuration assurance for AWS GuardDuty
* Enable GuardDuty masters in all AWS Regions present and future
* Aggregate all findings in one place, put in s3
* Uses [cloudformation cross account outputs](https://github.com/mozilla/cloudformation-cross-account-outputs/) service
* Manages relationships between master and sub account

Project: [MozDef](https://github.com/mozilla/MozDef) (The Mozilla Defense Platform)

* SIEM
* Ability to enrich, gather, and store logs
* Abililty to track the lifecycle of investigations and incidents
* Provide an interface to correlate multiple pieces of data: attackers, investigations, incidents
* Mozilla security principles - https://infosec.mozilla.org/fundamentals/security_principles


Project: ExposedKeyRemediator - v2

Some repos

* AWS security automation - https://github.com/awslabs/aws-security-automation
* https://threatresponse.cloud/
* https://github.com/ThreatResponse/aws_ir
* https://documentation.wazuh.com/current/amazon/

* Disables keys and keys created from those keys based on alerts from GuardDuty and Trusted Advisor

### Domain 2: Logging and Monitoring

2.1 Design and implement security monitoring and alerting.

ENT332-R - Best Practices for Centrally Monitoring Resource Configuration & Compliance [video](https://youtu.be/kErRv4YB_T4)

2.2 Troubleshoot security monitoring and alerting.
2.3 Design and implement a logging solution.

SEC323-R - Augmenting Security Posture and Improving Operational Health with AWS CloudTrail [video](https://youtu.be/YWzmoDzzg4U)

2.4 Troubleshoot logging solutions.

### Domain 3: Infrastructure Security

3.1 Design edge security on AWS.

SEC312-S - The Perimeter is Dead. Long Live the Perimeters. [video](https://youtu.be/nvVI3azDmOQ)

SEC326 - Orchestrate Perimeter Security Across Distributed Applications [video](https://youtu.be/ELIiF-jE0y8)

CTD201 Layered Perimeter Protection for Apps Running on AWS [video](https://youtu.be/MeFXknWDN3E)

3.2 Design and implement a secure network infrastructure. 

NET 202 - Securing Your Virtual Data Center in the Cloud [vide](https://youtu.be/2DF_EXmxbLM)

3.3 Troubleshoot a secure network infrastructure.
3.4 Design and implement host-based security.

SEC391 - Inventory, Track, and Respond to AWS Asset Changes within Seconds at Scale [video](https://youtu.be/_O6dIG5uqGg)

### Domain 4: Identity and Access Management

4.1 Design and implement a scalable authorization and authentication system to access AWS resources.
4.2 Troubleshoot an authorization and authentication system to access AWS resources.

### Domain 5: Data Protection

5.1 Design and implement key management and use.

SEC304 - AWS Secrets Manager: Best Practices for Managing, Retrieving, and Rotating Secrets at Scale [video](https://youtu.be/qoxxRlwJKZ4)

5.2 Troubleshoot key management.
5.3 Design and implement a data encryption solution for data at rest and data in transit. 

SEC325-R - Data Protection: Encryption, Availability, Resiliency, and Durability [video](https://youtu.be/FH6AXreSQWQ)

PARC - pricipals, actions, resources, conditions

IAM - declarative policy language, can be attached and of the principal/actor/identity, or attached to resources

Data - stored in multiple services - remember access control of resources via data. IAM can be used for management of resource actions. Data in databases is protected by the engine

Protect database and application credentials: AWS Secrets Manager

* Integrated with AWS IAM and services
* Controlling access to secrets is often the same as controlling access to the data
* To many humans handling secrets creates risk and vulnerability
* Automated secret rotation provides security and availability
    * Integration with RDS to update both clients and database server with new credentials

Features:

* Rotate secrets safely
* Build in, extensible integrations with lambda
* Scheduled versioned rotation
* Fine grained access control
* Encrypted storage
* Logging and monitoring

Data in AWS Storage Services

* S3, EBS, Glacier, EFS

Protecting data in AWS storage services

EBS

* Confidentiality: tag-based IAM policies
* Durability: share snapshots between accounts and copy between regions
* Integrity: block integrity automatically provided

EFS

* Confidentiality: IAM policies for attachments; POSIX permissions for file / directories
* Durability: share snapshots between accounts and copy between regions
* Integrity: file integrity automatically provided

S3

* Confidentiality: read/write object permissions (IAM and bucket resource policies)
* Durability: s3 cross region replication; versioning allows recovery of deleted objects; CRR can now be selective via tags
* Integrity: file integrity automatically provided

S3 Security Enhanced with VPC Endpoints

* Reach s3 privately without giving internet access to your vpc with an endpoint.
* Role accesses the endpoint to access the bucket: there is policy language that can be attached to the endpoint (separate from the identity and resource)
* IAM policy: what s3 API actions can this iam user/role perform under which conditions?
* Bucket policy: what iam users/roles can access this s3 bucket?
* Endpoint policy: which IAM users/roles can connect to which s3 buckets using this VPC?

Default Encryption for an S3 Bucket

* Any object placed into this bucket must be encrypted with a specific key

"I have control of the keys needed for decryption"

* Encrypt the data key with the master key
* Where does that master key get stored in plaintext?
* On prem HSM?
    * Pro: you control the device, autz. authn
    * Pro: familiar to auditors
    * Con: latency, availability, durability, scalability is your responsibility
    * Con: limited integration with cloud managed services
* In the cloud in your HSM (AWS CloudHSM)
    * Pro: you control the device, autz. authn
    * Pro: lower latency to your apps in the cloud
    * Pro: AWS makes it easy to have HA, durabiity, scalable
    * Pro: familiar to auditors
    * Con: limited integration with cloud managed services
* In the cluod in a managed HSM (AWS KMS)
    * Pro: you control the device, autz . authn
    * Pro: lower latency to your apps in the cloud
    * Pro: AWS makes it easy to have HA, durabiity, scalable
    * Pro: integration with cloud managed services
    * Con: looks unfamiliar to your auditors

Security Controls Enforced by KMS HSMs

* When operational with key material provisioned:
    * No AWS operator access to HSM; no software updates allowed
* AFter reboot and in a non-operational state:
    * No key material on host
    * Software can only be updated:
        * After multiple AWS employees have reviewed the code
        * Under a quorum of multiple authenticated KMS operators
* 3rd party verified evidence
    * FIPS 140-2 Level 2 with Level 3 physical security, design assurance, cryptography
    * SOC 1 - Control 4.5: Customer master keys used for cryptographic operations in KMS are logically secured so no single AWS employee can gain access to the key material

Each CMK has as resource policy to define permissions

Key Policy Sample permissions: 

* Can only be used for enc and dec by <these users and roles> in <these accounts>
* Can be used by application A to encrypt and only used by application B to decrypt
* Can be managed only by this set of administrator IAM users or IAM roles
* Can be used by <these external accounts>, but only for encryption/decryption, not administrative tasks

Where to encrypt your data

* Client-side encryption
* Server-sde encryption

AWS KMS custom key store

* Custom Key Store Connector
* [This blog](https://aws.amazon.com/blogs/security/are-kms-custom-key-stores-right-for-you/)

## Domain Notes - Services Notes

### Domain 1: Incident Response

1.1 Given an AWS abuse notice, evaluate the suspected compromised instance or  exposed access keys.
1.2 Verify that the Incident Response plan includes relevant AWS services.
1.3 Evaluate the configuration of automated alerting, and execute possible remediation of security-related incidents and emerging issues.

> Macie
> 
> Data Discovery and Classification
>
> * Amazon Macie enables you to identify business-critical data and analyze access patterns and user behavior:
> * Continuously monitor new data in your AWS environment
> * Use artificial intelligence to understand access patterns of historical data
> * Automatically access user activity, applications, and service accounts
> * Use natural language processing (NLP) methods to understand data
> * Intelligently and accurately assign business value to data and prioritize business-critical data based on your unique organization
> * Create your own security alerts and custom policy definitions
>
> Data Security
>
> * Amazon Macie enables you to be proactive with security compliance and achieve preventive security:
> * Identify and protect various data types, including PII, PHI, regulatory documents, API keys, and secret keys
> * Verify compliance with automated logs that allow for instant auditing
> * Identify changes to policies and access control lists
> * Observe changes in user behavior and receive actionable alerts
> * Receive notifications when data and account credentials leave protected zones
> * Detect when large quantities of business-critical documents are shared internally and externally


### Domain 2: Logging and Monitoring
2.1 Design and implement security monitoring and alerting.

* See Macie data security notes above

2.2 Troubleshoot security monitoring and alerting.
2.3 Design and implement a logging solution.
2.4 Troubleshoot logging solutions.

### Domain 3: Infrastructure Security
3.1 Design edge security on AWS.
3.2 Design and implement a secure network infrastructure. 
3.3 Troubleshoot a secure network infrastructure.
3.4 Design and implement host-based security.

### Domain 4: Identity and Access Management
4.1 Design and implement a scalable authorization and authentication system to access AWS resources.
4.2 Troubleshoot an authorization and authentication system to access AWS resources.

### Domain 5: Data Protection
5.1 Design and implement key management and use.
5.2 Troubleshoot key management.
5.3 Design and implement a data encryption solution for data at rest and data in transit. 
