
## Install 

>安装过程中，需要使用解压工具（unzip、7z）解压软件包，请提前安装其中的一个解压工具。
>>安装完成后，ossutil会安装到/usr/bin/目录下。

`sudo -v ; curl https://gosspublic.alicdn.com/ossutil/install.sh | sudo bash`

## Configure

```sh
cat > ~/.ossutilconfig <<-EOF
[Credentials]
language=EN
endpoint=oss-cn-shenzhen.aliyuncs.com
accessKeyID=
accessKeySecret=
EOF
```

## Usage

```sh
./ossutil64 -c ~/.ossutilconfig sync oss://bucketName/destfolder/
```

## Reference

- [ossutil install](https://help.aliyun.com/zh/oss/developer-reference/install-ossutil?spm=a2c4g.11186623.0.0.a2313943nj8lbi) 
- [ossutil help](https://help.aliyun.com/zh/oss/developer-reference/overview-59?spm=a2c4g.11186623.0.0.7db5255cbAxNwm)