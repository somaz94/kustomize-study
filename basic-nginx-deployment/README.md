# basic-nginx-deployment

This repository provides an example to understand and practice the usage of Kustomize.

<br/>

## Directory Structure

- `base`: The directory where the base resources are defined.
- `overlays`: The directory that contains resource configurations tailored to specific environments. It defines resource variations for each environment (dev, qa, prod).

<br/>

## How to Use

To generate Kubernetes resources applying the configurations for a specific environment, use the following commands:

<br/>

### Building kustomize resources:

```bash
# Generate manifests for the dev environment
kustomize build overlays/dev

# Generate manifests for the qa environment
kustomize build overlays/qa

# Generate manifests for the prod environment
kustomize build overlays/prod
```

<br/>

### Saving kustomize manifests:
```bash
kustomize build overlays/dev > manifests/dev-nginx-deployment.yaml

kustomize build overlays/qa > manifests/qa-nginx-deployment.yaml

kustomize build overlays/prod > manifests/prod-nginx-deployment.yaml
```

<br/>

### Building and deploying kustomize resources:
```bash
kustomize build overlays/dev | kubectl apply -f - --namespace=dev-namespace

kustomize build overlays/qa | kubectl apply -f - --namespace=qa-namespace

kustomize build overlays/prod | kubectl apply -f - --namespace=prod-namespace
```

<br/>

- Note: Ensure that you have a directory named manifests before attempting to save the kustomize manifests in the suggested paths.
