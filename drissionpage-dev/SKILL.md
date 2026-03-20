---
name: drissionpage-dev
description: >-
  处理 DrissionPage 相关的脚本编写、示例改写、网页调试、文档检索和上游仓库维护。
  当用户提到 DrissionPage、ChromiumPage、SessionPage、WebPage、ChromiumOptions、
  SessionOptions、locator 语法、dp CLI、dp_configs.ini、类型声明，或需要用 DrissionPage
  完成浏览器自动化、请求解析、模式切换、下载、等待、网络监听时使用。默认先读 skill
  内中文文档和实战示例，不主动阅读源码或 .pyi；只有在维护上游仓库，或文档不足且明确卡住时
  才最小范围例外核对。
---

# DrissionPage Dev

## 工作边界

- 处理两类任务：
  1. 在任意项目里编写或调试 DrissionPage 脚本。
  2. 在 DrissionPage 上游仓库里维护源码、CLI、类型声明或文档。
- 默认按“普通项目脚本”处理，除非当前工作区明显就是上游仓库，或用户明确要求维护仓库本身。

## 默认入口

- 先读 `references/docs-map.md`，按任务类型跳到最合适的文档，不要盲目遍历目录。
- 文档参考顺序固定为：`实战示例` → `入门指南` → 相关栏目文档。
- 选中文档后，只打开解决当前问题所需的那几个文件，不要整段扫读 `references/docs/` 目录。
- 需要浏览器调试或网站分析时，优先使用 `chrome-devtools-mcp`；只有 MCP 不可用时，才回退到 DrissionPage 的 `cdp()` / `run_cdp()` 能力。

## 源码约束

- 默认禁止主动阅读 DrissionPage 包源码、上游仓库源码和 `.pyi` 类型声明，也不要把 `.pyi` 当作文档入口。
- 只有在以下条件同时满足时，才允许例外核对源码或 `.pyi`：
  1. 已按优先级查过 `实战示例`、`入门指南`、相关栏目文档；
  2. 仍然无法确认关键 API 行为，或者文档与实际行为明显冲突；
  3. 已优先尝试通过 `chrome-devtools-mcp`、最小复现、运行验证来确认问题。
- 发生上述例外时，只读取解决当前阻塞所必需的最小范围，并在对用户的说明中明确指出“这是例外核对，不是默认流程”。
- 若只是为了补充信心、确认签名、查看类型或“保险起见”，不构成阅读源码或 `.pyi` 的正当理由。

## 任务流程

### 普通项目脚本

1. 复述目标站点、预期结果和输入输出约束。
2. 看 `references/docs-map.md`，选最近的示例和栏目文档。
3. 用 MCP 或最小脚本确认页面流程、关键 selector、请求名和等待点。
4. 再用 DrissionPage 落地最小可行实现。
5. 做针对性 smoke test。

### 上游仓库维护

1. 先看 `references/docs-map.md` 和 `references/architecture.md`。
2. 先用 skill 内文档理解公开行为，再决定是否需要例外核对源码。
3. 若改动公开 API、CLI、配置生成或类型声明，检查文档副本是否要同步更新。
4. 做最小验证，不要默认跑重型全量构建。

## 代码风格

- 优先参考 `references/docs/实战示例/`，尽量保持与示例一致的写法和注释风格。
- 直接类导入，不使用 `import DrissionPage` 后再取类。
- 先创建页面对象，再调用 `get()` 导航。
- 优先使用 DrissionPage 自身定位语法，不默认退回 CSS selector 或 XPath。
- 注释、日志和文档字符串优先跟随周围文件的既有语言风格；若是新建的中文脚本或示例，可默认使用中文。
- 沿用示例中的命名习惯：`page`、`tab`、`ele`、`btn`、`img`、`item`、`items`、`recorder`。
- 简单操作可链式调用；需要复用元素时先赋值。
- 默认先找“动作之后该等什么”；优先等待加载、目标元素或目标请求，不要反复盲等。

## 对象选型

| 场景 | 正确对象 | 示例参考 |
|------|----------|-----------|
| 纯浏览器控制（登录、截图、缓存图片） | `ChromiumPage` | `实战示例/🌠 Gitee 自动登录.md`, `实战示例/🌠 豆瓣图书封面下载.md`, `实战示例/🌠 猫眼电影TOP100采集.md` |
| 纯请求/解析（无需浏览器） | `SessionPage` | `实战示例/🌠 星巴克图片下载.md`, `入门指南/🗺️ 收发数据包.md` |
| 需要浏览器 + 请求双模式切换 | `WebPage` | `入门指南/🗺️ 模式切换.md` |
| 多标签页操作 | `ChromiumPage` + `get_tab()` | `实战示例/🌠 多线程多标签页采集.md` |

## 下载与保存

- 浏览器缓存图片优先用 `img.save(...)`。
- URL 资源下载优先用 `page.download(...)`。

## 协作补充

- 与周围文件保持同一种注释和日志语言，避免在现有英文项目里混入中文差异；新建中文示例时再优先用中文。
- 需要浏览器调试或网站分析时，默认按“三段式协作”：MCP 先诊断 → DrissionPage 最小复现 → DrissionPage 落地修复并回归。
- 协作交接时至少包含：目标 URL、最小复现步骤、关键定位信息（selector / 请求名）、期望与实际差异、错误文本。

## 验证方式

- 普通项目脚本优先做针对性 smoke test，不要默认假设存在完整 CI，也不要默认跑打包命令。
- 如果当前工作区就是 DrissionPage 上游仓库，再考虑以下仓库级验证命令：

```bash
python -m pip install -e .
python -m build
python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage; print(WebPage)"
dp --configs-to-here
```

- 涉及浏览器行为时，补一个最小可复现场景，明确浏览器路径、端口、配置文件或本地环境前提。
- 如果例外核对源码后发现文档与行为不一致，优先把差异补到 skill 内文档；如果当前任务就是维护上游仓库，再决定是否同步回仓库文档。
