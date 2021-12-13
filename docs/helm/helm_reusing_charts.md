# Reusing Charts

This guide shows you how to reuse existing charts in your own chart.

The examples are based on the official Bitnami chart for PostgreSQL. For the complete example code, please checkout 
GitHub repo [msgoat/cxp-helm-reuse-charts](https://github.com/msgoat/cxp-helm-reuse-charts).

## Introducing shared charts

One of the killer features of Helm is the ability to share charts with the community using
chart repositories. The most common chart repository to check for reusable charts is 
[Artifact Hub](https://artifacthub.io/).
 
Although Helm Hub and the Helm Charts repo offer various channels (incubator > test > stable), you
should definitely focus on the __stable__ channel.

!!! tip "Always checkout Helm Charts at GitHub first"
    If you are looking for a particular chart for a particular service (let's say PostgreSQL),
    search the GitHub repo first: it's easier to navigate and you can checkout the source code
    of the chart before you actually use it. 
    
!!! info "Popular charts are shared through vendor-owned chart repositories now"
    Many popular charts for commonly used services like PostgreSQL, Elasticsearch, Kibana
    are meanwhile shared through vendor-owned chart repositories now. So if you should 
    encounter a deprecated chart on Helm Charts, feel free to switch to chart repository
    of the particular vendor or community maintaining the chart.

## Chart reuse strategies

Basically there are two approaches how to reuse charts shared through a chart repository:

* reuse the shared chart directly
* wrapping the shared chart as a sub-chart

Each has it pros and cons. It's up to you to decide on a specific strategy that you are going to use.

### Reusing a shared chart directly

Reusing a shared chart directly is pretty forward:

1\. Add the chart repository to your local chart repo cache by running `helm repo add`:

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
```

2\. Create a `values.yaml` file with your specific configuration values based on the original values.yaml provided by the chart.

3\. Run `helm install` on the chart specifying a release name and your specific `values.yaml` file:

```shell
helm install cxp-postgres-direct bitnami/postgresql --namespace ${namespaceName} --values values.yaml --debug --atomic
```
 
### Wrapping a shared chart as a sub-chart 

#### Create a wrapper chart

Run `helm create cxp-postgres-wrapped` to create a wrapper chart named `cxp-postgres-wrapped`:

```shell
helm create cxp-postgres-wrapped
```

Remove all files from the chart folder except `Chart.yaml`, `values.yaml`, `_helpers.tpl` and `.helmignore`. 
    
#### Adding a chart dependency for a sub-chart

Before you can use a shared chart as a sub-chart for your own chart, you have to add it
as a dependency to your chart.

A Chart.yaml with dependencies looks like this:

```yaml 
apiVersion: v2
name: cxp-postgresql-wrapped
description: A wrapper chart for PostgreSQL
version: 0.1.0
appVersion: 11.16.0
dependencies:
  - name: postgresql
    version: "8.8.0"
    repository: "@bitnami"
```

The `dependencies` element defines all dependencies to external charts: __postgresql__.

Each dependency is specified through the attributes:

* `name`: the chart name of the external chart as exposed by the chart repository
* `version`: the version of the external chart
* `repository`: URL or alias name of the chart repository hosting the external chart

Edit your Chart.yaml accordingly.

If you are using alias names for chart repositories you have to register these 
repositories by their alias name first by running `helm repo add`:

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
```

#### Updating a chart dependency

In order to use the external charts you have to download them into the `charts` folder
of your own chart directory. Switch to the chart directory and run:

```shell
helm dependency update
```   

After a successful update your `charts` directory will contain one `.tgz` file which represents the compressed
chart source code.

```
cxp-postgresql-wrapped
|- charts
|  | postgresql-8.8.0.tgz 
| .helmignore
| Chart.yaml
| values.yaml
```

#### Adding values for a sub-chart

Now you have to configure the sub-charts by adding values to your own `values.yaml`.
To pass these values to the sub-charts, you need to put them under top elements, which 
are named like the sub-charts:

```yaml
# ----------------------------------
# your own chart's values
# ----------------------------------
someValue: "123"
# [..]

# ----------------------------------
# PostgreSQL subchart values
# ----------------------------------
postgresql:
  image:
    registry: docker.io
    repository: bitnami/postgresql
    tag: 11.7.0-debian-10-r73
  # [..]
``` 

A good starting point for subchart values is the source code of the original shared chart or you extract 
the `values.yaml` from the downloaded chart archive (`*.tgz`).

Modify the postgresql section according to your requirements and remove all default values which you did not change.
The default values will be taken from the `values.yaml` packaged in the shared chart.

#### Managing the wrapper chart

Now you are good to go: install, upgrade and uninstall your wrapper charts as shown in [Managing Helm Releases]
(helm_managing_releases.md).

