## 使用

```sh
docker compose up -d
```

https://localhost:8081/_explorer/index.html

### 身份验证

模拟器发出的每个请求都必须使用基于 TLS/SSL 的密钥进行身份验证

|                   | Value                                                                                                                                        |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| Endpoint          | localhost:8081                                                                                                                               |
| Key               | C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==                                                     |
| Connection string | AccountEndpoint=https://localhost:8081/;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==; |

### 导入模拟器证书

`ubuntu/debian` 环境执行

```sh
curl -k https://localhost:8081/_explorer/emulator.pem > ~/emulatorcert.crt
cp ~/emulatorcert.crt /usr/local/share/ca-certificates/
update-ca-certificates
```

`windows` 环境执行

```
双击打开证书文件 -> 安装证书 -> 选择本地机器 -> 选择将所有证书放在以下存储选项中，然后单击浏览。-> 选择受信任根证书颁发机构存储 -> 确定。
```