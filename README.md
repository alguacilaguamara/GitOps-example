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

## How to check
```sh
flux get sources git
flux get kustomizations
flux logs
```


