# cicd-lab-gitops

Deployment state for the demo service in the [cicd-lab-app](https://github.com/kinmiya18/cicd-lab-app) repo. Fleet watches this repo and rolls changes out with Helm.

```
demo-app/            Helm chart, one Fleet bundle (Chart.yaml, values.yaml, templates/, fleet.yaml)
fleet/gitrepo.yaml   Fleet GitRepo to apply to the cluster once
```

- `demo-app/values.yaml` → `image.tag` is rewritten by Jenkins on every successful build (commit `ci: bump demo-app image tag to <sha>`). Do not edit it by hand except to roll back.
- Roll back: `git revert <ci commit>` and push; Fleet redeploys the previous tag (shown in `docs/evidence/08-rollback.txt` of the app repo).
- Fleet picks the chart's own `values.yaml`; do not add `valuesFiles` to `fleet.yaml`, it made the chart render without its default values.
- One-time setup: `kubectl apply -f fleet/gitrepo.yaml`.

Validate locally:

```bash
helm lint demo-app
helm template demo-app demo-app
```
