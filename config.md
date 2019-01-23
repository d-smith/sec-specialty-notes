# AWS Config

From the FAQ:

> AWS Config is a fully managed service that provides you with an AWS resource inventory, configuration history, and configuration change notifications to enable security and governance. With AWS Config you can discover existing AWS resources, export a complete inventory of your AWS resources with all configuration details, and determine how a resource was configured at any point in time. These capabilities enable compliance auditing, security analysis, resource change tracking, and troubleshooting.

Config rules

* express desired configurations for a resource
* evaluated against config changes, see results on dashboard
* Rule types:
    * AWS managed - pre-built and managed ny AWS
    * Customer managed - custom rules, authored using lambda
* Rule evaluation
    * Change triggered - tag/key value, resource type, specific resource
    * Periodic (time trigger)

Configuration History

* Combine with cloud trail to know who made what change from which address.

Multi-account, multi-region aggregation capability

Visibility - foundational element of security
Security teams - CMDBs

Config - https://www.youtube.com/watch?v=sGUQFEZWkho

5 things

* Records changes
* Normalize into a consistent form
* Stores data, provides access API, delivers to S3 for offline analysis
* SNS stream - diff and CI
* CHecks configuration items against rules

Can do things like on demand snapshots

Configuration item - point in time configuration for a resource

* Metadata
* Common attributes
* Relationships
* Current configuration
* Related events - link between config and cloud trail (who, what, when, from where)

Resource inventory - can query it using tags

Evaluations: the result of evaluating a config rule against a resource

Use Cases:

* Security analysis - am i safe?
* Audit compliance - where is the evidence
* Change management - what will this change effect?
* Troubleshooting
* Discovery - what resources exist

Custom Rules

* Create lambda 
* Grant permissions
* Configure rule