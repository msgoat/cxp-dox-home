# HOW-TO delete an EKS cluster with eksctl

## Motivation

After we are finished working with our AWS EKS cluster we want to delete the cluster.

## Step 1: delete a cluster

In order to delete an AWS EKS cluster run command:

``` shell
eksctl delete cluster --name eks-${region}-${clusterName}
```

!!! danger "Make sure you are deleting the right cluster!"
    Since eksctl does not ask you to confirm the deletion of an AWS EKS before tearing it down, 
    make sure you got the right cluster before running the command. If you delete an AWS EKS cluster,
    the cluster will be irreversibly gone including all data stored on its persistent volumes!!!
