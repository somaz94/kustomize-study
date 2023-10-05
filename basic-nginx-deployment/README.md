# basic-nginx-deployment

This repository provides an example to understand and practice the usage of Kustomize.

<br/>

## Directory Structure

- `base`: The directory where the base resources are defined.
- `overlays`: The directory that contains resource configurations tailored to specific environments. It defines resource variations for each environment (dev, qa, prod).
- `manifests`: Contains pre-generated Kubernetes manifest files that are a result of applying the kustomize build command on the overlays. These can be directly applied to a Kubernetes cluster without the need for further Kustomize processing.

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
kustomize build overlays/dev > manifests/dev-nginx.yaml
kustomize build overlays/qa > manifests/qa-nginx.yaml
kustomize build overlays/prod > manifests/prod-nginx.yaml
```

<br/>

### Building and deploying kustomize resources:
```bash
kustomize build overlays/dev | kubectl apply -f - --namespace=dev-nginx

kustomize build overlays/qa | kubectl apply -f - --namespace=qa-nginx

kustomize build overlays/prod | kubectl apply -f - --namespace=prod-nginx
```

or

```bash
kubectl apply -k overlays/dev/
kubectl apply -k overlays/qa/
kubectl apply -k overlays/prod/
```

<br/>

## Post-Deployment Resource Check and Testing

Once you have deployed your resources, you can verify them in the specific namespace:
```bash
kubectl get po,svc,ingress -n dev-nginx
```

You should see an output similar to:
```bash
NAME                                       READY   STATUS    RESTARTS   AGE
pod/dev-nginx-deployment-8d545c96d-48rqx   1/1     Running   0          2m37s

NAME                        TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/dev-nginx-service   ClusterIP   10.233.6.66   <none>        80/TCP    2m37s

NAME                                          CLASS    HOSTS                        ADDRESS        PORTS   AGE
ingress.networking.k8s.io/dev-nginx-ingress   <none>   dev-nginx.fgn.nerdystar.io   10.10.100.22   80      2m37s


```

To test if your deployment is serving the expected content, you can send a request using curl:
```bash
curl 10.233.6.66

or

curl dev-nginx.somaz.link
```

The expected response would be the default nginx welcome page:
```bash
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
...
```

After testing, if you wish to clean up the resources, you can delete them:
```bash
kubectl delete -k overlays/dev
```

<br/>

- Note: Ensure that you have a directory named manifests before attempting to save the kustomize manifests in the suggested paths.
