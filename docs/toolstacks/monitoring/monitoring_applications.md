# Exposing Telemetry Data from Java Applications

These days, modern application servers or application containers are providing a lot
of useful telemetry data out-of-the-box. This page is going to show you how to expose
this valuable data to Prometheus.

## Activating Monitoring on Eclipse MicroProfile Applications

The `MicroProfile Metrics` feature allows you to expose telemetry data from MicroProfile
applications. 
Activating this feature usually varies between Microprofile implementations:
some require adding some dependencies to your Maven POM file, some enable the
feature by default.

An activated Microprofile Metrics feature will expose a new endpoint at  
 
```
${schema}://${hostname}[:${port}]/metrics
```

which Prometheus can use to scrape telemetry data from your application.

If you want to expose your custom metrics, simply use the Microprofile Metrics annotations:

| Annotation | Description | Prometheus Metric Type |
| --- | --- | --- |
| @Counted | Denotes a counter, which counts the invocations of the annotated object. | Counter | 
| @Gauge | Denotes a gauge, which samples the value of the annotated object. | Gauge |
| @Metered | Denotes a meter, which tracks the frequency of invocations of the annotated object. | |
| @Timed | Denotes a timer, which tracks duration of the annotated object. | | 

@see [Eclipse MicroProfile Metrics Spec](https://github.com/eclipse/microprofile-metrics/blob/master/spec/src/main/asciidoc/metrics_spec.adoc)
 
## Activating Monitoring on Spring Boot Applications

Although Spring Boot exposes telemetry data by default, they are not expose in Prometheus format.
Thus, you need to add [Micrometer](https://micrometer.io/) as a dependency to Spring Boot application.
Micrometer activates a meter registry in your Spring Boot application which capable of exposing 
telemetry data in various formats including Prometheus.

All telemetry data will be accessible through the endpoint:
 
```
${schema}://${hostname}[:${port}]/actuator/prometheus
```
 
In contrast to the MicroProfile Metrics feature, Micrometer lacks the full support of exposing
custom metrics through annotations. You will have to access the Micrometer API in order to 
provide your own counters, gauges etc.

| Annotation / Primitive | Description | Prometheus Metric Type |
| --- | --- | --- |
| Counter | The Counter interface allows you to increment by a fixed amount, which must be positive. | Counter | 
| FunctionCounter | A more infrequently used counter pattern that tracks a monotonically increasing function. | Counter |
| Gauge | A gauge is a handle to get the current value. | Gauge |
| @Timed / Timer | Timers are intended for measuring short-duration latencies, and the frequency of such events. | Histogram | 
| LongTaskTimer | The long task timer is a special type of timer that lets you measure time while an event being measured is still running. | |
| DistributionSummary | A distribution summary is used to track the distribution of events. | Summary | 

@see [Micrometer: Spring Boot 2's new application metrics collector](https://spring.io/blog/2018/03/16/micrometer-spring-boot-2-s-new-application-metrics-collector)

@see [Quick Guide to Micrometer](https://www.baeldung.com/micrometer)

## Activating Monitoring on Kubernetes

Unfortunately, exposing telemetry data from your application is not enough: you have to tell Prometheus which endpoint to scrape as well.

Registering an application as a target for Prometheus is done via Prometheus specific annotations attached to a pod:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cnj-monitoring-backend-spring
  labels:
    app.kubernetes.io/name: cnj-monitoring-backend-spring
    app.kubernetes.io/instance: cnj-monitoring-backend-spring
    app.kubernetes.io/managed-by: Helm
spec:
  # [..]
  template:
    metadata:
      labels:
        app.kubernetes.io/name: cnj-monitoring-backend-spring
        app.kubernetes.io/instance: cnj-monitoring-backend-spring
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/path: "/actuator/metrics"
        prometheus.io/port: "8080"
  # [..]
```

Since the Prometheus Server watches the Kubernetes API for deployment events, Prometheus will register the managed pods as targets 
as soon as this deployment is created. If the deployment is deleted from the cluster, all related targets will be dropped as well. 

!!! danger "Be careful what to annotate"
    Since Prometheus collects telemetry data on pod level, you have to make sure to specify all Prometheus annotations 
    at pod template level. Otherwise, Prometheus will not detect the pods managed by your deployment, stateful set or
    daemon set.
    
Here's a short description of these Prometheus specific annotations:

* `prometheus.io/scrape`: The default configuration will scrape all pods and, if set to false, this annotation will exclude the pod from the scraping process.
* `prometheus.io/path`: If the metrics path is not `/metrics`, define it with this annotation.
* `prometheus.io/port`: Scrape the pod on the indicated port instead of the pod’s declared ports (default is a port-free target if none are declared).
