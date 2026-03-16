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

## 推荐协作流程（最小闭环）

1. **DrissionPage 先复现**  
   用最小脚本稳定复现问题，并保留 URL、关键 selector、请求名、报错文本。
2. **MCP 做诊断**  
   在 DevTools 侧查看 Console/Network/Performance，定位失败阶段与根因线索。
3. **DrissionPage 落地修复**  
   把诊断结论转成代码调整（等待策略、定位策略、流程顺序、配置参数）。
4. **双向回归确认**  
   DrissionPage 再跑自动化流程；必要时让 MCP 复核关键网络请求与控制台状态。

> 建议每一步都输出“工具 + 结论 + 下一步”，避免职责重叠。

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

## 协作交接模板（可直接复用）

```text
【任务目标】
在 <URL> 完成 <操作目标>。

【DrissionPage 已完成】
1) 最小复现脚本：<步骤/关键代码>
2) 关键定位信息：<selector / 请求名 / 接口路径>
3) 期望结果：<预期>
4) 实际结果：<实际>，报错：<错误文本>

【请 MCP 诊断】
1) Console 是否有异常堆栈
2) Network 中 <请求名> 的状态码/耗时/失败原因
3) Performance 是否存在明显长任务或阻塞

【回到 DrissionPage 落地】
根据 MCP 结论给出最小改动方案，并提供回归验证步骤。
```
