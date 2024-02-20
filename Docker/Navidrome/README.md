## Usage

```sh
docker-compose up -d

# 使用 filebrowser 管理 music 目录
docker-compose --profile with-deps up -d
```

### Environment

| ENV                        | Explain                                                |
| :------------------------- | :----------------------------------------------------- |
| ND_LASTFM_APIKEY           | Last.fm 获取的 API Key                                 |
| ND_LASTFM_SECRET           | Last.fm 获取的 Shared Secret                           |
| ND_TRANSCODINGCACHESIZE    | 转码缓存的大小。设置 0 为禁用缓存，默认为 100MB        |
| ND_LASTFM_LANGUAGE         | 用于从 Last.fm 检索的语言的两个字母代码，简体中文为 zh |
| ND_SPOTIFY_ID              | Spotify 客户端 ID                                      |
| ND_SPOTIFY_SECRET          | Spotify 客户端 Secret                                  |
| ND_ENABLETRANSCODINGCONFIG | 设置为 true 才能支持转码功能                           |
| ND_TRANSCODINGCACHESIZE    | 转码缓存的大小。设置 0 为禁用缓存，默认为 100MB        |

### Client

- iOS: substreamer
- 桌面端: feishin（Windows/Linux/MacOS）
- Android: Ultrasonic

## Internal

### Use Webadv External Storage

>开机自动挂载 Webadv

```sh
# 安装davfs2工具
apt install davfs2
# 修改配置 use_locks的1改为0
vim /etc/davfs2/davfs2.conf 
# 修改secrets文件
vim /etc/davfs2/secrets
# 添加账号信息
https://pan.cloud.com/dav user password
# 添加开机挂载命令
vim /etc/rc.local
# 末尾添加挂载命令
mount -t davfs https://pan.cloud.com/dav /cloud
# 添加执行权限
chmod +x /etc/rc.local
```
`df -h` 查看是否挂载成功

## Reference

- [filebrowser docs](https://filebrowser.org/)
- [navidrome docs](https://www.navidrome.org/docs/)
- [navidrome support client](https://www.navidrome.org/docs/overview/#apps)
