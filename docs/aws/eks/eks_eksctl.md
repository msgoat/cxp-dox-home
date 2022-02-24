# Hands-On eksctl

`eksctl` is a good way to start managing AWS EKS for various reasons:

* it can create a VPC to host your AWS EKS cluster as well
* it supports all flavours of worker node groups like self-managed node groups and managed node groups
* it simplifies cluster upgrades and worker node upgrades a lot
* it is easy to configure

This documentation is going to walk you through a series of use cases you will encounter when dealing with AWS EKS clusters:

* [HOW-TO create an EKS cluster with eksctl](eks_eksctl_create_cluster.md) will show you how to create an AWS EKS cluster.
* [HOW-TO add node groups to an EKS cluster with eksctl](eks_eksctl_add_nodegroup.md) will show you how to add node groups to an existing AWS EKS cluster.
* [HOW-TO move workload between node groups with eksctl](eks_eksctl_move_workloads.md) will show you how to move workload between node groups of an AWS EKS cluster.
* [HOW-TO remove node groups from an EKS cluster with eksctl](eks_eksctl_remove_nodegroup.md) will show you how to remove node groups from an existing AWS EKS cluster.
* [HOW-TO delete an EKS cluster with eksctl](eks_eksctl_delete_cluster.md) will show you how to delete an existing AWS EKS cluster.

## Prerequisites

* You need to have an AWS access key/AWS secret key pair for an AWS account.
* You need to have `AWS CLI` installed on your machine (see: [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)).
* You need to have your AWS CLI properly configured with your AWS access key and AWS secret key.
* You need to have `eksctl` installed on your local machine (see: [Installing eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html))