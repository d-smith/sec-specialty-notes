# Identity and Access Management at Each Layer of the Cake

re:Invent 2018

Identity the subject: authN, authZ, audit, governance etc for your cloud workloads

Identity Cake

top layer - End User Applications
middle layer - Infrastructure Layer: OS, RDBs, etc
bottom layer - Amazon Web Services

Planes of Access

control plane - AWS API
data plane - vpc connection (ssh, rpc) distinct paths, credentials, protocols

DDB - unified approach: same authN and authZ model (all via the same API)

Identity for the Platform Layer

Challenge: get a lot of cloud devs strongly identified, assigned to roles, across a fleet of resource.

Options:

* SAML IdP to bring to IAM roles
* AWS single sign-on, AWS manages the identity providers
* Custom broker

Which?

1. Can you make auth decisions purely based on groups or attributes? No => Customer Broker
2. If (1) is yes... Do you have SAML inf? => Use SAML
    No SAML> Number accounts < 20 go to AWS SSO Otherwise go back to SAML

IAM Delegate Administration - Boundary Policies

Certain permissions for IAM could not be constrained

Bundary Policies - administrator can grant previously IAM management permissions, but specify a permissions boundary. Use case: allow devs ability to create principals for their application and attach policies, but with defined boundaries. 

Admin Step 1: create the permission boundary, expressing the constraints
Admin Step 2: allow role creation, but only within boundary
Admin Step 3: allow developer to pass role (use role) - leverage role paths

Dev: app developer creates role with delegated permissions 

Fixed: self service role creation with guardrails.

Demo - cloud formation macro to make creating permissions easier, 28 lines vs 250 lines of json

CFN macro - sends template to lambda as a preprocessor. 

* Execution role builder cloud formation macro - on github

## Identity in the Infrastructure Layer

Identity for the infrastructure
Identity of the infrastructure

Spectrum of traditional to utopia

Trad:

* domain joining
* ldap/kerb
* ssh keys 


Utopia:

* NoOps - immutable infrastructure

New option: IAM based auth

replace user name and passwork with ephemeral credentials
codify who has access to what within iam policies

IAM auth for EC2 - get a shell session, but turn off ssh (this is session manager)
IAM auth for RDS

AWS secrets manager - auto rotation for all your credentials

* Load db creds
* 1st call is to secrets manager to read creds
* COnnect to resource
* Safe rotation

Secrets Manager Workshop - Up on Github

## Application Layer

Human to Application
Service to Service

### Humans - Amazon Cognito

* Normalizing layer for applications
* Vends standard tokens 
* Clean integration with API gateway, ALB

Flows

1. Authentication request to identity pool
2. Handles all the redirects, etc
3. CUP tokens (cognito user pool)
4. Send tokens as the auth for the user to the APIs
5. Exchange the user token for an identity token for calling AWS services (gets STS token for request signing)

### Service to Service

Historical - TLS mutual authentication

Now - use combination of IAM based authentication and the API gateway

* Attach role to calling service, give role permissions to invoke API
* Configure API gateway for IAM authentication
* Attach permissions to the principal calling, or use a resource based policy to enable cross account invocation.
* Time to call: sig v4 sign the request, then send it

Now - service to service usig Amazon Cognito (OAuth)

* CLient credentials flow - retrieve client id and secret using SSM parameter store
* Go to cognito as an open id connect provider and execute client credentials flow
* Get CUP token
* Use Bearer token to API

Workshop - Serverless Authentication and Authorization Workshop

Demo: dynamic break glass access

Python Chalice Framework