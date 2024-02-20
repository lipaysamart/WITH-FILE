## Install

### Install CRDs

> CRD 和 程序不建议都用 Helm 装, 以便到时候 Helm 卸载时, CRD 不会被删除。

```sh
kubectl create -f https://github.com/apecloud/kubeblocks/releases/download/v0.8.1/kubeblocks_crds.yaml
```

### Helm Install

>云部署时请修改:

```sh
replicaCount: 3

# 必须 3 副本才能开启
admissionWebhooks:
  enabled: true

```

```sh
helm repo add kubeblocks https://apecloud.github.io/helm-charts
helm repo update

helm install kubeblocks kubeblocks/kubeblocks \
    --namespace kb-system --create-namespace \
    --set-json 'tolerations=[ { "key": "control-plane-taint", "operator": "Equal", "effect": "NoSchedule", "value": "true" } ]' \
    --set-json 'dataPlane.tolerations=[{ "key": "data-plane-taint", "operator": "Equal", "effect": "NoSchedule", "value": "true" } ]' \
    --dry-run
```

## Reference

- [KubeBlocks Install](https://kubeblocks.io/docs/preview/user_docs/installation/install-with-helm/install-kubeblocks-with-helm)
