# owner-info

This chart deploys a ConfigMap that contains owner info about a chart.

This chart **should not** be deployed stand-alone, it is meant to be used as a dependency
by other charts.

**Caveat:** Only use `owner-info` for the top-level chart (i.e. the chart that you're
deploying). If you use it for any dependencies of your top-level chart then you will get
multiple ConfigMaps with name clash.

## Usage

Add `owner-info` as a dependency to your chart's `Chart.yaml` file:

```yaml
dependencies:
  - name: owner-info
    repository: https://charts.eu-de-2.cloud.sap
    version: # use owner-info's current version from Chart.yaml
```

Run `helm dep up` to regenerate your chart's `Chart.lock` file. Then, add the owner-info metadata to your chart's `values.yaml` file:

```yaml
owner-info:
  helm-chart-url: 'https://github.com/sapcc/helm-charts/tree/master/system/example'
  maintainers:
    - Jane Doe
    - Max Mustermann
  support-group: example-group
  service: example-service
```

The following fields can appear within the `owner-info` section of your chart's `values.yaml` file:

| Parameter | Required? | Description |
| --------- | --------- | ----------- |
| `helm-chart-url` | **yes** | An HTTP(S) URL describing where to find the Helm chart in GitHub etc., e.g. `https://github.com/sapcc/helm-charts/tree/master/common/owner-info`. |
| `maintainers` | no | List of people that maintain the Helm chart. If multiple people can help with issues regarding the chart, feel free to include as many names as you like. The list should be ordered by priority, i.e. the primary maintainer should be at the top. |
| `support-group` | **yes** | For routing alerts/tickets regarding this deployment to the right support group. |
| `service` | no | Allows sorting alerts/tickets within the realm of a single support group. |

The values for `support-group` and `service` will be carried over into all Kubernetes objects belonging to the Helm release (both directly and indirectly), and appear in `.metadata.labels["ccloud/support-group"]` and `.metadata.labels["ccloud/service"]`, respectively. This mapping is automatically performed by the owner-label-injector component.
