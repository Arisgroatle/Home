---
title: "示例"
layout: default
cover: /assets/images/games/Default.jpg
summary: "一个示例。"
genre: "示例"
tech: "示例"
demo_status: "可演示"
video_file: /assets/videos/shooter-demo.mp4
---

# {{ page.title }}

![{{ page.title }}]({{ page.cover | relative_url }})

## 项目简介
这是一个联机对战 Demo，包含登录、大厅、建房、战斗和结算流程。

## 技术点
- 示例
- 示例
- 示例
- 示例

## 演示视频
<video controls playsinline width="100%" poster="{{ page.cover | relative_url }}">
  <source src="{{ page.video_file | relative_url }}" type="video/mp4">
  你的浏览器不支持 video 标签。
</video>
