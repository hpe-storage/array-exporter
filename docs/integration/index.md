# Overview

When an HPE Storage Array Exporter for Prometheus has been deployed, a Prometheus instance can be configured to collect metrics from it. The configuration requirements depend on multiple factors, including the ways Prometheus and the exporter are deployed and the desired monitoring practices. The examples provided here are intended to facilitate setup of the exporter as a scrape target in select scenarios.  Refer to Prometheus documentation for more complete guidance.

[TOC]

!!! important
    If the intention is to use the HPE provided Grafana dashboards, a label named "array" in scrape targets with the storage system designation (i.e. a hostname) is mandatory for the dashboards to work.

## Prometheus Standalone

It's most common to have scrape targets statically defined in a Prometheus configuration file when running Prometheus as a standalone application.

### Using a Prometheus Configuration File

When running Prometheus as either a standalone executable or a container, a [configuration file](https://prometheus.io/docs/prometheus/latest/configuration/configuration/) (conventionally named prometheus.yml) is used. In it, a scrape job can be added for the exporter with configuration content similar to the following example.

```markdown
scrape_configs:
  - job_name: 'hpe-array-exporter'
    # A scrape interval of 30 seconds or more is recommended
    scrape_interval: 1m
    static_configs:
      # Set the target to the host address and port at which the exporter serves its metrics
      - targets: ['localhost:8080']
        labels: 
          # Specify any labels to be added to the resulting metrics
```

!!! note
    It may be desirable to include a label to identify the storage system from which the metrics are collected.

## Prometheus running on Kubernetes

When running Prometheus on Kubernetes, scrape targets may be dynamically discovered. Different methods are used when Prometheus is deployed via a Helm chart or an Operator.

### Using a Kubernetes Service with Prometheus Helm Annotations

This example applies when the [Prometheus Helm chart](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus) and the exporter are deployed in the same Kubernetes cluster.

The Prometheus Helm chart uses a configuration file like the one described in the preceding section.  Its [default configuration](https://github.com/prometheus-community/helm-charts/blob/main/charts/prometheus/values.yaml) enables Pods and Services to be identified as scrape targets using annotations such as `prometheus.io/scrape: "true"` and `prometheus.io/scrape-slow: "true"`.  Annotations like these can be added to the exporter Service when created with either the [sample YAML files](https://github.com/hpe-storage/co-deployments/tree/master/yaml/array-exporter) or the [Helm chart](https://artifacthub.io/packages/helm/hpe-storage/hpe-array-exporter).

### Using a Kubernetes Service Monitor

This example applies when the Prometheus Operator and the exporter are deployed in the same Kubernetes cluster.

The [Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator) and the [Helm Kube Prometheus Stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) can discover scrape targets via `Service` and `ServiceMonitor` objects.

A Kubernetes [Service](https://kubernetes.io/docs/concepts/services-networking/service/) is created along with a Deployment when using either the [sample YAML files](https://github.com/hpe-storage/co-deployments/tree/master/yaml/array-exporter) or the [Helm chart](https://artifacthub.io/packages/helm/hpe-storage/hpe-array-exporter). By default it is a `ClusterIP` Service for access from within the cluster.  A `NodePort` Service can be configured instead to provide access from outside the cluster, for example in conjunction with a Prometheus configuration file as described above.

A ServiceMonitor enables a `Prometheus` custom resource (defined by the Prometheus Operator) to discover the exporter Service as a scrape target. Whether the Prometheus instance is created automatically (such as by a Helm chart installation) or manually, its `serviceMonitorNamespaceSelector` and `serviceMonitorSelector` fields control which ServiceMonitor objects it discovers.

The ServiceMonitor's `namespaceSelector` and `selector` fields in turn control how an exporter Service is identified.  In the following example, the `selector` identifies the exporter Service based on a matching `app` label.

#### ServiceMonitor Example

```markdown
kind: ServiceMonitor
apiVersion: monitoring.coreos.com/v1
metadata:
  name: hpe-array-exporter-servicemonitor
  namespace: hpe-storage
  labels:
    # A selector for this label is used by a Prometheus Operator,
    # installed via OLM
    k8s-app: prometheus
    # A "release" label selector is used by the Helm Kube Prometheus Stack,
    # with the value as the release specified on chart installation
    release: prometheus
spec:
  # Match the namespace of the target Array Exporter service,
  # or omit the namespaceSelector
  namespaceSelector:
    matchNames:
      - hpe-storage
  selector:
    matchLabels:
      app: hpe-array-exporter
  endpoints:
    - port: http-metrics
      scheme: http
      interval: 1m
  # Corresponding labels on the Array Exporter service are added to
  # the scraped metrics; customize as desired
  targetLabels:
    - array
```

!!! note
    It may be desirable for metrics from the exporter to include a label identifying the storage system from which they are collected.  In this example, the `targetLabels` configuration refers to an `array` label that must have been included in the Service object.

A ServiceMonitor example can also be found in the [sample YAML files](https://github.com/hpe-storage/co-deployments/tree/master/yaml/array-exporter).
