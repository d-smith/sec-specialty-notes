# AWS Organizations

Note - most of these notes taken verbatim from the organizations user guide.

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage. AWS Organizations includes consolidated billing and account management capabilities that enable you to better meet the budgetary, security, and compliance needs of your business. As an administrator of an organization, you can create accounts in your organization and invite existing accounts to join the organization.


## Security-related Features

* Control over the AWS services and API actions that each account can access

    > As an administrator of the master account of an organization, you can restrict which AWS services and individual API actions the users and roles in each member account can access. This restriction even overrides the administrators of member accounts in the organization. When AWS Organizations blocks access to a service or API action for a member account, a user or role in that account can't access any prohibited service or API action, even if an administrator of a member account explicitly grants such permissions in an IAM policy. Organization permissions overrule account permissions.

* Integration and support for AWS Identity and Access Management (IAM)

    > IAM provides granular control over users and roles in individual accounts. AWS Organizations expands that control to the account level by giving you control over what users and roles in an account or a group of accounts can do. The resulting permissions are the logical intersection of what is allowed by AWS Organizations at the account level, and what permissions are explicitly granted by IAM at the user or role level within that account. In other words, the user can access only what is allowed by both the AWS Organizations policies and IAM policies. If either blocks an operation, the user can't access that operation.

* Use cloud trail plus lambda to monitor inmportamt changes to your organization


## Organization Policies

Policies in AWS Organizations enable you to apply additional types of management to the AWS accounts in your organization. Policies are enabled only after you enable all features in your organization. You can apply policies to the following entities in your organization:

* A root – A policy applied to a root applies to all accounts in the organization
* An OU – A policy applied to an OU applies to all accounts in the OU and to any child OUs
* An account – A policy applied to an account applies only to that one account

Notes:

* Service control policies never apply to the master account, no matter which root or OU the master account is located in.
* Currently, service control policy (SCP) is the only supported policy type.

### Service control policies. 

Service control policies (SCPs) are similar to IAM permission policies and use almost the exact same syntax. However, an SCP never grants permissions. Instead, think of an SCP as a "filter" that enables you to restrict what service and actions can be accessed by users and roles in the accounts that you attach the SCP to. An SCP applied at the root cascades its permissions to the OUs below it. An OU at the next level down gets the mathematical intersection of the permissions flowing down from the parent root and the SCPs that are attached to the child OU. In other words, any account has only those permissions permitted by every OU and the parent root above it. If a permission is blocked at any level above the account, either implicitly (by not being included in an "Allow" policy statement) or explicitly (by being included in a "Deny" policy statement), a user or role in the affected account can't use that permission, even if the account administrator attaches the AdministratorAccess IAM policy with */* permissions to the user.