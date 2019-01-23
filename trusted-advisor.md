Checks available to all customers:

* Performance/service-limits
    * high utiliation ec2 instances (90% on 4 or more days in a 14 day period)
    * provisioned iops ebs volumes attached to non-ebs-optimized instances
    * large number of rules in security groups
    * large number of security groups applied to an instance, 
    * route 53 resource record set that can be alias record sets (alias records routed free of charge) 
    * over utilized ebs magnetic volumes (suggests more appropriate volume type for the workflow) 
    * cloudfront content delivery optimization - notifies when cloudfront is the better way to serve s3 content
    * cloud front header forwarding and cache hit ratio - notified when certain forwarded headers (Date, User-Agent) significantly reduce cache hit rate
    * ec2 to ebs throughput optimization - checks for volumes whos throughput might be limited by the max throughput capability of the instance they are attached to
    * cloudfront alternative domain names - checks for cloudfront alternative domain names with incorrectly configured DNS settings.
* Security Groups - Specific Ports Unrestricted, IAM Use, MFA on Root Account, EBS Public Snapshots, RDS Public Snapshots

Remaining checks available to business or enterprise support plans.

Categories

* Cost Optimization
* Performance
* Security
* Fault Tolerance
* Service Limits

Security

* Security groups - specific ports unrestricted (0.0.0.0/0)
* Security groups - unrestricted access 
* IAM use
* Amazon s3 bucket permissions
* MFA on root account
* IAM password policy
* Amazon RDS security group access risk
* Cloud trail logging - service enabled, write access to bucket
* Route 53 - check for SPF (sender policy framework) resource record set for each MX resource record set
* ELB listener security
* ELB security groups
* CloudFront Custom SSL Certs in IAM Certificate Store
* CloudFront SSL cert on origin server
* IAM access key rotation
* Exposed access keys 
* EBS public snapshots
* RDS public snapshots