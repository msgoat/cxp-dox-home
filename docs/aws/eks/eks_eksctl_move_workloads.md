# HOW-TO move workloads between node groups with eksctl

## Motivation

We want to move workload from one node group to another to be able to remove a node group
from an AWS EKS cluster without having any downtime of applications running on the cluster.
Actually we cannot __move__ pods from one node group to another node group, but we can __drain__
node groups. Draining means that all worker nodes of the node group are marked for eviction. 
Kubernetes will start to move pods running on these marked worker nodes to other available worker nodes.
After all worker nodes of a node group are drained, the node group can be safely removed from the cluster 
without causing any outages.

## Step 1: drain a node group

In order to drain a node group run command:

``` shell
eksctl drain nodegroup --cluster eks-${region}-${clusterName} --name mng-${region}a-${clusterName}
```

