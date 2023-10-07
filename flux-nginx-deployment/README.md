# FluxCD Setup and Deployment Guide

In this guide, we demonstrate the process of setting up and deploying resources using FluxCD. The resources are organized into a structured directory, and we leverage Flux's ability to work with Kustomize to apply configurations across different environments.

<br/>

## Installation and Setup:

```bash
# Create a namespace for Flux:
k create ns flux-system

# Install Flux in the created namespace:
flux install --namespace=flux-system

# Confirm the components are running:
k get all -n flux-system

# Create a secret for Flux to access the git repository:
flux create secret git flux-ssh-key \
  --url=ssh://git@github.com/somaz94/kustomize-study.git \
  --private-key-file=/home/somaz/.ssh/id_rsa_somaz94 \
  --export > flux-secret.yaml

k apply -f flux-secret.yaml -n flux-system

# Create a source from the git repository:
flux create source git nginx-source \
  --url=ssh://git@github.com/somaz94/kustomize-study.git \
  --branch=main \
  --namespace=flux-system \
  --secret-ref=flux-ssh-key \ 
  --export > git-source.yaml

kubectl apply -f git-source.yaml -n flux-system

# Setup Kustomizations for different environments:

# Development
flux create kustomization nginx-dev-kustomization \
  --source=nginx-source \
  --path="flux-nginx-deployment/overlays/dev" \
  --prune=true \
  --interval=1m \
  --namespace=flux-system

# Production
flux create kustomization nginx-prod-kustomization \
  --source=nginx-source \
  --path="flux-nginx-deployment/overlays/prod" \
  --prune=true \
  --interval=1m \
  --namespace=flux-system

# QA
flux create kustomization nginx-qa-kustomization \
  --source=nginx-source \
  --path="flux-nginx-deployment/overlays/qa" \
  --prune=true \
  --interval=1m \
  --namespace=flux-system


# Check the created resources:
k get kustomizations.kustomize.toolkit.fluxcd.io -n flux-system


# Cleanup:

# Delete Kustomizations
k delete kustomizations.kustomize.toolkit.fluxcd.io -n flux-system nginx-dev-kustomization

# Delete Git Repository source
k delete gitrepositories.source.toolkit.fluxcd.io -n flux-system nginx-source

# Delete Secret
k delete secrets -n flux-system flux-ssh-key


# Uninstall Flux:
flux uninstall --namespace=flux-system
```

<br/>

## References:
- [FluxCD official documentation](https://fluxcd.io)
- [Flux Command Reference](https://fluxcd.io/flux/cmd/)
