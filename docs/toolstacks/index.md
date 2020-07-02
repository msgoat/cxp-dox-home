# Cluster Tool Stacks

This page introduces some well-known cluster tool stacks which are mainly used to
constantly observe workload running on a Kubernetes or OpenShift cluster.

A typical set of cluster tool stacks includes:

* [Ingress Stack](ingress/ingress_index.md): ingress controller to expose services externally with Traefik
* [Logging Stack](logging/logging_index.md): cluster logging with Elasticsearch, Fluent Bit (or Logstash) and Kibana
* [Monitoring Stack](monitoring/monitoring_index.md): cluster monitoring with Prometheus and Grafana
* [Tracing Stack](tracing/tracing_index.md): cluster end-to-end tracing with Jaeger
* [IAM Stack](iam/iam_index.md): Identity und Access Management with KeyCloak

The first four stacks - Ingress, Logging, Monitoring and Tracing - are mandatory, the fifth one - IAM - is strictly optional.
Please keep in mind, that you don't need to run all these stacks on your own cluster: In an enterprise scenario it is
pretty common to host these stacks centrally and connect all clusters to this centrally managed solution.

!!! info "About these cluster tool stacks"
    Of course, this is an opinionated view especially regarding the selected 
    tools. If you want to use different tools you are free to do so!
    Nevertheless, you will find these stacks on many Kubernetes installations
    even if the specific tools that make up a particular stack, may differ. 