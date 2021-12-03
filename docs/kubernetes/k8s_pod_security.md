# Enforcing Pod Security on Kubernetes

## PodSecurityContext

A `pod security context` as a part of the pod template specification defines the privilege and access control settings 
of a pod or of a specific container inside the pod.

These security control settings cover:

* Container is / is not allowed to run in privileged mode
* Security context which all processes are running in based on user ID (UID) or group ID (GID)
* [Linux Capabilities](https://linux-audit.com/linux-capabilities-hardening-linux-binaries-by-removing-setuid/) based privileges
* [SELinux](https://en.wikipedia.org/wiki/Security-Enhanced_Linux) labels
* [AppArmor](https://kubernetes.io/docs/tutorials/clusters/apparmor/) profiles
* [Seccomp](https://en.wikipedia.org/wiki/Seccomp) filters
* Container is / is not allowed to gain more privileges than its parent process
* Container mounts root filesystem as read-only

Some of these security context setting can be defined on pod level, some on container level.

@see [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

@see [PodSecurityContext Fragment Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#podsecuritycontext-v1-core)

@see [SecurityContext Fragment Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#securitycontext-v1-core)

__Example: Deployment Manifest with Pod Security Context__

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cxp-hello-k8s
  labels:
    app.kubernetes.io/name: cxp-hello-k8s
    app.kubernetes.io/instance: cxp-hello-k8s
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: cxp-hello-k8s
      app.kubernetes.io/instance: cxp-hello-k8s
  template:
    metadata:
      labels:
        app.kubernetes.io/name: cxp-hello-k8s
        app.kubernetes.io/instance: cxp-hello-k8s
    spec:
      securityContext:
        runAsUser: 1000
        runAsGroup: 1000
        fsGroup: 1000
      containers:
        - name: cxp-hello-k8s
          image: "docker.at41tools.k8s.aws.msgoat.eu/cxp/cxp-hello-k8s:1.0.0"
          imagePullPolicy: IfNotPresent
          # [..]
          securityContext:
            allowPrivilegeEscalation: false
```

* `.spec.template.spec.securityContext.runAsUser`: 
sets the user ID of the user which runs all containers in the pod (this user must be present in the Docker image)
* `.spec.template.spec.securityContext.runAsGroup`: 
sets the group ID of the group which runs all containers in the pod (this group must be present in the Docker image)
* `.spec.template.spec.securityContext.fsGroup`: 
sets the special supplemental group which defines the ownership of mounted file systems
* `.spec.template.spec.containers.*.securityContext.allowPrivilegeEscalation`: 
prevents the process inside the container from gaining more privileges (no sudo!)

!!! tip "Always enforce non-root users and disallow privilege escalation"
    You should always apply the minimum set of security settings mentioned in the previous example:
    
    * Always run as non-root user (`runAsNonRoot` == true) 
    * Always specify a non-root user as `runAsUser`
    * Always specify a specific group as `runAsGroup` (if not set actual group will be 0!)
    * Always set `allowPrivilegeEscalation` to false
    
    Remember: most enterprise-grade Kubernetes clusters will enforce these minimum settings anyway!

## PodSecurityPolicy

A `pod security policy` (PSP) enforces a set of security policies for pods on a cluster level. 
Kubernetes will refuse to run any pod which violates any of these policies.

Activating pod security policies requires special authorization of the pod which wants to use them.
This authorization is granted through a role which is attached to the service account which runs the pod.

!!! info 
    Since pod security policies are a cluster-level resource they are mostly managed by cluster administrators.
    In many enterprise-grade Kubernetes cluster you will not be allowed to create your own PSP.

@see [Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)

@see [PodSecurityPolicy Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#podsecuritypolicy-v1beta1-policy)