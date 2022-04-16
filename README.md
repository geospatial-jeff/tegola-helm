# tegola-helm

Deploy [tegola](https://github.com/go-spatial/tegola) to k8s using helm.

## Installing the Chart
Write your [tegola config file](https://tegola.io/documentation/configuration/) and deploy
with helm:
```shell
helm upgrade --install my-release tegola/ \
  --set-file config=./tegola-config.toml
```

## Uninstalling the Chart
To uninstall/delete the `my-release` release:
```shell
helm delete --purge my-release
```


## Parameters
Customize behavior of the deployment through `values.yaml`:

| Name                     | Description                                                                             | Value           |
| ------------------------ | --------------------------------------------------------------------------------------- | --------------- |
| `nameOverride`           | String to partially override the name of the chart                                    | `""`            |
| `fullnameOverride`       | String to fully override the name of the chart                                       | `""`            |
| `image.repository`            | Tegola image repository                | `gospatial/tegola`            |
| `image.tag`           | Tegola image tag           | `latest`            |
| `ingress`      | Configure ingress to expose service outside of cluster, disabled by default.                                         | `{}`            |
| `env`          | Environment variables passed to the container.                                                    | `{}` |
| `config`            | Tegola config file.                                 | `""`            |
| `resources` | Configure resources (ex. memory, cpu, disk) allocated to the container. | `{}`         |
| `nodeSelector` | Configure a nodeSelector.                                | `{}`     |
| `tolerations` | Add tolerations.                               | `[]`     |
| `affinity` | Configure affinity policy                            | `{}`     |

