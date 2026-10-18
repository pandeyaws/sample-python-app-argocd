# Sample Python App Argo CD configuration

This repository contains the Argo CD `ApplicationSet` manifest at
[`argocd/sample-python-app-appset.yaml`](./argocd/sample-python-app-appset.yaml).
The parent Argo CD `Application` and `AppProject` are managed in
[`scf-k8s-argocd`](https://github.com/pandeyaws/scf-k8s-argocd).

## Generated applications

The `sample-python-app` ApplicationSet uses a list generator to create:

| Environment | Generated Application | Cluster | Namespace |
| --- | --- | --- | --- |
| `dev` | `sample-python-app-dev` | `in-cluster` | `sample-python-app-dev` |
| `prd` | `sample-python-app-prd` | `aks-cluster-dev-eus-02` | `sample-python-app-prd` |

Each generated Application deploys the Helm chart at `helm/sample-python-app`
from [`sample-python-app`](https://github.com/pandeyaws/sample-python-app).
Its base values (`values.yaml`) and environment overrides
(`values/dev.yaml` or `values/prd.yaml`) are also loaded from that chart
repository, at the revision configured for that environment in the generator.
The image tag is set to the same revision.

The app repository's GitHub Actions deployment workflow updates and syncs the
dev and production applications from this ApplicationSet. To change an
environment's deployed version or cluster, update its corresponding generator
entry. To change Helm settings, edit the values files in the app repository.
