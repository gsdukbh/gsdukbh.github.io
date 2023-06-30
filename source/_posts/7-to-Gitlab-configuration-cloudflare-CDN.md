---
title: 'Gitlab 配置 cloudflare CDN 和 SSH '
tags:  
    - gitlab
    - CDN
    - SSH

---

# 简单的开头

Gitlab 是一个基于Web的开源版本控制系统和代码管理系统,在企业中作为常见的代码版本管理工具。

我自己部署一个Gitlab 在家中的服务器上， 那么如何非家庭网络中访问呢？ 答案是内网穿透！



# Gitlab 部署

Gitlab 提供多种方便的部署方式， 具体的方法可以查阅这里，[Download and install GitLab | GitLab](https://about.gitlab.com/install/)



我自己使用Docker 进行部署,下面是我的` docker-compose.yml` 文件可以参考下,



```yaml
# docker-compose.yml
version: '3.7'
services:
  gitlab:
    image: gsdukbh/gitlab-ee-arm64:latest
    container_name: gitlab
    volumes:
      - './gitlab/config:/etc/gitlab'
      - './gitlab/log:/var/log/gitlab'
      - './gitlab/data:/var/opt/gitlab'
    restart: always
    ports:
      - '9000:80' 
      - '8090:8091' # pagas
      - '2222:22' # ssh
  git-runner:
    image: gitlab/gitlab-runner:latest
    container_name: runner
    volumes: 
      - '/var/run/docker.sock:/var/run/docker.sock'
      - './runner/config:/etc/gitlab-runner'
    restart: always
    links:
     - 'gitlab:gitlab'


```

因我是部署在树莓派上, 使用的架构是 `arm64`,但是Gitlab官方镜像中并没有`arm64`.这里使用的是我自己构建的镜像. 

构建镜像dockerfile 可以看这里  [GitHub - gsdukbh/docker-gitlab-ee-arm64: build gitlab-ee for arm64 images](https://github.com/gsdukbh/docker-gitlab-ee-arm64)



关于gitlab 部署的更多问题,可以参阅我的其他文章.



# 配置 Cloudflare Tunnel



`Cloudflare Tunnel` 是Cloudflare 推出的内网穿透的工具,可将任意内网子域或路径映射到外网域名,实现内网服务外网访问。

利用Cloudflare Tunnel 可以完美配置网络穿透,并且无需任何费用.



## 安装 Cloudflare Tunnel

使用命令行安装.



```bash
sudo wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i ./cloudflared-linux-amd64.de

```

推荐使用 软件包管理器进行安装  [官方 Linux 软件包](https://pkg.cloudflare.com/index.html)  在自己的软件源中添加 `pkg.cloudflare.com` 这样方便后续的更新.



登录 Cloudflare Tunnel

```bash

cloudflared login
```

输出, 点击连接通过网页登录认证.

![adee8479-5077-44cd-9bf9-91a463238c9c](/images/adee8479-5077-44cd-9bf9-91a463238c9c.png)



## 连接到Cloudflare

1. 创建一个连接通道.
   
   ```bash
   cloudflared tunnel create gitlab
   ```

cloudflared  会创建一个唯一id的连接通道, 比如 `6ff42ae2-765d-4adf-8112-31c55c1551ef` 然后我们使用这个来连接.

2. 配置本地连接网络.
   
   ```bash
   vim ~/.cloudflared/config.yml
   tunnel: 6ff42ae2-765d-4adf-8112-31c55c1551ef
   credentials-file: /root/.cloudflared/6ff42ae2-765d-4adf-8112-31c55c1551ef.json
   
   ingress:
     - hostname: gitlab.yourdomain.com
       service: http://localhost:80
     - hostname: gitlab-ssh.yourdomain.com
       service: ssh://localhost:22
     - service: http_status:404
   ```

3. 检查下配置是否正常.
   
   ```bash
   
   cloudflared tunnel ingress validate
   
   ```

4. 运行Tunnel  
   
   ```bash
   # 这个会直接运行在终端, 停止会让内网穿透失败
   cloudflared tunnel run
   ```

    5. 创建一个systemd 服务来长期运行  tunnel

```bash
#创建文件
nano /etc/systemd/system/cloudflared-tunnel.servcie

# /etc/systemd/system/cloudflared-tunnel.servcie


[Unit]
Description=cloudflared tunnel 
After=network.target

[Service]
TimeoutStartSec=0
Type=notify
ExecStart=/usr/bin/cloudflared tunnel run
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

## 配置DNS

先将自己域名添加到cloudflare, 然后在` 网站-> DNS` 添加DNS 记录. 选择 `CNAME` 然后填入

```bash
##名称
gitlab.yourdomain.com
## 目的
6ff42ae2-765d-4adf-8112-31c55c1551ef.cfargotunnel.com
# 前面guid 
```

gitlab-ssh 域名重复上.



等待几分钟 ,这个时候就可以访问 Gitlab了



# 配置 SSH

使用 `cloudflare tunnel ` 连接ssh 有些麻烦,必须在使用的电脑上安装`cloudflared` 才可以使用.



安装 `cloudflared` 在 用户目录 `.ssh/config` 配置

```bash
Host gitlab-ssh.yourdomain.com
  ProxyCommand /usr/local/bin/cloudflared access ssh --hostname %h
```



到此, Gitlab 就完全在 `cloudflare `的代理后面了,


