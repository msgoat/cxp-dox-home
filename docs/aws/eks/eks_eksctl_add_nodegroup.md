# HOW-TO add node groups to an EKS cluster with eksctl

## Motivation

We want to add another AKS-managed node group to the existing AWS EKS cluster.

This is actually a pretty common use case whenever:

* you want to add bigger worker nodes to your cluster
* you want to upgrade the Kubernetes version of an AWS EKS cluster node group by node group

## Step 1: update your cluster configuration file

* add another node group at the end of the `managedNodeGroups` element of your `eks-${region}-${clusterName}.yaml` configuration file:

``` yaml linenums="1"
# AKS-managed node groups
managedNodeGroups:
# [..]
  - name: mng-${region}a-${clusterName}-blue
    instanceType: m5.large
    minSize: 1
    desiredCapacity: 1
    maxSize: 4
    availabilityZones: ["${region}a"]
# [..]
```

* replace `${region}` with name of the AWS region you would like to use (e.g. `eu-central-1`)

* replace `${clusterName}` with a meaningful cluster name.

This adds a new managed node group called `mng-${region}a-${clusterName}-blue` to the first availability zone
your cluster spans.

## Step 2: create the EKS node group

In order to create the node group and add it to the AWS EKS cluster simply run command:

``` shell
eksctl create nodegroup -f eks-${region}-${clusterName}.yaml 
```

eksctl automatically detects that you added a new node group to the configuration and will create it accordingly!
