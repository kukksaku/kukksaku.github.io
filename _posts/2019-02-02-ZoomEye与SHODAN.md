---
layout:     post
title:      SHODAN与ZoomEye
subtitle:   SHODAN与ZoomEye的使用于区别
date:       2019-02-02
author:     kkkkuboy
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - 渗透测试
    - SHODAN
    - ZoomEye
    
---
# ZoomEye与SHODAN

### 一、SHODAN

地址：<https://www.shodan.io/>

##### 1、什么是SHODAN

> - Shodan 是一个搜索引擎，但它与 Google 这种搜索网址的搜索引擎不同，Shodan 是用来搜索网络空间中在线设备的，你可以通过 Shodan 搜索指定的设备，或者搜索特定类型的设备，其中 Shodan 上最受欢迎的搜索内容是：webcam，linksys，cisco，netgear，SCADA等等。
>
> - Shodan 是怎么工作的呢？Shodan 通过扫描全网设备并抓取解析各个设备返回的 banner 信息，通过了解这些信息 Shodan 就能得知哪一种 Web 服务器是最受欢迎的，或是网络中到底存在多少可匿名登录的 FTP 服务器。



##### 2、基本用法

和其他搜索引擎差不多 ，在主页的搜索框中输入想要搜索的内容即可，例如下面我搜索 "HTTP"：

![day6-4](https://github.com/kukksaku/kukksaku.github.io/blob/master/img/day6-4.JPG?raw=true)

图的搜索结果包含两个部分，左侧是大量的汇总数据包括：

> - Top countries - 搜索结果展示地图
> - Top services (Ports) - 使用最多的服务/端口
> - Top organizations (ISPs) - 使用最多的组织/ISP
> - Top operating systems - 使用最多的操作系统
> - Top products (Software name) - 使用最多的产品/软件名称

中间的主页面包含如下的搜索结果：

> - IP 地址
> - 主机名
> - ISP
> - 该条目的收录收录时间
> - 该主机位于的国家
> - Banner 信息

想要了解每个条目的具体信息，只需要点击每个条目下方的 details 按钮即可。此时，URL 会变成这种格式 `https://www.shodan.io/host/[IP]`，所以我们也可以通过直接访问指定的 IP 来查看详细信息。

![day6-5](https://github.com/kukksaku/kukksaku.github.io/blob/master/img/day6-5.JPG?raw=true)

![day6-6](https://github.com/kukksaku/kukksaku.github.io/blob/master/img/day6-6.JPG?raw=true)



##### 3、使用搜索过滤

（即一些高级搜索语法）

> - `hostname`：搜索指定的主机或域名，例如 `hostname:"google"`
> - `port`：搜索指定的端口或服务，例如 `port:"21"`
> - `country`：搜索指定的国家，例如 `country:"CN"`
> - `city`：搜索指定的城市，例如 `city:"Hefei"`
> - `org`：搜索指定的组织或公司，例如 `org:"google"`
> - `isp`：搜索指定的ISP供应商，例如 `isp:"China Telecom"`
> - `product`：搜索指定的操作系统/软件/平台，例如 `product:"Apache httpd"`
> - `version`：搜索指定的软件版本，例如 `version:"1.6.2"`
> - `geo`：搜索指定的地理位置，例如 `geo:"31.8639, 117.2808"`
> - `before/after`：搜索指定收录时间前后的数据，格式为dd-mm-yy，例如 `before:"11-11-15"`
> - `net`：搜索指定的IP地址或子网，例如 `net:"210.45.240.0/24"`



##### 4、搜索实例

（需要登录才行）

（1）查找位于贵阳的 Apache 服务器：

​               apache city:"Guiyang"

![day6-7](https://github.com/kukksaku/kukksaku.github.io/blob/master/img/day6-7.JPG?raw=true)

（2）查找位于国内的 Nginx 服务器：

​                nginx country:"CN"

（3）查找 GWS(Google Web Server) 服务器：

​                "Server: gws" hostname:"google"

（4）查找指定网段的华为设备：

​                 huawei net:"61.191.146.0/24"  



*点击 Shodan 搜索栏右侧的 "Explore" 按钮，就会得到很多别人分享的搜索语法*



##### 5、其他功能

Shodan 不仅可以查找网络设备，它还具有其他相当不错的功能。

- Exploits：每次查询完后，点击页面上的 "Exploits" 按钮，Shodan 就会帮我们查找针对不同平台、不同类型可利用的 exploits；
- 地图：每次查询完后，点击页面上的 "Maps" 按钮，Shodan 会将查询结果可视化的展示在地图当中。当然也可以通过直接访问网址来自行搜索：<https://exploits.shodan.io/welcome>；
- 报表：每次查询完后，点击页面上的 "Create Report" 按钮，Shodan 就会帮我们生成一份精美的报表；



### 二、ZoomEye

地址：<https://www.zoomeye.org/>

##### 1、什么是ZoomEye

> ZoomEye 是一个检索网络空间节点的搜索引擎。通过后端的分布式爬虫引擎（无论谁家的搜索引擎都是这样）对全球节点的分析，对每个节点的所拥有的特征进行判别，从而获得设备类型、固件版本、分布地点、开放端口服务等信息。

##### 2、搜索语法

其基本用法与SHODAN相同

其搜索过滤（高级语法）如下：

> - app:nginx　　组件名
> - ver:1.0　　版本
> - os:windows　　操作系统
> - country:”China”　　国家
> - city:”hangzhou”　　城市
> - port:80　　端口
> - hostname:google　　主机名
> - site:thief.one　　网站域名
> - desc:nmask　　描述
> - keywords:nmask’blog　　关键词
> - service:ftp　　服务类型
> - ip:8.8.8.8　　ip地址
> - cidr:8.8.8.8/24　　ip地址段

### 三、SHODAN与ZoomEye的区别

- shodan（撒旦）网络搜索引擎偏向网络设备以及服务器的搜索
- 钟馗之眼搜索引擎偏向web应用层面的搜索。

> Shodan 主要针对的是设备指纹，也就是与某些特定端口通信之后返回的 banner 信息的采集和索引。而 ZoomEye 除了设备指纹的检测，还针对某些特定服务加强了探测，比如 Web 服务的细节分析。