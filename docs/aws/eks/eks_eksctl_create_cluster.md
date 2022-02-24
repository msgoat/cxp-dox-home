# HOW-TO create an EKS cluster with eksctl

## Motivation

We want to create a simple AWS EKS cluster including a VPC to host it with public worker nodes
controlled by AKS-managed node groups. The AWS EKS cluster is supposed to span 3 availability zones
with one managed node-group per zone.

## Step 1: create a cluster configuration file

* create a file named `eks-${region}-${clusterName}.yaml` with the following contents:

``` yaml linenums="1"
# Simplest possible EKS cluster
---
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: eks-${region}-${clusterName}
  region: ${region}

# AKS-managed node groups
managedNodeGroups:
  - name: mng-${region}a-${clusterName}
    instanceType: m5.large
    minSize: 1
    desiredCapacity: 1
    maxSize: 4
    availabilityZones: ["${region}a"]
  - name: mng-${region}b-${clusterName}
    instanceType: m5.large
    minSize: 1
    desiredCapacity: 1
    maxSize: 4
    availabilityZones: ["${region}b"]
  - name: mng-${region}c-${clusterName}
    instanceType: m5.large
    minSize: 1
    desiredCapacity: 1
    maxSize: 4
    availabilityZones: ["${region}c"]

cloudWatch:
  clusterLogging:
    # enable specific types of cluster control plane logs
    enableTypes: ["audit", "authenticator", "controllerManager"]
    # all supported types: "api", "audit", "authenticator", "controllerManager", "scheduler"
    # supported special values: "*" and "all"
```

* replace `${region}` with name of the AWS region you would like to use (e.g. `eu-central-1`)

* replace `${clusterName}` with a meaningful cluster name.
 
This configuration file defines:

* an AWS EKS cluster named `eks-${region}-${clusterName}`
* with a VPC with public subnets and private subnets spanning all availability zones
* with one AKS-managed nodegroup called `mng-${region}[a|b|c]-${clusterName}` per availability zone; each with 1 EC2 worker node of instance type `m5.large`
running in the public subnets
* with CloudWatch cluster logging enabled for types `audit`, `authenticator` and `controllerManager`

## Step 2: create the EKS cluster

In order to create the AWS EKS cluster simply run command: 

``` shell
eksctl create cluster -f eks-${region}-${clusterName}.yaml 
```

After about 15 minutes your cluster should be up and running!