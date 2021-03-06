# Introducing Helm

Although kubectl is quite a capable command line tool to deploy applications, 
it`s a little cumbersome to use especially if comes to complex deployments with a lot of Kubernetes resources to manage.

During the last years, [Helm](https://helm.sh) became the defacto standard for complex application deployments.

Here's short list of key features which Helm provides:

* Kubernetes manifests can be managed as templates which are merged with configuration values during deployment time
* Deployments are managed through charts which can be shared via chart repositories
* Charts are composable with subcharts

!!! info "This documentation is based on Helm v3"
    The `Helm` section of this documentation is based on Helm version 3. If you are still using Helm version 2, 
    please check the official Helm documentation site. 
    
## References

| Link | Description |
| --- | --- |
| [Helm Documentation](https://helm.sh/docs/) | Official Helm documentation |
