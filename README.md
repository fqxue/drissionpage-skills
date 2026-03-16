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
        ├── chrome-devtools-mcp.md  # Chrome DevTools MCP 协作与接入说明
        ├── docs_zh/                # DrissionPage 中文文档（优先级高于 docs_en）
        │   ├── README.md           # 中文文档总览
        │   ├── index.json          # 文档索引
        │   ├── 入门指南/           # 安装、导入、基本概念、模式切换等（10 个文件）
        │   ├── 控制浏览器/         # 浏览器控制、元素交互、定位语法等（33 个文件）
        │   ├── SessionPage/        # SessionPage 相关文档（9 个文件）
        │   ├── 下载文件/           # 文件下载相关（4 个文件）
        │   ├── 进阶使用/           # 全局设置、命令行、打包等（9 个文件）
        │   └── 特性与示例/         # 特性介绍与对比示例（10 个文件）
        └── docs_en/                # 英文文档副本（含示例/demo，最优先参考）
            ├── usage_introduction.md
            ├── demos/              # 实战示例（豆瓣、猫眼、星巴克等）⬅ 最优先
            ├── get_start/          # 入门指南（概念、导入、准备工作）
            │   └── examples/       # 基础示例（浏览器控制、数据包、模式切换）⬅ 最优先
            ├── get_elements/       # 元素定位与查找语法
            └── features/
                └── features_demos/ # 特性演示（模式切换）
```

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

详见 [`SKILL.md` 参考优先级](skills/drissionpage-dev/SKILL.md#参考优先级)。

## 与 Chrome DevTools MCP 协作

本技能包可与 [Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp) 配合使用，DrissionPage 负责自动化流程，MCP 负责 DevTools 侧调试诊断，两者通过 CDP 互补。

详细协作流程、接入清单和交接模板见：[`chrome-devtools-mcp.md`](skills/drissionpage-dev/references/chrome-devtools-mcp.md)

## 参考示例

技能包内置了多个实战示例（位于 `references/docs_en/demos/`），涵盖图片下载、自动登录、数据采集、多线程多标签页等场景。

## 贡献指南

欢迎提交 PR 来改进技能包：

1. 新增或更新 `references/docs_en/` 或 `references/docs_zh/` 下的文档副本时，同步更新 `bundled-materials.md`
2. 修改工作流程或规则时，更新 `SKILL.md` 中的对应部分
3. 新增示例时，优先复用已有写法和命名风格
4. 使用中文编写注释和说明

## 许可证

本项目遵循与 [DrissionPage](https://github.com/g1879/DrissionPage) 相同的使用约定。参考文档版权归 DrissionPage 原作者所有。
