# ArgoCD First Steps

This project provides the initial steps required to manage a Kubernetes application using ArgoCD on an existing cluster.

## Install ArgoCD

### Prerequisites

- Kubernetes: >=1.25.0-0
- Helm v3.0.0+

### Installing the Chart

To install the chart with the release name `argocd`:

```sh
kubectl create namespace argocd
```

```sh
helm repo add argo https://argoproj.github.io/argo-helm
```

```sh
helm install argocd argo/argo-cd
```

## Access ArgoCD UI

We use the default installation, which generates a random password for the admin user. To get the password we run:

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Now we expose the argocd service locally:

```sh
kubectl port-forward service/argocd-server -n argocd 8080:443
```

Navigate to <http://localhost:8080>

## Create the first ArgoCD application

Create the ArgoCD applications for the workflows. This will generate one application for development and production environments. We use the [Apps of Apps Pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/).

```sh
kubectl apply -f bootstrap-argo-apps.yaml
```

## Uninstall ArgoCD

### Remove the ArgoCD applications

```sh
kubectl apply -f bootstrap-argo-apps.yaml
```

### Uninstall ArgoCD chart

```sh
helm install argocd argo/argo-cd -n argocd
```

### Remove ArgoCD namespace

```sh
kubectl delete ns argocd
```
