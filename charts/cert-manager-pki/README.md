# cert-manager-pki

![Version: 1.0.0](https://img.shields.io/badge/Version-1.0.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)

A Helm chart for installing a pki using cert-manager features in Kubernetes.

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
| clients | list | `[]` |  `example: clients: - name: example-client   namespace: cert-manager   issuer: mtls-issuer   duration: 2160h # 90d   renewBefore: 360h # 15d   privateKey:     algorithm: RSA     encoding: PKCS1     size: 2048   commonName: ExampleClient   keystores:     pkcs12:       create: true` |
| issuer[0].commonName | string | `"mtls-ca"` |  |
| issuer[0].duration | string | `"2880h"` |  |
| issuer[0].name | string | `"mtls-issuer"` |  |
| issuer[0].namespace | string | `"cert-manager"` |  |
| issuer[0].renewBefore | string | `"360h"` |  |
| servers | list | `[]` |  `example: servers: - name: example-server   namespace: cert-manager   issuer: mtls-issuer   duration: 2160h # 90d   renewBefore: 360h # 15d   privateKey:     algorithm: RSA     encoding: PKCS1     size: 2048   commonName: ExampleServer` |

## Usage with Ingress

### nginx

```

---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  namespace: example
  annotations:
    acme.cert-manager.io/http01-edit-in-place: "true"
    cert-manager.io/cluster-issuer: letsencrypt-lab
    kubernetes.io/ingress.class: nginx
    kubernetes.io/tls-acme: "true"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "3600"
    nginx.ingress.kubernetes.io/proxy-send-timeout: "3600"
    nginx.ingress.kubernetes.io/auth-tls-error-page: https://google.com
    nginx.ingress.kubernetes.io/auth-tls-pass-certificate-to-upstream: "true"
    nginx.ingress.kubernetes.io/auth-tls-secret: example/example-server-server-cert <-- generated server cert
    nginx.ingress.kubernetes.io/auth-tls-verify-client: "on"
    nginx.ingress.kubernetes.io/auth-tls-verify-depth: "1"
spec:
  tls:
  - hosts:
      - example.com
    secretName: example-tls
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          serviceName: example-service
          servicePort: 8443

```

## keystores

```

# store generated pkcs12 client file locally
kubectl get secrets -n example example-client-client-cert -o jsonpath --template="{.data.keystore\.p12}" | base64 -d > ~/example.p12

# store generated jks client file locally
kubectl get secrets -n example example-client-client-cert -o jsonpath --template="{.data.keystore\.jks}" | base64 -d > ~/example.jks

# get generated password for verification
kubectl get secrets -n example example-client -o jsonpath --template="{.data.pw}" | base64 -d

```
