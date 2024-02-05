## Usage

```sh
docker-compose up -d
```

## Certificate issuance

- 通过 `http` 请求验证，会向 `http://你的域名/.well-known/acme-challenge/` 发送一条请求。
- 通过 `dns` 解析，需要在域名解析中增加一条 `_acme-challenge.你的域名` 的 `TXT` 记录用于验证。

|     Flags      |    Method    |                                   Explain                                    |
| :------------: | :----------: | :--------------------------------------------------------------------------: |
|   `--manual`   | **手动配置** |                 自己选择 **http** 验证还是 **dns** 验证都行                  |
|   `--nginx`    |   **http**   | 自动修改 **nginx** 的配置文件，增加 **http** 请求的访问配置及后续的 ssl 配置 |
|  `--dns-xxx`   |   **dns**    |              配置好后会自动在域名解析中增加一条记录进行域名验证              |
| `--standalone` |   **http**   |                            需要保证80端口别被占用                            |
|  `--webroot`   |   **http**   |  需要手动配置 `/.well-known/acme-challenge/` 使其对应到自动生成的验证文件上  |

```sh
docker-compose run --rm  certbot certonly --manual --preferred-challenges=dns --email lipaysamart@gmail.com --server https://acme-v02.api.letsencrypt.org/directory --webroot-path /usr/share/certbot/www/ --dry-run -d YOUR_DOMAIN -d *.YOUR_DOMAIN

# 输入邮箱等信息，如果提示The dry run was successful则说明成功，可以去掉--dry-run参数来进行实际的获取证书
```

## Certificate renewal

```sh
# 1. 切换到docker-compose.yml所在目录；
# 2. 然后更新证书，只有距离过期时间30天内才会真正成功；
# 3. 然后重载 nginx 使新证书生效

0 0 1,15 * * cd /home/ubuntu/compose && /usr/local/bin/docker-compose run --rm certbot renew  && /usr/local/bin/docker-compose restart nginx 
```