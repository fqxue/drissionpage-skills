# 文档映射与任务手册

本文件只引用已经复制到当前 skill 内的资料，保证 `skills/drissionpage-dev` 可单独上传使用。

## 快速查找表

> 根据你要做的事情，直接找到对应文档。

| 我想要… | 首选文档 | 补充文档 |
|---------|----------|----------|
| 看代码示例写法 | `docs_en/demos/`、`docs_en/get_start/examples/` | — |
| 了解对象关系/选型 | `docs_zh/入门指南/☀️ 基本概念.md` | `docs_en/usage_introduction.md`、`docs_en/get_start/basic_concept.md` |
| 配置浏览器启动 | `docs_zh/控制浏览器/🛰️ 浏览器启动设置.md` | `docs_zh/控制浏览器/🛰️ 连接浏览器.md`、`docs_en/get_start/before_start.md` |
| 定位元素/查找失败 | `docs_zh/控制浏览器/🔦 定位语法.md` | `docs_zh/控制浏览器/🔦 页面或元素内查找.md`、`docs_en/get_elements/` |
| WebPage / 模式切换 | `docs_zh/入门指南/🗺️ 模式切换.md` | `docs_en/features/features_demos/switch_mode.md` |
| SessionPage 用法 | `docs_zh/SessionPage/` | — |
| 下载文件 | `docs_zh/下载文件/` | — |
| 全局设置/命令行/打包 | `docs_zh/进阶使用/` | — |
| DevTools MCP 协作 | `chrome-devtools-mcp.md` | `docs_zh/控制浏览器/🛰️ 页面交互.md`（`cdp()` 方法） |

> 以上路径均相对于 `references/`。所有文档查阅遵循"中文优先"原则。

## 常见任务执行顺序

### 修改页面对象行为

1. 先确认属于 `ChromiumPage`、`SessionPage` 还是 `WebPage`。
2. 读对应实现文件和相邻 `.pyi`。
3. 先看 `references/docs_en/demos/` 和 `references/docs_en/get_start/examples/` 是否已有接近案例。
4. 再用 `references/docs_zh/` 中文文档核对预期行为和参数说明。
5. 必要时再用 `references/docs_en/` 英文文档补充对照。
6. 只改一层逻辑，避免把问题扩散到多个对象。
7. 做最小 smoke test，必要时同步 skill 内文档副本与仓库原文档。

### 修改 locator 或元素行为

1. 先读 `DrissionPage/_functions/locator.py`。
2. 再看受影响元素类和页面类的调用链。
3. 对照 `references/docs_zh/控制浏览器/🔦 定位语法.md` 和 `references/docs_en/get_elements/` 的语法说明。
4. 如果是公开语法变化，必须同步 skill 内文档副本；若仓库文档也要维护，再同步回仓库。

### 修改配置、浏览器启动或 CLI

1. 先读 `DrissionPage/_functions/cli.py`。
2. 再读 `DrissionPage/_functions/tools.py` 和 `_configs/`。
3. 对照 `references/docs_zh/进阶使用/⚙️ 命令行的使用.md` 核对 CLI 行为。
4. 明确 `dp --configs-to-here`、浏览器路径、用户目录、端口处理是否受影响。
5. 需要时验证 `dp_configs.ini` 是否仍能正常生成。

### 编写示例或补文档

1. 默认先从 `references/docs_en/demos/` 选最接近的案例改写。
2. 如果 demos 不够，再参考 `references/docs_en/get_start/examples/`。
3. 示例仍不足时，从 `references/docs_zh/特性与示例/` 和 `references/docs_zh/入门指南/` 获取写法参考。
4. 复用现有写法和对象命名。
5. 示例尽量最小化，聚焦一个能力点。
6. 优先用中文写新增代码注释和打印日志，除非该文档上下文已固定为英文风格。
7. 示例若依赖浏览器环境，写清前提。

## 验证建议

| 改动类型 | 验证命令 |
|----------|----------|
| 纯导入/签名改动 | `python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage"` |
| 打包或入口改动 | `python -m build` 和 `dp --configs-to-here` |
| 浏览器相关改动 | 构造最小 `ChromiumPage()` 或 `WebPage()` 场景，记录本地浏览器与端口前提 |
