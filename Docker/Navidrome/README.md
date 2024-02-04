## Usage

```sh
docker-compose up -d
```

### Environment

| ENV                     | Explain                                                |
| :---------------------- | :----------------------------------------------------- |
| ND_LASTFM_APIKEY        | Last.fm 获取的 API Key                                 |
| ND_LASTFM_SECRET        | Last.fm 获取的 Shared Secret                           |
| ND_TRANSCODINGCACHESIZE | 转码缓存的大小。设置 0 为禁用缓存，默认为 100MB        |
| ND_LASTFM_LANGUAGE      | 用于从 Last.fm 检索的语言的两个字母代码，简体中文为 zh |
| ND_SPOTIFY_ID           | Spotify 客户端 ID                                      |
| ND_SPOTIFY_SECRET       | Spotify 客户端 Secret                                  |

### Client

- iOS: substreamer
- 桌面端: Sublime Music（Linux）和 feishin（Windows/Linux/MacOS）