# User Account Basics

This page provides you with some basic information about user and permission management in AWS.

## AWS Identity and Access Management (IAM)

[AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
is a web service provided by AWS which allows you to manage users, groups, roles and their permissions within an AWS account. 

## Users, Groups, Roles and Policies

### User
An __user__ represents an entity of IAM with certain permissions.

### Policy

A __policy__ represents a certain set of permissions on a particular AWS resource which may be attached to users, groups or roles. 

In AWS, policies are defined through JSON documents. AWS provides pre-built policies for common use cases through 
__managed policies__. 

### Role

A __role__ represents a set of policies which can be attached to users, services or virtual machines (EC2). Being able 
to link roles to services or virtual machines allows you to control the permissions of services and virtual machines 
without the need to use user IDs and credentials.

### Group

A __group__ represents a set of policies which should be attached to specific group of users.
 
## AWS IAM User Account Types

[IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) supports two types of users:

* __Console users__ which represent human users with access to the AWS web console.
* __Programmatic users__ which represent technical users with access to the AWS CLI.

### Console User

Console users are identified by their __user ID__ and authenticated through their __password__ (plus - in our case - 
with multi-factor authentication __MFA__).

### Programmatic User

Programmatic users are identified by their __access key__ and authenticated through their __secret key__. 

!!! tip "Always keep console user accounts and programmatic user accounts separate"
    Although console users are able to create their own access key/secret key pair it is recommended to use an additional
    programmatic user account for each console user account: each console user should be required to use secure authentication 
    with MFA. Unfortunately, this requirement is inherited to all access keys the console user may add to his personal user account.
    Have you ever tried to make MFA work when accessing AWS APIs programmatically with an access key? So always add the 
    requirement to use MFA to your console user account but never to your programmatic user account. 

## User Account Management

Both user accounts - console user account and programmatic user account - are provided to you by your AWS account admin.
Details about your accounts will be given through an invitation email including:

* AWS login URL
* Console user ID
* Programmatic user access key

Credentials like passwords and secret keys will be handed over through a different channel (probably Teams chat!).

Details about how-to setup your user accounts can be found in:

* []()  

## Password Policies

Password policies allow you to control the quality of the passwords in your AWS account.

In the Cloud Expert Program, the following password policies apply:

* Minimum password length is 10 characters
* Require at least one uppercase letter from Latin alphabet (A-Z)
* Require at least one lowercase letter from Latin alphabet (a-Z)
* Require at least one number
* Require at least one non-alphanumeric character (!@#$%^&*()_+-=[]{}|')
* Password expires in 90 day(s)
* Allow users to change their own password
* Remember last 5 password(s) and prevent reuse

!!! caution "Use password length of 20 chars"
    Although, a minimum password length of 10 chars is enforced, a password length of 20 chars is strongly recommended!
     
!!! tip "Use KeePass to manage your credentials"
    Use tools like KeePass (officially available through Matrix) to manage your secure passwords!
 
