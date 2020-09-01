### 配置

建议在第一次启动之前进行简单的配置，在`config`文件夹找到`zookeeper.properties`文件，将其中的`dataDir`配置为自行指定的文件夹：

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gib7p3ex2gj315s0t4487.jpg)

在Mac终端下，分别依次开启两个命令行窗口，并切换到kafka的主目录下，分别执行如下命令开启zookeeper和kafka

### 启动zookeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

### 启动kafka

```bash
bin/kafka-server-start.sh config/server.properties
```