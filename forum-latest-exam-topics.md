# Forum - Latest Topics

## First User

3 weeks ago, I mentioned I failed my first attempt to pass the Security Specialty exam, mentioning that the ACG course was covering about only 60% of the exam content.

I just retook and passed the exam (going from 620 points to 850) and want to share all the materials I used to learn all what's not covered in the ACG course.

This time again KMS was most of the exam, with IAM policies and Organizations SCP, and also EC2 forensic. Troubleshooting policies was again just 1 question. And again some questions about Artifact, Athena, and this time I also had two questions on Vault lock (which I was unprepared for. I really think ACG need to update their training material on this one as the exam as moved too far for the course content to be relevant anymore.

Must study topics are:

* AWS Organizations
* KMS Key policies (via Service & grants)
* Artifact
* Athena
* [Vault lock](https://docs.aws.amazon.com/amazonglacier/latest/dev/vault-lock.html)
* GuardDuty. Although I didn't saw it in the exam, I doubt it's going to be long before GuardDuty will show up in the exam

Most of the material I used are whitepapers, FAQs, AWS documentation and re:invent or other AWS event sessions. With re:invent 2018 I guess there will be several new interesting videos which should be added to this list soon.

Here they are by topic

KMS policies, Grants and ViaServices:

* [AWS KMS whitepaper](https://d0.awsstatic.com/whitepapers/aws-kms-best-practices.pdf)
* [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)
* [Deep dive into AWS encryption](https://www.youtube.com/watch?v=gTZgxsCTfbk)
* [Best practices for implementing AWS KMS](https://www.youtube.com/watch?v=X1eZjXQ55ec)
* [How do I share my KMS CMK across accounts?](https://www.youtube.com/watch?v=qS7P2DpJFZQ)

CloudTrail:

* [Deep drive into CloudTrail](https://www.youtube.com/watch?v=t0e-mz_I2OU)
* [AWS re:Invent 2017: Using AWS CloudTrail to Enhance Governance and Compliance of Ama (DEV311)](https://www.youtube.com/watch?v=mbdC6IhOROk)

AWS Organizations

* [FAQ](https://aws.amazon.com/organizations/faqs/)
* [Applying AWS organizations to complex structures](https://www.youtube.com/watch?v=pfetMIlo_2s)
* [About Service Control Policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgsmanagepolicies_about-scps.html)

IAM

* [AWS re:Invent 2016: Become an AWS IAM Policy Ninja in 60 Minutes or Less (SAC303)](https://www.youtube.com/watch?v=y7-fAT3z8Lo)
* [Delegating Access to Your AWS Environment](https://www.youtube.com/watch?v=0zJuULHFS6A)
* [The Evolution of Identity and Access Management on AWS - AWS Online Tech Talks](https://www.youtube.com/watch?v=2apSeOjDwZo)
* [Advanced Techniques for Federation of the AWS Management Console and Command Line Interface (CLI)](https://www.youtube.com/watch?v=t6WWda_AY04)
* [AWS re:Invent 2017: Soup to Nuts: Identity Federation for AWS (SID344)](https://www.youtube.com/watch?v=CJexxdv054c)
* [A Self-Directed Journey to AWS Identity Federation Mastery](http://federationworkshopreinvent2016.s3-website-us-east-1.amazonaws.com/)
* [Architecting Security and Governance Across a Multi-Account Stra (SID331)](https://www.youtube.com/watch?v=71fD8Oenwxc)

Forensic

* [AWS re:Invent 2017: Incident Response in the Cloud (SID319)](https://www.youtube.com/watch?v=ufmgB9M2WII)
* [Automating Incident Response and Forensics](https://www.youtube.com/watch?v=f_EcwmmXkXk)
* [AWS re:Invent 2017: Using AWS Lambda as a Security Team (SID301)](https://www.youtube.com/watch?v=oMlGHP8-yHU)
* [Modernize Your Threat Detection and Remediation Process Using Cloud Services](https://www.youtube.com/watch?v=ZYT8MHdQ410)

Athena

* [FAQ](https://aws.amazon.com/athena/faqs/)
* [User guide : querying AWS CloudTrail logs](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html)
* [Querying AWS CloudTrail logs with Amazon Athena](https://www.youtube.com/watch?v=cfojAdWoMWo)
* [Blog post](https://aws.amazon.com/blogs/big-data/aws-cloudtrail-and-amazon-athena-dive-deep-to-analyze-security-compliance-and-operational-activity/)

AWS System Manager (SSM)

* [FAQ](https://aws.amazon.com/systems-manager/faq/)
* [Amazon EC2 Systems Manager Introduction](https://www.youtube.com/watch?v=zwS8lssaY_k)
* [Deep Dive with Amazon EC2 Systems Manager](https://www.youtube.com/watch?v=BmpxZsk9N48)

Artifact

* [FAQ](https://aws.amazon.com/artifact/faq/)

And use the service download and read (partially) artifacts and agreements to see what they are

Lambda@Edge

[Introducing Lambda@Edge](https://www.youtube.com/watch?v=c_ZL3nOxEi8)
[AWS re:Invent 2017: Introduction to Amazon CloudFront and AWS Lambda@Edge (CTD201)](https://www.youtube.com/watch?v=wRaPw1tx6LA)

Guard Duty

[FAQ](https://aws.amazon.com/guardduty/faqs/)
[Deep Dive on Amazon GuardDuty - AWS Online Tech Talks](https://www.youtube.com/watch?v=o2YaIsps5LY)

VPC

* [AWS re:Invent 2017: Creating Your Virtual Data Center: VPC Fundamentals and Connection (NET201(](https://www.youtube.com/watch?v=Tff1mekxOJ4)
* [AWS Summit Tel Aviv 2017: Fundamentals of Networking and Security on AWS](https://www.youtube.com/watch?v=KtPambVS2-4)
* [AWS Summit Series 2016 | Chicago - Network Security and Access Control within AWS](https://www.youtube.com/watch?v=AcBcmILiQTo)

And unrelated to the exam but the best presentation of all:

[AWS re:Invent 2017: The AWS Philosophy of Security (SID322)](https://www.youtube.com/watch?v=KJiCfPXOW-U)


## Second User

 would like to take the time to tell you guys I took the test yesterday and made a 70 and failed. It was the hardest test I have ever taken and if you are looking here and just relying on ACloudGuru you will most likely fail. I would suggest using 2 or even 3 different resources for studying because this course goes in a weird direction and talks about things wrongly or things you dont need. For example section 2 you might as well skip as I did not get a single question about the shared responsibility model.

if you just want to use this course here are some extra things you should look at studying before taking the exam

1. Athena - came up like 10 times in answer choices

2. Quicksights

3. How to make sure that you do not use AWS provided DNS

4. NotPrinciple in IAM policies

5. why are your cloudtrail logs not logging? does the bucket not exist?

6. granting cross account access to auditors

7. Cognito groups and Cognito sync triggers and how to place a group of users in restricted

8. What port number do you need to open to successfully use SES

9. what is the bool MultiFactorAuth condition?

10. how can you recover data from a deleted CMK?

11. ElastiSearch

12. how you can inspect actually network traffic?

13. How should you secure ECS?

14. I had a lot of questions about organizations and restricted access to only certain resources in specified account of your org

15. what is the principle condition

16. how can you make a policy with write once read many archiving policy?

In conclusion do not treat this exam like an associate. Treat is more like a professional and I greatly encourage you to go out and study more in depth topics covered in this cours.

## More Perspecfives

* https://daviseford.com/blog/2018/08/17/aws-security-specialty-exam-tips.html
* 