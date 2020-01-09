## 简介

本文是方便快速的安装多节点的elasticsearch 集群，集群分为4种角色。

环境：Ubuntu 16.04 ansible 2.9 elasticsearch-6.7.0.deb

使用ansible完成服务器的配置、es安装、配置等。下面基于文件来说明各个脚本的作用。

## 准备

- 阿里云国内镜像配置：/etc/apt/sources.list 
- 提前下载：elasticsearch-6.7.0.deb
- 提前安装：ansible 2.9
- 提前生成ansible节点的公钥

## 文件说明

### hosts

ansible的基础配置文件，来配置需要操作的机器和变量。

其中配置了es-master、es-hot、es-warm、es-cold四种角色的节点。

es-master： 可选举为master的节点。

es-hot：热数据节点。存储写入、查询都很频繁的数据。

es-warm：温数据节点。存储查询较多但是写入较少的数据。

es-cold：冷数据节点。存储查询和写入都较少的数据。



### 1py-install.yaml

ansible依赖python，但是新的节点如果没有安装python，ansible默认自带的raw模块可以用来安装python，各个节点有了python之后就可以愉快的使用ansible的其他模块了。



### 2ssh-push.yaml

用来实现ansible节点和各个被操作节点无密码访问



### 3es-setup.yaml

elasticsearch安装时候的服务器配置、依赖安装、elasticsearch-6.7.0.deb安装等等：

1. 关闭swap
2. 备份、替换、更新ubuntu的源，使用国内阿里云的源
3. 安装htop工具
4. 把所有节点都增加到`/etc/hosts`文件中
5. 安装`openjdk-8-jre`
6. 维护`/etc/sysctl.conf` 的`net.ipv4.tcp_keepalive_intvl` `net.ipv4.tcp_keepalive_probes` `net.ipv4.tcp_keepalive_time` `vm.max_map_count`
7. 校验是否安装、拷贝elasticsearch-6.7.0.deb到各个节点
8. 配置ES的JVM xms和xmx参数为当前节点内存的一半
9. 在挂载的数据盘创建ES专用的数据目录
10. 修改各个节点的hostname
11. 修改多处ES 内存锁定参数
12. 根据角色修改ES的配置文件：/etc/elasticsearch/elasticsearch.yml



### 4xpack-config.yaml

安装启动完成kibana和elasticsearch之后，可以在management中上传商业版本的license，之后可以生成各个节点间、客户端和节点间的安全通信认证：

1. 创建认证文件存储的目录
2. 拷贝生成的两个生成文件到目录
3. 修改关于xpack的配置

## 其他操作

启动elasticsearch

```shell
ansible all -m systemd -a "state=restarted name=elasticsearch"
```

