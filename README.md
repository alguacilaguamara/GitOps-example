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

## Installation of ArgoRollout
```sh
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```
> Source https://argoproj.github.io/argo-rollouts/installation/#controller-installation

## Install Crossplane to deploy infrastructure on AWS
```sh
helm repo add \
crossplane-stable https://charts.crossplane.io/stable
helm repo update
```
```sh
helm install crossplane \
crossplane-stable/crossplane \
--namespace crossplane-system \
--create-namespace
```
Verifying crossplane installation
```sh
kubectl get pods -n crossplane-system
```
```sh
kubectl api-resources  | grep crossplane
```
Now we are going to take the steps to be able to deploy infrastructure in aws. For this we need the AWS provider.
```sh
cat <<EOF | kubectl apply -f -
apiVersion: pkg.crossplane.io/v1
kind: Provider
metadata:
  name: provider-aws-s3
spec:
  package: xpkg.upbound.io/upbound/provider-aws-s3:v0.37.0
EOF
```
And check the new provider
```sh
kubectl get providers
```
Now you need to create a new aws-credentials.txt
```
[default]
aws_access_key_id = 
aws_secret_access_key = 
```
And create a secret with your credentials
```sh
kubectl create secret \
generic aws-secret \
-n crossplane-system \
--from-file=creds=./aws-credentials.txt
```
To finish we create a providerconfig with your AWS credentials
```sh
cat <<EOF | kubectl apply -f -
apiVersion: aws.upbound.io/v1beta1
kind: ProviderConfig
metadata:
  name: default
spec:
  credentials:
    source: Secret
    secretRef:
      namespace: crossplane-system
      name: aws-secret
      key: creds
EOF
```

> Source: https://docs.crossplane.io/latest/getting-started/provider-aws/


