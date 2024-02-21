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

## Usage

### CreateCluster

#### Mysql
```sh
# 创建一个单副本
kbcli cluster create <clusterName> --cluster-definition mysql --cluster-version 5.7.42 --set cpu=0.5,memory=1Gi,storage=20Gi,replicas=1 
```
#### Apecloud-Mysql

```sh
# 创建一个单副本
kbcli cluster create <clusterName> --cluster-definition apecloud-mysql --cluster-version ac-mysql-8.0.30 --set cpu=0.5,memory=1Gi,storage=20Gi,replicas=1 

# 查看事件输出
kbcli cluster list-events <clusterName> -ndemo
```

##### SubCommand

```sh
--tolerations=[]
--pvc name=dataName,size=20Gi
--set storageClass=''
--backup-enabled
```

### BenchMark

```sh
# 创建一个 Mysql BenchMark
kbcli bench sysbench <testName> --cluster=<clusterName> --tables=10  --database=demo --size=25000 --user=root --password=frqtmqp7  -ndemo

# 查看测试 
kbcli bench list -ndemo

# 查看 Pod 日志
k -ndemo logs testingdemo-run-0-l9b8p

# 删除测试
kbcli bench delete <testName> -ndemo
```

## TroubleShooting

- no persistent volumes available for this claim and no storage class is set

原因: 未设置默认`SC`导致

```yaml
# 创建一个默认 SC
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    # 设置默认 SC
    storageclass.kubernetes.io/is-default-class: "true"
  name: nfs-csi-stateful
mountOptions:
- hard
- nfsvers=4.1
parameters:
  server: 172.16.10.254
  share: /mnt/nfs-server/bigdata
provisioner: nfs.csi.k8s.io
reclaimPolicy: Retain
volumeBindingMode: WaitForFirstConsumer
```

## Reference

- [KubeBlocks Install](https://kubeblocks.io/docs/preview/user_docs/installation/install-with-helm/install-kubeblocks-with-helm)
