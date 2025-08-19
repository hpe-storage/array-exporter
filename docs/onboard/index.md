# Overview

The HPE Alletra Storage MP B10000 onboard exporter can be enabled by a storage administrator in the array UI. Learn how at the [HPE Support Center](https://support.hpe.com/hpesc/public/docDisplay?docId=sd00004645en_us&page=GUID-6F1E4554-D279-4688-AC5F-5A424197AF16.html&docLocale=en_US).

[TOC]

## Example Prometheus Scrape Configuration

The onboard exporter requires a username and password to be scraped. Credentials are sent over SSL using basic HTTP authentication. Below is an example scrape configuration for Prometheus.

```
...
- job_name: my-array-0
  metrics_path: /metrics
  scrape_interval: 30s
  static_configs:
    - targets: ['192.168.1.100:443']
  basic_auth:
    username: user
    password: prometheus
  scheme: https
  tls_config:
    insecure_skip_verify: true
...
```

!!! hint
    The `scrape_interval` may not be lower than 30 seconds. See [limitations](#limitations).

## Metrics

These are the metrics reported by the onboard exporter up to release 10.5.

### Disk

| Metric | Type | Description |
| :--- | :--- | :--- | 
| hpe_greenlake_for_block_storage_disk_read_bytes_per_second | gauge | Read throughput in bytes per second |
| hpe_greenlake_for_block_storage_disk_read_latency_seconds | gauge | Read service time in seconds |
| hpe_greenlake_for_block_storage_disk_reads_per_second | gauge | Read operations per second |
| hpe_greenlake_for_block_storage_disk_write_bytes_per_second | gauge | Write throughput in bytes per second |
| hpe_greenlake_for_block_storage_disk_write_latency_seconds | gauge | Write service time in seconds |
| hpe_greenlake_for_block_storage_disk_writes_per_second | gauge | Write operations per second |
| hpe_greenlake_for_block_storage_disk_total_bytes_per_second | gauge | Total throughput in bytes per second |
| hpe_greenlake_for_block_storage_disk_total_operations_per_second |gauge | Total operations per second |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| array | Array name |
| location | Disk location (N:S:P) |
| node | Node number |
| slot | Slot number |
| port | Port number |

### Port
 
| Metric | Type | Description |
| :--- | :--- | :--- | 
| hpe_greenlake_for_block_storage_port_read_bytes_per_second | gauge | Read throughput in bytes per second |
| hpe_greenlake_for_block_storage_port_read_latency_seconds | gauge | Read service time in seconds |
| hpe_greenlake_for_block_storage_port_reads_per_second | gauge | Read operations per second |
| hpe_greenlake_for_block_storage_port_write_bytes_per_second | gauge | Write throughput in bytes per second |
| hpe_greenlake_for_block_storage_port_write_latency_seconds | gauge | Write service time in seconds |
| hpe_greenlake_for_block_storage_port_writes_per_second | gauge | Write operations per second |
| hpe_greenlake_for_block_storage_port_total_bytes_per_second | gauge | Total throughput in bytes per second |
| hpe_greenlake_for_block_storage_port_total_operations_per_second | gauge | Total operations per second |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| array | Array name |
| location | HBA location (N:S:P) |
| node | Node number |
| slot | Slot number |
| port | Port number |

### QoS

| Metric | Type | Description |
| :--- | :--- | :--- | 
| hpe_greenlake_for_block_storage_qos_read_bytes_per_second | gauge | Read throughput in bytes per second |
| hpe_greenlake_for_block_storage_qos_read_latency_seconds | gauge | Read service time in seconds |
| hpe_greenlake_for_block_storage_qos_reads_per_second | gauge | Read operations per second |
| hpe_greenlake_for_block_storage_qos_write_bytes_per_second | gauge | Write throughput in bytes per second |
| hpe_greenlake_for_block_storage_qos_write_latency_seconds | gauge | Write service time in seconds |
| hpe_greenlake_for_block_storage_qos_writes_per_second | gauge | Write operations per second |
| hpe_greenlake_for_block_storage_qos_total_bytes_per_second | gauge | Total throughput in bytes per second |
| hpe_greenlake_for_block_storage_qos_total_operations_per_second | gauge | Total operations per second |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| array | Array name |
| qos | QoS ID | 

### VLUNs

| Metric | Type | Description |
| :--- | :--- | :--- | 
| hpe_greenlake_for_block_storage_vlun_read_bytes_per_second | gauge | Read throughput in bytes per second |
| hpe_greenlake_for_block_storage_vlun_read_latency_seconds | gauge | Read service time in seconds |
| hpe_greenlake_for_block_storage_vlun_reads_per_second | gauge | Read operations per second |
| hpe_greenlake_for_block_storage_vlun_write_bytes_per_second | gauge | Write throughput in bytes per second |
| hpe_greenlake_for_block_storage_vlun_write_latency_seconds | gauge | Write service time in seconds |
| hpe_greenlake_for_block_storage_vlun_writes_per_second | gauge | Write operations per second |
| hpe_greenlake_for_block_storage_vlun_total_bytes_per_second | gauge | Total throughput in bytes per second |
| hpe_greenlake_for_block_storage_vlun_total_operations_per_second | gauge | Total operations per second |

Each of these metrics includes the following label.

| Label | Description |
| :--- | :--- |
| array | Array name |
| host | Host name or IP address |
| hostwwn | Host WWN name |
| volume | Volume name |
| location | HBA location (N:S:P) |
| node | Node number |
| slot | Slot number |
| port | Port number |

### Volumes 

| Metric | Type | Description |
| :--- | :--- | :--- | 
| hpe_greenlake_for_block_storage_volume_read_bytes_per_second | gauge | Read throughput in bytes per second |
| hpe_greenlake_for_block_storage_volume_read_latency_seconds | gauge | Read service time in seconds |
| hpe_greenlake_for_block_storage_volume_reads_per_second | gauge | Read operations per second |
| hpe_greenlake_for_block_storage_volume_write_bytes_per_second | gauge | Write throughput in bytes per second |
| hpe_greenlake_for_block_storage_volume_write_latency_seconds | gauge | Write service time in seconds |
| hpe_greenlake_for_block_storage_volume_writes_per_second | gauge | Write operations per second |
| hpe_greenlake_for_block_storage_volume_total_bytes_per_second | gauge | Total throughput in bytes per second |
| hpe_greenlake_for_block_storage_volume_total_operations_per_second | gauge | Total operations per second |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| array | Array name |
| volume | Volume name |

## Limitations

The onboard exporter up to 10.5 comes with these current known limitations.

- The onboard exporter runs on a throttled thread what only allows being scraped every 30 seconds. Any scrape attempt outside the allotted window will return an HTTP error.
- The onboard exporter does not provide any capacity metrics.
