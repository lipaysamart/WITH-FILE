## Helm Install

```sh
helm repo add gitlab https://charts.gitlab.io
helm repo update gitlab
helm upgrade --install gitlab/gitlab-runner -f values.yaml --namespace gitlab-system --version 0.52.1
```

## Reference

- https://archives.docs.gitlab.com/15.11/runner/install/
- https://archives.docs.gitlab.com/15.11/runner/configuration/
- https://archives.docs.gitlab.com/15.11/runner/faq/