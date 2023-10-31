# GitOps-example
## Installation of FluxCD in the Cluster

flux bootstrap github \
  --owner=alguacilaguamara \
  --repository=GitOps-example \
  --branch=main \
  --path=./ \
  --personal

Source: https://fluxcd.io/flux/guides/image-update/

## How to check
flux get sources git
flux get kustomizations
flux logs

