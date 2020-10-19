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