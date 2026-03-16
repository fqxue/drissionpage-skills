# 文档映射与任务手册

本文件只引用已经复制到当前 skill 内的资料，保证 `skills/drissionpage-dev` 可单独上传使用。

## 快速查找表

> 根据你要做的事情，直接找到对应文档。

| 我想要… | 首选文档 | 补充文档 |
|---------|----------|----------|
| 看代码示例写法 | `docs/实战示例/` | `docs/入门指南/🗺️ 自动登录.md`、`docs/入门指南/🗺️ 收发数据包.md`、`docs/入门指南/🗺️ 模式切换.md` |
| 了解对象关系/选型 | `docs/入门指南/☀️ 基本概念.md` | — |
| 配置浏览器启动 | `docs/控制浏览器/🛰️ 浏览器启动设置.md` | `docs/控制浏览器/🛰️ 连接浏览器.md` |
| 定位元素/查找失败 | `docs/控制浏览器/🔦 定位语法.md` | `docs/控制浏览器/🔦 页面或元素内查找.md`、`docs/控制浏览器/🔦 语法速查表.md` |
| WebPage / 模式切换 | `docs/入门指南/🗺️ 模式切换.md` | `docs/特性与示例/⭐ 模式切换.md` |
| SessionPage 用法 | `docs/SessionPage/` | — |
| 下载文件 | `docs/下载文件/` | `docs/实战示例/🌠 星巴克图片下载.md`、`docs/实战示例/🌠 豆瓣图书封面下载.md` |
| 全局设置/命令行/打包 | `docs/进阶使用/` | — |
| DevTools MCP 协作 | `chrome-devtools-mcp.md` | `docs/控制浏览器/🛰️ 页面交互.md`（`cdp()` 方法） |

> 以上路径均相对于 `references/`。所有文档均为中文。

## 常见任务执行顺序

### 修改页面对象行为

1. 先确认属于 `ChromiumPage`、`SessionPage` 还是 `WebPage`。
2. 读对应实现文件和相邻 `.pyi`。
3. 先看 `references/docs/实战示例/` 是否已有接近案例。
4. 再用 `references/docs/` 各栏目文档核对预期行为和参数说明。
5. 只改一层逻辑，避免把问题扩散到多个对象。
6. 做最小 smoke test，必要时同步 skill 内文档副本与仓库原文档。

### 修改 locator 或元素行为

1. 先读 `DrissionPage/_functions/locator.py`。
2. 再看受影响元素类和页面类的调用链。
3. 对照 `references/docs/控制浏览器/🔦 定位语法.md` 和 `references/docs/控制浏览器/🔦 语法速查表.md` 的语法说明。
4. 如果是公开语法变化，必须同步 skill 内文档副本；若仓库文档也要维护，再同步回仓库。

### 修改配置、浏览器启动或 CLI

1. 先读 `DrissionPage/_functions/cli.py`。
2. 再读 `DrissionPage/_functions/tools.py` 和 `_configs/`。
3. 对照 `references/docs/进阶使用/⚙️ 命令行的使用.md` 核对 CLI 行为。
4. 明确 `dp --configs-to-here`、浏览器路径、用户目录、端口处理是否受影响。
5. 需要时验证 `dp_configs.ini` 是否仍能正常生成。

### 编写示例或补文档

1. 默认先从 `references/docs/实战示例/` 选最接近的案例改写。
2. 如果实战示例不够，再参考 `references/docs/入门指南/` 中的入门示例。
3. 示例仍不足时，从 `references/docs/特性与示例/` 获取写法参考。
4. 复用现有写法和对象命名。
5. 示例尽量最小化，聚焦一个能力点。
6. 优先用中文写新增代码注释和打印日志。
7. 示例若依赖浏览器环境，写清前提。

## 验证建议

| 改动类型 | 验证命令 |
|----------|----------|
| 纯导入/签名改动 | `python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage"` |
| 打包或入口改动 | `python -m build` 和 `dp --configs-to-here` |
| 浏览器相关改动 | 构造最小 `ChromiumPage()` 或 `WebPage()` 场景，记录本地浏览器与端口前提 |
