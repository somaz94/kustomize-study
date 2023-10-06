# argocd-applicationset-nginx-deployment

This repository demonstrates the usage of ArgoCD ApplicationSet in combination with Kustomize.

<br/>

## Directory Structure

- `app/nginx/base`: Contains the base resources for the Nginx application.
- `app/nginx/overlays`: Houses resource configurations tailored for specific environments (dev, qa, prod).
- `appsets`: Contains the definition for ArgoCD ApplicationSet.
- `github-secret`: Defines the secret for accessing the GitHub repository.

<br/>

## Deployment Workflow

1. **Create Secret and Project in ArgoCD**: Establish a connection to the GitHub repository by creating the necessary Secret and Project in ArgoCD.
2. **Write and Upload Templates**: After designing the necessary templates, upload them to the GitHub repository.
3. **Deploy ApplicationSet in ArgoCD**: Once the templates are in place, deploy the ApplicationSet using ArgoCD.

<br/>

## How to Use

To generate Kubernetes resources with configurations for a specific environment, use the following commands:

<br/>

### Building kustomize resources:

```bash
# Generate manifests for the dev environment
kustomize build app/nginx/overlays/dev

# Generate manifests for the qa environment
kustomize build app/nginx/overlays/qa

# Generate manifests for the prod environment
kustomize build app/nginx/overlays/prod
```

<br/>

### Building and deploying kustomize resources:

```bash
kustomize build app/nginx/overlays/dev | kubectl apply -f - --namespace=dev-nginx

kustomize build app/nginx/overlays/qa | kubectl apply -f - --namespace=qa-nginx

kustomize build app/nginx/overlays/prod | kubectl apply -f - --namespace=prod-nginx

or

kubectl apply -k app/nginx/overlays/dev/
kubectl apply -k app/nginx/overlays/qa/
kubectl apply -k app/nginx/overlays/prod/
```

<br/>

### Post-Deployment Resource Check and Testing
Once you've deployed your resources, you can verify them for specific namespaces:

For dev:
```bash
k get po,svc,ingress -n dev-nginx
```

Expected output:
```bash
NAME                                       READY   STATUS    RESTARTS   AGE
pod/dev-nginx-deployment-8d545c96d-2w7sw   1/1     Running   0          88s

NAME                        TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/dev-nginx-service   ClusterIP   10.233.21.0   <none>        80/TCP    88s

NAME                                          CLASS    HOSTS                        ADDRESS        PORTS   AGE
ingress.networking.k8s.io/dev-nginx-ingress   <none>   dev-nginx.fgn.nerdystar.io   10.10.100.22   80      88s
```


