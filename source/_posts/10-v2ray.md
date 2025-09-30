---
title: 'v2ray'
tags:  
    - vps
    - v2ray
    - docker
    - nginx
    - 科学上网
    - shadowsocks
    - trojan
    - clash
    - 教程
    - 阿里云
---

> 写于 2021-4-20 部分信息可能过时。


> # 重要提示
> 下面的使用 服务器 `ip`的方式是不推荐的。建议使用域名+ TLS 的方式进行伪装。
> 不建议使用阿里云服务器搭建，下面是例子。


## 第一部分 注册阿里云



[TOC]





1. 阿里云注册地址 [阿里云](https://cn.aliyun.com/) 。 可以直接使用支付宝扫码完成注册，以及信息实名。

![image-20210419103027922](https://i.loli.net/2021/04/19/ZXSe7mcFObnCRuT.png)

<!-- more -->

2. 购买轻量服务器。
   注册账号并登陆到阿里云控制台，然后在顶部搜索栏搜索 `轻量应用服务器` 
   另一种方法是，点击左侧 `阿里云` 旁边的菜单，找到产品与服务--> 搜索。轻量应用。见下图。

![image-20210419104246053](https://i.loli.net/2021/04/19/596olFyGOK2Damg.png)

进入轻量应用服务器后，会提示你购买服务器。我这里已经购买过了，没有这个提示。点击右侧的`创建服务器`进行购买。

![image-20210419105756618](https://i.loli.net/2021/04/19/npuckir7qaCt1l3.png)

购买页面如下图。

![image-20210419105952553](https://i.loli.net/2021/04/19/YiXtcGwQmgnbEeH.png)

![image-20210419110635811](https://i.loli.net/2021/04/19/qiPnO1wHveb9zQU.png)

在这个页面，地区选择`香港`，镜像使用系统镜像` ubuntu 20.04` ,套餐根据需要选择，数据盘同理。然后点击购买，转跳到下图所示。

![image-20210419110810451](https://i.loli.net/2021/04/19/S6Pa7IQtJFyqmZs.png)

购买完成后，会跳回服务器列表。然后选择地区 为`香港` 的服务器然后选择 `远程连接`

![image-20210419114811012](https://i.loli.net/2021/04/19/IYNo8xc9D2hOa4T.png)

## 第二部分 登陆服务器控制台



接上图，进入远程连接后，阿里云会提示你设置服务器密码。

> 不建议使用 密码访问服务器，容易被攻击。可以直接使用阿里云提供的远程连接，或者密钥连接

![image-20210419113410553](https://i.loli.net/2021/04/19/1ZJsqhUFe7BNRMd.png)

> IP 很重要，不要随便让人知道。 

直接点击远程连接，会弹出如下图的终端窗口。

![image-20210419134343646](https://i.loli.net/2021/04/19/yDl3MpUnL4JBNgG.png)

> 默认用户是 admin 我们需要在root 用户下进行。

然后我们给root 设置密码。在控制台输入 `passwd root`

```bash
passwd root # 设置root 用户密码，只用设置密码后才能赚到root用户
> Changing password for root.
  (current) UNIX password: # 输入密码，光标闪烁，但是不会有提示，需要二次确认
```

然后在控制台输入 `sudo -i ` 切换换到root用户。

![image-20210419134757239](https://i.loli.net/2021/04/19/ar1lYOHehCpnPFG.png)

看到root@开头说明切换成功。



## 第三部分 轻量应用服务器防火墙设置

找到左侧的防火墙，然后添加规则。如下图所示，开放`80、443、8888、10086` 等端口。

![image-20210419163813755](https://i.loli.net/2021/04/19/mUF6teQcIsJaPki.png)



## 第四部分 使用docker安装nginx（可选）

这部分，根据自己的需要安装。安装`nginx` 可以方便伪装

> 此步，为了配置 WebSocket + TLS + Web，若你连 vi/vim 、nano 都不会用，建议不用尝试。

### 安装 docker、

```bash
apt install docker.io # 按y确认安装。
docker -v # Docker version 19.03.8, build afacb8b7f0 说明安装成功
mkdir nginx # 用于存放网站和nginx 配置文件。
docker pull nginx # 拉取nginx 镜像。
```

#### 准备nginx.conf

在`nginx`目录下创建 `nginx.conf` 文件。

```bash
nano nginx/nginx.conf # ubuntu 自带 nano 编辑器 
```

然后填入：请先仔细阅读，

```nginx
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
    gzip  on;
    map $http_upgrade $connection_upgrade {
        default upgrade;
        '' close;
      }
      server {
      listen 80;
      listen 443 ssl http2;
      # server_name 在这里填写你的 域名;
      index index.php index.html index.htm default.php default.htm default.html;
      root /usr/share/nginx/html;
      # 配置ssl 根据自身需要配置
      ssl_certificate    /usr/share/nginx/fullchain.pem; 
      ssl_certificate_key    /usr/share/nginx/privkey.pem;
      ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
      ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
      ssl_prefer_server_ciphers on;
      ssl_session_cache shared:SSL:10m;
      ssl_session_timeout 10m;
      # 代理路由需要和v2ray里面的路由 path 一致，建议使用uuid作为路由，但是不要和v2ray的uuid 一致
      location /lurk {
          proxy_pass  http://127.0.0.1:10086;
          proxy_redirect off;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
            proxy_set_header Host $http_host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }    
            #access_log  /var/nginx/access.log;
        #error_log  /var/nginx/error.log;
        }
    #include /etc/nginx/conf.d/*.conf;
}
```

#### 启动 nginx。

```bash
docker run -d -p 80:80 -p 443:443 -v $PWD/nginx:/usr/share/nginx -v $PWD/nginx/nginx.conf:/etc/nginx/nginx.conf --name nginx-demo nginx 
# 运行nginx
docker ps 
root@leee:~# docker ps 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                      NAMES
10f2d09a6051        nginx               "/docker-entrypoint.…"   8 seconds ago       Up 7 seconds        0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   nginx-demo
# 可以看到如上所示，说明nginx运行成功。
```

4. 测试nginx是否运行成功。
   在浏览器中填写服务器的IP地址结果如下图所示。说明搭建成功

![image-20210419204306838](https://i.loli.net/2021/04/19/2Lez9osx8kfaGgJ.png)

5. 为了更加逼真伪装成一个网站，你可以直接在`/root/nginx/` 目录下放一个真实的网站。

## 第五部分 安装v2ray以及配置



### 安装v2ray

v2ray[的官方文档在这里](https://www.v2fly.org)(已经被墙)，官方文档上有详尽的说明和搭建教程。以下内容全部来自官方文档

这里我们使用的`Linux`服务器所以使用官方提供的脚本。

GitHub [地址](https://github.com/v2fly/fhs-install-v2ray) 

执行命令：

```bash
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

> 如果你的服务器无法访问 https://raw.githubusercontent.com， 请尝试先下载脚本然后再执行。

这一步会帮你自动安装`v2ray` 。

以下是`v2ray` 相关安装目录

```txt
installed: /usr/local/bin/v2ray
installed: /usr/local/bin/v2ctl
installed: /usr/local/share/v2ray/geoip.dat
installed: /usr/local/share/v2ray/geosite.dat
installed: /usr/local/etc/v2ray/config.json
installed: /var/log/v2ray/
installed: /var/log/v2ray/access.log
installed: /var/log/v2ray/error.log
installed: /etc/systemd/system/v2ray.service
installed: /etc/systemd/system/v2ray@.service
```

测试v2ray是否正确安装：

```bash
v2ray -version
V2Ray 4.37.3 (V2Fly, a community-driven edition of V2Ray.) Custom (go1.16.3 linux/amd64)
A unified platform for anti-censorship.
# 安装完成
systemctl enable v2ray # 把v2ray 加入 systemd 并让v2ray 开机自启
systemctl start v2ray # 启动
```



### 基础配置



#### VMess

服务端`/usr/local/etc/v2ray/config.json` 的配置。

```bash
nano /usr/local/etc/v2ray/config.json # 使用nano 打开
```

然后输入一下配置

==**！！！！ 重要，请先看说明再复制**==

##### 服务端的配置：

```json

{
    "inbounds": [
        {
            "port": 10086, // 服务器监听端口，需要防火墙开放这个端口
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "你的uuid" // 请使用uuid 生成工具生成并替换 在后面工具里有
                    }
                ]
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}

```

##### 客户端配置

在你的 PC（或手机）中，需要用以下配置运行 V2Ray ：

```json
{
    "inbounds": [
        {
            "port": 1080, // SOCKS 代理端口，在浏览器中需配置代理并指向这个端口
            "listen": "127.0.0.1",
            "protocol": "socks",
            "settings": {
                "udp": true
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "address": "server", // 服务器地址，请修改为你自己的服务器 ip 或域名
                        "port": 10086, // 服务器端口
                        "users": [
                            {
                                "id": " 和服务器的id 一致"// uuid 
                            }
                        ]
                    }
                ]
            }
        },
        {
            "protocol": "freedom",
            "tag": "direct"
        }
    ],
    "routing": {
        "domainStrategy": "IPOnDemand",
        "rules": [
            {
                "type": "field",
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "direct"
            }
        ]
    }
}
```

重启 v2ray 让 配置生效

可以在浏览器输入ip+ 端口号 的方式测试配置是否生效。若有效，可以看到`Bad Request `的字样。



#### HTTP

本节举例 HTTP(S)代理的配置.

##### 服务器的配置

使用命令 `nano /usr/local/etc/v2ray/config.json`  编辑

```json
{
  "inbounds": [
    {
      "port": 1024, // 监听端口，需要防火墙开放
      "protocol": "http",
      "settings": {
        "timeout:":0,
        "accounts":[
          {
            "user":"my-username",// 将my-username改为你的用户名.
            "pass":"my-password" //将my-password改为你的密码
          }
        ],
        "allowTransparent":false,
        "userLevel":0
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",  
      "settings": {}
    }
  ]
}
```



##### 客户端配置

```json
{
  "inbounds": [
    {
      "port": 1080, // 监听端口
      "protocol": "socks", // 入口协议为 SOCKS 5
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth"  // 不认证
      }
    }
  ],
  "outbounds": [
      {
        "protocol": "http",
        "settings": {
          "servers": [
            {
              "address": "your server",//服务器IP
              "port": 1024,//服务器端口
              "users": [
                {
                  "Username": "my-username",//将my-username改为你的用户名.
                  "Password": "my-password" //将my-password改为你的密码
                }
              ] 
            }
          ]
        },
        "streamSettings": {
          "security": "none", //如果是HTTPS代理,需要将none改为tls
          "tlsSettings": {
            "allowInsecure": false
            //检测证书有效性
        }
      }
    }
  ]
}
```



### 高级配置

#### Mux

Mux 意为多路复用(multiplexing)，在目前的科学上网工具中仅 V2Ray 有此功能(2018-03-15注：也有其他软件实现了类似的功能)。它能够将多条 TCP 连接合并成一条，节省资源，提高并发能力。

Mux 实质上不能提高网速，但对并发连接比较有效，如浏览图片较多的网页，看直播等。从使用效果来说，V2Ray 的 Mux 应该类似于 Shadowsocks 的 TFO(TCP Fast Open)，因为两者的目的都是减小握手时间，只是实现方式不一样。只是 TFO 要设置系统内核才能打开，而 Mux 是纯粹在软件层面上实现，从配置难易度上 V2Ray 较好一些。

##### 配置 客户端

Mux 只需在客户端开启，服务器会自动识别，所以只给客户端的配置。也就是只要在 outbound 或 outboundDetour 加入 `"mux": {"enabled": true}` 即可：

```javascript
{
  "inbounds": [
    {
      "port": 1080, // 监听端口
      "protocol": "socks", // 入口协议为 SOCKS 5
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth"  // 不认证
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess", // 出口协议
      "settings": {
        "vnext": [
          {
            "address": "serveraddr.com", // 服务器地址，请修改为你自己的服务器 ip 或域名
            "port": 16823,  // 服务器端口
            "users": [
              {
                "id": "b831381d-6324-4d53-ad4f-8cda48b30811",  // 用户 ID，必须与服务器端配置相同
                "alterId": 64 // 此处的值也应当与服务器相同
              }
            ]
          }
        ]
      },
      "mux": {"enabled": true}
    }
  ]
}

```



#### WebSocket + TLS + Web

注意: V2Ray 的 Websocket+TLS 配置组合并不依赖 Nginx / Caddy / Apache，只是能与其搭配使用而已，没有它们也可以正常使用。

##### 服务器配置

这次 TLS 的配置将写入 Nginx / Caddy / Apache 配置中，由这些软件来监听 443 端口（443 比较常用，并非 443 不可），然后将流量转发到 V2Ray 的 WebSocket 所监听的内网端口（本例是 10000），V2Ray 服务器端不需要配置 TLS。

##### 服务器 V2Ray 配置

```javascript
{
  "inbounds": [
    {
      "port": 10000,
      "listen":"127.0.0.1",//只监听 127.0.0.1，避免除本机外的机器探测到开放了 10000 端口
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "  ",// 请使用 uuid 生成工具生成，同时保证上下文中的ID是一致的，后面不再赘述
            "alterId": 64 // 额外ID
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/lurk" // 这是目录 需要与上下文中的 代理配置一致，如第4部分提到的nginx 中的配置
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

##### Nginx 配置

配置中使用的是域名和证书使用 TLS 小节的举例，请替换成自己的。

```
server {
  listen  443 ssl;
  ssl on;
  ssl_certificate       /etc/v2ray/v2ray.crt;
  ssl_certificate_key   /etc/v2ray/v2ray.key;
  ssl_protocols         TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers           HIGH:!aNULL:!MD5;
  server_name           mydomain.me;
        location /lurk { # 与 V2Ray 配置中的 path 保持一致
        proxy_redirect off;
        proxy_pass http://127.0.0.1:10000;#假设WebSocket监听在环回地址的10000端口上
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;

        # Show realip in v2ray access.log
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
```

##### Caddy 配置

因为 Caddy 会自动申请证书并自动更新，所以使用 Caddy 不用指定证书、密钥。  

```
mydomain.me
{
  log ./caddy.log
  proxy /lurk localhost:10000 { # 与 V2Ray 配置中的 path 保持一致
    websocket
    header_upstream -Origin
  }
}
```

##### Apache 配置

同样地，配置中使用的是域名和证书使用 TLS 小节的举例，请替换成自己的。

```
<VirtualHost *:443>
  ServerName mydomain.me
  SSLCertificateFile /etc/v2ray/v2ray.crt
  SSLCertificateKeyFile /etc/v2ray/v2ray.key

  SSLProtocol -All +TLSv1 +TLSv1.1 +TLSv1.2
  SSLCipherSuite HIGH:!aNULL

  <Location "/lurk/"> 
    ProxyPass ws://127.0.0.1:10000/ray/ upgrade=WebSocket
    ProxyAddHeaders Off
    ProxyPreserveHost On
    RequestHeader append X-Forwarded-For %{REMOTE_ADDR}s
  </Location>
</VirtualHost>
```

##### 客户端配置

```javascript
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": ["http", "tls"]
      },
      "settings": {
        "auth": "noauth",
        "udp": false
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "mydomain.me",
            "port": 443,
            "users": [
              {
                "id": " ", // uuid 
                "alterId": 64 //额外ID
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "tls",// tls 
        "wsSettings": {
          "path": "/lurk"
        }
      }
    }
  ]
}
```

##### 注意事项

- V2Ray 暂时不支持 TLS1.3，如果开启并强制 TLS1.3 会导致 V2Ray 无法连接

- 较低版本的nginx的location需要写为 /ray/ 才能正常工作

- 如果在设置完成之后不能成功使用，可能是由于 SElinux 机制(如果你是 CentOS 7 的用户请特别留意 SElinux 这一机制)阻止了 Nginx 转发向内网的数据。如果是这样的话，在 V2Ray 的日志里不会有访问信息，在 Nginx 的日志里会出现大量的 "Permission Denied" 字段，要解决这一问题需要在终端下键入以下命令：
  
  ```
  setsebool -P httpd_can_network_connect 1
  ```

- 请保持服务器和客户端的 wsSettings 严格一致，对于 V2Ray，`/lurk` 和 `/lurk/` 是不一样的
  
  
  
  

## 第六部分 有用的工具

1. UUID [生成工具](https://www.uuidgenerator.net/) 或者在Linux 下使用命令`cat /proc/sys/kernel/random/uuid`

2. 图形客户端÷
   [Qv2ray](https://github.com/Qv2ray/Qv2ray) 跨平台 V2Ray 客户端，支持 Linux、Windows、macOS，可通过插件系统支持 SSR / Trojan / Trojan-Go / NaiveProxy 等协议
    [V2RayNG ](https://github.com/2dust/v2rayNG) V2RayNG 是一个基于 V2Ray 内核的 Android 应用，它可以创建基于 VMess 的 VPN 连接。
    [V2rayN](https://github.com/2dust/v2rayN) V2RayN 是一个基于 V2Ray 内核的 Windows 客户端。
    [V2rayU](https://github.com/yanue/V2rayU) V2rayU，基于 V2Ray 核心的 macOS 客户端，使用 Swift 4.2 编写，支持 VMess、Shadowsocks、SOCKS5 等服务协议，支持订阅，支持二维码、剪贴板导入、手动配置、二维码分享等。
   [v2rayA](https://github.com/v2rayA/v2rayA) 基于 web GUI 的 Linux 客户端，支持全局透明代理，兼容SS、SSR、Trojan、PingTunnel协议
   
   

# 最后



==请不要利用本文，用于非法途径。并且云服务有监控审核，慎重==


