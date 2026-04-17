---
title: "联机射击 Demo"
layout: default
cover: /assets/images/games/shooter-cover.jpg
summary: "一个双人联机射击原型，包含大厅、建房、对战和结算。"
genre: "多人联机"
tech: "Unity / C# / TCP + KCP / Protobuf"
demo_status: "可演示"
video_file: /assets/videos/shooter-demo.mp4
download_file: /assets/downloads/shooter-demo.zip
download_label: 下载 Demo
---

<div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:16px;">
  <h1 style="margin:0;">{{ page.title }}</h1>
  <a
    href="{{ page.download_file | relative_url }}"
    download
    style="display:inline-block;padding:10px 18px;border-radius:999px;background:linear-gradient(135deg,#2563eb,#1d4ed8);color:#ffffff;font-weight:600;text-decoration:none;box-shadow:0 10px 24px rgba(37,99,235,0.22);white-space:nowrap;"
  >
    {{ page.download_label }}
  </a>
</div>

![{{ page.title }}]({{ page.cover | relative_url }})

## 项目简介
这是一个联机对战 Demo，包含登录、大厅、建房、战斗和结算流程。

## 技术点
- TCP 处理登录、大厅、建房等可靠流程
- KCP 处理位置、动画、子弹等高频同步
- Protobuf 协议与消息路由
- 断线重连与弱网处理

## 演示视频
<video controls playsinline width="100%" poster="{{ page.cover | relative_url }}">
  <source src="{{ page.video_file | relative_url }}" type="video/mp4">
  你的浏览器不支持 video 标签。
</video>
