# Managing Helm Releases

This guide will walk you through a complete Helm release lifecycle:
 
* you will start with a new chart from scratch
* you will install the chart as a release
* you will upgrade the release after modifying your chart
* you will uninstall the release 

## Create a new Helm chart

Helm supports scaffolding: simply run `helm create ${chartName}` to create a prototype
chart:

```shell 
helm create cxp-hello-helm
```

This command will create a new chart directory named `cxp-hello-helm` with the following 
contents:

```
cxp-hello-helm 
|- charts
|- templates 
|  |- test 
|  |  |- test-connection.yaml
|  | _helpers.tpl 
|  | deployment.yaml 
|  | hpa.yaml 
|  | ingress.yaml 
|  | NOTES.txt 
|  | service.yaml 
|  | serviceaccount.yaml
| .helmignore
| Chart.yaml
| values.yaml
```

* `charts`: directory for sub-charts your chart depends on 
* `templates/test`: directory with test templates 
* `templates/_helpers.tpl`: helper templates defining reusable templates included in chart YAMLs
* `templates/deployment.yaml`: template YAML for a deployment manifest
* `templates/hpa.yaml`: template YAML for a horizontal pod autoscaler manifest
* `templates/ingress.yaml`: template YAML for an ingress manifest
* `templates/NOTES.txt`: chart documentation 
* `templates/service.yaml`: template YAML for a service manifest
* `templates/serviceaccount.yaml`: template YAML for a serviceaccount manifest
* `.helmignore`: list of file to be excluded from Helm templating
* `Chart.yaml`: chart descriptor with metadata about the chart
* `values.yaml`: placeholder values for templating

## Update a Helm chart

Now it's your turn:

* to add missing templates for additional Kubernetes resources you might need
* to add missing information to Chart.yaml (see: [The Chart YAML file](https://helm.sh/docs/topics/charts/#the-chartyaml-file))
* to add missing values to values.yaml

!!! tip "Versioning your Helm chart"
    The chart descriptor file `Chart.yaml` contains two versions: 
    
    * the chart version (element `version`) which is related to the chart itself
    * the application version (element `appVersion`) which is related to the application you are deploying with this chart
    
    Both should be using semantic versioning. 
    The chart version only changes if you modify the chart itself, not the application that it deploys. 
    The application version should be kept in synch with the Maven or Gradle project version that builds the application.
 
## Verify a Helm chart

Before you actually start deploying your new or updated Helm chart, you should verify its contents running `helm lint` on your charts folder:

```shell
helm lint cxp-hello-helm
```

## Install a Helm chart as a new release

To install a Helm chart as a new release, you have to run `helm install`:

```shell
helm install cxp-hello-helm cxp-hello-helm --namespace cxp-team-miket92 --debug --atomic
```

Here's a list of useful command line arguments:

* `--namespace ${namespaceName}`: specifies the target namespace on a Kubernetes cluster; required starting from Helm v3
* `--debug`: displays the rendered Kubernetes manifests used to install your release (useful to troubleshooting!)
* `--atomic`: waits for all pods to become ready within a certain period of time (5 minutes by default); automatically rolls back on timeout
* `--dry-run`: simulates the install without actually performing it

For a complete list of command line arguments please run `helm install --help`.

## Upgrade an existing Helm release with a modified Helm chart

To upgrade an existing release after you modified your chart, you have to run `helm upgrade`:

```shell
helm upgrade cxp-hello-helm cxp-hello-helm --namespace cxp-team-miket92 --debug --atomic
```

Helm will only perform an incremental upgrade of your existing release: only the missing or modified parts are updated; all unchanged parts will not be touched.

Here's a list of useful command line arguments:

* `--namespace ${namespaceName}`: specifies the target namespace on a Kubernetes cluster; required starting from Helm v3
* `--debug`: displays the rendered Kubernetes manifests used to upgrade your release (useful to troubleshooting!)
* `--atomic`: waits for all pods to become ready within a certain period of time (5 minutes by default); automatically rolls back on timeout
* `--dry-run`: simulates the upgrade without actually performing it
* `--install`: runs an install instead of an upgrade, if the specified release does not exist

For a complete list of command line arguments please run `helm upgrade --help`.

## Uninstall an existing Helm release

To remove an existing release from the cluster, you have to run `helm uninstall`:

```shell
helm uninstall cxp-hello-helm --namespace cxp-team-miket92 --debug
```

Here's a list of useful command line arguments:

* `--namespace ${namespaceName}`: specifies the target namespace on a Kubernetes cluster; required starting from Helm v3
* `--debug`: displays the rendered Kubernetes manifests used to upgrade your release (useful to troubleshooting!)
* `--dry-run`: simulates the uninstall without actually performing it
* `--keep-history`: removes the release from the cluster but retains its history

For a complete list of command line arguments please run `helm upgrade --help`.

## View existing Helm releases

To show all existing releases in a particular namespace, you have to run `helm list`:

```shell
helm list --namespace cxp-team-miket92
```

Here's a list of useful command line arguments:

* `--namespace ${namespaceName}`: specifies the target namespace on a Kubernetes cluster; required starting from Helm v3
* `--all-namespaces`: list releases on all namespaces

For a complete list of command line arguments please run `helm list --help`.

## View status of an existing Helm

To check the status of a specific releases in a particular namespace, you have to run `helm status`:

```shell
helm status cxp-hello-helm --namespace cxp-team-miket92
```

Here's a list of useful command line arguments:

* `--namespace ${namespaceName}`: specifies the target namespace on a Kubernetes cluster; required starting from Helm v3

For a complete list of command line arguments please run `helm status --help`.
