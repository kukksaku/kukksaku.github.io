---
layout:     post
title:      使用dockerfile创建镜像
subtitle:   使用dockerfile定制自己的docker镜像，创建容器并运行
date:       2019-06-05
author:     kkkkuboy
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - docker
   
    
    
---

# 使用dockerfile创建镜像

Dockerfile是一个文本格式配置文件，用户可以使用Dockerfile快速创建自定义的镜像

## 基本结构

- dockerfile由许多行命令语句组成，支持以#开头为注释行

- dockerfile分为四部分：基础镜像信息、维护者信息、镜像操作指令、容器启动时执行指令

  如：

  ```
  # This dockerfile user the ubuntu image
  # VERSION 2 - EDITON 1
  # Author: docker_user
  # Command format: Instruction [arguments / command] ..
  
  # 第一行必须指定基于的基础镜像
  FROM ubuntu
  
  # 维护者信息
  MAINTAINER  docker_user docker_user@email.com
  
  # 镜像的操作指令
  RUN echo "deb http://archive.ubuntu.com/ubuntu/ raring main universe" >> /etc/apt/sources.list
  RUN apt-get update && apt-get install -y nginx
  
  # 容器启动时执行指令
  CMD /usr/sbin/nginx
  ```

  



## 指令

### 1. FROM

格式为 `FROM <image>` 或 `FROM<image>:<tag>`。
例如： FROM ubuntu或FROM ubuntu：16.04

### 2. MAINTAINER

格式为`MAINTAINER <name>`， 指定维护者维息。
例如：MAINTAINER docker_user [docker_user@email.com](https://link.jianshu.com/?t=mailto:docker_user@email.com)

### 3. RUN

格式为 `RUN <command>` 或 `RUN ["executable", "param1", "param2"]`。
每条RUN 指令将在当前镜像基础上执行指定命令，将提交为新的镜像。**当命令较长时可以使用 \ 来换行。**

### 4. CMD

是指定启动容器时执行的命令，**每个Dockerfile 只能有一条 CMD命令**。如果**指定了多条命令，只有最后一条会被执行**。
 如果用户启动容器时指定了运行的命令，则会覆盖掉 CMD 指定的命令。

支持三种格式：

- `CMD ["executable","param1',"param2"]` 使用 exec 执行，推荐方式。
- `CMD command param1 param2` 在 /bin/sh 中执行，提供给需要交互的应用。
- `CMD ["param1","param2"]` 提供给 **ENTRYPOINT 的默认参数**。

### 5. EXPOSE

`EXPOSE` 指令是**声明**运行时容器**提供服务端口**，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。在 Dockerfile 中写入这样的声明有两个好处，一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；另一个用处则是在运行时使用随机端口映射时，也就是 `docker run -P` 时，会自动随机映射 `EXPOSE` 的端口。

格式为 `EXPOSE <port> [<prot>...]`
例如：
EXPOSE 22 80 8443

### 6. ENV

格式有两种：

- `ENV <key> <value>`
- `ENV <key1>=<value1> <key2>=<value2>...`

改指令是设置环境变量，在后面的其他指令，如`RUN`，还是运行时的应用，都可以直接使用这里定义的环境变量。

如：

```
ENV VERSION=1.0 DEBUG=on \
    NAME="Happy Feet"
#这个例子中演示了如何换行，以及对含有空格的值用双引号括起来的办法，这和 Shell 下的行为是一致的。
```

后续指令使用定义的环境变量，如：

```bash
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
```

### 7. ADD

格式为 `ADD <src><dest>`

该命令将复制指定的<src>到容器中的<dest>。其中<src>可以是Dockerfile 所在目录的一个相对路径(文件或目录)；也可以是一个URL；还可以是一个 tar 文件（自动解压为目录）

### 8. COPY

格式为 `COPY <src><dest>`
复制本地主机的`<src>` 为容器中的`<dest>`。目标路径不存在时，会自动创建。
当使用本地目录为源目录时，推荐使用COPY

### 9. ENTRYPOINT

与`RUN`格式一样，分为`exec`格式和 `shell` 格式。分别为：

- `ENTRYPOINT ["executable", "param1", "param2"]`
- `ENTRYPOINT command param1 param2 `(shell 中执行)

`ENTRYPOINT` 的目的和 `CMD` 一样，都是在指定容器启动程序及参数。`ENTRYPOINT` 在运行时也可以替代，不过比 `CMD` 要略显繁琐，需要通过 `docker run` 的参数 `--entrypoint` 来指定。

当指定了 `ENTRYPOINT` 后，`CMD` 的含义就发生了改变，不再是直接的运行其命令，而是将 `CMD` 的内容作为参数传给 `ENTRYPOINT` 指令，换句话说实际执行时，将变为：

```bash
<ENTRYPOINT> "<CMD>"
```

#### 使用ENTRYPOINT的好处

##### 一：让镜像变成像命令一样使用

假设我们需要一个得知自己当前公网 IP 的镜像，那么可以先用 `CMD` 来实现：

```Dockerfile
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "https://ip.cn" ]
```

假如我们使用 `docker build -t myip .` 来构建镜像的话，如果我们需要查询当前公网 IP，只需要执行：

```bash
$ docker run myip
当前 IP：61.148.226.66 来自：北京市 联通
```

嗯，这么看起来好像可以直接把镜像当做命令使用了，不过命令总有参数，如果我们希望加参数呢？比如从上面的 `CMD` 中可以看到实质的命令是 `curl`，那么如果我们希望显示 HTTP 头信息，就需要加上 `-i` 参数。那么我们可以直接加 `-i` 参数给 `docker run myip` 么？

```bash
$ docker run myip -i
docker: Error response from daemon: invalid header field value "oci runtime error: container_linux.go:247: starting container process caused \"exec: \\\"-i\\\": executable file not found in $PATH\"\n".
```

我们可以看到可执行文件找不到的报错，`executable file not found`。之前我们说过，跟在镜像名后面的是 `command`，运行时会替换 `CMD` 的默认值。因此这里的 `-i` 替换了原来的 `CMD`，而不是添加在原来的 `curl -s https://ip.cn` 后面。而 `-i` 根本不是命令，所以自然找不到。

那么如果我们希望加入 `-i` 这参数，我们就必须重新完整的输入这个命令：

```bash
$ docker run myip curl -s https://ip.cn -i
```

这显然不是很好的解决方案，而使用 `ENTRYPOINT` 就可以解决这个问题。现在我们重新用 `ENTRYPOINT` 来实现这个镜像：

```Dockerfile
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "https://ip.cn" ]
```

这次我们再来尝试直接使用 `docker run myip -i`：

```bash
$ docker run myip
当前 IP：61.148.226.66 来自：北京市 联通

$ docker run myip -i
HTTP/1.1 200 OK
Server: nginx/1.8.0
Date: Tue, 22 Nov 2016 05:12:40 GMT
Content-Type: text/html; charset=UTF-8
Vary: Accept-Encoding
X-Powered-By: PHP/5.6.24-1~dotdeb+7.1
X-Cache: MISS from cache-2
X-Cache-Lookup: MISS from cache-2:80
X-Cache: MISS from proxy-2_6
Transfer-Encoding: chunked
Via: 1.1 cache-2:80, 1.1 proxy-2_6:8006
Connection: keep-alive

当前 IP：61.148.226.66 来自：北京市 联通
```

可以看到，这次成功了。这是因为当存在 `ENTRYPOINT`后，`CMD` 的内容将会作为参数传给 `ENTRYPOINT`，而这里 `-i` 就是新的 `CMD`，因此会作为参数传给 `curl`，从而达到了我们预期的效果。

##### 二：应用运行前的准备工作

启动容器就是启动主进程，但有些时候，启动主进程前，需要一些准备工作。

比如 `mysql` 类的数据库，可能需要一些数据库配置、初始化的工作，这些工作要在最终的 mysql 服务器运行之前解决。

此外，可能希望避免使用 `root` 用户去启动服务，从而提高安全性，而在启动服务前还需要以 `root` 身份执行一些必要的准备工作，最后切换到服务用户身份启动服务。或者除了服务外，其它命令依旧可以使用 `root` 身份执行，方便调试等。

这些准备工作是和容器 `CMD` 无关的，无论 `CMD` 为什么，都需要事先进行一个预处理的工作。这种情况下，可以写一个脚本，然后放入 `ENTRYPOINT` 中去执行，而这个脚本会将接到的参数（也就是 `<CMD>`）作为命令，在脚本最后执行。比如官方镜像 `redis` 中就是这么做的：

```Dockerfile
FROM alpine:3.4
...
RUN addgroup -S redis && adduser -S -G redis redis
...
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 6379
CMD [ "redis-server" ]
```

可以看到其中为了 redis 服务创建了 redis 用户，并在最后指定了 `ENTRYPOINT` 为 `docker-entrypoint.sh` 脚本。

```bash
#!/bin/sh
...
# allow the container to be started with `--user`
if [ "$1" = 'redis-server' -a "$(id -u)" = '0' ]; then
    chown -R redis .
    exec su-exec redis "$0" "$@"
fi

exec "$@"
```

该脚本的内容就是根据 `CMD` 的内容来判断，如果是 `redis-server` 的话，则切换到 `redis` 用户身份启动服务器，否则依旧使用 `root` 身份执行。比如：

```bash
$ docker run -it redis id
uid=0(root) gid=0(root) groups=0(root)
```



## 创建镜像

在dockerfile文件目录下使用命令

`docker build [OPTIONS] <PATH | URL | ->`

如：

`docker build -t myubuntu:v1 .`

命令介绍：

- docker：docker命令
- build：编译
- -t:镜像的名字及tag，通常`name：tag`或者`name`格式；可以在一次构建中为一个镜像设置多个tag
- myubuntu：生成的镜像的名称
- v1:生成的镜像的版本号
- . :点符号“.”表示的意思是，指定镜像构建过程中的上下文环境的目录



### 查看生成的镜像

`docker images`

### 创建容器并运行

使用镜像“myubuntu:v8”以交互模式启动一个容器，将其命名为“myubuntu_test”，在容器内执行“/bin/bash”，而且绑定容器的 80 端口，并将其映射到本地主机 127.0.0.1 的 8111 端口上：

```bash
docker run -it -p 127.0.0.1:8111:80 --name myubuntu_test myubuntu:v8 /bin/bash
```

当然，也可以以后台模式启动一个容器：

```bash
docker run -p 127.0.0.1:8111:80 -d --name myubuntu_test myubuntu:v8
```

然后在登录到容器中：

```bash
docker run -it myubuntu_test /bin/bash
```



