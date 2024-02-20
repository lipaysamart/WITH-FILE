## Install

### Install CRDs

> CRD 和 程序不建议都用 Helm 装, 以便到时候 Helm 卸载时, CRD 不会被删除。

```sh
kubectl create -f https://github.com/apecloud/kubeblocks/releases/download/v0.8.1/kubeblocks_crds.yaml
```

### Helm Install

>云部署时请注释:

```sh
replicaCount: 3
dnsPolicy: ClusterFirstWithHostNet
hostNetwork: true

snapshot-controller:
  volumeSnapshotClasses:
    - name: csi-alicloud-disk-efficiency

# 必须 3 副本才能开启
admissionWebhooks:
  enabled: true
```

```sh
helm repo add kubeblocks https://apecloud.github.io/helm-charts
helm repo update
```

## Reference

- [KubeBlocks Install](https://kubeblocks.io/docs/preview/user_docs/installation/install-with-helm/install-kubeblocks-with-helm)
