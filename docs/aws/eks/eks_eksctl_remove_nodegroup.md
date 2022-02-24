# HOW-TO remove node groups from an EKS cluster with eksctl

## Motivation

We want to remove a node group from an AWS EKS cluster.

## Step 1: delete an EKS node group

In order to remove a node group from an AWS EKS cluster and to delete it simply run command:

``` shell
eksctl delete nodegroup --cluster eks-${region}-${clusterName}.yaml --name ${nodeGroupName} 
```

!!! danger "Explicitly drain node groups before deleting them"
    Although eksctl drains a node group before deleting it, it's a good idea to do so explicitly before
    running `delete` on a node group.

## Step 2: update your cluster configuration file

* remove the node group element referring to the deleted node group from your cluster configuration file.
