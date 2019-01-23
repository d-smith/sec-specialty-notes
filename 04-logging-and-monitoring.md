# Logging and Monitoring

## AWS Cloud Trail

* Records API calls for your account and delivers log files to you.
* For all AWS

```console
        control  
user -> plane   -> AWS cloud
        (API)
```

* Logs the API calls, but not the things you do on an EC2 instance via ssh for instance.

Enables:

* After the fact incident investigation
* Near real time intrusion detection
* Industry & regulatory compliance

Provides:

* Logs of API call details for supported services

What is logged?

* Metadata around the API call
* The identity of the caller
* The time of the API call
* The source IP address of the caller
* The request parameters
* The response elements returned by the service

CloudTrail Event Log:

* Sent to an S3 bucket
* You manage retention
* Logs delivered every 5 (active) minutes with up to a 15 minute delay
* Notifications available
* Can be aggregated across regions
* Can be aggregated across accounts

Set Up:

* Enabled by default for 7 days
* Provision it for longer
    * Create a new cloud trail, apply it to all regions
    * Select event types (pick all)
    * Pick the bucket to store it in

Digest Files

* Logs delivered with a digest file
* Mechanism to determine if someone has tampered with them, or if they were deleted.
* Enabled via 'log file integrity checking' when provisoning cloud trail
* Can validate using the command line
* SHA-256 Hashing, SHA-256 with RSA for digital signing

## CloudTrail - Protecting Your Logs

Why secure?

* Can include PII like users names and team membership
* Can include detail config info like dynamodb table and key names
* Could prove valuable to an attacker

How to prevent unauthorized access?

* Place employees who have a security role into an IAM group with attached policies that enable access to the logs.
      * IAM - create two groups - CloudTrailAdmin 
      * Admin - grant AWSCloudTrailFull access AWS 
      * Auditors - AWSCloudTrailReadOnlyAccess

* How can we be notified that a log file has been created, then validate it has not been modified?
      * Configure SNS notifications and log file validation on the 'Trail'
      * Consume the notification with a solution that will validate the logs using the provided digest file.
      * Probably better to look for mods to files in the bucket via a lambda.

* How can we prevent logs from being deleted?
      * Restrict delete access with IAM and bucket policies.
      * Use s3 MFA delete
      * Validate logs have not been deleted using log file validation.

* How can we ensure that logs are retained for X years in accordance with our compliance standards?
      * Use S3 object lifecycle management

Exam tips:

* API calls to control plane are logged by CloudTrail
* Metadata around calls is logged (see above)
* Events sent to s3 bucket - use s3 to manage the retention and archiving and purging of the files.
* Delivered every 5 active minutes, can be a 15 minute delay
* Notifications available
* Can aggregate logs across regions, and aggregate across accounts
* Can validate log file integrity (digests on the hour, every hour, AWS has the private key)
* Secure the logs, check their integrity

## AWS CloudWatch

Amazon CloudWatch is a monitoring service for AWS cloud resources and the applications you run on AWS.

* Real time
* Metrics
* Alarms
* Notifications
* Custom Metrics

Enables:

* Resource utilization, operational performance monitoring
* Log aggregation and basic analysis

Provides:

* Real-time monitoring within AWS for resources and applications
* Hooks to event triggers

Real-time: standard every 5 minutes, detailed every minute

Alarms - for metrics, threshold and periods => action
Custom metrics can be created

CloudWatch Logs

* Pushed from some service, from your applications/systems
* Metrics from log entry metrics
* Stored indefinitely (not user s3)

Can install agents to forward logging to cloud watch logs

CloudTrail can be a cloud watch event source, use cloud watch event rules to send events to event target.


CloudWatch Events

* Near real-time stream of system events
  * API calls to the control generate the events
* Events
        * AWS resource state changes
        * AWS CloudTrail events
        * Custom events (code)
        * Scheduled
* Rules - match events and route them to targets
* Targets - lambda, SNS, SQS, kinesis streams

Exam Hint: Read the AWS CloudWatch FAQs (CloudWath + CloudTrail are a big part of the exam)

Exam tips:

* Remember the key components: CloudWatch, CloudWatch Logs, CloudWatch Events
* CloudWatch - real time, metrics, alarms, notifications, custom metrics
* CloudWatch Logs - push from some service, from applications and systems, metrics from log entries, stored indefinitely
* CloudWath events - near real time stream of system events, state changes, cloud trail logs, custom events, scheduled, rules, targets => event driven security

## AWS Config 101

AWS Config is a fully managed service that provides you with an AWS resource inventory, configuration history, and configuration change notifications to enable security and governance.

Enables:

* Compliance auditing 
* Security analysis
* Resource tracking

Provides:

* Configuration snapshots and logs config changes of AWS resources
* Automated compliance checking

Key Components:

* Config Dashboard
* Config Rules
  * Managed
  * Custom (using Lambda)
* Resources
* Settings

Resource change -> Event -> AWS config bucket
                         -> Event Target (lambda, standard rule or custom rule)

Lambda can't take action, only indicates if rule broken

Terminology

* Configuration items - point-in-time attributes of resource
* Configuration snapshots - collection of configuration items
* Configuration stream - stream of change configuration items
* Configuration history - collection of config items for a resource over time
* Configuration recorder - the configuration of AWS Config that records and stores configuration items 

Recorder Setup

* Logs config for account in a region
* Stores in S3
* Notifies via SNS

What we can see:

* Resource type and id
* Compliance
* Timeline
  * Configuration details
  * Relationships
  * Changes
  * CloudTrail events

Compliance Checks

* Trigger
  * Periodic
  * Configuration changes (filterable, when a recorded resource is  created, changed, or deleted)
* Managed rules 
  * About 40 (at time of recording)
  * Basic, but fundamental

## AWS Config Lab

* Services > Management Tools > Config
* Set up on a regional basis - inventories your resources, stream config changes to an SNS topic
* Rules - ssh open to the world - trigger type (config changes, periodic), AWS manage rule
* Flag non-compliant resources - provides link to it
* Config timeline - changes over time, linked to cloud trail

## AWS Config Wrap Up

* Assests in AWS, change generates an event to AWS config, can evaluate standard or custom rules via lambda, writes into the AWS config s3 bucket, rules that are violated can generate a notification
* Its use provides visibility, compliance, auditability

Exam tips:

* Types of triggers: periodic, configuration snapshot delivery (filterable)
* Managed rules: about 40, basic but fundamental
* Permissions needed: IAM role - console can create for you
  * read only permissions to all AWS resources
  * write access to the S3 bucket
  * publish access  on SNS. Console can create these for you.
* Restricting access
  * Users need to be authenticated in AWS and have the appropriate permissions set via IAM policies to gain access
  * Only admins needing to set up and manage config require full access
  * Provide read only permissions for config on day to day basis
* Monitoring: Use CloudTrail with Config to provide deeper insight
* Use CloudTrail to monitor access to Config, such as someone stopping the Config recorder

Config makes up a big part of the exam, so read through the FAQ.

## Set Up Alert if Root User Logs In

* Cloud trail + cloud watch logs + custom metrics
  * Console > CloudTrail
  * Select your trail, scroll down to CloudWatch Logs, click configure
  * Use new or existing log group, for example CloudTrail/DefaultLogGroup, continue
  * Allow on the role, can use the role created here - Allow
  * Now, go to CloudWatch Logs and select the log group for cloud watch noted above
  * Click Create Metric Filter
    * FIlter Pattern - `{ $.userIdentity.type = "Root" && $.userIdentity.invokedBy NOT EXISTS && $.eventType != "AwsServiceEvent" `
    * Click Assign Metric
    * Filter Name: RootAccountUsage
    * Metric namespace: CloudTrail
    * MetricName: RootAccountUsageCount
    * Advance metric settings - metric value 1
    * CreateFilter
  * Create alarm
    * Name - Root Account Usage
    * Description - the root user has logged in
    * >= 1 for 1 consecutive periods, period 5 minutes
    * Send notification to NotifyMe

Need to know the steps for the exam.

Video procedure doesn't work - info above modified using in from [this AWS doc](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html#cloudwatch-alarms-for-cloudtrail-root-example)

## AWS CloudHSM

Dedicated hardware security module (HSM) in the AWS cloud.

The AWS CloudHSM service helps you meet corporate, contractual, and regulatory compliance requirements for data security using dedicated Hardware Security Module (HSM) appliances with the AWS cloud.

Enables:

* Control of data
* Evidence of control
* Meet tough compliance standards

Provides:

* Secure key storage
* Cryptographic operations
* Tamper-resistant HSM

| | CloudHSM | KMS
--- | --- | ---
Tenancy | single | multi
scale & ha | ha service from aws | ha service from from aws
key control | you | you + aws
integration | broad aws support | broad aws support
symmetry | symmetric and asymmetric | symmetric
compliace | FIPS 140-2 & EAL-4 | good
price | $$ | $

Process

* Create a cluster and VPC with public and private subnet
* Create the cluster
* Verify HSM identity
* Initialize the cluster
* Launch a client instance
* Install and configure the client
* Activate the cluster
* Set up users
* Generate keys

How is it secured?

* AWS does not have access to your keys
* Separation of duties and role-based access control is part of the design of the HSM
* AWS can only administer the appliance, not the HSM Partitions where the keys are stored
* AWS can (but probably won't) destroy your keys, but otherwise have no access

Tampering

* If the CloudHSM detects physical tampering the keys will be destroyed
* If the CloudHSM detects five unsuccessful attempts to access an HSM partition as crypto officer the HSM appliance erases itself
* If the CloudHSM detects five unsuccessfult atempts to access the HSM with Crypto User (CU) credentials. the user will be locked and must be unlocked by a CO

Monitoring

* Use cloud trail to monitor api calls made to Cloud HSM

Read:

* CloudHSM FAQs

## Inspector vs Trusted Advisor

What is AWS inspector?

* Amazon inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Inspector automatically assesses applications for vulnerabilities or deviations from best practices. After performing an assessment, inspector produces a details list of security findings prioritized by level of severity. These findings can be reviewed directly or as part of detailed assessment reports which are available vis the Amazon Inspector console or API.

How does it work?

* Create an assessment target
* Install agents on EC2 instances
* Create assessment template
* Perform assessment run
* Review findings against rules

Rules packages

* Common vulerabilities and exposures
* CIS OS security config
* Security best practices
* Runtime behavior analysis

Severity levels - high, medium, low, informational

It will:

* Monitor the network, file system, and process activity within the target
* compare what it sees to security rules
* report on security issues observed withing target during run
* report findings and advise remediations

It will not:

* Relieve you of your responsibilities under the shared responsibility model
* Perform miracles

Try it out

* Launch an ec2 instance
* Security, Identity, and Compliance > Inspector
* Get started - create role
* Tag your ec2 instances - manage tags, add tag, e.g. ProductionWebServer, Yes
* Install the inspector agent on the ec2 instance
* Define assessment target - use the tag from earlier
* Define assessment template - includes a duration
  * May want to spin up your ami somewhere else just for the purposes of a longer running scan - anything that shows up over time probably shows up on the others as well
* Run the template
* After run go to assessment run

Exam tip

* Know the diffs between the two
* Scenario where you install an agent or generate a report => inspector
* Non-security scenarios => trusted advisor
* Basic security stuff (like security group ports, iam use, mfa on root) => probably trusted advisor

What is trusted advisor?

* An online resource to help you reduce cost, increase performance, and improve security by optimizing your AWS environment. Advisor will advise you on 
  * Cost Optimization
  * Performance
  * Security
  * Fault Tolerance.
* Core checks and recommendations
* Locked at not support/developer support
* Full trusted advisor - business and enterprise companies only, unlocks additional things

Find it under Management Tools

## Logging

Services:

* CloudTrail - API calls
* AWS Config - configuration of your environment and what assests you have, point in time with historical view
* VPC Flow Logs - network traffic of ENIs in a VPC
* AWS CloudWatch - performance of your AWS assests

Read: Security at Scale: Logging in AWS (developed using ISO 27001:2005, PCI DSS v2.0, FedRamp)

Control Access to Log Files

* Prevent unauthorized access
  * IAM users, groups, roles and policies
  * S3 bucket policies
  * MFA
* Ensure role-based access
  * IAM users, groups, roles and policies
  * S3 bucket policies

Alerts when logs are created or fail:

* CloudTrail notifications
* AWS config rules

Alerts are specific, but don't divulge detail

* CloudTrail SNS notifications only point to log file location

Log changes to system components, control modifications.

Logs are stored for at least on year, look at s3 and life cycle management.

Exam tips: understand the logging services and the differences between them.

## Section 4 Summary

Updated above sections based on summary presentation.