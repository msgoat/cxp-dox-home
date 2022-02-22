# Amazon EKS-specific features, requirements and conventions

Although any AWS EKS cluster feels like a normal Kubernetes cluster, 
there are some EKS-specific features, requirements and conventions worth mentioning.

## VPC considerations and requirements

* Use a dedicated VPC for each AWS EKS cluster.
* Each VPC hosting node groups for an AWS EKS cluster should provide public and private subnets. 
The public subnets will host load balancers (either managed by EKS or your own), 
the private subnets will host the EC2 instances representing your worker nodes.
* Each subnet hosting worker nodes should have sufficient amount of IP addresses available.

TO BE CONTINUED...