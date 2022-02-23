# Amazon EKS-specific features, requirements and conventions

Although any AWS EKS cluster feels like a normal Kubernetes cluster, 
there are some EKS-specific features, requirements and conventions worth mentioning.

## VPC considerations and requirements

* Use a dedicated VPC for each AWS EKS cluster.
* Each VPC hosting node groups for an AWS EKS cluster should provide public and private subnets. 
The public subnets will host load balancers (either managed by EKS or your own), 
the private subnets will host the EC2 instances representing your worker nodes.
* Each subnet hosting worker nodes should have sufficient amount of IP addresses available.
* Before EKS v1.19, VPC's and subnets had to be tagged according to a specific pattern. 

!!! warning "NAT Gateway is required for private worker nodes"
    If you place your worker nodes into a private subnet AND your worker nodes need to access the
    internet, you must add a NAT gateway to your VPC and add a corresponding route to the route tables
    of your private subnets. Please remember, that your worker nodes will need access to the internet
    to join the cluster, if you don't apply specific configuration steps as mentioned in 
    [Private Clusters](https://docs.aws.amazon.com/eks/latest/userguide/private-clusters.html).

@see: [Creating a VPC for your Amazon EKS cluster](https://docs.aws.amazon.com/eks/latest/userguide/creating-a-vpc.html)

@see: [Cluster VPC and subnet considerations](https://docs.aws.amazon.com/eks/latest/userguide/network_reqs.html)

### VPC with public subnets only

In this topology scenario, all worker nodes reside in public subnets. The worker nodes can access the control plane,
the internet and all other AWS services directly.

!!! danger "Do not use this scenario for production environments" 
    Exposing your worker nodes with public IP addresses to the outside world is not a good idea
    for production environments.

### VPC with private and public subnets

In this topology scenario, all worker nodes reside in private subnets. 
The public subnets are restricted to application load balancers and NAT gateways in front of the worker nodes.
Since the worker nodes are hosted in private subnets, they need a NAT gateway to connect to the control plane, 
other AWS services or the internet.

!!! info "Worker nodes in private subnets can join the EKS cluster without a NAT gateway"
    If you want to attach your worker nodes to your EKS cluster without going through a NAT gateway, you
    can do so by applying the same special configuration steps as mentioned in [VPC with private subnets only](#vpc-with-private-subnets-only)

!!! tip "Prefer private VPC Interface Endpoints to access AWS services"
    If you need to access other AWS services from your worker nodes it's always a good idea
    to keep this access private through VPC Interface Endpoints via AWS Private Link. Thus,
    you do not need to route traffic from private worker nodes to the AWS services through a NAT Gateway.  

### VPC with private subnets only

In this topology scenario, all worker nodes reside in private subnets and the VPC does not provide any public subnets at all.
This scenario offers a good protection of your worker nodes on network level but comes with a lot of restrictions:

* Each docker image supposed to be downloaded into the cluster must be copied to an Amazon Elastic Container Registry (ECR),
which needs to be accessible through private VPC Interface Endpoints.
* Each AWS service required by the workload running on the cluster must be accessible through private VPC Interface Endpoints.
* Of course, this includes the control plane part of the AWS EKS cluster as well.
* The bootstrap arguments of the worker nodes must be adapted in order to let the worker nodes join the EKS cluster. 

## Kubernetes networking through Amazon VPC Container Network Interface (CNI)

Amazon EKS comes by default with the `Amazon VPC Container Network Interface (CNI)` activated.
The Amazon CNI plugin is an AWS-specific Kubernetes-compliant network plugin provided by AWS
as Open Source, which seamlessly integrates Kubernetes networking into the AWS VPC networking:

* Each pod gets it's own VPC IP address from the VPC address space. Thus, it is directly accessible
inside the VPC via it's assigned VPC IP address.

This support of native VPC IP addresses for pods has some implications on the VPC which hosts the worker nodes 
and the network capabilities of the worker node itself:

* The CIDR block of the subnet hosting a worker node must be big enough to provide IP addresses for all pods running inside it.
* The instance type of the EC2 instance representing a worker node must support enough IP addresses for all pods running on it.

!!! tip "Best practices for VPCs hosting EKS clusters"
    Always assign big CIDR ranges from the beginning to the VPC and the subnets hosting your worker nodes:

    * VPCs hosting EKS worker nodes should start with a `/16` CIDR block (i.e. ~65000 IP addresses).
    * Subnets hosting EKS worker nodes should start with a `/20` CIDR block (i.e. ~4096 IP addresses).


!!! tip "Best practices for EC2 instances representing EKS worker nodes"
    Always use instance types for EC2 instances which provided a sufficient amount of IP addresses:

    * The network interface of your EC2 instance type should support about `30` assignable IP addresses.

@see: [Pod networking (CNI)](https://docs.aws.amazon.com/eks/latest/userguide/pod-networking.html)

If this behaviour of the Amazon CNI plugin conflicts with your requirements regarding Kubernetes networking, Amazon EKS
can be switched to the following compatible CNI plugins (although they are not officially supported by AWS!):

| Vendor | Product |
| --- | --- |
| Tigera | [Calico](https://www.tigera.io/project-calico/) | 
| Isovalent | [Cilium](https://cilium.io/) | 
| Weaveworks | [Weave Net](https://www.weave.works/oss/net/) |
| VMWare | [Antrea](https://antrea.io/) | 

!!! tip "Always stick to network plugins supported by many Cloud Service Providers"
    When you cannot stick to the Kubernetes network plugin provided by a specific cloud service provider, you always
    should choose an alternative which is supported by a wide range of competitors. `Calico` for example is supported by
    AWS and Azure. Remember: Kubernetes is a cloud-agnostic platform!

!!! info "Choice of network plugins is fully transparent to Kubernetes workloads"
    Which network plugin you choose is completely unnoticed by the workload you run on your Kubernetes cluster.

## AWS Load Balancer Controller as a Kubernetes ingress controller

The `AWS Load Balancer Controller` add-on is a Kubernetes compliant ingress controller provided by AWS as Open Source
which can be used as a replacement or extension for standard ingress controllers like `NGiNX` or `Traefik`.

This specific ingress controller automatically:

* Creates or reuses an `AWS Application Load Balancer` in front of your EKS cluster for each Kubernetes Ingress you deploy with your application. Each Ingress rule is mapped to a rule inside the application load balancer.
* Creates or reuses an `AWS Network Load Balancer` in front of your EKS cluster for each Kubernetes Service of type `LoadBalancer` you deploy with your application. 

@see: [Installing the AWS Load Balancer Controller add-on](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html)

## Extended Kubernetes Cluster Authentication with IAM

Amazon EKS uses the Amazon IAM service to identify and authenticate identities which want to access the Kubernetes API of your EKS cluster (like a Kubernetes admin running `kubectl` on a local machine).

Thus, kubeconfig files referring to an AWS EKS cluster contain the following AWS CLI command `aws eks get-token` instead of username and credentials:

```yaml
users:
- name: arn:aws:eks:eu-west-1:************:cluster/***************************************
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      args:
      - --region
      - eu-west-1
      - eks
      - get-token
      - --cluster-name
      - my-eks-cluster
      command: aws
      env:
      - name: AWS_PROFILE
        value: my-aws-profile
```

The authorization part of the request to the Kubernetes API is handled as usual by the Kubernetes RBAC system.

@see: [Cluster authentication](https://docs.aws.amazon.com/eks/latest/userguide/cluster-auth.html)

## Optional Kubernetes Cluster Authentication with OpenID Connect

Amazon EKS supports using OpenID Connect providers as an authentication method on your EKS cluster. This authentication 
method may replace or may be used with the default IAM authentication method.

@see: [Authenticating users for your cluster from an OpenID Connect identity provider](https://docs.aws.amazon.com/eks/latest/userguide/authenticate-oidc-identity-provider.html)

## Cluster Autoscaling with the Kubernetes Cluster Autoscaler

The Kubernetes Cluster Autoscaler is an essential add-on which allows your EKS cluster automatically adapt to 
changes in the workload running on your EKS cluster:

* If the workload increases and exceeds the capacity of your existing worker nodes, new worker nodes are automatically added to the cluster.
* If the workload decreases and can be handled by less worker nodes, superfluous worker nodes are automatically removed from the cluster. 

@see: [Autoscaling](https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html)

## Keep one worker node group per Availability Zone

Technically, an Autoscaling Group or a Managed Node Group attached to an EKS cluster may span more than one availability zone.
In most cases, worker node storage or Kubernetes Persistent Volumes are backed by EBS volumes attached to EC2 instances.
Unfortunately, EBS volumes are bound to the availability zone they were originally created in. So if a worker node
crashes, EKS has to make sure to resurrect it in the same availability zone it was originally created in to be able to retain the data on the attached EBS volumes. 
Otherwise, all storage attached to the EC2 instance would be lost including all data.

Thus, you have to make sure to restrict each worker node group to a specific availability zone. 
Since EKS can handle multiple worker node groups, you simply create one worker node group for each availability zone you like your cluster to span.
Just activate the `--balance-similar-node-groups` feature during EKS startup and the control plane will make sure that crashed EC2 instances are always
restarted in the same availability zone they were originally started in without losing any data.