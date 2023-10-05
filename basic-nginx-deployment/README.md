# basic-nginx-deployment

This repository provides an example to understand and practice the usage of Kustomize.

<br/>

## Directory Structure

- `base`: The directory where the base resources are defined.
- `overlays`: The directory that contains resource configurations tailored to specific environments. It defines resource variations for each environment (dev, qa, prod).

## How to Use

To generate Kubernetes resources applying the configurations for a specific environment, use the following commands:

### Building kustomize resources:

```bash
# Generate manifests for the dev environment
kustomize build overlays/dev

# Generate manifests for the qa environment
kustomize build overlays/qa

# Generate manifests for the prod environment
kustomize build overlays/prod
