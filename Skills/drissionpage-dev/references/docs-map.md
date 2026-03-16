# 文档映射与任务手册

本文件只引用已经复制到当前 skill 内的资料，保证 `Skills/drissionpage-dev` 可单独上传使用。

## 参考优先级

编写代码时按以下顺序查阅：**① 示例/demo → ② 中文文档(docs_zh) → ③ 英文文档(docs_en)**

## 文档映射

- 看代码生成示例，**最优先**：
  `references/docs_en/demos/`
  `references/docs_en/get_start/examples/`
- 看对象关系或选型：
  `references/docs_zh/入门指南/☀️ 基本概念.md`（中文优先）
  `references/docs_en/usage_introduction.md`
  `references/docs_en/get_start/basic_concept.md`
- 看浏览器启动和配置：
  `references/docs_zh/控制浏览器/🛰️ 浏览器启动设置.md`（中文优先）
  `references/docs_zh/控制浏览器/🛰️ 连接浏览器.md`
  `references/docs_en/get_start/before_start.md`
  `references/docs_en/get_start/import.md`
- 看元素定位和查找失败：
  `references/docs_zh/控制浏览器/🔦 定位语法.md`（中文优先）
  `references/docs_zh/控制浏览器/🔦 页面或元素内查找.md`
  `references/docs_en/get_elements/introduction.md`
  `references/docs_en/get_elements/usage.md`
  `references/docs_en/get_elements/not_found.md`
- 看 `WebPage` 或模式切换补充示例：
  `references/docs_zh/入门指南/🗺️ 模式切换.md`（中文优先）
  `references/docs_en/features/features_demos/switch_mode.md`
- 看 SessionPage 相关：
  `references/docs_zh/SessionPage/`（中文优先）
- 看下载文件：
  `references/docs_zh/下载文件/`（中文优先）
- 看进阶用法（全局设置、命令行、打包、异常等）：
  `references/docs_zh/进阶使用/`（中文优先）

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

- 纯导入/签名改动：
  `python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage"`
- 打包或入口改动：
  `python -m build`
  `dp --configs-to-here`
- 浏览器相关改动：
  构造一个最小 `ChromiumPage()` 或 `WebPage()` 场景，记录本地浏览器与端口前提。
