# Overview

HPE Storage Array Exporter for Prometheus is a unified exporter that supports the HPE primary storage portfolio. The [metrics](../metrics/index.md) provided differ by storage system type and release.

## Delivery Vehicles

The exporter is delivered as:

- [Standalone binaries](https://github.com/hpe-storage/array-exporter/releases) for Linux through GitHub releases.
- [Container images](https://quay.io/repository/hpestorage/array-exporter) hosted on Quay, deployable via standalone Docker or via Kubernetes (with object definitions in [YAML](https://github.com/hpe-storage/co-deployments/tree/master/yaml/array-exporter) available in GitHub).
- [Helm charts](https://artifacthub.io/packages/helm/hpe-storage/hpe-array-exporter/) hosted at Artifact Hub.

### HPE Storage Array Exporter for Prometheus 1.0.0

Release highlights:

- Initial version

<table>
 <tr>
   <th>Binaries and Release Notes</th>
   <td>
    <a href="https://github.com/hpe-storage/array-exporter/releases/tag/v1.0.0">1.0.0</a> on GitHub
   </td>
 </tr>
 <tr>
   <th>Helm Chart</th>
   <td>
    <a href="https://artifacthub.io/packages/helm/hpe-storage/hpe-array-exporter/1.0.0">1.0.0</a> on Artifact Hub
   </td>
 </tr>
 <tr>
   <th>Platforms</th>
   <td>
     Alletra OS 9000 9.3.0<br />
     Alletra OS 6000 6.0.0 or later<br />
     Nimble OS 5.0.10.x or later<br />
     Primera OS 4.0.x, 4.1.x, 4.2.x, 4.3.x<br />
     3PAR OS 3.3.1, 3.3.2
   </td>
 </tr>
 <tr>
   <th>Blogs</th>
   <td>
    <a href="https://community.hpe.com/t5/Around-the-Storage-Block/HPE-CSI-Driver-for-Kubernetes-enhancements-with-monitoring-and/ba-p/7158137">Release blog</a> on Around The Storage Block<br />
    <a href="https://developer.hpe.com/blog/get-started-with-prometheus-and-grafana-on-docker-with-hpe-storage-array-exporter/">Tutorial using Docker</a> on HPE Developer Community
   </td>
 </tr>
</table>
