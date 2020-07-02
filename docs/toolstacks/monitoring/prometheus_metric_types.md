# Prometheus Metric Types

Prometheus supports the following metric types:

@see [Prometheus Metric Types](https://prometheus.io/docs/concepts/metric_types/) 

## Counter

A `counter` is a monotonically increasing counter, whose value can only increase or be reset to zero.
You cannot use a gauge to expose a value that can decrease; use a [gauge](#gauge) instead.
 
__Example__: Number of processed requests, processed tasks or errors occurred

## Gauge

A `gauge` represents a single numerical value that can arbitrarily go up and down.

__Example__: Number of concurrent requests, temperature

## Histogram

A `histogram` samples observations and counts them in configurable buckets.
It also provides sum of all observed values.

Since a histogram exposes multiple values, it must be queried by multiple metric names sharing 
a common base name:

* `${baseName}_bucket`: cumulative counters for the observation buckets
* `${baseName}_sum`: total sum of all observed values
* `${baseName}_count`: count of observed values

Histograms are suitable for the calculation of [quantiles](https://en.wikipedia.org/wiki/Quantile).

__Example__: Duration of processed requests or tasks

## Summary

A `summary` is similar to a [histogram](#histogram), but it calculates quantiles over a sliding time window as well.


