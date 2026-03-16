# 贡献指南

感谢你对 DrissionPage Skills 的关注！欢迎提交 PR 来改进技能包。

## 提交流程

1. Fork 本仓库
2. 创建特性分支：`git checkout -b feature/your-feature`
3. 提交改动：`git commit -m '描述你的改动'`
4. 推送分支：`git push origin feature/your-feature`
5. 创建 Pull Request

## 改动须知

### 文档更新

- 新增或更新 `references/docs_en/` 或 `references/docs_zh/` 下的文档副本时，**必须**同步更新 `bundled-materials.md` 中的清单
- 新增中文文档时，同步更新 `references/docs_zh/index.json`（使用 POSIX 路径分隔符 `/`）
- 文档文件名直接使用标题（含 emoji 前缀），与官网保持一致

### 工作流程与规则

- 修改工作流程或规则时，更新 `SKILL.md` 中的对应部分
- 涉及架构变更时，同步更新 `architecture.md`
- 涉及文档映射变更时，同步更新 `docs-map.md`

### 示例与代码

- 新增示例时，优先复用已有写法和命名风格
- 使用中文编写注释和说明
- 严格遵循 `SKILL.md` 中的"代码风格规范（强制）"章节

### Agent 配置

- 新增 Agent 平台支持时，在 `agents/` 目录下添加对应配置文件
- 确保 `default_prompt` 包含参考优先级说明

## 代码风格

- Markdown 文件使用 UTF-8 编码
- 使用 POSIX 路径分隔符（`/`），不使用 Windows 风格（`\\`）
- 中文与英文、中文与数字之间加空格
- 保持 4 空格缩进

## 问题反馈

- 使用 GitHub Issues 提交 Bug 或建议
- 提交 Issue 时请说明你使用的 AI 助手平台（Claude Code / Codex CLI / OpenCode）
