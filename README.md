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
skills/
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

## 安装

本仓库遵循 [Agent Skills 规范](https://agentskills.io/specification)，可用于 Claude Code、Codex CLI、OpenCode 等兼容技能的 AI 编程助手。

### npx skills（推荐）

```bash
npx skills add git@github.com:fqxue/drissionpage-skills.git
```

### 手动安装

#### Claude Code

将仓库完整克隆到 Claude skills 目录（全局或项目内均可）：

```bash
git clone https://github.com/fqxue/drissionpage-skills.git ~/.claude/skills/drissionpage-skills
```

#### Codex CLI

将仓库完整克隆到 Codex skills 目录：

```bash
git clone https://github.com/fqxue/drissionpage-skills.git ~/.codex/skills/drissionpage-skills
```

#### OpenCode

将仓库完整克隆到 OpenCode skills 目录：

```bash
git clone https://github.com/fqxue/drissionpage-skills.git ~/.opencode/skills/drissionpage-skills
```

请不要只复制内部 `skills/` 目录，需保留完整仓库结构，确保技能入口路径为 `.../drissionpage-skills/skills/drissionpage-dev/SKILL.md`。

### 项目内引用（可选）

如果你更习惯在项目内显式声明规则，可在 `AGENTS.md` 或 `CLAUDE.md` 中加入以下内容：

```markdown
处理 DrissionPage 相关任务时，优先参考：
1. skills/drissionpage-dev/references/docs_en/demos/
2. skills/drissionpage-dev/references/docs_zh/
3. skills/drissionpage-dev/references/docs_en/ 其余文件
```

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

详细流程参见 [`SKILL.md`](skills/drissionpage-dev/SKILL.md)。

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
