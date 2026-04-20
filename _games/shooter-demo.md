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
- 客户端/服务端分离架构：
客户端基于 Unity 与 C# 实现，服务端为独立 .NET 工程，职责拆分清晰，便于联机逻辑、业务逻辑和表现层解耦。

- TCP + KCP 双通道通信：
稳定业务消息如登录、房间、聊天、任务等走 TCP，位置同步、动画同步、发弹、受击、技能施放等高频实时消息走 KCP，兼顾可靠性与实时性。

- Protobuf 协议序列化：
使用统一协议定义客户端与服务端消息结构，降低通信字段不一致带来的维护成本，同时减少消息体积。

- 主线程队列解耦网络线程：
网络消息不会直接操作 Unity 场景对象，而是先进入线程安全队列，再回到主线程统一处理，保证线程安全和运行时稳定性。

- 大厅、房间与战斗全流程联机：
完整实现了登录注册、房间列表、创建/加入房间、准备同步、开局倒计时、实时战斗、死亡与观战等联机主流程。

- 集中式子弹管理 + 对象池优化：
子弹系统未采用传统“一颗子弹一个 MonoBehaviour”的模式，而是由 BulletManager 统一维护子弹实体列表，集中处理推进、生命周期、碰撞检测和回收；同时结合对象池减少高频 Instantiate / Destroy 带来的性能开销，这部分是明显借鉴 ECS / 数据导向 思想的优化实现。

- 技能系统与网络同步：
技能支持解锁、背包管理、槽位装备、冷却管理和网络同步施放。客户端先执行本地即时表现，服务端再做技能拥有状态与槽位合法性校验，并广播至房间内其他玩家。

- Buff 与战斗修正系统：
增益效果会直接作用于移动速度、后坐力、投射物数量、投射间距、子弹尺寸、飞行速度和击退力度等运行时参数，形成技能、Buff 与战斗表现之间的联动闭环。

- 分层有限状态机：
角色行为控制采用分层有限状态机，基础层处理移动与环境状态，动作层处理攻击、持枪和连招切换，并结合动画事件驱动关键时机。

- 数据驱动工作流：
配置以 Excel / Json -> ScriptableObject 的方式进入项目，结合 Addressables 进行资源编目和异步加载，实现武器、技能、音频、图标等内容的统一管理。

- 任务成长系统：
任务系统支持静态配置加载、玩家任务快照同步、进度推进、领奖、金币结算、后续任务解锁以及技能奖励解锁，并与技能背包系统联动。

- 运行时设置系统：
支持音量、分辨率、全屏、帧率上限、鼠标灵敏度和按键重绑定等设置的即时生效与本地持久化。

- 云端部署验证：
服务端已部署在 阿里云 环境，完成公网接入与远程联机验证，不仅能本地联调，也具备基础的云端运行能力。



## 演示视频（若视频或图片无法加载请连接VPN）
<video controls playsinline width="100%" poster="{{ page.cover | relative_url }}">
  <source src="{% if page.video_file contains '://' %}{{ page.video_file }}{% else %}{{ page.video_file | relative_url }}{% endif %}" type="video/mp4">
  你的浏览器不支持 video 标签。
</video>

<style>
  .demo-lightbox-gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 16px;
    margin-top: 16px;
  }

  .demo-lightbox-trigger {
    display: block;
    width: 100%;
    padding: 0;
    border: 0;
    background: transparent;
    cursor: zoom-in;
  }

  .demo-lightbox-trigger img {
    display: block;
    width: 100%;
    height: auto;
    border-radius: 16px;
    box-shadow: 0 10px 30px rgba(15, 23, 42, 0.12);
  }

  .demo-lightbox-overlay[hidden] {
    display: none;
  }

  .demo-lightbox-overlay {
    position: fixed;
    inset: 0;
    z-index: 9999;
    padding: 24px;
    background: rgba(3, 7, 18, 0.82);
    backdrop-filter: blur(6px);
  }

  .demo-lightbox-panel {
    width: min(96vw, 1400px);
    height: min(88vh, 920px);
    margin: 0 auto;
    display: grid;
    grid-template-rows: auto 1fr auto;
    gap: 14px;
  }

  .demo-lightbox-toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    color: #f8fafc;
  }

  .demo-lightbox-caption {
    font-size: 15px;
    color: rgba(248, 250, 252, 0.88);
  }

  .demo-lightbox-controls {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
  }

  .demo-lightbox-zoom {
    min-width: 56px;
    text-align: center;
    font-size: 14px;
    color: rgba(248, 250, 252, 0.86);
  }

  .demo-lightbox-btn {
    border: 0;
    border-radius: 999px;
    padding: 10px 14px;
    background: rgba(255, 255, 255, 0.14);
    color: #fff;
    font: inherit;
    cursor: pointer;
  }

  .demo-lightbox-btn:hover {
    background: rgba(255, 255, 255, 0.24);
  }

  .demo-lightbox-stage {
    position: relative;
    overflow: hidden;
    min-height: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 24px;
    background: rgba(2, 6, 23, 0.78);
    border: 1px solid rgba(255, 255, 255, 0.12);
    touch-action: none;
    user-select: none;
  }

  .demo-lightbox-image {
    display: block;
    max-width: 100%;
    max-height: 100%;
    user-select: none;
    transform-origin: center center;
    will-change: transform;
    pointer-events: none;
  }

  .demo-lightbox-hint {
    text-align: center;
    color: rgba(248, 250, 252, 0.76);
    font-size: 14px;
  }

  @media (max-width: 720px) {
    .demo-lightbox-overlay {
      padding: 12px;
    }

    .demo-lightbox-panel {
      height: min(92vh, 960px);
    }

    .demo-lightbox-toolbar {
      align-items: flex-start;
      flex-direction: column;
    }
  }
</style>

## 框架
<div class="demo-lightbox-gallery">
  <button
    type="button"
    class="demo-lightbox-trigger"
    data-demo-lightbox-src="{{ page.pic_network_sync_framework | relative_url }}"
    data-demo-lightbox-alt="{{ page.title }} 网络同步框架（用位置同步示例）"
  >
    <img
      src="{{ page.pic_network_sync_framework | relative_url }}"
      alt="{{ page.title }} 网络同步框架（用位置同步示例）"
      loading="lazy"
      style="cursor:zoom-in;"
    >
  </button>
  <button
    type="button"
    class="demo-lightbox-trigger"
    data-demo-lightbox-src="{{ page.pic_skill_system | relative_url }}"
    data-demo-lightbox-alt="{{ page.title }} 技能系统"
  >
    <img
      src="{{ page.pic_skill_system | relative_url }}"
      alt="{{ page.title }} 技能系统"
      loading="lazy"
      style="cursor:zoom-in;"
    >
  </button>
  <button
    type="button"
    class="demo-lightbox-trigger"
    data-demo-lightbox-src="{{ page.pic_task_system | relative_url }}"
    data-demo-lightbox-alt="{{ page.title }} 任务系统"
  >
    <img
      src="{{ page.pic_task_system | relative_url }}"
      alt="{{ page.title }} 任务系统"
      loading="lazy"
      style="cursor:zoom-in;"
    >
  </button>
</div>

<div class="demo-lightbox-overlay" data-demo-lightbox hidden>
  <div class="demo-lightbox-panel">
    <div class="demo-lightbox-toolbar">
      <div class="demo-lightbox-caption" data-demo-lightbox-caption>图片预览</div>
      <div class="demo-lightbox-controls">
        <span class="demo-lightbox-zoom" data-demo-lightbox-zoom>100%</span>
        <button type="button" class="demo-lightbox-btn" data-demo-lightbox-action="zoom-out">缩小</button>
        <button type="button" class="demo-lightbox-btn" data-demo-lightbox-action="zoom-in">放大</button>
        <button type="button" class="demo-lightbox-btn" data-demo-lightbox-action="reset">重置</button>
        <button type="button" class="demo-lightbox-btn" data-demo-lightbox-action="close">关闭</button>
      </div>
    </div>
    <div class="demo-lightbox-stage" data-demo-lightbox-stage>
      <img class="demo-lightbox-image" data-demo-lightbox-image alt="">
    </div>
    <div class="demo-lightbox-hint">滚轮可缩放，拖动可查看局部，按 Esc 可关闭。</div>
  </div>
</div>

<script>
  (function () {
    var overlay = document.querySelector("[data-demo-lightbox]");
    if (!overlay) {
      return;
    }

    var triggers = document.querySelectorAll("[data-demo-lightbox-src]");
    var stage = overlay.querySelector("[data-demo-lightbox-stage]");
    var image = overlay.querySelector("[data-demo-lightbox-image]");
    var caption = overlay.querySelector("[data-demo-lightbox-caption]");
    var zoomLabel = overlay.querySelector("[data-demo-lightbox-zoom]");
    var controls = overlay.querySelectorAll("[data-demo-lightbox-action]");
    var bodyOverflow = "";
    var scale = 1;
    var minScale = 1;
    var maxScale = 4;
    var zoomStep = 0.25;
    var offsetX = 0;
    var offsetY = 0;
    var dragging = false;
    var activePointerId = null;
    var dragStartX = 0;
    var dragStartY = 0;

    function clamp(value, min, max) {
      return Math.min(max, Math.max(min, value));
    }

    function clampOffset() {
      var maxOffsetX = Math.max(0, (image.offsetWidth * (scale - 1)) / 2);
      var maxOffsetY = Math.max(0, (image.offsetHeight * (scale - 1)) / 2);
      offsetX = clamp(offsetX, -maxOffsetX, maxOffsetX);
      offsetY = clamp(offsetY, -maxOffsetY, maxOffsetY);
    }

    function render(animate) {
      clampOffset();
      image.style.transition = dragging || animate === false ? "none" : "transform 120ms ease";
      image.style.transform = "translate(" + offsetX + "px, " + offsetY + "px) scale(" + scale + ")";
      stage.style.cursor = scale > 1 ? (dragging ? "grabbing" : "grab") : "zoom-in";
      zoomLabel.textContent = Math.round(scale * 100) + "%";
    }

    function resetView() {
      scale = 1;
      offsetX = 0;
      offsetY = 0;
      dragging = false;
      activePointerId = null;
      render(false);
    }

    function closeLightbox() {
      overlay.hidden = true;
      document.body.style.overflow = bodyOverflow;
      image.removeAttribute("src");
      image.alt = "";
      resetView();
    }

    function openLightbox(src, alt) {
      bodyOverflow = document.body.style.overflow;
      overlay.hidden = false;
      document.body.style.overflow = "hidden";
      image.src = src;
      image.alt = alt || "";
      caption.textContent = alt || "图片预览";
      resetView();
    }

    function setScale(nextScale) {
      scale = clamp(nextScale, minScale, maxScale);
      if (scale === minScale) {
        offsetX = 0;
        offsetY = 0;
      }
      render();
    }

    function endDrag(pointerEvent) {
      if (!dragging) {
        return;
      }
      if (pointerEvent && activePointerId !== null && pointerEvent.pointerId !== activePointerId) {
        return;
      }
      dragging = false;
      if (activePointerId !== null) {
        try {
          stage.releasePointerCapture(activePointerId);
        } catch (error) {
        }
      }
      activePointerId = null;
      render(false);
    }

    Array.prototype.forEach.call(triggers, function (trigger) {
      trigger.addEventListener("click", function () {
        openLightbox(
          trigger.getAttribute("data-demo-lightbox-src"),
          trigger.getAttribute("data-demo-lightbox-alt")
        );
      });
    });

    Array.prototype.forEach.call(controls, function (control) {
      control.addEventListener("click", function () {
        var action = control.getAttribute("data-demo-lightbox-action");
        if (action === "zoom-in") {
          setScale(scale + zoomStep);
        } else if (action === "zoom-out") {
          setScale(scale - zoomStep);
        } else if (action === "reset") {
          resetView();
        } else if (action === "close") {
          closeLightbox();
        }
      });
    });

    overlay.addEventListener("click", function (event) {
      if (event.target === overlay) {
        closeLightbox();
      }
    });

    stage.addEventListener(
      "wheel",
      function (event) {
        event.preventDefault();
        setScale(scale + (event.deltaY < 0 ? zoomStep : -zoomStep));
      },
      { passive: false }
    );

    image.addEventListener("load", function () {
      resetView();
    });

    stage.addEventListener("dblclick", function () {
      if (scale === 1) {
        setScale(2);
      } else {
        resetView();
      }
    });

    stage.addEventListener("pointerdown", function (event) {
      if (scale <= 1) {
        return;
      }
      event.preventDefault();
      dragging = true;
      activePointerId = event.pointerId;
      dragStartX = event.clientX - offsetX;
      dragStartY = event.clientY - offsetY;
      stage.setPointerCapture(activePointerId);
      render(false);
    });

    stage.addEventListener("pointermove", function (event) {
      if (!dragging || event.pointerId !== activePointerId) {
        return;
      }
      offsetX = event.clientX - dragStartX;
      offsetY = event.clientY - dragStartY;
      render(false);
    });

    stage.addEventListener("pointerup", endDrag);
    stage.addEventListener("pointercancel", endDrag);

    window.addEventListener("keydown", function (event) {
      if (overlay.hidden) {
        return;
      }
      if (event.key === "Escape") {
        closeLightbox();
      } else if (event.key === "+" || event.key === "=") {
        setScale(scale + zoomStep);
      } else if (event.key === "-" || event.key === "_") {
        setScale(scale - zoomStep);
      } else if (event.key === "0") {
        resetView();
      }
    });

    window.addEventListener("resize", function () {
      if (!overlay.hidden) {
        render(false);
      }
    });
  })();
</script>
