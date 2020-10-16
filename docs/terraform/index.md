# Terraform

## Essentials

* Terraform is an OpenSource Tool by HashiCorp
* A huge active community makes sure that support for new cloud provider features is added very fast
* Its DSL called HCL (HashiCorp Configuration Language) is easy to learn
* There is support for all public cloud providers through providers
* It offers separate PLAN and APPLY phases
* Complex concepts can be encapsulated into modules shared as GIT repositories
* No Fuzz approach: comes as command line tool only

## Commands (excerpt)

| Command | Description |
| --- | --- |
| init | initialises a Terraform directory |
| validate | validates all Terraform files in a directory |
| plan | generates and displays an execution plan |
| apply  | creates new infrastructure or modifies existing infrastructure according to the execution plan |
| destroy | destroys all infrastructure managed by Terraform |

## Scripts, Variables and Outputs

All Terraform scripts are written in HashiCorp Configuration Language (HCL) stored as files with extension `*.tf`.

By convention, each Terraform directory contains a least three files:

* `main.tf` main entrypoint of all Terraform scripts in this directory that usually contains the actual infrastructure specifications.
* `variables.tf` declaration of all variables or input parameters that can be passed to main.tf.
* `outputs.tf` declaration of all outputs values or output parameters that Terraform prints to the console.

If the number of resources managed by Terraform increases, `main.tf` is usually split into separate `*.tf` files. 

## Modules

Common infrastructure patterns can be encapsulated in and shared through Terraform `modules`.

A module is a separate Terraform folder with its own main.tf, variables.tf and output.tf files. This separate folder can be referenced
from a consumer script using the `module` statement.

Terraform modules may be shared via:

* Filesystem
* GitHub or any shared git repository
* S3
* Webservers
* Terraform Registry

## State Management

When you run Terraform `apply` all meta data about infrastructure managed by Terraform will be stored as Terraform state.
By default, the state will be stored in the filesystem of your local machine. In order to share this state with your
team members, you can use a remote backend to store the state.

Terraform supports various remote backends for state management. On AWS, the most common one will be a S3 bucket 
plus a DynamoDB table for locking.  
  
## Resources 

[Offical homepage of the Terraform tool](https://www.terraform.io) 