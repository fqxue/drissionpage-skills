# DrissionPage Skills

> 为 AI 编程助手提供的 [DrissionPage](https://github.com/g1879/DrissionPage) 开发技能包，帮助 AI 更准确地理解和操作 DrissionPage 源码、文档与调试任务。

## 项目简介

本仓库是一个 **Copilot / AI Agent 技能（Skill）** 项目，目的是让 AI 编程助手在处理 DrissionPage 相关任务时，能够：

- 快速定位源码模块和文档位置
- 遵循 DrissionPage 的分层架构和命名规范
- 基于已有示例生成高质量代码
- 正确同步 API 变更涉及的所有联动文件

DrissionPage 是一个基于 Python 的网页自动化工具库，整合了数据包收发和浏览器控制两种模式，提供 `ChromiumPage`、`SessionPage` 和 `WebPage` 三种核心页面对象。

## 目录结构

```
Skills/
└── drissionpage-dev/
    ├── SKILL.md                    # 技能定义文件（入口）
    ├── agents/
    │   └── openai.yaml             # OpenAI Agent 接口配置
    └── references/
        ├── architecture.md         # 架构速览：包结构、核心对象、高影响模块
        ├── docs-map.md             # 文档映射与常见任务执行手册
        ├── bundled-materials.md    # 已打包到 Skill 内的资料清单
        ├── docs_zh/                # DrissionPage 中文文档（优先级高于 docs_en）
        │   ├── 入门指南/           # 安装、导入、基本概念、模式切换等
        │   ├── 控制浏览器/         # 浏览器控制、元素交互、定位语法等
        │   ├── SessionPage/        # SessionPage 相关文档
        │   ├── 下载文件/           # 文件下载相关
        │   ├── 进阶使用/           # 全局设置、命令行、打包等
        │   └── 特性与示例/         # 特性介绍与对比示例
        └── docs_en/                # 英文文档副本（含示例/demo，最优先参考）
            ├── usage_introduction.md
            ├── demos/              # 实战示例（豆瓣、猫眼、星巴克等）⬅ 最优先
            ├── get_start/          # 入门指南（概念、导入、准备工作）
            │   └── examples/       # 基础示例（浏览器控制、数据包、模式切换）⬅ 最优先
            ├── get_elements/       # 元素定位与查找语法
            └── features/
                └── features_demos/ # 特性演示（模式切换）
```

## 核心文件说明

| 文件 | 用途 |
|------|------|
| `SKILL.md` | 技能入口，定义了 AI 处理 DrissionPage 任务的完整流程、规则和验证方式 |
| `architecture.md` | 快速了解 DrissionPage 包结构、三大 Page 对象关系和改动检查清单 |
| `docs-map.md` | 根据任务类型快速映射到对应的参考文档，包含常见任务的执行步骤 |
| `bundled-materials.md` | 列出所有已复制到技能包内的文档（docs_en 和 docs_zh），确保脱离原仓库也能独立工作 |

## 安装与使用

本技能包支持多种 AI 编程助手。以下详细介绍在 **OpenAI Codex CLI** 和 **Anthropic Claude Code** 中的安装与使用方法。

---

### 在 OpenAI Codex CLI 中使用

[Codex CLI](https://github.com/openai/codex) 是 OpenAI 推出的终端 AI 编程助手，通过 `AGENTS.md` 文件加载项目级指令。

#### 第一步：安装 Codex CLI

```bash
npm install -g @openai/codex
```

确保已设置 `OPENAI_API_KEY` 环境变量：

```bash
export OPENAI_API_KEY="your-api-key"
```

#### 第二步：将技能包引入项目

进入你的 DrissionPage 项目目录，将本仓库的技能包复制进去：

```bash
cd /path/to/your-drissionpage-project

# 方式 A：克隆整个仓库到项目中
git clone https://github.com/fqxue/drissionpage-skills.git .drissionpage-skills

# 方式 B：只复制 Skills 目录
cp -r /path/to/drissionpage-skills/Skills ./Skills
```

#### 第三步：创建 AGENTS.md 配置文件

在项目根目录创建 `AGENTS.md`，引用技能包中的指令：

```markdown
# DrissionPage 开发指引

处理 DrissionPage 相关任务时，请遵循以下技能包中的规范和流程。

## 参考资料位置

- 技能入口定义：`Skills/drissionpage-dev/SKILL.md`
- 架构速览：`Skills/drissionpage-dev/references/architecture.md`
- 文档映射：`Skills/drissionpage-dev/references/docs-map.md`

## 参考优先级

编写代码时，按以下优先级查阅参考资料：

1. **示例 / Demo 最优先** — `Skills/drissionpage-dev/references/docs_en/demos/` 和 `Skills/drissionpage-dev/references/docs_en/get_start/examples/`
2. **中文文档其次** — `Skills/drissionpage-dev/references/docs_zh/`
3. **英文文档补充** — `Skills/drissionpage-dev/references/docs_en/` 其余文件

## 核心规则

- 以源码为准，用文档核对公开行为和参数语义
- 保持改动局部化，优先延续现有分层和命名
- 公开 API 变化时同步检查 `.pyi`、`__init__.py` 和文档示例
- 不要混淆 ChromiumPage（浏览器控制）、SessionPage（请求/解析）、WebPage（双模式）的职责
```

#### 第四步：启动 Codex

```bash
# 在项目目录下直接启动交互式会话
codex

# 或直接传入任务
codex "帮我用 ChromiumPage 写一个自动登录脚本"
```

Codex 会自动读取 `AGENTS.md` 和技能包中的参考文件，按照 `SKILL.md` 定义的流程处理 DrissionPage 相关任务。

#### 进阶：全局配置（可选）

如果你经常在多个项目中使用 DrissionPage，可以在全局指令文件中添加通用说明：

```bash
# 编辑全局指令
vi ~/.codex/instructions.md
```

在文件中添加：

```markdown
处理 DrissionPage 任务时，优先查阅项目内 Skills/drissionpage-dev/ 目录下的技能包。
```

---

### 在 Anthropic Claude Code 中使用

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) 是 Anthropic 推出的终端 AI 编程助手，通过 `CLAUDE.md` 文件加载项目级指令。

#### 第一步：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

确保已设置 `ANTHROPIC_API_KEY` 环境变量：

```bash
export ANTHROPIC_API_KEY="your-api-key"
```

#### 第二步：将技能包引入项目

进入你的 DrissionPage 项目目录，将本仓库的技能包复制进去（与 Codex 方式相同）：

```bash
cd /path/to/your-drissionpage-project

# 方式 A：克隆整个仓库到项目中
git clone https://github.com/fqxue/drissionpage-skills.git .drissionpage-skills

# 方式 B：只复制 Skills 目录
cp -r /path/to/drissionpage-skills/Skills ./Skills
```

#### 第三步：创建 CLAUDE.md 配置文件

在项目根目录创建 `CLAUDE.md`，引用技能包中的指令：

```markdown
# DrissionPage 开发指引

处理 DrissionPage 相关任务时，请遵循以下技能包中的规范和流程。

## 参考资料位置

- 技能入口定义：`Skills/drissionpage-dev/SKILL.md`
- 架构速览：`Skills/drissionpage-dev/references/architecture.md`
- 文档映射：`Skills/drissionpage-dev/references/docs-map.md`

## 参考优先级

编写代码时，按以下优先级查阅参考资料：

1. **示例 / Demo 最优先** — `Skills/drissionpage-dev/references/docs_en/demos/` 和 `Skills/drissionpage-dev/references/docs_en/get_start/examples/`
2. **中文文档其次** — `Skills/drissionpage-dev/references/docs_zh/`
3. **英文文档补充** — `Skills/drissionpage-dev/references/docs_en/` 其余文件

## 核心规则

- 以源码为准，用文档核对公开行为和参数语义
- 保持改动局部化，优先延续现有分层和命名
- 公开 API 变化时同步检查 `.pyi`、`__init__.py` 和文档示例
- 不要混淆 ChromiumPage（浏览器控制）、SessionPage（请求/解析）、WebPage（双模式）的职责
```

#### 第四步：启动 Claude Code

```bash
# 在项目目录下启动交互式会话
claude

# 或直接传入任务
claude "帮我用 ChromiumPage 写一个自动登录脚本"
```

Claude Code 会自动读取 `CLAUDE.md` 和技能包中的参考文件，按照定义的流程处理 DrissionPage 相关任务。

#### 进阶：使用 /add-memory 持久化指令（可选）

在 Claude Code 交互式会话中，你可以用 `/add-memory` 命令将常用指令写入持久记忆：

```
/add-memory "处理 DrissionPage 任务时，优先查阅项目内 Skills/drissionpage-dev/ 目录下的技能包。编写代码时按优先级参考：① 示例/demo → ② 中文文档(docs_zh) → ③ 英文文档(docs_en)。"
```

这条记忆会被保存到 `~/.claude/CLAUDE.md` 中，在所有项目中生效。

#### 进阶：全局配置（可选）

如果你经常在多个项目中使用 DrissionPage，也可以直接编辑全局指令文件：

```bash
vi ~/.claude/CLAUDE.md
```

在文件中添加：

```markdown
处理 DrissionPage 任务时，优先查阅项目内 Skills/drissionpage-dev/ 目录下的技能包。
```

---

### 触发条件

无论使用 Codex 还是 Claude Code，当任务涉及以下关键词时，AI 助手应按照技能包流程工作：

- `DrissionPage`、`ChromiumPage`、`SessionPage`、`WebPage`
- `ChromiumOptions`、`SessionOptions`
- locator 语法、`dp` CLI、`dp_configs.ini`
- `.pyi` 类型声明
- 基于 DrissionPage 源码/文档实现新功能、修复行为、核对 API、补文档示例

### AI 工作流程概览

```
判断改动落点 → 读取最小必要文件 → 以源码为准核对文档 → 改动局部化 → 同步联动文件 → 验证
```

详细流程参见 [`SKILL.md`](Skills/drissionpage-dev/SKILL.md)。

## DrissionPage 核心概念

| 对象 | 职责 |
|------|------|
| `ChromiumPage` | 纯浏览器控制，负责标签页、窗口、下载、CDP 相关能力 |
| `SessionPage` | 纯请求/解析，围绕 requests、response、headers 工作 |
| `WebPage` | 双模式（d/s），同时继承浏览器控制和请求解析，支持 cookie 同步 |

## 参考优先级

编写代码时，按以下优先级查阅参考资料：

1. **示例 / Demo 最优先** — `references/docs_en/demos/` 和 `references/docs_en/get_start/examples/`
2. **中文文档其次** — `references/docs_zh/`（入门指南、控制浏览器、SessionPage、下载文件、进阶使用、特性与示例）
3. **英文文档补充** — `references/docs_en/` 其余文件

## 参考示例

技能包内置了多个实战示例（位于 `references/docs_en/demos/`）：

- **豆瓣图书封面下载** — 演示 `save()` 方法直接从浏览器缓存保存图片
- **Gitee 自动登录** — 演示浏览器控制进行表单填写和提交
- **猫眼 TOP100 采集** — 演示数据抓取和结构化处理
- **多线程多标签页采集** — 演示 `get_tab()` 配合多线程同时操控多个标签页
- **星巴克产品图片下载** — 演示 `download()` 方法下载网络资源

## 贡献指南

欢迎提交 PR 来改进技能包：

1. 新增或更新 `references/docs_en/` 或 `references/docs_zh/` 下的文档副本时，同步更新 `bundled-materials.md`
2. 修改工作流程或规则时，更新 `SKILL.md` 中的对应部分
3. 新增示例时，优先复用已有写法和命名风格
4. 使用中文编写注释和说明

## 许可证

本项目遵循与 [DrissionPage](https://github.com/g1879/DrissionPage) 相同的使用约定。参考文档版权归 DrissionPage 原作者所有。
