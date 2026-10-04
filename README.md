# Sample Python App deployment values

This repository holds the Helm values used to deploy `sample-python-app`. Argo CD ownership is centralized in [`scf-k8s-argocd`](https://github.com/pandeyaws/scf-k8s-argocd), which defines the app's `AppProject` and `ApplicationSet`. The ApplicationSet combines the chart from the app repository with `argocd/values/dev.yaml` from this repository.

The central ApplicationSet uses Argo CD multi-source Applications, which require Argo CD 2.6 or later.

The app repository publishes an image for each `vMAJOR.MINOR.PATCH` tag and sends a `repository_dispatch` event here. The workflow updates `argocd/values/dev.yaml`, which the central Argo CD configuration then reconciles.

The app repository's `INFRA_REPO_TOKEN` Actions secret must be a fine-grained token with Contents read/write access to this repository. The default branch must be named `master`, as both the central ApplicationSet and update workflow target that branch.
