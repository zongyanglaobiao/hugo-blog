---
title: Shadowrocket 支持 tailscale 功能
description: 最新版的 Shadowrocket 支持 tailscale 功能这意味着 tailscale 和 shadowrocket 的代理可以共存
# 默认url路径是title如果不写slug
slug: shadowrocketandtailscale
date: 2026-07-09 16:38:58+0000
# 是否生成目录
toc: true
categories:
  - shadowrocket
  - tailscale
tags:
  - shadowrocket
  - tailscalekeywords
id: baeb703a-b516-4a36-9690-90a952715825
# 是否可以添加评论
comments: true
---

## Shadowrocket 使用 tailscale 教程

原先在 iPhone 上使用 rustdesk 连接访问 tailscale 的局域网其他设备，这需要开启 tailscale 客户端程序，但同时 Shadowrocket 就无法使用，在 iPhone 多个 vpn 只能开启一个但是在最新版 Shadowrocket(2.2.90) 增加 tailscale 的功能就解决使用 tailscale 时不能使用 Shadowrocket

### 创建 tailscale auth key


![Snipaste_2026-07-09_16-52-40.png](img/vpn/Snipaste_2026-07-09_16-52-40.png)


### 客户端连接 tailscale

需要下载最新版 Shadowrock 客户端，按照步骤最后这里会显示已连接

![Snipaste_2026-07-09_16-57-25.png](img/vpn/Snipaste_2026-07-09_16-57-25.png)

此时浏览器就可以访问 tailscale 局域网内其他设备的服务

![Snipaste_2026-07-10_10-28-46.png](img/vpn/Snipaste_2026-07-10_10-28-46.png)

### rustdesk 无法通过 Shadowrocket tailscale 使用

虽然显示已连接但是通过 rustdesk 使用 tailscale 提供的 IP 去连接其他设备是失败的，从其他设备使用`tailscale ping 100.x.y.z`测试 iPhone 手机也是显示在线，单独开启 tailscale 客户端是可以正常连接上

![Snipaste_2026-07-09_17-05-15.png](img/vpn/Snipaste_2026-07-09_17-05-15.png)

通过 Shadowrocket 日志分析，发现 rustdesk 发出的请求走了官方地址 `rs-ny.rustdesk.com` 而没有 100.x.y.z 多次调整 rustdesk、shadowrocket的设置最终无果，希望有知晓的人指导一下
