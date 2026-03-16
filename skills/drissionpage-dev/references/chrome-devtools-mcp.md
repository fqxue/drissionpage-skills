# Chrome DevTools MCP 协作与接入

## 目标

在使用本 skill 处理 DrissionPage 任务时，可以同时接入 [`chrome-devtools-mcp`](https://github.com/ChromeDevTools/chrome-devtools-mcp)，让 AI 助手根据任务自动选择：

- 用 DrissionPage 完成页面控制、元素定位、流程自动化；
- 用 Chrome DevTools MCP 完成 DevTools 面板能力、调试与诊断；
- 必要时通过 CDP 语义进行互补。

## 协作边界（默认约定）

- **DrissionPage 负责**：`ChromiumPage` / `WebPage` 自动化流程、元素操作、下载、等待、模式切换。
- **Chrome DevTools MCP 负责**：DevTools 侧调试观察、性能/网络面板相关诊断能力。
- **共享协议基础**：Chrome DevTools Protocol（CDP）。DrissionPage 可通过 `cdp()` 调用（见 `references/docs_zh/控制浏览器/🛰️ 页面交互.md`）。

## 用户最小接入清单

1. 按本仓库 README 安装本 skill（保留完整仓库结构）。
2. 在你的 AI 客户端中安装并启用 `chrome-devtools-mcp`（按其官方 README 配置）。
3. 发起任务时明确说明“本任务可同时使用 DrissionPage skill + chrome-devtools-mcp”。
4. 对涉及浏览器行为的任务，优先让 AI 给出“工具分工说明”（哪一步用 DrissionPage，哪一步用 MCP）。

## 推荐提示词模板

```text
请使用 drissionpage-dev skill 完成自动化流程，并在需要 DevTools 诊断时接入 chrome-devtools-mcp。
输出时请标注每一步使用的工具（DrissionPage 或 MCP）以及原因。
```
