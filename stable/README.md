# Alucard Helm Charts
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/alucard)](https://artifacthub.io/packages/search?repo=alucard)

## Overview

Welcome to Alucard the Universal Helm Chart repository! This Helm Chart is designed to provide a flexible and customizable solution for deploying applications on Kubernetes. It supports various features, including custom volume mounting, secret mounting, multiple ingress hosts and paths, and more.

## Features

- **Custom Volume Mounting:** Easily attach custom volumes to your pods for persistent data storage.

- **Secret Mounting:** Securely inject secrets into your application through Kubernetes secrets.

- **Multiple Ingress Configuration:** Define multiple ingress hosts and paths for better routing and accessibility.

- **Configurable Options:** Fine-tune your deployment with a wide range of customizable options and parameters.

- **Scalability:** Scale your application horizontally by adjusting replica counts and resource allocations.

- **Compatibility:** Works seamlessly with different Kubernetes distributions and cloud providers.

## Prerequisites

- [Helm](https://helm.sh/docs/intro/install/) installed on your Kubernetes cluster.

## Installation

```bash
helm repo add alucard https://privyinfra.github.io/alucard/stable/  
helm repo update
helm install my-alucard alucard/alucard [--version=1.0.0]
```

## Configuration

The Helm Chart provides extensive configuration options to tailor the deployment to your specific needs. Review the [`values.yaml`](./charts/universal-chart/values.yaml) file for a comprehensive list of available parameters.

Example:

```bash
helm install my-alucard  alucard/alucard -f my-values.yaml
```

## Usage

Describe how users can customize and use your Helm Chart in their own projects. Provide examples and best practices for configuration.

### Example Deployment

```yaml
# Example values.yaml
image:
  repository: "alucard-example" 
  tag: "stable"
fullnameOverride: "alucard-example"
nameOverride: "alucard-example"
namespace: alucard
replicaCount: 1
service:
  type: ClusterIP
  configs:
    - name: http
      port: 80
      targetPort: 6969
      protocol: TCP
    - name: grpc
      port: 4054
      targetPort: 4053
      protocol: TCP
  headless: true
liveness:
  type: exec
  port: 4054
migrations:
  enabled: true
  type: init-container
  args:
  - "cd /go/src/alucard-example && ./main db:migrate"
secrets:
  configs:
    enabled: true
    autorestart:
      enabled: true
    path: secrets/configs/alucard-example
  creds:
    enabled: true
    autorestart:
      enabled: true
    path: secrets/creds/alucard-example
volumes:
  - name: service-account
    secret:
      secretName: alucard-example-sa
volumeMounts:
  - name: service-account
    mountPath: /mnt
resources: 
  limits:
    cpu: 300m
    memory: 500Mi
  requests:
    cpu: 200m
    memory: 384Mi
autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 2
  averageUtilMem: 80
  averageUtilCpu: 80
ingress:
  internal:
    enabled: true
    className: "internal-nginx"
  rules:
    - host: alucard.thanos.my.id
      http:
        paths:
          - path: /(.*)
            pathType: Prefix
            backend:
              service:
                name: alucard-example
                port:
                  number: 80
```

## Contributing

We welcome contributions! If you find a bug or have an idea for improvement, please open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](./LICENSE).
```

Remember to replace the placeholder URLs, names, and images with your actual details. Customize the sections as needed for your specific Helm Chart.
