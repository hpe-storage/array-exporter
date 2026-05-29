Example Grafana dashboards, provided as is, are hosted on [grafana.com](https://grafana.com/orgs/hpestorage/dashboards).

## Red Hat Advanced Cluster Management

[Red Hat Advanced Cluster Management](https://www.redhat.com/en/technologies/management/advanced-cluster-management) (ACM) includes an Observability component that provides multi-cluster monitoring with a built-in Grafana instance. Custom dashboards can be loaded into this Grafana instance via Kubernetes `ConfigMap` resources. The dashboards hosted on grafana.com require several modifications to work correctly in this environment.

[TOC]

### Dashboard ConfigMap

ACM Grafana discovers custom dashboards from `ConfigMap` resources in the `open-cluster-management-observability` namespace that carry the `grafana-custom-dashboard: "true"` label. The dashboard JSON is stored as a data key ending in `.json`.

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: hpe-array-exporter-dashboard
  namespace: open-cluster-management-observability
  labels:
    cluster.open-cluster-management.io/backup: ""
    grafana-custom-dashboard: "true"
data:
  hpe-exporter-dashboard.json: |
    {
      # Dashboard JSON content goes here
    }
```

### Adapting Grafana.com Dashboards

Dashboards exported from grafana.com include sections and settings that are not compatible with ACM Grafana. The following modifications are required when wrapping a grafana.com dashboard export into a `ConfigMap`.

**Remove export-only sections.** The `__inputs` and `__requires` sections at the top of the dashboard JSON are used during manual Grafana UI imports and serve no purpose in a `ConfigMap`-based deployment. These sections should be removed entirely.

**Fix datasource references.** Grafana.com exports reference a datasource variable such as `${DS_PROMETHEUS}` that relies on the `__inputs` substitution mechanism. This substitution does not occur when the dashboard is loaded via a `ConfigMap`. All datasource references must be replaced with an explicit object reference.

Replace any occurrence of `"datasource": null` or `"datasource": "${DS_PROMETHEUS}"` with:

```json
"datasource": {"type": "prometheus", "uid": "default"}
```

**Update annotation datasource.** The annotation datasource `"datasource": "-- Grafana --"` is a deprecated string format. Replace it with the object format:

```json
"datasource": {"type": "datasource", "uid": "grafana"}
```

**Update schema version.** Grafana.com exports may use an older `schemaVersion`. Update the value to match the version supported by the ACM Grafana instance (e.g. `39`).

**Remove stale fields.** Fields such as `gnetId`, `iteration`, and `"style": "dark"` are grafana.com export artifacts and can be removed.

!!! important
    The dashboard metric names must match the platform-specific prefix used by the exporter. The exporter uses prefixes such as `hpealletrastoragemp_`, `hpealletra9000_`, `hpeprimera_`, `hpe3par_`, `hpealletra6000_`, and `hpenimble_` rather than a generic `hpe_` prefix. All PromQL expressions in the dashboard must be updated to use the correct prefix for the target storage platform. Refer to the [Metrics](../metrics/index.md) page for the full list of metric names per platform.

### Metrics Allowlist

ACM Observability collects metrics from managed clusters and forwards them to the hub cluster. Only metrics included in a custom allowlist `ConfigMap` are collected. The exporter runs as a user workload, so its metrics must be added to the `uwl_metrics_list.yaml` section of the allowlist rather than the `metrics_list.yaml` section.

!!! important
    The `metrics_list.yaml` section is for metrics scraped by the platform Prometheus instance (in the `openshift-monitoring` namespace). The `uwl_metrics_list.yaml` section is for metrics scraped by the User Workload Monitoring Prometheus instance. The exporter is a user workload and its metrics are scraped by the latter. Placing entries in the wrong section results in no data being collected.

The following example adds a match rule for HPE Alletra Storage MP B10000 metrics to the allowlist `ConfigMap` on the hub cluster.

```yaml
---
kind: ConfigMap
apiVersion: v1
metadata:
  name: observability-metrics-custom-allowlist
  namespace: open-cluster-management-observability
data:
  uwl_metrics_list.yaml: |
    matches:
    - __name__=~"hpealletrastoragemp_.*"
```

!!! note
    The match pattern must correspond to the metric prefix used by the exporter for the target storage platform. Refer to the [Metrics](../metrics/index.md) page for the metric names and prefixes per platform.
