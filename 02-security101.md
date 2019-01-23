## Shared Responsibility Model

See [here](https://aws.amazon.com/compliance/shared-responsibility-model/)

* Amazon - security of the cloud
    * global infrastructure
    * hardware, software, etc.
    * managed services
* Customer - security in the cloud
    * iaas
    * updates, security patches
    * AWS firewall (security groups, network ACLs, etc)

Different service types:

* Infrastructure
    * compute like ec2, ebs, autoscale, vpc
    * architect and build systems like on prem
    * you control the OS, config, etc
    * EC2 - customer responsible for AMIs, OS, applications, data in transit, data at rest, data stores, credentials, policies and config, etc.
* Container
    * network setup and controls/firewall rules, platform level identity and access  management apart from IAM. Examples: elastic beanstalk, EMR, RDS
* Abstracted
    * High level storage, messaging, and database services like S3, Glacier, DynamoDB, SQS, SES
    * Abstract the platform or management layer

Example: S3 - you are responsible for bucket policies

## Security in AWS

Cloud Controls

* Visibility
    * What assests do you have? AWS Config
* Auditability
    * Who's going into our AWS environment, who's making changes. AWS CloudTrail
* Controllability
    * How is my data controlled? AWS KMS, AWS CloudHSM.
    * KMS is multitenant, CloudHSM is dedicated hardware. FIPS 142 compliance - CloudHSM, can't use multitenant hardware.
* Agility
    * How quickly can you adapt to changes? AWS CloudFormation, AWS Elastic Beanstalk
* Automation
    * Are our processes repeatable? AWS OpsWorks, AWS CodeDeploy
* Scale
    * Every customer gets the same AWS security foundation

www.slideshare.net/AmazonWebServices

These apply across all controls: AWS IAM, AWS CloudWatch, AWS TrustedAdvisor

## Exam Tips

* Understand the 3 models
    * CIA (confidentiality, integrity, availability)
        * Confidentiality: IAM, MFA
        * Integrity: Certificate manager, IAM, Bucket Policies
        * Availability: Autoscaling, multiple AZ, multiple regions, etc.
    * AAA (authentication, authorization, accounting)
        * Authentication: IAM
        * Authorization: Policies
        * Accounting: cloud trail
    * Non-repudiation (can't deny)
        * Cloud Watch, Cloud Trail, MFA, IAM

* Know how Amazon secures themselves
* Why trust AWS? => Compliance certifications
    * PCI - gap audit

* Understand the shared responsibility model (see above)
    * Diff responsibilities for infrastructure, container, abstracted

