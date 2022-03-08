# Docker 安装ttnode

## **ttbode 项目资源**

{% embed url="https://hub.docker.com/r/tiptime/ttnode" %}
ttnode 项目地址
{% endembed %}

**Docker Pull Command**

```bash
docker pull tiptime/ttnode
```

## **一、 通过 host 模式部署（单实例）**

```bash
sudo docker run -d \
  -v /mnt/host/dir:/mnt/data/ttnode \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name ttnode \
  --hostname ttnode \
  --net=host \
  --restart=always \
  --memory=2g \
  --cpus=1.5 \
  tiptime/ttnode:latest
```

**参数解释**

1. `-v /mnt/data/ttnode:/mnt/data/ttnode` (可自行修改本机缓存目录) /mnt/host/dir 表示本机缓存目录， /mnt/data/ttnode docker 内部缓存目录不需要修改
2. `-v /var/run/docker.sock:/var/run/docker.sock` 支持自动更新
3. `--name ttnode` 容器名称
4. `--net=host` (单实例不推荐修改) 目前推荐 docker host 模式，就是 docker 网络环境与设备环境一致，适合单拨。
5. `--hostname ttnode` (不推荐修改) ttnode uid 由 mac 地址和 hostname 决定，mac 地址和hostname 不变，ttnode uid 才能不变。mac 地址是主机地址了，所以指定 hostname 给容器，就可以固定 uid 了。
6. `--restart=always` (不推荐修改) 自动重启，除非 docker 出错
7. `--memory=2g` 限制内存 2g
8. `--cpus=1.5` 限制 cpu 核心数

## 二、 通过脚本部署 maclvan 模式（支持多实例）

```shell
wget https://gitee.com/jimyfar/ttnode_auto_deploy/raw/master/setup_docker.sh
sudo chmod +x setup_docker.sh

# 第一次运行

# --eth 网卡
# --cache_dir 缓存目录
# --ttnode_count  ttnode docker实例数
sudo ./setup_docker.sh --eth=eth0 --cache_dir=/mnt/ttnode --memory=1g --cpus=1.5 --ttnode_count=2 


# 之后运行用下面这行即可，保证 mac 地址不变
sudo ./setup_docker.sh # 直接读取配置文件
```

配置文件在 /etc/ttnode/config.txt # 包含 容器名 hostname mac地址等信息

## 三、 查看 ttnode 状态

```shell
sudo docker container ls # 查看所有容器
```

**浏览器**

```shell
sudo docker inspect 容器名 -f "{{json .NetworkSettings.Networks.macnet.IPAddress }}" # 查看容器ip
http://容器IP:1024  # 浏览器 查看二维码  端口
```

**命令行**

```shell
sudo docker logs -f 容器名 # 查看二维码  端口
```

**删除 ttnode**

```shell
sudo docker rm -f 容器名  # 删除容器
```

#### 检查 ttnode 挂载

```shell
docker inspect -f '{{ .Mounts }}'  容器名

# 预期 [{bind  /mnt/host/dir /mnt/data/ttnode   true rprivate} ......]
# 左边是本机挂载目录 /mnt/host/dir ，右边必须是 /mnt/data/ttnode 
```
