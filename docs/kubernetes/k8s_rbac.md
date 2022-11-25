# Role Based Access Control (RBAC) on Kubernetes

Like many other systems, Kubernetes uses role-based access control to regulate access control
to the Kubernetes cluster and the Kubernetes API.

This RBAC authorization system is based on these Kubernetes objects/resources:

* `ServiceAccount` defines a subject roles can be bound to.
* `Role` defines a set of policies which a subject is allowed to perform within a specific namespace
* `RoleBinding` attaches a `Role` to a `ServiceAccount` for a specific namespace
* `ClusterRole` is similiar to `Role` but it is not restricted to a specific namespace
* `ClusterRoleBinding` attaches a `ClusterRole` to a `ServiceAccount`

@see [Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

## ServiceAccount

A `service account` represents an identity for processes running in pods.
All processes automatically inherit the roles or cluster roles which are bound to this service account.
All access to the Kubernetes API from a pod running in the context of a service account will be checked against
the granted policies before it is actually allowed.

Service accounts are namespace scopes objects. Each namespace has a default service account called `default` (well that was easy!).
If you do not specify a service account in your pod template, the pod will automatically run in the context
of this default service account.

!!! info "Other subjects supported by Kubernetes"
    Besides `service accounts`, Kubernetes supports `user`s and `group`s as well.  

@see [Configure Service Accounts for Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

@see [ServiceAccount Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#serviceaccount-v1-core)

## Role

A `role` defines a set of policies for a specific namespace.

@see [Role Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#role-v1-rbac-authorization-k8s-io)

## ClusterRole

A `cluster role` defines a set of policies for Kubernetes cluster.

@see [ClusterRole Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#clusterrole-v1-rbac-authorization-k8s-io)

## RoleBinding

A `role binding` attaches a specific [role](#role) to a specific subject like a [service account](#serviceaccount).

@see [RoleBinding Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#rolebinding-v1-rbac-authorization-k8s-io)

## ClusterRoleBinding

A `cluster role binding` attaches a specific [cluster role](#clusterrole) to a specific subject like a [service account](#serviceaccount).

@see [ClusterRoleBinding Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#clusterrolebinding-v1-rbac-authorization-k8s-io)
