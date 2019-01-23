# Amazon Guard Duty

https://www.youtube.com/watch?v=Imjbh0WPSR4

Threat Detection Service Re-Imagined for the Cloud

* Continuously monitors and protects AWS accounts, along with the applications and services running within them.
* Detects known and unknown threats (zero-days)
* Makes use of AI and machine learning
* Integrated threat intelligence
* Operates on CloudTrail, VPC Flow Logs, DNS
* Detailed and actionable findings

Regional service - enable it in all regions

Detecting Known Threats

* GuardDuty consumes feeds from various sources
    * AWS Security
    * Commercial feeds
    * Open source feeds
    * Customer provided thread intel

* Known malware infected hosts
* Anonynizing proxies
* Sites hosting malware and hacker tools
* Cryto-currency mining pools and wallets
* Great catch-all for suspicious and malicious activity

Detecting Unknown Threats

* Anomoly Detection

    * Algs to detect unusual behavior
        * Inspecting signal patterns for signatures
        * Profiling normal and looking at deviations
        * Machine learning classifiers

    * Larger R&D effort
    * Develop theoretical models
    * Experiment with implementations
    * Testing, tuning, and validation

What can the service detect?

* RDP brute force
* RAT (remote access trojan) installed
* Exfiltrate temp IAM credentials over DNS
* Probe API with temp creds
* Attempt to compromise account

Finding Type Categories: Recon, Backdoor, Trojan, Unauthorized Access ,Stealth, Crypto Mining

Virtuous Flywheel - use of GuardDuty across the ecosystem yields benefits to all customers using the service.

GuardDuty - Cloud watch events to lambda to slack, pager duty, etc.

Multi-account support

* multi-account on-boarding workflow - pick a master account, send invites to member accounts, accept to centralize findings in the master account.

Bring Your Own Threat Intelligence

* Styx and OTX format, alien format, etc
* Other customizations