# AWS Accounts

## What is an AWS Account?

Here's a direct quote from the __AWS Glossary__:

> __account__: 
> 
> A formal relationship with AWS that is associated with all of the following:
> 
> - The owner email address and password 
> - The control of resources created under its umbrella 
> - Payment for the AWS activity related to those resources 
> 
> The AWS account has permission to do anything and everything with all the AWS account resources. This is in contrast to a user, which is an entity contained within the account.

In other words: an AWS account is something like an AWS cloud tenant who can manage his own private AWS resources
which are by default invisible to other AWS cloud tenants.

Access to an AWS account requires a master or root user identified by an email address and a password. Since the root user 
of an AWS account can do anything within the account, the root user should only be used for initial setup which in most 
cases should be setting up an initial AWS IAM user with administrative privileges. All further
access is then delegated to this particular AWS IAM admin users and the root user is never used again.

AWS accounts of a particular company may be managed with a single AWS organization which enables AWS to send one consolidated 
bill for all AWS accounts (which tremendously simplifies accounting!)

!!! note "Contact msg Automotive Technology if you need an AWS account"
    Creating a new AWS account is easy: just give your email address and your credit card information and you are done!
    Getting the invoices through msg Accounting on the other hand is HARD! So if you think to create your own AWS
    account, please contact __msg Automotive Technology__ first!
     
## AWS Accounts vs AWS IAM Users

Although AWS accounts and AWS IAM users have many things in common (like user ID and password), they are completely 
different from a conceptual point of view:

* an __AWS account__ represents a tenant in the AWS cloud who always has full access to all resource linked to the AWS account.
* an __AWS IAM user__ represents a single user within the AWS account who needs specific policies, roles and groups to access resources within the AWS account. 
## References

[IAM Best Practices "Lock Away Your AWS Account Root User Access Keys"](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials) 

