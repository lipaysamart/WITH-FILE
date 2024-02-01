## Helm Install

```sh
helm repo add gitlab-operator https://gitlab.com/api/v4/projects/18899486/packages/helm/stable
helm repo update
helm upgrade --install gitlab-operator gitlab-operator/gitlab-operator --create-namespace --namespace gitlab-system --version 0.28.1
```

## Reference

- https://archives.docs.gitlab.com/15.11/operator/