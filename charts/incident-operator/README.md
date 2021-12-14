# incident-operator

![Version: 1.0.2](https://img.shields.io/badge/Version-1.0.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.2.3](https://img.shields.io/badge/AppVersion-0.2.3-informational?style=flat-square)

A Helm chart for installing an operator for managing quarantines of workflows for Kubernetes.

## Installation

```

helm repo add charts https://soer3n.github.io/charts/charts
helm upgrade --install incident-operator charts/incident-operator

```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/soer3n/incident-operator"` |  |
| image.tag | string | `"v0.2.3"` |  |
| imagePullSecrets | list | `[]` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| operator.resources.limits.cpu | string | `"100m"` |  |
| operator.resources.limits.memory | string | `"300Mi"` |  |
| operator.resources.requests.cpu | string | `"100m"` |  |
| operator.resources.requests.memory | string | `"200Mi"` |  |
| tolerations | list | `[]` |  |
| webhook.certDir | string | `"/tmp/k8s-webhook-server/serving-certs/"` |  |
| webhook.resources.limits.cpu | string | `"100m"` |  |
| webhook.resources.limits.memory | string | `"300Mi"` |  |
| webhook.resources.requests.cpu | string | `"100m"` |  |
| webhook.resources.requests.memory | string | `"200Mi"` |  |
| webhook.service.name | string | `"quarantine-webhook"` |  |
| webhook.service.port | int | `443` |  |
| webhook.service.type | string | `"ClusterIP"` |  |

