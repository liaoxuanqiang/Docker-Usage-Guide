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

### 设置 <a href="#toc_1" id="toc_1"></a>

#### 常规设置 <a href="#toc_2" id="toc_2"></a>

文字介绍已经很好理解了，按需设置即可。重点是以下几个，如果你尚处于单身状态，那么就不要开启，否则会影响生理卫生知识的学习。

* **使用 AdGuard【家长控制】服务**：如果家中有尚未成年的孩子，建议开启，屏蔽成人内容。
* **强制安全搜索**：在 Bing、Google、Yandex、YouTube 等网站上强制使用安全搜索，屏蔽 NSFW 内容。

#### DNS 设置 <a href="#toc_3" id="toc_3"></a>

**上游 DNS 服务器**

中国大陆

```
tls://dns.pub
https://dns.pub/dns-query
tls://dns.alidns.com
https://dns.alidns.com/dns-query
```

国际网络环境

```
tls://dns.google
https://dns.google/dns-query
tls://dns11.quad9.net
https://dns11.quad9.net/dns-query
```

**Bootstrap DNS 服务器**

同样的这里根据网络环境的不同推荐两组：

```
中国大陆
119.29.29.29
119.28.28.28
223.5.5.5
223.6.6.6
国际网络
8.8.8.8
8.8.4.4
9.9.9.11
149.112.112.11
```

**DNS 服务设定**

* **速度限制**：`0`
* **使用 EDNS** ：前面提及的上游 DNS 服务器都是支持 EDNS (ECS) 的，它有助于获取到更合适的 CDN 节点，建议勾选。
* **使用 DNSSEC** : 用于效验 DNS 记录的签名，防止 DNS 缓存被投毒，建议勾选。勾选后会在日志页面请求列显示小绿锁图标。
* **禁用 IPv6** ：丢弃 IPv6 的 DNS 查询。在本地网络和网站都支持 IPv6 会优先使用 IPv6 去访问网站，但目前 IPv6 的建设还处于初级阶段，大多数地区的 IPv6 网络体验都一般。还有一些代理软件对 IPv6 支持不佳，开启后可能会影响国际互联网的访问。如果对此没有特殊需求，那么直勾选即可，这样既不影响 BT 软件连接 IPv6 网络，又可以优先使用 IPv4 来上网。如果只有 IPv4 ，那么是否勾选没有区别。



**DNS 缓存配置**

先简单科普一下 TTL ，它是英语 Time To Live 的简称，中文翻译为 “存活时间”。放在 DNS 解析中意为一条域名解析记录在 DNS 服务器中的存留时间，单位是秒。

正常情况下 TTL 默认 `0` 即可，即从上游 DNS 服务器获取 TTL 值。如果你所部署的网络环境到上游 DNS 服务器的延迟比较高，那么可以适当增加 TTL 值，让缓存更持久，短时间内请求同样域名的解析会直接从缓存中读取，实现秒解析。不过 TTL 值不宜过大，不然会导致记录不能及时更新，结果是网站无法正常打开。据博主观察目前多数域名的 TTL 值普遍在 300 以内，所以给出以下设置参考值：

* 覆盖最小 TTL 值：`600`
* 覆盖最大 TTL 值：`3600`

#### 加密设置 <a href="#toc_8" id="toc_8"></a>

设置管理页面使用 HTTPS 加密以及 Ad­Guard Home 自身的 DoH/​DoT 功能，如果不对外开放服务，只是在本地局域网使用是用不到的。对外开放 DNS 服务在中国大陆可能会有 “法律” 风险，而部署在国外网络速度缓慢，所以对于普通用户而言加密设置就成了摆设。

#### 客户端设置 <a href="#toc_9" id="toc_9"></a>

在这里可以单独设置每个设备单独使用的上游 DNS 及过滤规则，需要将设备 DNS 设置为 Ad­Guard Home 服务器的在 IP ，或者使用下面将要提到的 DHCP 设置。每个人的需求不一样，所以这个部分就不详细说明了。

#### DHCP 设置 <a href="#toc_10" id="toc_10"></a>

使用 Ad­Guard Home 作为 DHCP 服务器去代替路由器上的 DHCP 服务器，这个功能的主要作用是自动分配 Ad­Guard Home 的 DNS 给到设备，然后配合**客户端设置**去做精细化 DNS 和过滤规则设置。除非是你的路由设备的 DHCP 功能缺斤少两，否则一般是用不到的。目前这个功能处于实验阶段，稳定性还有待考证。有兴趣的小伙伴可以自己去深入研究，这里不做过多赘述。

### 过滤器 <a href="#toc_11" id="toc_11"></a>

#### DNS 封锁清单 <a href="#toc_12" id="toc_12"></a>

这里是人民群众喜闻乐见的去广告环节。

#### DNS 允许清单 <a href="#toc_13" id="toc_13"></a>

在这里你可以设置排除封锁清单中的被屏蔽的域名。比如做淘宝客、广告联盟之类的人群可能会用得到，毕竟封锁清单基本涵盖了他们的业务范围。

#### DNS 重写 <a href="#toc_14" id="toc_14"></a>

在这里你可以方便的把一个域名指向一个 IP ，简单来说这个功能相当于 hosts 。

最典型的一个使用场景是把 DoH/​DoT DNS 服务器的域名直接指向它们的 IP ，这样就不再需要进行我查我自己这样浪费时间的迷惑操作了，可进一步加快解析的速度。一般来说 DoT/​DoH DNS 服务器的 IP 是固定的，而且 IP 地址和它们自家的传统 DNS 服务器的 IP 是一致的。这里需要注意的是处在公测阶段的 DNS­Pod 是个例外

#### 已阻止服务 <a href="#toc_15" id="toc_15"></a>

在这里你可以一键禁止访问一些热门网站和服务，比如 Face­book 、Twit­ter 。

