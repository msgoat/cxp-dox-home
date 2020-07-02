# Cluster Monitoring Best Practices

## Focus on what's going wrong

When you start to run an application or a larger software system, the sheer
amount of incoming telemetry data can get a little overwhelming. In order
to detect problems early, you should focus on the metrics which indicate problems
rather than spending time with metrics which indicate success.

Aggregate your telemetry data accordingly: it's essential to be able 
to recognize anomalies on a small set of dashboards rather than have to browse
through a large set of charts and tables which show any possible data provided by 
your monitoring stack.

A good approach to focus on what's going wrong is to start with diagrams which 
provide answers to the following questions:

* what are the top ten services which consumed the most memory/CPU/storage in the last 24 hours?
* what are the top ten services with the highest count of failed HTTP requests in the last 24 hours?
* what are the top ten services with the highest response times (mean or percentiles) in the last 24 hours?

I call this approach the `shitcreek billboard approach` since we are always tracking the
top ten services with abnormal behaviour from different perspectives.

## Use all sources of Telemetry data

Grafana is capable of using multiple datasources besides Prometheus. 
So if you want to analyze and visualize data stored in the Elasticsearch DB 
of your logging stack, just go for it! 