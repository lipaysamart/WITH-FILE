## Usage

```sh
docker compose up -d
```

https://localhost:8081/_explorer/index.html

### Authentication

模拟器发出的每个请求都必须使用基于 TLS/SSL 的密钥进行身份验证

|                   | Value                                                                                                                                        |
| :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| Endpoint          | localhost:8081                                                                                                                               |
| Key               | C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==                                                     |
| Connection string | AccountEndpoint=https://localhost:8081/;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==; |

### Import Emulator Certificate

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

## TroubleShooting

> 该问题一般由 mem 或 cpu 不足引起, 调高后解决。-- 相关 issue 查看 [isues-77](#reference)

```sh
2023-11-07 13:16:41 Started 1/4 partitions
2023-11-07 13:16:42 Started 2/4 partitions
2023-11-07 13:16:43 Started 3/4 partitions
2023-11-07 13:16:54 Started 4/4 partitions
2023-11-07 13:16:54 Started
2023-11-07 13:17:15 
2023-11-07 13:17:15 Unhandled Exception: System.AggregateException: One or more errors occurred. ---> System.ComponentModel.Win32Exception: The system cannot find the file specified
2023-11-07 13:17:15    at System.Diagnostics.Process.StartWithCreateProcess(ProcessStartInfo startInfo)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Common.ProcessUtil.Run(String fileName, String args)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Common.EtwManifest.<>c__DisplayClass7_0.<UnInstallEtwManifest>b__0()
2023-11-07 13:17:15    at Microsoft.Azure.Documents.BackoffRetryUtility`1.<ExecuteRetryAsync>d__5`2.MoveNext()
2023-11-07 13:17:15 --- End of stack trace from previous location where exception was thrown ---
2023-11-07 13:17:15    at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
2023-11-07 13:17:15    at Microsoft.Azure.Documents.ShouldRetryResult.ThrowIfDoneTrying(ExceptionDispatchInfo capturedException)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.BackoffRetryUtility`1.<ExecuteRetryAsync>d__5`2.MoveNext()
2023-11-07 13:17:15    --- End of inner exception stack trace ---
2023-11-07 13:17:15    at System.Threading.Tasks.Task.Wait(Int32 millisecondsTimeout, CancellationToken cancellationToken)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Common.EtwManifest.UnInstallEtwManifest(String manifestFile)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Common.EtwManifest.InstallEtwManifest(String manifestFile)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Services.StartupEntryPoint.ServiceSetupHelper.InstallRuntimeSourceManifest(String manifestPath)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Services.StartupEntryPoint.GatewayServiceSetupCommand.GatewayInit(CodePackageActivationContext context)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Services.StartupEntryPoint.GatewayServiceSetupCommand.<>c.<AddCommand>b__3_1()
2023-11-07 13:17:15    at Microsoft.Extensions.CommandLineUtils.CommandLineApplication.Execute(String[] args)
2023-11-07 13:17:15    at Microsoft.Azure.Documents.Services.StartupEntryPoint.ServiceStartupEntryPoint.Main(String[] args)
2023-11-07 13:17:15 Failed to create CoreCLR, HRESULT: 0x8007000E
...
```

## Reference

- [issues-77](https://github.com/Azure/azure-cosmos-db-emulator-docker/issues/77)