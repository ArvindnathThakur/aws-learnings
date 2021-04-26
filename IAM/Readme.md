# Identity and Access Management - IAM
* Allows to amanage users and their level of access to AWS console. It is universal and is not locked to a region.
* It provides:
    * Centralised control of AWS account
    * Shared access to AWS account
    * Granular Permission
    * Identity Federation
    * Multi factor Authentication
    * Temporary accessor users/devices
    * Password policies
    * Integrates with other AWS services
    * PCI DSS compliance

## Entities

There are three primary **Entities** of IAM
1. **Users**: The IAM user *represents the person or service* who uses the IAM user to interact with AWS. A primary use for IAM users is to give people the ability to sign in to the AWS Management Console for interactive tasks and to make programmatic requests to AWS services using the API or CLI. 
    > 1 User => 1 Physical person
2. **Groups**: An IAM group is a *collection of IAM users*. You can use groups to specify permissions for a collection of users, which can make those permissions easier to manage for those users.
3. **Roles**: It is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS. IAM user can assume roles to temporarily take on permissions for different tasks.
    > 1 Role => 1 Application, Used for access management within AWS resources.

> **Policies**: Are permissions that can be attached to the aforementioned IAM entities. They are JSON documents which define what the entities can and cannot do. It is advisible to foolow the `least priviledge principle` approach.

## IAM Federation
Provisions the integration of Active directory using **SAML standard** with IAM. Used for login into AWS using company's credentials.

## AWS Directory services
* Family of managed services used for connecting AWS resources to on-premise active directory.
* Allows for single sign on to domain joined ec2s using corporate credentials.
* 2 domain controllers in their own AZ
* AD Trust can be used to extend existing AD to on-premise infrastructure.

## Simple AD
* Standalone managed service
* Basic AD features
* Small <= 500; Large <=5000 users
* Easier to manage EC2 
* Linux integration to LDAP
* Can't join this to on premise

## AD Connector
* Acts as directory gateway or proxy for on-premise AD
* Helps avoid caching information in the cloud
* Can join existing EC2 instances to on premise domain

## Amazon Resource Name - ARN
Uniquely defines a resource
Syntax: `arn:partition:service:region:account_id:resource_type/resource`

## IAM Policies
Are JSON document which defines permissions. It is a list of statements.
- Any permissions which are not explicitly allowed are implicitly denied.
- If a policy explictly denies an action, that overrides any other policies.
- All the policies are aggreagated when AWS evaluates the permission.

### Identity Policy
- Are attached to user, group or role.
- Defines what identity can do.
### Resource Policies
- Attached to resources such EC2 instances, S3 buckets etc.
- Who has access to the resource
### Permission Boundaries
- Used for delegating administration to other users.
- Can be used for Users and Roles 
- It is a feature for using a managed policy to set the max permission that Identity based policy can grant to IAM entity.
- Is used for preventing priviledge escalation broad permissions.
- An action is allowed only when it is allowed by identity based policy and permission boundary.
 
 ## Resource Access Manager - RAM
 Is used for sharing the resources betwenn 2 or more accounts. Organization can have multiple accounts. Resources can be shared between those accounts using RAM.

 ## AWS Single Sign On
 - Allows for Active Directory and SAML integration
 - Use existing identities to log in to AWS and 3rd party applications.
 - Can be used for account level persmissions

## IAM Security Tools
### IAM Credentials Report
* Account level
* Lists all account's users and their statuses.

### IAM Access Advisor
* User level
* Lists the service permission granted to a user and when the services were last accessed.
* Can be used for auditing permissons
