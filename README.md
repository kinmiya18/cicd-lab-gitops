# cicd-lab-gitops

Deployment state for the demo service in the **cicd-lab** app repo. Fleet watches this repo and rolls changes out with Helm.

```
demo-app/            Helm chart, one Fleet bundle (Chart.yaml, values.yaml, templates/, fleet.yaml)
fleet/gitrepo.yaml   Fleet GitRepo to apply to the cluster once
```

- `demo-app/values.yaml` → `image.tag` is rewritten by Jenkins on every successful build. Do not edit it by hand except to roll back.
- Roll back: `git revert <ci commit>` and push; Fleet redeploys the previous tag.
- One-time setup (namespace, Harbor pull secret, GitRepo) is described in the app repo README.

Validate locally:

```bash
helm lint demo-app
helm template demo-app demo-app
```
