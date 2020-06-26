# Moving Parts of Helm


## Helm version < 3

![](img/helm2_moving_parts.png)

All Helm versions before version 3 used a cluster-side component called `Tiller` to
manage Helm releases within Kubernetes clusters. Thus, `Tiller` had to be installed on
Kubernetes running `helm init` before any Helm deployment could be performed.

`Tiller` translated all commands sent by `helm` to Kubernetes API calls and stored the 
release metadata in the `etcd` database of the cluster.

`helm` uses a `chart` and the associated `values` to manage `releases` on a Kubernetes cluster.
A `release` is an installation of a `chart` on a Kubernetes cluster.
 

## Helm version 3+

![](img/helm3_moving_parts.png)

Starting with version `3`, `helm` basically works as before from a user´s point of view.
The most obvious change is that helm running on your local machine manages all `releases`
on its own: so there is no `Tiller` anymore.

All release metadata is stored as secrets in the Kubernetes namespace, the helm release is
installed to. Thus, all Helm command referring to a release, require a new command line
argument `--namespace` now.