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

## Secret Management
You may use environment variables and k8s secrets to inject database credentials if you don't want
to leave that information in the tegola config file (which is mounted as a configmap).  First update tegola
config to [pull database information from the environment](https://tegola.io/documentation/configuration/#env-var):

```toml
# config.toml
[[providers]]
user = "${PGUSER}"           # postgis database user
password = "${PGPASSWORD}"   # postgis database password
```

Next create a k8s secret with username/password and deploy to your namespace:

```yaml
# secret.yaml
apiVersion: v1
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
metadata:
  name: db-credentials
kind: Secret
type: Opaque
```

Finally, update the container environment to inject these environment variables from the k8s secret:

```yaml
# values.yaml
env:
  - name: PGUSER
    valueFrom:
      secretKeyRef:
        name: db-credentials
        key: username
  - name: PGPASSWORD
    valueFrom:
      secretKeyRef:
        name: db-credentials
        key: password
```
