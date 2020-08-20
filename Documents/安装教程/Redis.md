# Mac OS 下安装 Redis

## 安装

安装的命令也挺简单的，不过要留意中间有可能出现编译失败的错误。我就出现了。

```
# 解压
tar -zxvf redis-5.0.5.tar.gz
# 拷贝的local目录下
sudo cp -rf redis-5.0.5 /usr/local/
# 进入相应目录下
cd /usr/local/redis-5.0.5
# 编译 - 时间有点长，可能要等几分钟
sudo make test
# 安装
sudo make install
# 建立相应目录
sudo mkdir bin etc db
# 拷贝启动文件
sudo cp src/mkreleasehdr.sh src/redis-benchmark src/redis-check-rdb src/redis-cli src/redis-server bin/
```

> 注意：如果像我一样在在编译过程中出现错误。命令清理了再编译一下： `sudo make distclean && sudo make && sudo make test`

这个时候就可以启动 Redis 玩耍了。

## 服务启动

直接命令行启动。

```
redis-server
复制代码
```

启动后，就会看到命令行出现 Redis 的图案，这样启动时是使用默认的配置，也就是目录下的 `redis.conf`。

个人建议：如果说非必要，或者不清楚修改配置的后果和作用，还是使用默认配置，可以避免很多麻烦，当然如果是用在大型生产环境，那还是请教专业的 DB大神 如何配置部署吧。

结束redis服务：

- 直接结束进程窗口；
- 新开命令行窗口，通过启动 Redis 客户端 关闭

```
redis-cli
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> SHUTDOWN
not connected> exit
复制代码
```

## Redis 客户端

上方说到直接 `redis-cli` 启动 Redis 客户端 客户端基本命令

|     命令      |       用途        |
| :-----------: | :---------------: |
|    keys *     | 查看所有key的内容 |
|  exists key   | 查看 key 是否存在 |
| set key value |   设置key的内容   |
|    get key    |   获取key的内容   |
|   flushall    |     清空所有      |