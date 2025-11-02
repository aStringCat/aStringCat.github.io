---
title: 使用docker封装easy-connect (新手向经验帖)
date: 2025-11-01 10:30:25
tags: [network, docker]
---
# 记录一次用 Docker 隔离 EasyConnect 实践记录

> 在访问校园网资源时，EasyConnect（EC）都是我们绕不开的工具。
>
> 然而，EC客户端 对操作系统的侵入式行为和运行后残留的可疑进程，让我们的系统环境饱受“污染”，即使退出程序，相关的服务进程或守护进程仍会残留在后台运行，造成系统资源占用和潜在的隐私顾虑。
>
> 有没有一种方式，能让 EC 乖乖地只处理内网流量，并且将其所有流氓行为彻底隔离在一个沙箱中呢？
>
> 答案是肯定的！利用 Docker 的隔离性，我们可以完美地实现这个目标。本文将详细记录我的实践过程，实现 EC 的“容器化”与“服务化”。

<!-- more -->

## 第一步：环境配置

1. ~~神秘的小猫软件~~

2. Docker 环境：

   - 对于 macOS 和 Linux 用户，直接在[官网](https://www.docker.com/)上安装 Docker Desktop 即可。
   - 对于 Windows 用户，你需要先确保系统启用了 [WSL2 ](https://learn.microsoft.com/zh-cn/windows/wsl/install)，之后才能在微软商店或[官网](https://www.docker.com/)下载并运行 Docker Desktop。
   - 安装完成后，运行以下命令验证 Docker 是否安装成功：

   VNC 客户端： 我们需要一个 VNC 客户端来连接到 Docker 容器内部的图形界面。

   - macOS 用户可以直接使用自带的 “屏幕共享” app。
   - Windows 用户可以在微软商店或[官网](https://www.realvnc.com/en/connect/download/viewer/)下载 RealVNC Viewer 客户端。

3. 安装完成后，在终端中运行以下命令验证 Docker 是否安装成功：

   ```bash
   docker --version 
   # 运行一个测试容器，如果看到 "Hello from Docker!" 则表示成功
   docker run hello-world
   ```

## 第二步：Docker配置

1. 进入 Docker Desktop，使用内置的终端，输入

   ```bash
   docker run --device /dev/net/tun --cap-add NET_ADMIN -ti -e PASSWORD=xxxx -e URLWIN=1 -v $HOME/.ecdata:/root -p 127.0.0.1:5901:5901 -p 127.0.0.1:1080:1080 -p 127.0.0.1:8888:8888 hagb/docker-easyconnect:latest
   ```

   arm64 架构的设备（如高通windows和m芯片的mac）需要加入 `-e DISABLE_PKG_VERSION_XML=1` 

2. 进入 VNC 客户端，地址输入`127.0.0.1`，端口: `5901`, 密码 `xxxx`；

3. 成功连接上后可以看到EC的登录界面，登录即可。

   ![VNC](VNC.png)

4. 下次启动时，只需要container点击启动

   ![docker-desktop](docker-desktop.png)

## 第三步：代理配置

打开我们的神秘小猫软件，修改其中的配置即可。我们的容器连接了主机的`1080`和`8888`端口，分别对应了sock5和http代理，之后我们针对撰写代理配置文件即可。

对于不会修改配置文件的同学，可以直接复制我的：

```yaml
port: 7890
socks-port: 7891
mode: Rule
log-level: info

proxies:
  - name: vpn
    type: socks5
    server: "127.0.0.1"
    port: 1080

proxy-groups:
  - name: FINAL-GROUP
    type: select
    proxies:
      - vpn
      - DIRECT

rules:
  - DOMAIN-SUFFIX,xxx.xxx.xxx.xxx,FINAL-GROUP 
  (这里的xxx修改成你需要代理的网站的后缀，如你需要代理大学内网，可以修改成xxx.edu.cn)
```

注意，在连接的过程中，不要打开 tun 模式(虚拟网卡)，目前主播暂时不清楚冲突的原因。