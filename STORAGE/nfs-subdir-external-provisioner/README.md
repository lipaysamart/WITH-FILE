## NFS - 实践

### 环境说明

* Ubunut 20.04

### 安装

* 服务端
```shell
$ apt update
$ apt install nfs-kernel-server

# 创建共享目录&权限
$ mkdir -p /mnt/nfs-server
$ chmod 755 /mnt/nfs-server

# 添加配置文件 
$ cat >> /etc/exports <<-EOF
/mnt/nfs-server  *(rw,sync,no_root_squash)
EOF

# 启动程序
$ systemctl enable nfs-kernel-server
$ systemctl status nfs-kernel-server
```

* CSI插件
```shell
# 添加仓库
$ helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
$ helm repo update
# 安装
$ helm upgrade -n kube-system --install nfs-storage \
nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
--version 4.0.8 \
--set nfs.server=<Your NFS ServerIP> \
--set nfs.path=<Your Mount Path> \
--set image.repository=registry.cn-guangzhou.aliyuncs.com/kubernetes-default/nfs-subdir-external-provisioner \
--set image.tag=v4.0.2 

$ k get sc
--------
NAME            PROVISIONER                                             RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
nfs (default)   cluster.local/nfs-csi-nfs-subdir-external-provisioner   Delete          Immediate           true                   55m
```

### 使用

```shell
$ k apply -f ci/test.yaml

---
$ k get po 
NAME                     READY   STATUS    RESTARTS   AGE
mysql-6f49b5469c-92n4k   1/1     Running   0          76s

$ k get pvc 
NAME        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pvc   Bound    pvc-2a67e6e6-8d2c-4ae1-9ab6-d54cddb407be   1Gi        RWO            nfs            89s
```

##### 配置文件说明
```text
/var/lib/k8s/data：是共享的数据目录
*：表示任何人都有权限连接，当然也可以是一个网段，一个 IP，也可以是域名
rw：读写的权限
sync：表示文件同时写入硬盘和内存
no_root_squash：当登录 NFS 主机使用共享目录的使用者是 root 时，其权限将被转换成为匿名使用者，通常它的 UID 与 GID，都会变成 nobody 身份
```

