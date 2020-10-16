# Infrastructure as Code

## Motivation

__Fully automated provisioning of Cloud Infrastructure is mandatory__

Manual provisioning of cloud infrastructure is cumbersome, error-prone and (in most cases) not reproducible.

The manual setup of a reference network on AWS using the web console takes approx. 1 hour!!!

So what are you going to do if you want to setup and teardown your infrastructure let‘s say each day?

## Infrastructure-as-Code Tool Requirements

* Setting up and changing infrastructure is reproducible
* Non-destructive changes on existing infrastructure
* Meta-information about managed infrastructure (a.k.a. state) can be shared 
* Dry-Run support: changes can be reviewed before they are actually applied
* Support for multiple cloud infrastructure providers

## Shortlist of popular tools

* AWS CloudFormation
* AWS CDK
* Azure Resource Manager Templates
* Hashicorp Terraform
* Hashicorp Packer
* RedHat Ansible
