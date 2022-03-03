# Amazon EKS Best Practices and Security Baselines

The AWS EKS security baselines mentioned here should be applied to any AWS EKS cluster running in an production environment.
All baselines are only briefly listed. For further information, please refer to the linked documentation.

!!! tip "Best Practices and Security Baselines should be coded"
    `Infrastructure-as-Code` tools which support modularization are an excellent way to ensure
    that all your AWS EKS clusters are following best practices and are compliant to security
    baselines.

## Amazon EKS Best Practices Guide for Security

AWS provides a detailed best practices guide for AWS EKS which is a MUST READ for everyone who wants
to provision enterprise-grade Kubernetes clusters based on AWS EKS.

@see: [Amazon EKS Best Practices Guide for Security](https://aws.github.io/aws-eks-best-practices/security/docs/)

## CIS Amazon Elastic Kubernetes Service (EKS) Benchmark

This overview of the CIS benchmark refers to 
the `CIS Amazon Elastic Kubernetes Service (EKS) Benchmark v1.0.1 - 08-31-2020`.

@see [CIS Benchmark Homepage](https://www.cisecurity.org/cis-benchmarks/)

### Control Plane

#### Logging

* Enable audit Logs (Manual)

### Worker Nodes

#### Worker Node Configuration Files

* Ensure that the kubeconfig file permissions are set to 644 or more restrictive (Manual)
* Ensure that the kubelet kubeconfig file ownership is set to root:root (Manual)
* Ensure that the kubelet configuration file has permissions set to 644 or more restrictive (Manual)
* Ensure that the kubelet configuration file ownership is set to root:root (Manual)

#### Kubelet

* Ensure that the --anonymous-auth argument is set to false (Automated)
* Ensure that the --authorization-mode argument is not set to AlwaysAllow (Automated)
* Ensure that the --client-ca-file argument is set as appropriate (Manual)
* Ensure that the --read-only-port is secured (Manual)
* Ensure that the --streaming-connection-idle-timeout argument is not set to 0 (Manual)
* Ensure that the --protect-kernel-defaults argument is set to true (Automated)
* Ensure that the --make-iptables-util-chains argument is set to true (Automated)
* Ensure that the --hostname-override argument is not set (Manual)
* Ensure that the --eventRecordQPS argument is set to 0 or a level which ensures appropriate event capture (Automated)
* Ensure that the --rotate-certificates argument is not set to false (Manual)
* Ensure that the RotateKubeletServerCertificate argument is set to true (Manual)

### Policies

#### RBAC and Service Accounts

* Ensure that the cluster-admin role is only used where required (Manual)
* Minimize access to secrets (Manual)
* Minimize wildcard use in Roles and ClusterRoles (Manual)
* Minimize access to create pods (Manual)
* Ensure that default service accounts are not actively used. (Manual)
* Ensure that Service Account Tokens are only mounted where necessary (Manual)

#### Pod Security Policies
 
* Minimize the admission of privileged containers (Automated)
* Minimize the admission of containers wishing to share the host process ID namespace (Automated)
* Minimize the admission of containers wishing to share the host IPC namespace (Automated)
* Minimize the admission of containers wishing to share the host network namespace (Automated)
* Minimize the admission of containers with allowPrivilegeEscalation (Automated)
* Minimize the admission of root containers (Automated)
* Minimize the admission of containers with the NET_RAW capability (Automated)
* Minimize the admission of containers with added capabilities (Automated)
* Minimize the admission of containers with capabilities assigned (Manual)

#### CNI Plugin

* Ensure latest CNI version is used (Manual)
* Ensure that all Namespaces have Network Policies defined (Automated)

#### Secrets Management

* Prefer using secrets as files over secrets as environment variables (Manual)
* Consider external secret storage (Manual)

#### Extensible Admission Control

* Configure Image Provenance using ImagePolicyWebhook admission controller (Manual)

#### General Policies

* Create administrative boundaries between resources using namespaces (Manual)
* Apply Security Context to Your Pods and Containers (Manual)
* The default namespace should not be used (Automated)

### Managed Services

#### Image Registry and Image Scanning

* Ensure Image Vulnerability Scanning using Amazon ECR image scanning or a third party provider (Manual)
* Minimize user access to Amazon ECR (Manual)
* Minimize cluster access to read-only for Amazon ECR (Manual)
* Minimize Container Registries to only those approved (Manual)

#### Identity and Access Management

* Prefer using dedicated EKS Service Accounts (Manual)

#### AWS Key Management Service (KMS)

* Ensure Kubernetes Secrets are encrypted using Customer Master Keys (CMKs) managed in AWS KMS (Automated)

#### Cluster Networking 

* Restrict Access to the Control Plane Endpoint (Manual)
* Ensure clusters are created with Private Endpoint Enabled and Public Access Disabled (Manual)
* Ensure clusters are created with Private Nodes (Manual)
* Ensure Network Policy is Enabled and set as appropriate (Manual)
* Encrypt traffic to HTTPS load balancers with TLS certificates (Manual)

#### Authentication and Authorization

* Manage Kubernetes RBAC users with AWS IAM Authenticator for Kubernetes (Manual)

#### Other Cluster Configurations

* Consider Fargate for running untrusted workloads (Manual)

