# GitOps-example
## Installation of FluxCD in the Cluster

```sh
flux bootstrap github \
  --owner=alguacilaguamara \
  --repository=GitOps-example \
  --branch=main \
  --path=./ \
  --personal
```

Make your changes in the repo and then:

```sh
flux reconcile kustomization flux-system --with-source
```

> Source: https://fluxcd.io/flux/guides/image-update/

### How to check
```sh
flux get sources git
flux get kustomizations
flux logs
```

## Installation of ArgoCD in the Cluster

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
> Source: https://argo-cd.readthedocs.io/en/stable/

## Installation of ArgoRollout in the Cluster
```sh
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```
> Source https://argoproj.github.io/argo-rollouts/installation/#controller-installation
