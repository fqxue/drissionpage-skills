# 架构速览

## 公开入口

- `DrissionPage/__init__.py`
  暴露 `Chromium`, `ChromiumPage`, `SessionPage`, `WebPage`, `ChromiumOptions`, `SessionOptions`。
- `DrissionPage/__init__.pyi`
  公开类型声明；公开签名变更时要同步。
- `DrissionPage/version.py`
  版本号来源。
- `setup.py`
  当前仓库更接近真实发布入口，包含依赖和 `dp` 命令行入口。

## 包结构

- `DrissionPage/_base/`
  浏览器、页面、元素的底层基类和连接能力。
- `DrissionPage/_pages/`
  页面对象主实现，重点关注 `chromium_page.py`、`session_page.py`、`web_page.py`、`chromium_tab.py`、`mix_tab.py`。
- `DrissionPage/_elements/`
  `ChromiumElement`、`SessionElement` 及相关对象。
- `DrissionPage/_configs/`
  `ChromiumOptions`、`SessionOptions`、配置读取与默认 ini。
- `DrissionPage/_functions/`
  locator 解析、cookies、工具函数、CLI、文本和通用 web 处理。
- `DrissionPage/_units/`
  setter、waiter、rect 等配套能力。

## 三大 Page 对象

- `ChromiumPage`
  纯浏览器控制对象；负责标签页、窗口、下载、CDP 相关能力。实现里有 `_PAGES` 单例缓存，修改构造逻辑时要谨慎。
- `SessionPage`
  纯请求/解析对象；围绕 requests、response、headers、session element 工作。
- `WebPage`
  同时继承 `SessionPage` 和 `ChromiumPage`，核心价值是 d/s 双模式切换与 cookie 同步。改这里时优先检查：
  `change_mode()`
  `cookies_to_session()`
  `cookies_to_browser()`
  `get()/post()/ele()/eles()`

## 高影响模块

- `DrissionPage/_functions/locator.py`
  定位语法转换中心；定位器行为变更通常会影响大量页面对象和文档示例。
- `DrissionPage/_functions/tools.py`
  包含端口寻找、配置复制、错误转换等通用能力；`configs_to_here()` 会生成 `dp_configs.ini`。
- `DrissionPage/_functions/cli.py`
  `dp` 命令入口，主要处理浏览器路径、用户目录、配置文件复制和启动浏览器。
- `DrissionPage/_functions/texts.py`
  文本与语言相关输出；修改报错文本或提示时可检查这里。

## 改动检查清单

1. 是否影响公开导入路径或构造参数。
2. 是否需要同步 `.pyi`。
3. 是否影响 `references/docs_zh/` 或 `references/docs_en/` 中的示例或参数说明。
4. 是否破坏 `WebPage` 的模式语义或 cookie 同步。
5. 是否影响 `dp` CLI 或 `dp_configs.ini` 生成流程。
