---
name: drissionpage-dev
description: 针对 DrissionPage 仓库进行源码阅读、功能修改、调试、示例编写、文档对照和兼容性修复。Use when tasks mention DrissionPage, ChromiumPage, SessionPage, WebPage, ChromiumOptions, SessionOptions, locator 语法, dp CLI, dp_configs.ini, docs_en, docs_zh, `.pyi` 类型声明，或要求基于当前仓库源码/文档实现新功能、修复行为、核对 API、补文档示例。
---

# DrissionPage Dev

## 参考优先级

编写代码时，按以下优先级查阅参考资料：

1. **示例 / Demo 优先**：先查 `references/docs_en/demos/` 和 `references/docs_en/get_start/examples/`，复用已验证的写法和调用链。
2. **中文文档其次**：示例不足时，查阅 `references/docs_zh/`（入门指南、控制浏览器、SessionPage、下载文件、进阶使用、特性与示例）获取详细说明和参数语义。
3. **英文文档补充**：中文文档未覆盖或需要对照时，再查 `references/docs_en/` 的其余文件。

## 快速流程

1. 先判断改动落点，再读取最小必要文件。
- 页面对象与模式切换：读 `references/architecture.md`，再看 `DrissionPage/_pages/`。
- 元素定位与解析：看 `DrissionPage/_functions/locator.py`、`DrissionPage/_elements/`，必要时对照 `references/docs_zh/控制浏览器/` 和 `references/docs_en/get_elements/`。
- 浏览器启动、配置和 CLI：看 `DrissionPage/_configs/`、`DrissionPage/_functions/tools.py`、`DrissionPage/_functions/cli.py`。
- 对外行为和示例：优先读取 skill 内已复制的示例和 `references/docs_zh/`，再用 `references/docs_en/` 补充，不要依赖 skill 外部文档路径。

2. 以源码为准，用 `references/docs_zh/` 和 `references/docs_en/` 核对公开行为、参数语义和示例。

3. 公开 API 一旦变化，同步检查这些位置。
- 对应模块的 `.pyi`
- `DrissionPage/__init__.py` 和 `DrissionPage/__init__.pyi`
- 受影响的 `references/docs_zh/` 和 `references/docs_en/` 示例或说明

4. 如果用户没有特别指定参考来源，优先从 `references/docs_en/demos/` 和 `references/docs_en/get_start/examples/` 提取代码结构、对象选型和调用顺序；其次从 `references/docs_zh/` 获取详细用法和参数说明；再落到源码实现核对细节。

5. 保持改动局部化，优先延续现有分层和命名，不随意重排内部 `_` 模块职责。

## 仓库内规则

- 优先使用中文编写新增注释、说明性示例注释和打印日志；只有当周围文件明确采用英文且保持一致更重要时，才跟随英文。
- 保持 4 空格缩进、UTF-8 源文件、`PascalCase` 类名、`snake_case` 方法名。
- 不要把 `ChromiumPage`、`SessionPage`、`WebPage` 的职责混淆：
  `ChromiumPage` 只负责浏览器控制；
  `SessionPage` 只负责请求/解析；
  `WebPage` 负责 d/s 双模式与 cookie 同步。
- 生成示例代码时，默认先复用 demos 中已经验证过的写法，再按当前任务收缩或扩展，不要无依据自创新调用链。
- 处理配置或安装问题时，优先参考 `setup.py`、`requirements.txt` 和实际代码；仓库内的 `pyproject.toml` 可能只是本地工作区配置，不要直接把它当成发布事实。

## 验证方式

- 这个仓库当前没有现成测试套件时，优先做针对性 smoke test，而不是假设存在完整 CI。
- 常用验证命令：

```bash
python -m pip install -e .
python -m build
python -c "from DrissionPage import ChromiumPage, SessionPage, WebPage; print(WebPage)"
dp --configs-to-here
```

- 涉及浏览器行为时，补一个最小可复现场景，明确浏览器路径、端口、配置文件或本地环境前提。
- 如果源码行为与 `references/docs_zh/` 或 `references/docs_en/` 中的副本不一致，优先指出差异；若仓库内原始文档也存在，再决定是否同步修正文档。

## 参考文件

- `references/architecture.md`
  用于快速了解包结构、三大 Page 对象关系、配置/CLI 入口、以及改 API 时的联动点。
- `references/docs-map.md`
  用于把任务快速映射到已复制进 skill 的文档副本，并给出常见改动的执行顺序。
- `references/docs_en/`
  保存从当前仓库复制进来的英文文档资料。默认优先查 `demos/`，其次查 `get_start/`、`get_elements/` 和 `features/features_demos/`。
- `references/docs_zh/`
  保存从 DrissionPage 中文站点整理的完整中文文档。包含入门指南、控制浏览器、SessionPage、下载文件、进阶使用、特性与示例六大板块。优先级高于 `docs_en/`（示例除外）。
- `references/bundled-materials.md`
  列出已复制到 skill 内的资料清单，便于脱离仓库单独上传时核对。
