# WarehouseAPI Helm Chart

This Helm chart is used to deploy the WarehouseAPI application on a Kubernetes cluster.

## Prerequisites

- Kubernetes 1.16+
- Helm 3.0+

## Installation

To install the chart with the release name `warehouseapi`:

```sh
helm install warehouseapi .
```

## Uninstallation

To uninstall/delete the `warehouseapi` deployment:

```sh
helm uninstall warehouseapi
```

## Configuration

The following table lists the configurable parameters of the WarehouseAPI chart and their default values.

| Parameter            | Description                        | Default           |
|----------------------|------------------------------------|-------------------|
| `replicas`           | Number of replicas                 | `2`               |
| `image`              | Image                              | `latest`          |
| `serviceType`        | Kubernetes service type            | `NodePort`        |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example:

```sh
helm install warehouseapi . --set image.tag=1.0.0
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example:

```sh
helm install warehouseapi . -f values.yaml
```

## License

This project is licensed under the MIT License.