# cert-manager-pki

![Version: 1.0.0](https://img.shields.io/badge/Version-1.0.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)

A Helm chart for installing a pki with using cert-manager features in Kubernetes.

## Requirements

- [cert-manager](https://github.com/jetstack/cert-manager) 0.14 with [experimental flags](https://cert-manager.io/docs/release-notes/release-notes-0.14/#experimental-bundle-format-support-jks-and-pkcs-12) enabled
- or [cert-manager](https://github.com/jetstack/cert-manager) 0.15+

## Installation

```

helm repo add charts https://soer3n.github.io/charts/charts
helm upgrade --install pki charts/cert-manager-pki

```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| certmanager.apiVersion | string | `"cert-manager.io/v1"` |  |
| clients | list | `[]` |  example: clients: - name: example-client   namespace: cert-manager   issuer: mtls-issuer   duration: 2160h # 90d   renewBefore: 360h # 15d   privateKey:     algorithm: RSA     encoding: PKCS1     size: 2048   commonName: ExampleClient   keystores:     pkcs12:       create: true |
| issuer[0].commonName | string | `"mtls-ca"` |  |
| issuer[0].duration | string | `"2880h"` |  |
| issuer[0].name | string | `"mtls-issuer"` |  |
| issuer[0].namespace | string | `"cert-manager"` |  |
| issuer[0].renewBefore | string | `"360h"` |  |
| servers | list | `[]` |  example: servers: - name: example-server   namespace: cert-manager   issuer: mtls-issuer   duration: 2160h # 90d   renewBefore: 360h # 15d   privateKey:     algorithm: RSA     encoding: PKCS1     size: 2048   commonName: ExampleServer |
