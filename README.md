# drissionpage

一个用于**编写、修改和审查 DrissionPage 浏览器自动化 / 爬虫代码**的 Agent 技能（Skill）。

> **前置依赖**：本技能必选依赖 [playwright-cli](https://github.com/microsoft/playwright-cli)（用于写代码前对目标网页做结构分析），其安装与配置请自行参考官方文档。

## 这是什么

本技能让 AI 编码助手在处理 DrissionPage 任务时遵循一套经过实跑验证的规范：

- **唯一 API 依据**：以《骚神实战教学代码》（25 课完整存档）为准，只使用文档中验证过的 API，禁止凭记忆编造
- **平铺过程式代码风格**：不做无意义的抽象封装，产出完整可直接运行的脚本
- **先分析再动手**：写代码前先用浏览器工具打开目标网页做结构分析，而不是盲写选择器
- **不确认不编写**：文档未覆盖的 API，强制先读本机已安装的 DrissionPage 库源码确认后再使用
- **内置踩坑经验**：沉淀了 8 条实跑验证的常见坑与调试经验

## 安装

把仓库中的 `drissionpage` 文件夹**整体复制**到对应 Agent 的技能目录即可（仓库根目录的 `README.md` 仅供展示，不需要复制）：

| 运行环境 | 技能目录 |
|---|---|
| DSH Desktop (Windows) | `%APPDATA%\dsh-desktop\harness\skills\` |
| Codex | `~/.codex/skills/` |
| Claude Code | `~/.claude/skills/` |

安装完成后的目录结构：

```text
<技能目录>/
└── drissionpage/
    ├── SKILL.md            # 技能主文件：角色、工作流程、代码风格、编码规范、常见坑、API 速查
    └── references/
        └── 实战代码.md      # 骚神实战教学代码全文（25 课），唯一 API 依据
```

重启 Agent 或开启新会话即可生效。

## 工作流程

技能加载后，AI 会按以下流程工作：

1. **先读文档再写代码**：动笔前完整阅读 `references/实战代码.md`，禁止跳过这一步
2. **分析目标网页**：默认用 playwright-cli（有头模式）打开网页做结构分析——快照、定位元素、观察网络请求
3. **映射示例课**：把需求映射到文档中最接近的示例课，沿用其写法与风格组合代码
4. **源码确认兜底**：文档里找不到的功能先标注 `# 此处需确认 API`，读已安装库源码确认后再写
5. **交付前自查**：import 齐全、API 均有出处、变量名英文 snake_case、代码从第一行到最后一行可直接运行

## 内置常见坑（节选）

技能中沉淀了 8 条均经实跑验证的调试经验，例如：

- `'.xxx'` / `'#xxx'` 是整串精确匹配，**不是 CSS 类名匹配**——多类元素须用 `@class:xxx` 模糊匹配或 `css:` 前缀
- 页面跳转后旧元素引用全部失效——跳转前必须把数据读成纯 Python 数据
- `wait.eles_loaded()` 超时不抛异常、只返回 `False`——必须对返回值做判断
- `.attr('href')` 返回的是绝对化 URL——取链接一律用 `.link`
- 长循环采集必须对单页操作做异常隔离，配合逐项落盘 + 断点续传
- 选择器作用域宁窄勿宽，先锁定最小数据容器再取子元素

完整清单及 API 速查表见 [`drissionpage/SKILL.md`](drissionpage/SKILL.md)。

## 环境要求

- DrissionPage >= 4.1.0.0（`pip show drissionpage` 检查）
- 本机装有 Chrome 或其他 Chromium 内核浏览器（Edge / QQ浏览器 / 360浏览器等）
- playwright-cli（必选，用于网页结构分析）：安装与配置请自行参考官方文档 https://github.com/microsoft/playwright-cli
