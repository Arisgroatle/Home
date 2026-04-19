---
title: "联机射击 Demo"
layout: default
cover: /assets/images/games/shooter-cover.jpg
summary: "一个双人联机射击原型，包含大厅、建房、对战和结算。"
genre: "多人联机"
tech: "Unity / C# / TCP + KCP / Protobuf"
demo_status: "可演示"
video_file: https://github.com/Arisgroatle/Home/releases/latest/download/shooter-demo.mp4
download_file: https://github.com/Arisgroatle/Home/releases/latest/download/shooter-demo.zip
download_label: 下载 Demo
pic_network_sync_framework: /assets/images/games/shooter-demo/network-sync-framework.png
pic_skill_system: /assets/images/games/shooter-demo/skill-system.png
pic_task_system: /assets/images/games/shooter-demo/task-system.png
---

<div style="display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:16px;">
  <h1 style="margin:0;">{{ page.title }}</h1>
  <a
    href="{% if page.download_file contains '://' %}{{ page.download_file }}{% else %}{{ page.download_file | relative_url }}{% endif %}"
    {% unless page.download_file contains '://' %}download{% endunless %}
    target="_blank"
    rel="noopener"
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
  <source src="{% if page.video_file contains '://' %}{{ page.video_file }}{% else %}{{ page.video_file | relative_url }}{% endif %}" type="video/mp4">
  你的浏览器不支持 video 标签。
</video>

## 框架
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px;margin-top:16px;">
  <img
    src="{{ page.pic_network_sync_framework | relative_url }}"
    alt="{{ page.title }} 网络同步框架（用位置同步示例）"
    loading="lazy"
    style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);"
  >
  <img
    src="{{ page.pic_skill_system | relative_url }}"
    alt="{{ page.title }} 技能系统"
    loading="lazy"
    style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);"
  >
  <img
    src="{{ page.pic_task_system | relative_url }}"
    alt="{{ page.title }} 任务系统"
    loading="lazy"
    style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);"
  >
</div>
