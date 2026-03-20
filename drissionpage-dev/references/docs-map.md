# 文档映射与任务手册

本文件只引用已经复制到当前 skill 内的资料，保证 `skills/drissionpage-dev` 可单独上传使用。

## 快速查找表

> 根据你要做的事情，直接找到对应文档。
> 选中后只打开当前任务真正需要的那几个文件，不要整批扫读整个 `docs/` 树。

| 我想要… | 首选文档 | 补充文档 |
|---------|----------|----------|
| 看代码示例写法 | `docs/实战示例/` | `docs/入门指南/🗺️ 自动登录.md`、`docs/入门指南/🗺️ 收发数据包.md`、`docs/入门指南/🗺️ 模式切换.md` |
| 了解对象关系 / 选型 | `docs/入门指南/☀️ 基本概念.md` | `architecture.md`（仅上游仓库维护时） |
| 配置浏览器启动 | `docs/控制浏览器/🛰️ 浏览器启动设置.md` | `docs/控制浏览器/🛰️ 连接浏览器.md` |
| 定位元素 / 查找失败 | `docs/控制浏览器/🔦 定位语法.md` | `docs/控制浏览器/🔦 页面或元素内查找.md`、`docs/控制浏览器/🔦 语法速查表.md` |
| WebPage / 模式切换 | `docs/入门指南/🗺️ 模式切换.md` | `docs/特性与示例/⭐ 模式切换.md` |
| SessionPage 用法 | `docs/SessionPage/` | — |
| 下载文件 | `docs/下载文件/` | `docs/实战示例/🌠 星巴克图片下载.md`、`docs/实战示例/🌠 豆瓣图书封面下载.md` |
| 全局设置 / 命令行 / 打包 | `docs/进阶使用/` | `architecture.md`（仅上游仓库维护时） |
| DevTools MCP 协作 | `chrome-devtools-mcp.md` | `docs/控制浏览器/🛰️ 页面交互.md`（`cdp()` 方法） |

> 以上路径均相对于 `references/`。所有文档均为中文。

## 常见任务执行顺序

### 普通项目里编写或修改脚本

1. 先确认属于 `ChromiumPage`、`SessionPage` 还是 `WebPage`。
2. 先看 `references/docs/实战示例/` 是否已有接近案例。
3. 再用 `references/docs/` 各栏目文档核对预期行为和参数说明。
4. 涉及真实网页时，优先用 `chrome-devtools-mcp` 或最小复现确认请求、等待点和失败阶段。
5. 只改当前任务所需的一层逻辑，不要无依据扩散到多个对象。
6. 做最小 smoke test。

### 排查 locator 或元素查找问题

1. 先看 `references/docs/控制浏览器/🔦 定位语法.md`。
2. 再看 `references/docs/控制浏览器/🔦 页面或元素内查找.md` 和 `references/docs/控制浏览器/🔦 语法速查表.md`。
3. 用最小复现确认到底是定位写法问题、页面结构变化，还是等待时机问题。
4. 只有文档不足且明确卡住时，才最小范围例外核对 locator 相关源码。
5. 如果确认公开语法或行为应补充说明，先更新 skill 内文档副本；若当前任务是维护上游仓库，再决定是否同步回仓库。

### 修改配置、浏览器启动或 CLI（上游仓库维护）

1. 先读 `references/architecture.md` 了解影响面。
2. 对照 `references/docs/进阶使用/⚙️ 命令行的使用.md` 和 `references/docs/进阶使用/⚙️ 配置文件的使用.md` 核对公开行为。
3. 只有在文档不足且已明确卡住时，才最小范围例外核对 `cli.py`、`tools.py`、`_configs/`。
4. 明确 `dp --configs-to-here`、浏览器路径、用户目录、端口处理是否受影响。
5. 源码核对补充：
   - `dp --launch-browser 0` 表示使用配置文件中的端口；
   - `configs_to_here()` 默认生成 `dp_configs.ini`，可传 `save_name` 改名。
6. 需要时验证 `dp_configs.ini` 是否仍能正常生成。

### 编写示例或补文档

1. 默认先从 `references/docs/实战示例/` 选最接近的案例改写。
2. 如果实战示例不够，再参考 `references/docs/入门指南/` 中的入门示例。
3. 示例仍不足时，从 `references/docs/特性与示例/` 获取写法参考。
4. 复用现有写法和对象命名。
5. 示例尽量最小化，聚焦一个能力点。
6. 注释和日志优先跟随目标文件现有语言；新建中文示例时可优先用中文。
7. 示例若依赖浏览器环境，写清前提。

### 维护 DrissionPage 上游仓库源码

1. 先读 `references/architecture.md`，确认模块边界和高影响文件。
2. 先用 skill 内文档理解公开行为和对外承诺。
3. 只有在文档不足、行为冲突或必须落源码修复时，才最小范围例外阅读源码或 `.pyi`。
4. 若改动公开 API、CLI、配置文件或类型声明，检查 skill 内文档副本是否需要同步补充。

## 验证建议

| 改动类型 | 验证命令 |
|----------|----------|
| 普通项目脚本 | 最小脚本直接运行，或构造最小 `ChromiumPage()` / `WebPage()` 场景 |
| 纯导入 / 签名改动（上游仓库） | `python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage"` |
| 打包或入口改动（上游仓库） | `python -m build` 和 `dp --configs-to-here` |
| 浏览器相关改动 | 记录本地浏览器、端口、配置文件前提后做 smoke test |
