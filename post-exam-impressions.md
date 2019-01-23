# Impressions - Post Exam

So much ground covered by the exam...

First, knowing how to take an AWS certification exam helps.

* Read the question
* Look to quickly eliminate the obviously wrong answers
* Re-read the question carefully - this can help you hone in on the correct answers based on situational context
* Don't assume things not given - in practice exams I would assume when having to provide wo steps in a scenario a third step would be next thing I'd do, but the question required an answer without assuming follow on steps.

General things...

* Needed a more in depth understanding of Guard Duty - the different findings types, trusted IPs, etc.
* Need to know KMS inside and out 
* Need to know IAM inside and out, including policy conditions
* Need to know all the scenarios for cross account access - anything given in IAM docs, s3 docs, and blog posts seem to be fair game.
* Quite a few questions related to AD and the different offerings on AWS and how they federate back to on premise servers.
* Beyond AD specifics knowing federation in depth is essential - SAML, Cognito, AD, etc
* Knowing how to secure SES came up a couple times
* Need to know your networking in depth - how to control DNS resolution in VPC, how to integrate with on prem networks (direct connect, VPNs)
* Need to know cert manager
* Need to know how cloud front is administered - even things like knowing you use us-east-1 for admin and thus that's where you set up cert manager with cloud front regardless of the region your origin server sits in.

How I prepared:

* Completed the A Cloud Guru course
* Read forum tips that alerted me to the fact the course did not cover everything needed for the exam. At the time I prepared the exam had gone from beta, the with drawn, to being reoffered, so the scope was in flux.
* Studied the items others thought were relevant
* Spent time watching re:Invent presentations related to security
* Found some practice exams - Linux academy had a challenge exam, Udemy had a course with 3 practice exams. Took each exam, and filled in knowledge gaps after each exam.

Finally, when sitting the exam trust your preparation, and don't panic if questions come up that you are not familiar with or are uncertain about - some of them you can reason through or at least have a better guess at.