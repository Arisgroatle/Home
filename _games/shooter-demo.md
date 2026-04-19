---
title: "多人联机动作射击 Demo"
layout: default
cover: /assets/images/games/shooter-demo/shooter-cover.png
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
这是一个基于 Unity 2022.3 LTS 开发的多人联机动作射击原型项目，采用客户端与独立服务端分离的 Client-Server 架构。项目覆盖了账号登录、大厅与房间、准备阶段、实时战斗、任务成长、技能解锁与装备、运行时设置，以及云端部署验证等完整链路。
客户端主要负责输入采样、表现驱动与本地即时反馈；服务端负责房间管理、战斗同步、任务推进、技能合法性校验和玩家数据持久化。整体目标不是单一玩法演示，而是构建一套具备系统边界、分层职责和可扩展性的联机项目原型。

## 技术点
- TCP 处理登录、大厅、建房等可靠流程
- 客户端/服务端分离架构
客户端基于 Unity 与 C# 实现，服务端为独立 .NET 工程，职责拆分清晰，便于联机逻辑、业务逻辑和表现层解耦。

- TCP + KCP 双通道通信
稳定业务消息如登录、房间、聊天、任务等走 TCP，位置同步、动画同步、发弹、受击、技能施放等高频实时消息走 KCP，兼顾可靠性与实时性。

- Protobuf 协议序列化
使用统一协议定义客户端与服务端消息结构，降低通信字段不一致带来的维护成本，同时减少消息体积。

- 主线程队列解耦网络线程
网络消息不会直接操作 Unity 场景对象，而是先进入线程安全队列，再回到主线程统一处理，保证线程安全和运行时稳定性。

- 大厅、房间与战斗全流程联机
完整实现了登录注册、房间列表、创建/加入房间、准备同步、开局倒计时、实时战斗、死亡与观战等联机主流程。

- 集中式子弹管理 + 对象池优化
子弹系统未采用传统“一颗子弹一个 MonoBehaviour”的模式，而是由 BulletManager 统一维护子弹实体列表，集中处理推进、生命周期、碰撞检测和回收；同时结合对象池减少高频 Instantiate / Destroy 带来的性能开销，这部分是明显借鉴 ECS / 数据导向 思想的优化实现。

- 技能系统与网络同步
技能支持解锁、背包管理、槽位装备、冷却管理和网络同步施放。客户端先执行本地即时表现，服务端再做技能拥有状态与槽位合法性校验，并广播至房间内其他玩家。

- Buff 与战斗修正系统
增益效果会直接作用于移动速度、后坐力、投射物数量、投射间距、子弹尺寸、飞行速度和击退力度等运行时参数，形成技能、Buff 与战斗表现之间的联动闭环。

- 分层有限状态机
角色行为控制采用分层有限状态机，基础层处理移动与环境状态，动作层处理攻击、持枪和连招切换，并结合动画事件驱动关键时机。

- 数据驱动工作流
配置以 Excel / Json -> ScriptableObject 的方式进入项目，结合 Addressables 进行资源编目和异步加载，实现武器、技能、音频、图标等内容的统一管理。

- 任务成长系统
任务系统支持静态配置加载、玩家任务快照同步、进度推进、领奖、金币结算、后续任务解锁以及技能奖励解锁，并与技能背包系统联动。

- 运行时设置系统
支持音量、分辨率、全屏、帧率上限、鼠标灵敏度和按键重绑定等设置的即时生效与本地持久化。

- 云端部署验证
服务端已部署在 阿里云 环境，完成公网接入与远程联机验证，不仅能本地联调，也具备基础的云端运行能力。



## 演示视频
<video controls playsinline width="100%" poster="{{ page.cover | relative_url }}">
  <source src="{% if page.video_file contains '://' %}{{ page.video_file }}{% else %}{{ page.video_file | relative_url }}{% endif %}" type="video/mp4">
  你的浏览器不支持 video 标签。
</video>

## 框架
<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px;margin-top:16px;">
  <a href="{{ page.pic_network_sync_framework | relative_url }}" target="_blank" rel="noopener" style="display:block;">
    <img
      src="{{ page.pic_network_sync_framework | relative_url }}"
      alt="{{ page.title }} 网络同步框架（用位置同步示例）"
      loading="lazy"
      style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);cursor:zoom-in;"
    >
  </a>
  <a href="{{ page.pic_skill_system | relative_url }}" target="_blank" rel="noopener" style="display:block;">
    <img
      src="{{ page.pic_skill_system | relative_url }}"
      alt="{{ page.title }} 技能系统"
      loading="lazy"
      style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);cursor:zoom-in;"
    >
  </a>
  <a href="{{ page.pic_task_system | relative_url }}" target="_blank" rel="noopener" style="display:block;">
    <img
      src="{{ page.pic_task_system | relative_url }}"
      alt="{{ page.title }} 任务系统"
      loading="lazy"
      style="display:block;width:100%;height:auto;border-radius:16px;box-shadow:0 10px 30px rgba(15,23,42,0.12);cursor:zoom-in;"
    >
  </a>
</div>
