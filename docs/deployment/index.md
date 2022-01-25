An HPE Storage Array Exporter for Prometheus deployment provides Prometheus metrics for a single storage system. A deployment can use an executable file or a container image.

[TOC]

# Configuration 

The address and credentials of the target storage system must be specified in a configuration file, using the format shown in this example `storage-system.yaml` file.

```markdown
address: 10.10.10.1
username: exampleuser
password: examplepassword
```

The `address` value is either a resolvable hostname or IP address of the management interface on the storage system. The `username` value identifies a storage system user with privileges described below.

| Storage System                               | User Type   | Minimal Role   |
| :------------------------------------------- | :---------- | :------------- |
| HPE Alletra 9000, Primera, 3PAR              | System User | Browse         |
| HPE Alletra 6000, Nimble Storage             | System User | Guest          |
| HPE Alletra 6000, Nimble Storage<sup>1</sup> | Tenant      | N/A            |

<sup>1</sup> = NimbleOS 6.0 and above only.

# Command Options

| Option | Default | Description |
| :--- | :--- | :--- |
| --accept-eula | false | Confirms your acceptance of the [HPE license restrictions](../legal/eula/index.md) |
| --log.path | *None* | A file location to write log messages, in addition to stdout |
| --metrics.disable-introspection | false | Excludes metrics about the metrics provider itself, with prefixes such as `promhttp`, `process`, and `go` |
| --telemetry.addr | :8080 | The host:port address at which to provide metrics |
| --telemetry.path | /metrics | The endpoint path at which to provide metrics |

# Using an Executable File

A Linux executable file is provided through GitHub [releases](https://github.com/hpe-storage/array-exporter/releases).

When an executable file has been copied to your server, it can be invoked with this command syntax:

```markdown
hpe-array-exporter [OPTION]... CONFIG-PATH
```

Available OPTIONs are described in the [Command Options](#command_options) section.

CONFIG-PATH is the location of the storage system [configuration](#configuration) file.

## Command Example

```markdown
./hpe-array-exporter --log.path=/var/log/hpe-array-exporter.log /etc/config/storage-system.yaml
```

!!! important
    Include the `--accept-eula` option or set the environment variable `ACCEPT_HPE_STANDARD_EULA=yes` to confirm your acceptance of the [HPE license restrictions](../legal/eula/index.md).


# Using a Container Image

A container image is hosted at `quay.io/hpestorage/array-exporter:v1.0.0`, with v1.0.0 replaced by the desired release version.

When deploying the array exporter as a container, the configuration file must be mounted as a volume.

Available options, including the `--log.path` used in the example below, are described in the [Command Options](#command_options) section.

## Docker Example

In this example, the configuration file at `/tmp/storage-system.yaml` is bound to the container's `/etc/config/` directory as a volume using Docker's `-v` command option. The configuration file location inside the container is then given as a command argument. In addition, the `-p` option is used to map the container's port 8080 to port 9090 on the Docker host.

```markdown
docker run -it --name hpe-array-exporter -p 9090:8080 \
     -v /tmp/storage-system.yaml:/etc/config/storage-system.yaml \
     quay.io/hpestorage/array-exporter:v1.0.0 \
     --log.path /var/log/hpe-array-exporter.log \
     /etc/config/storage-system.yaml
```

!!! important
    Include the `--accept-eula` option or set the environment variable `ACCEPT_HPE_STANDARD_EULA=yes` to confirm your acceptance of the [HPE license restrictions](/legal/eula/index.html).

Consult the [Docker command line documentation](https://docs.docker.com/engine/reference/commandline/run/) for more information on running containers using Docker.

# Using a Kubernetes Deployment

Kubernetes deployment facilities are hosted in the [co-deployments repository](https://github.com/hpe-storage/co-deployments), including a [Helm chart](https://artifacthub.io/packages/helm/hpe-storage/hpe-array-exporter/) (via Artifact Hub) and sample [YAML files](https://github.com/hpe-storage/co-deployments/tree/master/yaml/array-exporter).
