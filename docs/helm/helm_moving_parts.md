# Moving Parts of Helm

## Helm version 3+

![](img/helm3_moving_parts.png)

Starting with version `3`, `helm` basically works as before from a user´s point of view.
The most obvious change is that helm running on your local machine manages all `releases`
on its own: so there is no `Tiller` anymore.

All release metadata is stored as [secrets](../kubernetes/k8s_configuring_applications.md#secret) in the Kubernetes namespace the helm release is
installed to. Thus, all Helm command referring to a release, require a new command line
argument `--namespace` now.