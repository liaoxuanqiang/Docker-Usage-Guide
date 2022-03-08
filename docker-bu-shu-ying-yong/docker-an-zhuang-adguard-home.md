# Docker 安装AdGuard Home

AdGuard Home 是一款全网广告拦截与反跟踪软件。在您将其安装完毕后，它将保护您所有家用设备，同时您不再需要安装任何客户端软件。随着物联网与连接设备的兴起，掌控您自己的整个网络环境变得越来越重要。

{% embed url="https://adguard.com/zh_cn/welcome.html" %}
AdGuard Home 官网
{% endembed %}

## AdGuard Home 项目资源

{% embed url="https://github.com/AdguardTeam/AdguardHome" %}
AdGuard Home - Github
{% endembed %}

## AdGuard Home 安装

**Docker Pull AdGuardHome Image**

```bash
docker pull adguard/adguardhome
```

### ‎创建并运行容器‎

```bash
docker run -d \
    --name adguardhome \
    -v $PWD/adguardhome/work:/opt/adguardhome/work \
    -v $PWD/adguardhome/conf:/opt/adguardhome/conf \
    -p 53:53/tcp \
    -p 53:53/udp \
    -p 67:67/udp \
    -p 68:68/tcp \
    -p 68:68/udp \
    -p 80:80/tcp \
    -p 443:443/tcp \
    -p 853:853/tcp \
    -p 3000:3000/tcp \
    adguard/adguardhome
```

```bash
docker run --name adguardhome\
    --restart unless-stopped\
    -v /my/own/workdir:/opt/adguardhome/work\
    -v /my/own/confdir:/opt/adguardhome/conf\
    -p 53:53/tcp -p 53:53/udp\
    -p 67:67/udp -p 68:68/udp\
    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp\
    -p 853:853/tcp\
    -p 784:784/udp -p 853:853/udp -p 8853:8853/udp\
    -p 5443:5443/tcp -p 5443:5443/udp\
    -d adguard/adguardhome
```



打开浏览器并导航到 ‎[‎http://127.0.0.1:3000/‎](http://127.0.0.1:3000)‎ 以控制您的 AdGuard 家庭版服务

不要忘记使用您自己的‎**‎数据‎**‎和‎**‎配置‎**‎目录！

`-p 53:53/tcp -p 53:53/udp`‎:‎‎纯域名解析‎.&#x20;

`-p 67:67/udp -p 68:68/tcp -p 68:68/udp`: ‎将 AdGuard Home 用作 DHCP 服务器

`-p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp`: ‎使用 AdGuard Home 的管理面板以及将 AdGuard Home 作为 ‎‎HTTPS/DNS-over-HTTPS‎‎ 服务器运行

`-p 853:853/tcp`: ‎添加是否要将 AdGuard Home 作为 ‎‎DNS-over-TLS‎‎ 服务器运行‎.&#x20;

`-p 784:784/udp -p 853:853/udp -p 8853:8853/udp`: ‎将 AdGuard Home 作为 ‎‎DNS-over-QUIC‎‎ 服务器运行。您只能留下其中的一两个‎.。

`-p 5443:5443/tcp -p 5443:5443/udp`: ‎将AdGuard Home作为‎‎DNSCrypt‎‎服务器运行。

### ‎控制容器‎

开启：`start adguardhome`&#x20;

停止：`docker stop adguardhome`&#x20;

删除：`docker rm adguardhom`

### 更新到较新版本‎

1.  ‎从 Docker Hub 拉出新版本:

    ```
    docker pull adguard/adguardhome
    ```
2.  停止并删除当前正在运行的容器（假设容器被命名为‎`adguardhome`):

    ```
    docker stop adguardhome
    docker rm adguardhome
    ```
3. ‎使用上一节中的命令使用新映像创建和启动容器‎.
