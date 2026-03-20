# drissionpage-dev

`drissionpage-dev` 用来处理 DrissionPage 相关的脚本编写、网页调试、示例改写，以及在必要时维护上游仓库中的源码、CLI、类型声明和文档。

## 适用任务

- 在普通项目中编写或修改 `ChromiumPage`、`SessionPage`、`WebPage` 脚本
- 排查页面定位、等待时机、请求监听、下载保存、模式切换等问题
- 根据现有示例改写自动化脚本或最小复现
- 维护 DrissionPage 上游仓库中的公开 API、CLI、配置生成、类型声明或文档

## 默认工作方式

1. 先判断当前任务属于普通项目脚本，还是 DrissionPage 上游仓库维护。
2. 默认先读 `references/docs-map.md`，按任务类型跳到最合适的中文文档和实战示例。
3. 处理真实网页时，优先用浏览器调试工具确认页面流程、关键 selector、请求名和等待点。
4. 只有在文档不足且已经明确卡住时，才最小范围例外核对源码或 `.pyi`。
5. 完成实现后做最小 smoke test，而不是默认跑重型全量验证。

## 安装到 Codex

如果你是从 GitHub 仓库下载这个 skill，目录通常会是 `skills/drissionpage-dev/`。下载完成后，把整个 `drissionpage-dev` 文件夹复制到你本机的 Codex 技能目录中即可。

1. 下载或克隆仓库。
2. 打开仓库里的 `skills/drissionpage-dev/`。
3. 复制整个 `drissionpage-dev` 文件夹。
4. 粘贴到本机 Codex 技能目录：
   - Windows 示例：`C:\Users\你的用户名\.codex\skills\`
   - macOS / Linux 示例：`~/.codex/skills/`
5. 重新打开 Codex，或开始一个新会话，让技能被重新加载。

安装完成后，技能目录应类似这样：

```text
~/.codex/skills/
└── drissionpage-dev/
    ├── SKILL.md
    ├── agents/
    └── references/
```

## 目录结构

```text
drissionpage-dev/
├── SKILL.md
├── agents/
└── references/
    ├── docs-map.md
    ├── architecture.md
    ├── chrome-devtools-mcp.md
    └── docs/
```

## 关键资源

| 资源 | 作用 |
|------|------|
| `SKILL.md` | 规定技能的触发条件、默认流程、对象选型和验证边界 |
| `references/docs-map.md` | 文档导航入口，帮助按任务快速定位到最相关资料 |
| `references/docs/实战示例/` | 优先参考的写法来源，适合改写脚本和复用模式 |
| `references/docs/入门指南/` | 补充基本概念、模式切换、请求处理等基础说明 |
| `references/architecture.md` | 仅在维护上游仓库、排查源码级问题时使用的架构速览 |

## 使用要点

- 默认不要把源码和 `.pyi` 当作文档入口。
- 编写脚本时优先沿用现有示例的对象命名、注释风格和调用习惯。
- 处理浏览器行为时，优先明确“动作之后该等什么”，避免盲等。
- 维护上游仓库时，若改动公开 API、CLI、配置文件或类型声明，需要同步检查文档副本是否受影响。

## 验证建议

- 普通项目脚本：运行最小复现或最小 `DrissionPage` 脚本做 smoke test。
- 导入或公开签名改动：验证 `ChromiumPage`、`SessionPage`、`WebPage` 是否仍可正常导入。
- CLI 或配置改动：补测 `dp --configs-to-here` 以及相关浏览器配置流程。
- 浏览器相关改动：记录本地浏览器路径、端口、配置文件等前提后再回归。
