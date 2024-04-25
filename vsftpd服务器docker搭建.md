#  vsftpd服务器docker搭建 

```bash
docker pull fauria/vsftpd
```

| 环境变量名            | 默认值                  | 可接受值                                        | 描述                                                         |
| :-------------------- | :---------------------- | :---------------------------------------------- | :----------------------------------------------------------- |
| FTP_USER              | admin                   | 任意字符串，避免使用空白和特殊字符              | 默认 FTP 账号的用户名                                        |
| FTP_PASS              | 随机字符串              | 任意字符串                                      | 默认 FTP 账号的密码，如果不指定，将生成一个 16 位的随机字符串。可以通过容器日志获取该值 |
| PASV_ADDRESS          | Docker 主机 IP / 主机名 | 任何 IPv4 地址或主机名 (参见 PASV_ADDR_RESOLVE) | 如果没有指定用于被动模式的 IP 地址，将使用 Docker 主机路由的 IP 地址。请注意这可能是一个本地地址 |
| PASV_ADDR_RESOLVE     | NO                      | <NO                                             | YES>                                                         |
| PASV_ENABLE           | YES                     | <NO                                             | YES>                                                         |
| PASV_MIN_PORT         | 21100                   | 任何有效的端口号                                | 用于被动模式端口范围的下限。请记住使用 docker -p 参数发布端口 |
| PASV_MAX_PORT         | 21110                   | 任何有效的端口号                                | 用于被动模式端口范围的上限。使用大量已发布端口启动容器将需要更长的时间 |
| XFERLOG_STD_FORMAT    | NO                      | <NO                                             | YES>                                                         |
| LOG_STDOUT            | 空字符串                | 任意字符串以启用，空字符串或未定义以禁用        | 通过 STDOUT 输出 vsftpd 日志，以便通过 container logs 访问   |
| FILE_OPEN_MODE        | 0666                    | 文件系统权限                                    | 上传的文件创建时的权限。掩码将应用于此值之上。如果您想要 uploaded 文件可执行，则可以更改为 0777 |
| LOCAL_UMASK           | 077                     | 文件系统权限                                    | 为本地用户设置文件创建的掩码的值。注意！如果您想指定八进制值，请记住 "0" 前缀，否则该值将被视为十进制整数！ |
| REVERSE_LOOKUP_ENABLE | YES                     | <NO                                             | YES>                                                         |
| PASV_PROMISCUOUS      | NO                      | <NO                                             | YES>                                                         |
| PORT_PROMISCUOUS      | NO                      | <NO                                             | YES>                                                         |



docker run -d -v /kfqjcy/vsftpd:/home/vsftpd \
-p 2020:2020 -p 2021:2021 -p 21100-21110:21100-21110 \
-e FTP_USER=root -e FTP_PASS=kfqjcy@2014 \
-e PASV_ADDRESS=192.168.2.20 -e PASV_MIN_PORT=21100 -e PASV_MAX_PORT=21110 \
--name vsftpd --restart=always fauria/vsftpd





这个Docker命令用于运行一个vsftpd（Very Secure File Transfer Protocol Daemon）容器，提供FTP服务。以下是各个参数的中文解释：

- `-d`：以后台（detached）模式运行容器。

- `-v /my/data/directory:/home/vsftpd`：将本地系统中`/my/data/directory`目录挂载到容器内的`/home/vsftpd`目录，实现数据持久化。

- ```
  -p 20:20 -p 21:21 -p 21100-21110:21100-21110
  ```

  ：将容器内的端口映射到主机上。具体来说：

  - 端口20用于FTP数据连接。
  - 端口21用于FTP控制连接。
  - 端口21100-21110用于被动模式（PASV mode）的数据连接范围。

- `-e FTP_USER=myuser -e FTP_PASS=mypass`：设置FTP用户的用户名和密码。在这里，用户名为`myuser`，密码为`mypass`。

- ```
  -e PASV_ADDRESS=127.0.0.1 -e PASV_MIN_PORT=21100 -e PASV_MAX_PORT=21110
  ```

  ：配置被动模式的相关参数。具体来说：

  - `PASV_ADDRESS`指定被动模式数据连接的地址，这里设置为`127.0.0.1`。
  - `PASV_MIN_PORT`和`PASV_MAX_PORT`指定了被动模式数据连接的端口范围，这里设置为21100-21110。

- `--name vsftpd`：为容器指定一个名称，这里是`vsftpd`。

- `--restart=always`：设置容器总是在退出时重新启动。

- `fauria/vsftpd`：指定要使用的vsftpd镜像。