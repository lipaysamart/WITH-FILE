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

>KubeBlocks 支持创建两种类型的 MySQL 集群：Standalone 和 RaftGroup Cluster。 Standalone仅支持一份副本，可用于对可用性要求较低的场景。对于可用性要求较高的场景，建议创建RaftGroup集群，即创建一个三副本集群。并且为了保证高可用性，所有副本默认分布在不同的节点上。

- 查看可用的集群 `kbcli cd ls`
- 查看可用的集群版本 `kbcli cv ls`


#### Community-Mysql

```sh
# 创建单副本
kbcli cluster create <clusterName> --cluster-definition mysql --cluster-version 5.7.42 --set cpu=0.5,memory=1Gi,storage=20Gi,replicas=1

# 创建多副本
kbcli cluster create <clusterName> --cluster-definition mysql --cluster-version 5.7.42 --set cpu=0.5,memory=1Gi,storage=20Gi,replicas=3

# 查看事件输出
kbcli cluster list-events <clusterName> -ndemo

# 查看集群详细信息
kbcli cluster describe <clusterName> -ndemo

# 获取数据库密码
k get secret -ndemo demo-stand-conn-credential  -ojsonpath='{.data.\password}' | base64 -d

# kubectl 连接数据库
k exec -it demo-stand-mysql-0 -ndemo -- bash
```

##### SubCommand

```sh
--tolerations=[]
--set storageClass=''
--backup-enabled
```

#### Apecloud-Mysql

```sh
# 创建单副本
kbcli cluster create mysql <clusterName>  --version ac-mysql-8.0.30 --cpu=0.5 --memory=1 --storage 20 --replicas 1 --mode standalone -ndemo

# 创建高可用
kbcli cluster create mysql <clusterName>  --version ac-mysql-8.0.30 --cpu=0.5 --memory=1 --storage 20 --replicas 3 --mode raftGroup -ndemo

# 查看集群
kbcli cluster list <clusterName> -ndemo

# 查看事件输出
kbcli cluster list-events <clusterName> -ndemo

# 查看集群详细信息
kbcli cluster describe <clusterName> -ndemo

# kbcli 连接数据库
kbcli cluster connect <clusterName> -ndemo
```

### VerticalScaling

```sh
# 确认状态为 Running
kbcli list <clusterName> -ndemo

# 确认集群 Components
kbcli cluster list-components -ndemo

# 使用 kbcli 扩容
kbcli cluster vscale <clusterName> -ndemo --components="mysql" --memory="4Gi" --cpu="2"

# 查看 ops 请求
kbcli cluster describe-ops demo-stand-verticalscaling-tk544 -n demo

# 查看资源实例
kbcli cluster list-instances -ndemo

```

### ExpandVolume

```sh
# 确认状态为 Running
kbcli list <clusterName> -ndemo

# 确认集群 Components
kbcli cluster list-components -ndemo

# 使用 kbcli 扩容
kbcli cluster volume-expand <clusterName> --components="mysql" --volume-claim-templates="data" --storage="25Gi" -ndemo

# 查看 ops 请求
kbcli cluster describe-ops demo-stand-volumeexpansion-4dm5n -n demo

# 查看资源实例
kbcli cluster list-instances -ndemo 
```

### OpsObject

```sh
# 查看所有 ops
kbcli cluster list-ops -ndemo --status all

# 列出 ops
kbcli cluster list-ops -ndemo 

# 删除 ops
kbcli cluster delete-ops --name demo-stand-volumeexpansion-4dm5n -ndemo

# 查看 ops 
kbcli cluster describe-ops --name demo-stand-volumeexpansion-4dm5n -ndemo
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

## Tips

### Lifecycle

>您可以停止/启动集群以节省计算资源。当集群停止时，该集群的计算资源被释放，也就意味着Kubernetes的Pod被释放，但存储资源被保留。如果您想恢复集群资源，可以通过快照的方式重新启动该集群

#### Stop/Start

```sh
# 停止集群
kbcli cluster stop <clusterName>

# 启动集群
kbcli cluster start <clusterName>

# 重启集群
kbcli cluster restart demo-stand --components="mysql" --ttlSecondsAfterSucceed=30  -ndemo 
```

- `ttlSecondsAfterSucceed` 描述了OpsRequest作业在重启成功后的生存时间。

#### Scaling

>扩容会触发 pod 重启，所有 pod 都会按照 learner -> follower ->leader 的顺序重启，并且操作完成后，leader pod 可能会发生变化。

- Scaling 操作之后有三个状态表示

```sh
STATUS=VerticalScaling 表示正在进行垂直缩放
STATUS=Running 表示垂直缩放操作已应用
STATUS=Abnormal 表示垂之缩放异常
```

### HandleAnException

| Status          | Information                                                                                                                  |
| :-------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| Abnormal        | 集群可以访问，但部分Pod异常。这可能是操作过程的中间状态，系统会自动恢复，无需执行任何额外操作。等待集群状态更改为 Running 。 |
| ConditionsError | 集群状态正常，但出现异常。可能是配置丢失或异常导致操作失败。                                                                 |
| Failed          | 无法访问集群。检查 status.message 字符串并获取异常原因                                                                       |
| Creating        | CreateClusterCompPhase 表示正在创建组件。                                                                                    |
| Deleting        | DeletingClusterCompPhase 指示当前正在删除该组件。                                                                            |
| Running         | RunningClusterCompPhase 指示该组件具有超过零个副本，并且所有 pod 都是最新的并处于“正在运行”状态。                            |
| Stopped         | StoppedClusterCompPhase 表示该组件的副本数为零，并且所有 pod 均已被删除。                                                    |  |
| Stopping        | StoppingClusterCompPhase 表示该组件有零个副本，并且有 pod 正在终止。                                                         |
| Updating        | UpdatingClusterCompPhase 表示组件的副本数超过零，并且没有失败的 pod，当前正在更新。                                          |

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
- [KubeBlocks Command Line](https://kubeblocks.io/docs/release-0.8/user_docs/cli)