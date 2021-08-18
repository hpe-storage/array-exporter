# Overview

The metrics provided by the HPE Storage Array Exporter for Prometheus vary according to the storage system to which it is connected.

[TOC]

## HPE Alletra 9000 and Primera

The metrics provided for HPE Alletra 9000 and Primera storage systems (3PAR is also supported) are equivalent, with the metric names reflecting the storage system type.

### Common Provisioning Group (CPG) Space

| Metric | Type | Description |
| :--- | :--- | :--- |
| hpealletra9000_cpg_capacity_bytes<br/>hpeprimera_cpg_capacity_bytes<br/>hpe3par_cpg_capacity_bytes | Gauge | Total capacity allocated to a CPG |
| hpealletra9000_cpg_used_bytes<br/>hpeprimera_cpg_used_bytes<br/>hpe3par_cpg_used_bytes | Gauge | CPG capacity in use |
| hpealletra9000_cpg_available_bytes<br/>hpeprimera_cpg_available_bytes<br/>hpe3par_cpg_available_bytes | Gauge | CPG capacity available |
| hpealletra9000_cpg_volume_used_bytes<br/>hpeprimera_cpg_volume_used_bytes<br/>hpe3par_cpg_volume_used_bytes | Gauge | CPG capacity reserved for volumes |
| hpealletra9000_cpg_snapshot_used_bytes<br/>hpeprimera_cpg_snapshot_used_bytes<br/>hpe3par_cpg_snapshot_used_bytes | Gauge | CPG capacity reserved for snapshots |

Each of these metrics includes the following label.

| Label | Description |
| :--- | :--- |
| cpg | CPG name |

### Volume Space

| Metric | Type | Labels | Description |
| :--- | :--- | :--- | :--- |
| hpealletra9000_volume_size_bytes<br/>hpeprimera_volume_size_bytes<br/>hpe3par_volume_size_bytes | Gauge | cpg, provisioning, volume | Volume data capacity |
| hpealletra9000_volume_used_bytes<br/>hpeprimera_volume_used_bytes<br/>hpe3par_volume_used_bytes | Gauge | cpg, provisioning, volume | Space used for volume data, including reserved space for a fully-provisioned volume |
| hpealletra9000_volume_snapshot_used_bytes<br/>hpeprimera_volume_snapshot_used_bytes<br/>hpe3par_volume_snapshot_used_bytes | Gauge | cpg, provisioning, snap_cpg, volume | Space used for volume snapshots |

The labels have the following meanings.

| Label | Description |
| :--- | :--- |
| cpg | Name of the CPG containing the volume |
| provisioning | Indicates whether storage space is reserved upon volume creation ("thick") or as needed ("thin") |
| snap_cpg | Name of the CPG containing the volume snapshots |
| volume | Volume name |

### Volume Performance

| Metric | Type | Description |
| :--- | :--- | :--- |
| hpealletra9000_volume_reads_per_second_avg5m<br/>hpeprimera_volume_reads_per_second_avg5m<br/>hpep3par_volume_reads_per_second_avg5m | Gauge | Read operations performed on the VLUNs for a volume per second, averaged over a fixed five-minute interval (read IOPS) |
| hpealletra9000_volume_writes_per_second_avg5m<br/>hpeprimera_volume_writes_per_second_avg5m<br/>hpe3par_volume_writes_per_second_avg5m | Gauge | Write operations performed on the VLUNs for a volume per second, averaged over a fixed five-minute interval (write IOPS) |
| hpealletra9000_volume_read_bytes_per_second_avg5m<br/>hpeprimera_volume_read_bytes_per_second_avg5m<br/>hpe3par_volume_read_bytes_per_second_avg5m | Gauge | Bytes read from the VLUNs for a volume per second, averaged over a fixed five-minute interval (read throughput) |
| hpealletra9000_volume_write_bytes_per_second_avg5m<br/>hpeprimera_volume_write_bytes_per_second_avg5m<br/>hpe3par_volume_write_bytes_per_second_avg5m | Gauge | Bytes written to the VLUNs for a volume per second, averaged over a fixed five-minute interval (write throughput) |
| hpealletra9000_volume_seconds_per_read_avg5m<br/>hpeprimera_volume_seconds_per_read_avg5m<br/>hpe3par_volume_seconds_per_read_avg5m | Gauge | Seconds elapsed during a single read from the VLUNs for a volume, averaged over a fixed five-minute interval (read latency) |
| hpealletra9000_volume_seconds_per_write_avg5m<br/>hpeprimera_volume_seconds_per_write_avg5m<br/>hpe3par_volume_seconds_per_write_avg5m | Gauge | Seconds elapsed during a single write to the VLUNs for a volume, averaged over a fixed five-minute interval (write latency) |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| cpg | Name of the CPG containing the volume |
| volume | Volume name |

## HPE Alletra 6000 and Nimble

The metrics provided for HPE Alletra 6000 and Nimble Storage systems are equivalent, with the metric names reflecting the storage system type.

### Storage Pool Space

| Metric | Type | Description |
| :--- | :--- | :--- |
| hpealletra6000_pool_capacity_bytes<br/>hpenimble_pool_capacity_bytes | Gauge | Total bytes that can be written into a storage pool |
| hpealletra6000_pool_used_bytes<br/>hpenimble_pool_used_bytes | Gauge | Storage pool capacity in use |
| hpealletra6000_pool_available_bytes<br/>hpenimble_pool_available_bytes | Gauge | Storage pool capacity available to provision volumes |
| hpealletra6000_pool_unused_reserve_bytes<br/>hpenimble_pool_unused_reserve_bytes | Gauge | Storage pool capacity that is reserved for existing volumes but is not in use |
| hpealletra6000_pool_volume_used_bytes<br/>hpenimble_pool_volume_used_bytes | Gauge | Total logical size of all volumes in a storage pool |
| hpealletra6000_pool_snapshot_used_bytes<br/>hpenimble_pool_snapshot_used_bytes | Gauge | Total logical size of all snapshots in a storage pool |
| hpealletra6000_pool_data_reduction_bytes<br/>hpenimble_pool_data_reduction_bytes | Gauge | Storage pool capacity that would be in use if not for storage system optimizations, including compression, deduplication, and clones, but excluding thin provisioning |

Each of these metrics includes the following label.

| Label | Description |
| :--- | :--- |
| pool | Storage pool name |

### Volume Space

| Metric | Type | Description |
| :--- | :--- | :--- |
| hpealletra6000_volume_size_bytes<br/>hpenimble_volume_size_bytes | Gauge | Volume data capacity |
| hpealletra6000_volume_used_bytes<br/>hpenimble_volume_used_bytes | Gauge | Space used for volume data |
| hpealletra6000_volume_snapshot_used_bytes<br/>hpenimble_volume_snapshot_used_bytes | Gauge | Space used for volume snapshots |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| pool | Name of the storage pool containing the volume |
| provisioning | Indicates whether storage space is reserved upon volume creation ("thick") or as needed ("thin") |
| volume | Volume ID |
| volume_name | Volume name |

### Volume Performance

| Metric | Type | Description |
| :--- | :--- | :--- |
| hpealletra6000_volume_reads_per_second_avg5m<br/>hpenimble_volume_reads_per_second_avg5m | Gauge | Read operations performed on a volume per second, averaged over the preceding five minutes (read IOPS) |
| hpealletra6000_volume_writes_per_second_avg5m<br/>hpenimble_volume_writes_per_second_avg5m | Gauge | Write operations performed on a volume per second, averaged over the preceding five minutes (write IOPS) |
| hpealletra6000_volume_read_bytes_per_second_avg5m<br/>hpenimble_volume_read_bytes_per_second_avg5m | Gauge | Bytes read from a volume per second, averaged over the preceding five minutes (read throughput) |
| hpealletra6000_volume_write_bytes_per_second_avg5m<br/>hpenimble_volume_write_bytes_per_second_avg5m | Gauge | Bytes written to a volume per second, averaged over the preceding five minutes (write throughput) |
| hpealletra6000_volume_seconds_per_read_avg5m<br/>hpenimble_volume_seconds_per_read_avg5m | Gauge | Seconds elapsed during a single read from a volume, averaged over the preceding five minutes (read latency) |
| hpealletra6000_volume_seconds_per_write_avg5m<br/>hpenimble_volume_seconds_per_write_avg5m | Gauge | Seconds elapsed during a single write to a volume, averaged over the preceding five minutes (write latency) |

Each of these metrics includes the following labels.

| Label | Description |
| :--- | :--- |
| pool | Name of the storage pool containing the volume |
| volume | Volume ID |
| volume_name | Volume name |
