# Getting started with Kubernetes

This article will provide you with some basics about Kubernetes.

## What is Kubernetes?

Kubernetes is a portable, extensible, open-source platform for managing containerized workloads and services,
that facilitates both declarative configuration and automation.
It has a large, rapidly growing ecosystem.
Kubernetes services, support, and tools are widely available.

The name `Kubernetes` originates from Greek, meaning helmsman or pilot.
Google open-sourced the Kubernetes project in 2014.
Kubernetes combines over 15 years of Google’s experience running production workloads at scale with best-of-breed
ideas and practices from the community.

Kubernetes runs on almost any platform: from a single virtual machine to a highly available large cluster with up to 5000 nodes
hosted by a public cloud provider like AWS or Azure.

Kubernetes can run anything that works in a docker container for you.

## Kubernetes key features

### Automatic container placement aimed at optimal resource utilization

Kubernetes automatically runs your container on a cluster of worker nodes always aiming at the optimal resource
utilization possible.

### Horizontal scaling

Kubernetes supports horizontal scaling on two levels:

* Node level: if the current workload on the cluster exceeds the capacity of the existing nodes, Kubernetes is able to
add more nodes to the cluster (if the underlying infrastructure supports that).

* Container level: if the workload on the currently running containers exceeds specific thresholds (like CPU or memory consumption),
Kubernetes will start more containers.

### Zero downtime deployments through rolling updates and rollbacks

Since Kubernetes uses rolling updates by default, new versions of your application can be rolled out on a cluster without
causing any visible downtime for your customers.

### Storage orchestration

Kubernetes comes with a very powerful storage abstraction which allows you to bind arbitrary persistent storage to your
containers: anything is possible from locally attached SSDs to remotely attached block devices or high-performance file
systems.

### Self-healing containers

If your container dies, Kubernetes will automatically resurrect it for you.

### Service discovery and load balancing

Any application deployed on Kubernetes will get a unique service name which can be used to address this particular application.
Traffic routed to the service will automatically load-balanced among the container instances which will handle the incoming requests.

### Secret and configuration management

Through the concept of secrets, Kubernetes can handle sensitive credentials for you. Configuration data can be
stored as config maps on a Kubernetes cluster and attached to containers upon container start.

### Batch execution

Besides running applications, Kubernetes support one-time jobs and scheduled cron jobs as well.

### Multi-tenancy through namespaces

Multiple tenants may use the same Kubernetes cluster without interfering with each other through the concept of namespaces.
Each namespaces feels like a dedicated virtual cluster to its user including predefined resource quotas like number of CPU cores,
amount of memory and amount of storage.

### Federation

A group of geographically spread Kubernetes clusters can be managed as one logical cluster using the federation feature.

## Popular Kubernetes distributions

* [Amazon EKS](https://aws.amazon.com/eks/): managed Kubernetes-as-a-Service on AWS
* [Azure Kubernetes Service](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes): managed Kubernetes-as-a-Service on Azure
* [kops](https://github.com/kubernetes/kops): production-ready Kubernetes clusters on AWS
* [kubespray](https://github.com/kubernetes-sigs/kubespray): production-ready Kubernetes clusters with Ansible on various platforms
* [minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/): single-node Kubernetes cluster for developers
* [MicroK8s](https://microk8s.io/): Single node Kubernetes done right for developers


## References

| Link | Description |
| --- | --- |
| [Getting started](https://kubernetes.io/docs/setup/) | Official Kubernetes documentation |

