# Identity Access Management, S3 & Security Policies

## IAM Recap

For this speciailty, key to passing exam is understanding IAM and policies inside and out, and know how they relate to s3 policies and access policies.

IAM - allows you to manage users and their level of access to the AWS console.

IAM gives you:

* Centralized control of your AWS account
* Shared access to your AWS account
* Granular permissions
* Identity federation (including AD, facebook, linked in, etc.)
* Multifactor authentication
* Provide temp access for users/devices and services where needed
* Set up you own password rotation policy
* Integrates with many different AWS services
* Supports PCI DSS compliance


Terms

* Users - end users
* Groups - collection of users under one set of permissions
* Roles - create then assign to AWS resources
* Policies - a document that defines one or more permissions

1st access to AWS console: root access

## IAM Root User Scenario

Scenario - you are taking over for a previous admin who has left on bad terms, and who used the root login for all their work

* Small company, small group of admins
* Root level access keys, MFA

Steps

* Log into the console - note IAM is global
* Under security status, or via My Security Credentials - prompt to work at the root level or at the IAM - go to root level (security credentials)
* Change the password, disable MFA, then re-enable MFA.
* Look at access keys - delete them. Don't use access keys for root account.
* Check on users - look at last activity


Hint: take a photo of your QR code, store it in s3 - if you lose your smart phone you can re-activate an authenticator method using the QR code. Google authenticator - add a new method using the QR code you saved.


Resetting Root Users

* Create a new root user password and strong password policy
* Delete previous 2FA and re-create
* Check if root has an access key id and secret access key - if so delete
* Check other user accounts, delete if not legit

## IAM Policies 101

* IAM policies specify what you are allowed to do with any AWS resource.
* Global, applies to all areas of AWS.
* Attached to IAM users, groups, or roles.
* Define what a principal can do in your AWS environment


3 Types

* AWS managed
    * Administered by AWS
    * Note these can be updated by AWS
* Customer managed
    * Standalone policy you administer in your account
* Inline
    * Useful to maintain a strict one to one relationship between a policy and the principal entity that it's applied to.
    * Useful to ensure you don't inadvertantly associate the policy with anyone else.

Power Users do not have access to IAM.


## S3 Bucket Policies

* Attached only to s3 buckets
* Specify what actions are allowed or denied *on the bucket*, can be broken down to the user level

Use Cases

* Simple way to grant cross account access to s3
* Use when IAM policies bump up against size limits (2kb for users, 5 kb for groups, 10kb for roles). S3 supports bucket policies up to 20 kb.
* You prefer to keep access control policies in the s3 environment. Think denying access to a specific bucket in a 40,000 user org.

Scenario

* S3 bucket policy denies user access, IAM policy grants read/write/delete to the IAM user => s3 bucket policy wins here

Steps

1. login with admin privileges, create two buckets
2. user IAM allows read and write, apply a bucket policy that allows delete to one of the buckets => policy generator does not supply the '/*' needed by some actions, like delete. You must know this for the exam
3. Normal bucket - can't delete
4. Bucket with delete policy - can delete
5. Admin back to bucket, add policy with deny all actions to the user arn
6. Now add access for a user as a second policy allow on the bucket. Policy has a deny all, plus an allow for one specific user. => *Explicit deny always overrides an allow*!

Explicit deny always overrides an allow

* Deny in IAM, allow in bucket policy => Deny takes precedent

## S3 ACLs

ACLs are a legacy access control mechanism that predates IAM.

* Use when you must *apply policies on individual objects*.
* Can use as a work around if bucket policy is too large.

CLI or API - need account number and canonical user id. `aws s3api list-buckets` - gives canonical id for individual user in the output

Access for the account, individual users (CLI or API), other accounts, public

Scenario:

1. Make an object public from an ACL point of view
2. Go to IAM, edit policy, full read, limited write, edit json and change allow to deny. All read plus one write - Deny.
3. Log in as IAM user - can read the bucket object via the link, no IAM in scope. IAM user access - deny applies.

## Policy Conflicts

IAM vs s3 policy vs s3 ACL conflicts...

Whenever an AWS orincipal (user, group, role) issues a request to s3, the authorization decision depends on the union of all the IAM policies, s3 bucket policies, and s3 ACLs that apply.

With least privilege:

* Decisions always default to DENY.
* Explicit deny always trumps an ALLOW.
* If no method specifies an ALLOW then the request will be denied by default
* Only if no method specifies a deny and one or more methods specify an ALLOW will the request be allowed.

1. Decision starts at Deny
2. Evaluate all applicable policies
3. Is there an explicit deny?
    ==> DENY
4. If there was no deny, is there an expliciy allow?
    ==> ALLOW
5. If no explicit deny, and no explicit allow, deny.

## Forcing Encryption on S3

Use a bucket policy with two statements:

1. First statement, allows s3:GetObject on bucket/*
2. Second statement, same as first, except it is a deny, and the bool condition is aws:SecureTransport false.

````console
{
    "Condition":{
        "Bool": {
            "aws:SecureTranspart" : false
        }
    }
}
````


## Cross Region Replication and S3

By default cross region replication is done using SSL/TLS - you do not have to create bucket or IAM policies to force SSL.

You can replicate objects from a source bucket to only one destination bucket. After s3 replicates an object, it cannot be replicated again.

CRR requirements:

* Source and dest must have versioning turned on
* Must be in different regions
* S3 must have permissions to replicate objects from that source bucket to the destination bucket on your behalf.
* If the source bucket owner also owns the object, the object owner has full permissiones to replicate the object. If not, the object owner must grant the bucket owner the READ and READ_ACP permissions via the object ACL.

## Cross-Account CRR

* The IAM role must have permissions to replicate objects in the destination bucket.
* In the replication configuration, you can optionally direct S3 to change the ownership of object replicas to the AWS account that owns the destination bucket.

Standard architecture solution to secure cloud trail - replicate cloud trail logs to a second AWS account, originating account can't read or access replicated logs.

What is replicated?

* Any new objects created after you add a replication configuration
* In addition to unencrypted objects, s3 replicates objects encrypted using s3 managed keys (SSE-S3) or AWS KMS managed keys (SSE-KMS, you have to turn it on).
* Object metadata
* Any object ACL updates
* Any object tags
* Amazon s3 replicates only objects in the source bucket for which the bucket owner has permissions to read objects and read access control lists (ACLs)

What is replicated? - Deletes

* If you use just a delete marker, then that delete marker is replicated

What is not replicated?

* Anything created before CRR is turned on.
* Objects created with server-side encryption using customer-provided (SSE-C) encryption keys.
* Objects created with server-side encryption using AWS KMS-managed encryption (SSE-KMS) keys, unless you explicitly enable this option.
* Objects in the source bucket for which the bucket owner does not have permissions. This can happen when the object owner is different from the bucket owner.
* Delete versions of objects. This is a security mechanism.

Exam tips:

* You do not need to use a bucket policy with aws:SecureTransport to replicate objects using SSL
* Delete markers are replicated, deleted versions of files are not.
* Versioning must be enabled
* It is possible to use CRR from one AWS account to another - the IAM role must have permissions to replicate objects in the destination bucket.

## S3 Encryption

* Server-side - request s3 to encrypt your object before writing it to disk, and decrypt it when downloading objects
    * SSE-S3 - server side encryption with s3-managed keys
    * SSE-KMS - server side encryption with KMS managed keys
    * SSE-CMK - SSE with customer provided key
* Client-side - you encrypt the data client-side and upload encrypted data. You manage the keys, encryption process, etc.

## Securing S3 Using CloudFront

Scenario - don't want users to browse directly to our s3 bucket;
we want them to go through CloudFront.

Lab

* From console, create the bucket
* From console, create a new CloudFront distribution
    * Restrict bucket access - can restrict at distro create time
    * Custom domain name? Import a cert through ACM, note this is different from any load balancer cert that might sit in front of a non-s3 origin
* Add object to s3, make it public - verify via url
* In CloudFront, distribution settings, origin, select and edit:
    * Restrict bucket access, need to reuse an origin access identity, or create a new one, and grant read permissions on the bucket to the origin access identity.
* Note it might take a while to take effect - could be anywhere between 4 and 48 hours.

## Securing S3 Using Presigned URLs

* Usually done using the SDKs
* You can do this using the command line
    * Can create a role that you can assign to an EC2 role

```console
aws s3 presign s3://bucket/object --expires-in 300
```

* URLs are valid for a specific length of time, with a default of one hour.
* Use the --expires-in option to change the length of time the URL is available for.

## Security Token Services with Active Directory

Security Token Service - grants users limited and temporary access to AWS resources. Users can come from three sources:

* Federation (typically active directory)
    * Uses SAML
    * Grants temporary access based off the user's Active Directory credentials. Does not need to be an IAM user.
    * SSO allows signing into the console without assigning IAM credentials
* Federation with mobile apps
    * Use facebook/google/amazon or other open id connect providers
* Cross account access
    * Lets users in one aws account use resources in another

Terms:

* Federation: combining or joining a list of users in one domain (such as IAM) with a list of users in another domain (such as active directory, facebook, etc)
* Identity Broker: a service that lets you take an identity from point A and join it (federate it) with point B
* Identity Store - services such as active directory, facebook, google, etc
* Identities - a user of a service such as facebook etc

### Scenario

You are hosting a company web site on some ec2 web servers in your vpc. Users of the web site must log in to the site which then authenticates against the company's active directory servers which are based in a site at the company HQ. Your VPC is connected to HQ via a secure IPSEC VPN. Once logged in the user has access only to their own s3 bucket. How to set up?

1. Employee enters the user name and password 
2. The identity broker (which you wrote) uses the org's LDAP directory to validate the employee's identity using the provided credentials
3. The Identity Broker calls the GetFederationToken function using IAM credentials. The call must include an IAM policy and duration (1-36 hours) along with a policy that specifies the permissions to be granted to the temporary security credentials
4. The security token service confirms the policy of the IAM user making the call to GetFederationToken gives permission to create new tokens and then returns four values: an access key, a secret access key, a token, and a duration (the token's lifetime)

Exam Tip: know this flow

## S3, IAM & CloudFront Summary

* Resetting root users
* IAM Policies
* S3 Bucket Policies
* S3 ACLs
* Policy Conflicts
* Secure Transport
* CRR
* Pre-signed URLs
