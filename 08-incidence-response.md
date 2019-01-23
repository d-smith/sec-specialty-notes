# Incidence Response and AWS in the Real World

## DDoS - Distributed Denial of Service

Based on the whitepaper.

TODO: Read the whitepaper, keep shield in mind if the whitepaper has not been updated.

What is a DDoS attack

* Attack that attempts to make your website or appication unavailable to your end users.
* Can be achived via multiple mechanisms, such as
  * Large packet floods
  * Combination of amplication and reflection techniques
  * Using large botnets

Applification/Reflection Attacks

* Uses things like NTP, SSDP, DNS, Chargen, SNMP, etc.
* Attacker sends third party server request using a spoofed IP address
* Server responds to the request with a greater payload than the request (28x - 58x greater) to the spoofed address
  * For example 64 byte NTP request returns 3,456 bytes
* Attackers can coordinate and use multiple third party servers to send legitimate traffic to the target.

Layer 7 Attacks

* Send multiple get requests
* Slowloris - open multiple connections to the server, partial http requests that do no complete

How to Mitigate DDoS

* Minimize the attack surface area
* Be ready to scale to absorb the attack
* Safeguard exposed resources
* Learn normal behavior
* Create a plan for attacks

Minimize attack surface area

* Avoid having lots of access points - direct ssh or rdp to web servers, application servers, and db servers for management
* Minimize using bastion hosts/jump boxes scoped to known IP addresses, move web, app, and db servers to private subnets.
* Limit exposure to a few hardened entry points

Be Ready to Absorb the Attack

* The key strategy behind a DDoS attack is to bring infrastructure to breaking point. This strategy assumes one thing: that you can't scale to meet the attack.
* To defeat this strategy, design infrastructure to scale as, and when, it is needed.
* Look at both vertical and horizontal scale options

Scaling Benefits

* The attack is spread over a larger area
* Attackers have to counter attack, which uses up more of their resources
* Scaling buys you time to analyze the attack
* Scalaing has the added benefit of providing you with additional levels of redundancy

Safeguard Exposed Resources

* In situations where you cannot eliminate internet entry points to your applications, you'll need to take additional measures to restrict access and protect those entry points without interrupting legitimate end user traffic.
* Three that can provide this control and flexibility
  * CloudFront
  * Route 53
  * Web Application Firewalls

Cloud Front

* Geo restriction and blocking
* Origin access identity

Route 53 

* Alias record sets - immediately redirect your traffic to a cloud front distribution, or a different elb load balancer with higher capacity ec2 instances running WAFs or your own security tools. No DNS changes, no need to worry about propagation
* Private DNS 0 allows you to manage internal DNS names for your application resources without exposing this information to the public internet

WAFs

* Use WAFs to protect against layer 7 attacks
* Use AWS WAF or providers from the marketplace

Learn Normal Behavior

* Be aware of normal and unusual behavior
  * Know the different types of traffic and what normal levels of this traffic should be 
  * Understand expected and unexpected resource spikes
* What are the benefits?
  * Allows you to spot abnormalities fast
  * You can create alarms to alert you of abnormal behavior
  * Helps you collect forensic data to understand the attack

Create a Plan for Attacks

* Having a plan in place before an attack ensures that:
  * You've validated the design of your architecture
  * You understand the costs for your increased resiliency and already know what techniques to employ when you come under attack
  * You know who to contact when an attack happens

AWS Shield

* Free service that protects all AWS customers on ELC, CLoudFront, and Route 53
* Protects against syn/udp floods, reflection attacks, and other layer 3/layer 4 attacks
* Advanced provides enhanced protections
  * Always on, flow based monitoring of network traffic and active application monitoring to provide near real-time notifications of DDoS attacks
  * DDoS respons team 24x7 to manage and mitigate application layer DDoS attacks
  * Protects you AWS bill against higher fees due to ELB, CLoudFront, and Route 53 usage spikes during a DDoS attack

Exam Tips

* Read the white paper - https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf
* Remember the technologies used to mitigate a DDoS attack
  * CloudFront
  * Route 53
  * ELBs
  * WAFs
  * Autoscaling (both for web servers and WAFs)
  * CloudWatch

## WAF Integration into AWS

Exam Tip

* Integrate with Application Load Balancers
* Integrate with CloudFront

## EC2 Has Been Hacked - What Should You Do?

What steps should you take?

* Stop the instance immediately
* Take a snapshot of the EBS volume
* Deploy an instance in to a totally isolated environment. Isolated VPC, no internet access - ideally a private subnet
* Access the instance using a forensic workstation
* Read through the logs to figure out how (Windows Event Logs)

Isolate

* The instance you are investigating
* The machine you are using to perform the investigation from (the forsensic work station)

## I've Leaked My Keys on Github!

IAM Console > User > Access Keys

* Disable the leaked key
* Create a new key
* Delete the old key id

## Reading CloudTrail Logs

* JSON object with multiple records of API calls
* All API calls

Be sure to replicate the logs to another account for safekeeping

## Pen Testing

Landing page - [AWS Pen Testing](https://aws.amazon.com/security/penetration-testing/)

Always need permission to do pen testing.

Resources that may be pen tested:

* EC2
* RDS
* Aurora
* CloudFront
* API Gateway
* Lambda
* Lightsail
* DNS walking

EC2 Marketplace - search for penetration testing

* Check out Kali Linux
  * Aircrack ng - look for youtube videos

## AWS Certificate Manager

Route 53

* Can buy/register domains through AWS
* Can extend registration for up to 9 years, turn on auto-renew

AWS Certificate Manager

* AWS certificate manager makes it easy to provision, manage, deploy, and renew TLS/SSL certificates on the AWS platform
* Can import your own certificates from other CAs
* Can enter your own domain name to obtain cert, then do DNS validation or email.
  * DNS validation - add cname record to the domain in route 53, then wait about 5 - 10 minutes
* For certs provisioned through ACM, AWS will automatically renew them (and the ARN remains the same)
  * Not so for imported certs, or certs associated with private zones
* Cannot export ACM certificates - use only with AWS services

Integration

* CloudFront integration - reference cert ARN for customer domain name
* EC2 - load balancers, listeners, cert type - select from ACM - applied at the ALB

Exam Tips

* SSL certificates renew automatically, provided you purchased the domain name from Route 53 and it's not for a Route 53 private hosted domain.
* You can use Amazon SSL certificates with both load balancers and cloud front
* You cannot export the certificates

## Perfect Forward Secrecy and ALBs

Wikipedia article on [perfect forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy)

* Compromises of long term keys do not compromise past session keys
* If someone recorded your traffic, then compromised your key, then can't go back and decrypt the recorded traffic

ALB

* Add a listener, choose a protocol, https uses port 443 (remember), etc...
* Pick certificate type, look at the security policies - documented [here](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html#describe-ssl-policies)
  * To enable perfect forward secrecy, you need the ECDHE cipher.
  * 2016-08 has this, will give you perfect forward secrecy

## API Gateway, Throttling & Caching

Throttling

* To prevent your API from being overwhelmed by too many requests, Amazon API GW throttles requests
* When request submissions exceed steady-state and burst limit, API fails the limit-exceeding requests and returns a 429
* By default, steady state request rate is 10,000 rps
* By default, limits the burst to 5,000 across all APIs within an AWS Request.
* Account-level rate limit and burst limit can be increased upon request.

Request Processing

* If a caller sends 10,000 requests in one second spaced evenly (for example 10 requests/ms) API GW processes all requests without dropping any
* If a caller sends 10,000 requests in the first millisecond, API GW servces 5,000 and throttles the request in the one second period
* If a caller sends 5,000 requests in the first millisecond, and evenly spreads another 5,000 across the remaining 999 milliseconds, then all requests are processed.

Caching

* You can enable API caching in Amazon API Gateway to cache your endpoint's response.
  * Fewer calls to endpoint
  * Improved latency
* Cache entries have a TTL
  * Default TTL (in seconds) is 300
  * Max is 3600
  * TTL of 0 means caching disabled
* Enabled at the stage level

## AWS Systems Manager Parameter Store

Systems Manager used to manage fleets for ec2 instances at scale.

* Accessed under EC2 in the console
* Use secure string for sensitive information
* Can reference in cloud formation

Exam Tips

* Confidential information like passwords, database connection strings, license codes can be store in SSM parameter store
* You can store values as plain text or encrypted
* You can then reference the value by using their names
* You can use this service with EC2, CloudFormation, EC2 run command, Lambda, etc.

## AWS Systems Manager EC2 Run Command

Scenario

* You work as a sys admin managing a large number of ec2 instances and on premise systems
* You would like to automate common admin tasks and ad hoc configuration changes, e.g. installing apps, applying the latest patches, joining new instances to a Windows domain without having to log into each instance

Systems Manager Run

* Need to define a role, and launch the ec2 instances using that role, and tags you might use to classify instance types based on your needs
* In Systems Manager, pick run command
  * Can pick preconfigured commands, select targets by tag or by instance id

Exam Tips

* Commands can be applied to a group of systems based on AWS instance tags or selecting them manually
* SSM Agent needs to be installed on all your managed instances
* The commands and parameters are defined in a systems manager document
* Commands can be issued using AWS console, AWS cli, AWS tools for windows powershell, Systems Manager API, or Amazon SDKs
* You can use this service with your own on-premise systems as well as EC2 instances

Where's it show up in the exam?

* Scenario where you need to apply security updates at scale (but you must have the SSM agent enabled, need to have the role assigned to them allowing systems manager to talk to them, apply to instances by selecting them manually or by tags)

## Compliance Frameworks

Main 3:

* PCI DSS
* ISO
* HIPAA

AWS Compiance Page [here](https://aws.amazon.com/compliance)

* Read up about the 3 on the complance page.

ISO 27001:2005/10/13

* Specifies the requirements for establishing, implementing, operating, monitoring, reviewing, maintaining and improving a documented information Security Mangement System within the context of the organization's overall business riskss

FedRAMP

* The Federal Risk and Autorization Management Program is a government-wide program that provides a standardized approach to security assessment, authorization, and continuous monitoring for cloud products and services.

HIPAA

* HIPAAA is the health insurance portability and accountability act of 1996.
* Goal of the law is to make it easier for people to heep health insurance, protect the confidentiality and security of healthcare information and help the healthcare industry control administrative costs.

NIST

* National institute of standards and technology Framework for Improving Critical Infrastructure Cybersecurity
* Set of standards and best practices to help organizations manage cybersecurity risks

PCI DSS v3.2

* Payment Card Industry Data Security Standard
* Widely accepted set of policies and procedures intended to optimize the security or credit, debit, and cash card transactions and protect cardholders against misuse of their personal information.

PCI DSS v3.2

* Req 1 - install and maintain a firewall configuration to protect cardholder data
* Req 2 - do not use vendor supplied defaults for system passwords and other security parameters
* Req 3 - protect stored cardholder data (for example encrypt the data, protect the keys)
* Req 4 - encrypt transmission of cardholder data across open, public networks
* Req 5 - protect all systems against malware and regularly update anti-virus software or programs
* Req 6 - develop and maintain secure systems and applications
* Req 7 - restrict access to cardholder data by business need to know
* Req 8 - identify and authenticate access to system components
* Req 9 - Restrict physical access to cardholder data
* Req 10 - Track and monitor all access to network resources and cardholder data
* Req 11 - Regularly test security systems and processes
* Req 12 - Maintain a policy that addresses information security for all personel

Other Frameworks

* SAS70 - statement on auditing standards no. 70
* SOC1 - service organization controls - accounting standards
* FISMA - federal information security modernization act
* FIPS 140-2 - US government standard used to approve cryptographic modules, rated from level 1 to 4 with 4 being the highest. Cloud HSM meets the level 3 standard.

Exam Tips

* Be familiar with the top 3
* Read the compliance page
* Be aware of FIPS 140-2 for Cloud HSM vs KMS scenarios - use CloudHSM if you need FIPS 140-2 compliance to level 3

## Section 8 Wrap Up

* Shield - know the services it protects and the type of protections
* DDoS - read the white paper, know the technologies you can use to mitigate the attacks (cloud front, route 53, elbs, WAFs, autoscaling, cloudwatch)
* WAFs - know the integrations (ALB, CloudFront)
* EC2 hacked - know what to do
* Keys leaked - know what to do
* Review VPC flow logs
* Reading cloud trail logs - might come up
* Pen testing - know the services you can pen test
* Certificate manager
* Perfect forward secrecy
* API gateway throttling - know the default behavior and the 429 error
* API gateway caching
* AWS systems manager parameter store
* AWS systems manager run command
* Compliance - check out the compliance site
* FIPS 140-2 - think cloud hsm