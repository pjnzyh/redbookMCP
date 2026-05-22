## \---

## base: /rebbookMCP/

## \---

\---r
title: 小红书自动搜索评论工具（MCP Server 2.0）
theme: seriph
layout: center
class: lead
author: 项目维护者
footer: Redbook-Search-Comment-MCP2.0
paginate: true
---

# 小红书自动搜索评论工具（MCP Server 2.0）

> \\\*\\\*一站式自动化 · 智能评论 · 高效内容抓取\\\*\\\*

\---

<div style="font-size:2.2em;color:#2563eb;font-weight:bold;">Redbook-Search-Comment-MCP2.0</div>
<div style="font-size:1.2em;color:#3b82f6;">基于 Playwright + MCP 协议，集成 AI 评论生成</div>

\---

## 目录

1. 项目简介
2. 主要特点
3. 系统架构与文件说明
4. 安装与环境准备
5. 配置示例（MCP Client）
6. 启动与演示流程
7. 常见问题与排查
8. 路线图与扩展
9. 联系方式

\---

## 项目简介

* **目标**：自动化登录、搜索、抓取小红书笔记，并通过 MCP Client 调用大模型生成高质量评论，实现一站式内容运营。
* **应用场景**：

  * 新媒体/自媒体内容采集与评论自动化
  * 研究与数据分析
  * 自动化测试与 AI 辅助内容生成（仅限合规使用）
* **技术栈**：Python · Playwright · FastAPI/uvicorn · fastmcp

\---

## 主要特点

* <span style="color:#2563eb;font-weight:bold;">持久化登录</span>：浏览器上下文复用，扫码一次长期有效
* <span style="color:#2563eb;font-weight:bold;">多策略笔记抓取</span>：支持滚动、等待、DOM 解析，适应页面变化
* <span style="color:#2563eb;font-weight:bold;">模块化流程</span>：分析 → 生成 → 发布，接口清晰，易于扩展
* <span style="color:#2563eb;font-weight:bold;">AI 智能评论</span>：集成 MCP Client，自动生成高质量评论
* <span style="color:#2563eb;font-weight:bold;">批量与定时任务</span>：支持批量采集、定时调度，适合自动化运营

\---

## 系统架构与文件说明

```mermaid
flowchart LR
  A\\\[MCP Client] <--> B\\\[MCP Server (xiaohongshu\\\_mcp.py)] --> C\\\[Playwright 浏览器] --> D\\\[小红书页面]
  B --> E\\\[data/ 抓取结果]
  B --> F\\\[browser\\\_data/ 持久化会话]
```

**核心文件结构：**

* `xiaohongshu\\\_mcp.py`：主服务入口，MCP 协议接口，负责登录、搜索、抓取、分析、评论发布
* `test\\\_mcp.py`：异步测试脚本，示例调用接口
* `browser\\\_data/`：浏览器持久化数据，扫码一次长期免登录
* `data/`：抓取结果存储（Markdown/JSON）
* `requirements.txt`：依赖列表

\---

## 关键文件说明

* `xiaohongshu\\\_mcp.py`：主服务入口，推荐通过 MCP Client 调用 `--stdio` 启动
* `test\\\_mcp.py`：异步测试脚本，示例 `login/get\\\_note\\\_content` 调用
* `requirements.txt`：安装所需依赖包
* `.vscode/mcp.json`：MCP Client 配置示例（见下）

\---

## MCP Client 配置示例

> \\\*\\\*Windows 配置（请替换为你的虚拟环境绝对路径）\\\*\\\*

```json
{
  "servers": {
    "xiaohongshu-mcp-server": {
      "command": "D:/.../venv/Scripts/python.exe",
      "args": \\\[
        "D:/.../xiaohongshu\\\_mcp.py",
        "--stdio"
      ],
      "type": "stdio"
    }
  }
}
```

<span style="color:#3b82f6;">请务必使用虚拟环境的绝对 python 路径，避免与系统 python 冲突。</span>

\---

## 安装与环境准备

**环境要求：**

* Windows 10/11
* Python 3.8 及以上
* 推荐使用虚拟环境

  **安装步骤：**

  ```powershell
python -m venv venv

  python -m venv venv
.\\venv\\Scripts\\Activate.ps1
pip install -r requirements.txt
pip install fastmcp
playwright install

  # 可选：升级 pip/setuptools

  pip install -U pip setuptools

  ```

  \\---

  ## 启动与测试流程

\* \*\*开发模式启动服务器：\*\*

  ```powershell
  python xiaohongshu\\\_mcp.py
  ```

* **用 test 脚本验证接口：**

  ```powershell
  python test\\\_mcp.py
  ```

* **MCP Client 调用（推荐）：**

  * 配置好 `.vscode/mcp.json` 后，直接在 VS Code 侧边栏一键启动

  \---

  ## 推荐演示流程

1. 启动 MCP Server（如上）
2. 浏览器首次扫码登录（会话持久化，无需重复扫码）
3. MCP Client 发起搜索任务（如“重庆 美食 攻略”）
4. 获取前 N 条笔记，自动导出为 Markdown/JSON（data/）
5. 一键调用 AI 生成评论，支持多种评论风格（引流/点赞/咨询/专业）
6. 选择发布/预审，支持批量与定时任务

   \---

   ## 常见问题与排查

* **页面抓取不到内容？**

  * 增加等待时间，检查网络与 User-Agent，清理 browser\_data 后重试
* **Playwright 无法启动浏览器？**

  * 执行 `playwright install`，检查本地浏览器二进制权限
* **MCP Client 无法连接？**

  * 检查 mcp.json 配置路径，确认虚拟环境已激活
* **Slidev/预览问题？**

  * 确认 slides.md 内容，使用 `npx @slidev/cli dev --entry ./slides.md --port 3030` 启动

  \---

  ## 调优与安全提示

* <span style="color:#2563eb;">请勿高频自动评论，严格遵守平台规则</span>
* 对抓取数据做去重、速率控制，防止账号异常
* 生产部署建议配置代理与限速，保障安全合规

  \---

  ## Roadmap（后续计划）

* 支持多账号池与代理，提升批量运营能力
* 引入 Web UI 结果分析面板，提升可视化体验
* 适配更多平台（如微博/知乎）
* 自动化测试与 CI/CD 集成，提升稳定性

  \---

  ## 联系方式与致谢

* 本项目基于 JonaFly/RednoteMCP 优化，by windsurf
* 欢迎 issue / PR / 讨论，合作请联系作者
* GitHub: [Redbook-Search-Comment-MCP2.0](https://github.com/JonaFly/RednoteMCP)

  \---

  # 附件与资源

* `README.md`（项目说明）
* `slides.md`（本幻灯片）
* `dev.md`（Slidev 默认入口）
* 若需同步 dev.md 并重启 Slidev，请回复“同步并重启”

  \---

  <div style="font-size:1.1em;color:#2563eb;">感谢体验！如需定制化开发或技术支持，欢迎联系。</div>

