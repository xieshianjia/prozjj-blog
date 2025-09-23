---
title: headscale部署使用
published: 2025-09-22
description: ""
image: ""
tags: ["vpn", "headscale"]
category: ""
draft: true
lang: "cn"
---

> 本次安装 headscale 的环境为 Linux 环境，使用 docker 部署服务。
>
> 宿主机目录 `/data/headscale/`为软件工作目录。

## 准备 config.yaml 文件

在工作目录创建 `config`目录，执行下述命令下载配置文件：

```bash
curl -o config.yaml https://github.com/juanfont/headscale/blob/main/config-example.yaml
```

## 准备 docker-compose 文件

```yaml
version: "3.8"

services:
  headscale:
    image: headscale/headscale:sha-474ea236
    container_name: headscale
    restart: unless-stopped
    command: headscale serve
    ports:
      - "8080:8080"
      - "3478:3478/udp"
    volumes:
      - /data/headscale/config/config.yaml:/etc/headscale/config.yaml:ro
      - /data/headscale/config/acl.hujson:/etc/headscale/acl.hujson:ro
      - /data/headscale/data:/var/lib/headscale
      - /data/headscale/run:/var/run/headscale
      - /etc/localtime:/etc/locatime:ro
```

headscale 的 docker 镜像版本可使用 `docker search headscale`进行查询。因为我的服务器问题，无法直接访问 docker.io，所以手动去 docker-hub 查询 headscale 最新镜像版本，地址 [headscale/headscale - Docker Image | Docker Hub](https://hub.docker.com/r/headscale/headscale) 。在官网可查询获得最新版本镜像命令为 `docker pull headscale/headscale:sha-474ea236`，版本号是 `sha-474ea236`。

## 常用操作命令
