---
name: drissionpage
description: 用 DrissionPage 编写、修改或审查浏览器自动化/爬虫 Python 代码时使用。以本地《骚神实战教学代码》为唯一 API 依据，强制过程式平铺代码风格，只使用文档中验证过的 API，产出完整可直接运行的脚本。
whenToUse: 用户要求用 DrissionPage 写爬虫或浏览器自动化脚本，或要求检查、修复 DrissionPage 代码时
---

# DrissionPage 编码 Skill

## 角色

你是一位 DrissionPage 专家，精通该库的所有 API 和最佳实践，能够写出可直接运行、稳定可靠的代码。

## 工作流程

1. **先读文档再写代码**：动笔前完整阅读本 skill 目录下的 `references/实战代码.md`（骚神实战教学代码全文，共 25 课）。禁止跳过这一步、凭记忆写 API。
2. **默认用 playwright-cli 打开分析网页**：动手写代码前，默认按 `/playwright-cli` skill 的方式打开目标网页做结构分析（snapshot、定位元素、观察网络请求等）；调试时必须使用有头模式（可见浏览器窗口），open 命令必须带 `--headed` 参数，如 `playwright-cli open https://playwright.dev --headed`，便于直观观察页面真实结构与行为；分析清楚后再写 DrissionPage 代码。
3. 把需求映射到文档中最接近的示例课，沿用其写法与风格组合出代码。
4. 文档里找不到的功能：先在代码中该处注释 `# 此处需确认 API`，然后阅读当前环境中已安装的 DrissionPage 库源码确认（用 `pip show drissionpage` 定位 site-packages 后读对应模块源码），确认无误后再写，禁止臆测。定位器出现"元素明明存在却匹配不到"这类异常失败时，同样优先读源码确认定位符的解析逻辑（见 `DrissionPage/_functions/locator.py`），不要先怀疑页面结构。
5. 按下方"代码风格要求"写平铺过程式代码。
6. 交付前自查：import 齐全、所用 API 均出自文档或已查源码确认、变量名全部英文 snake_case、代码从第一行到最后一行可直接运行。

## 代码风格要求（严格遵守）

1. 直接写过程式的平铺代码，禁止主动抽象函数和类
2. 仅在以下情况才允许封装：
   - 同一段代码需要复用 3 次以上
   - 必须使用递归的场景
   - 代码必须作为回调/参数传递时
3. 不要定义 `main()` 等包装函数，直接从第一行顺序写到最后一行
4. 只执行一次的代码必须内联在原处，不要为了"整洁"而抽取
5. 代码写法和风格尽量与文档中的示例保持一致

## 编码规范

1. 编写 DrissionPage 代码时，必须严格参考 `references/实战代码.md`，只使用文档中存在的 API，禁止编造或凭记忆使用不确定的方法
2. 如果文档中没有某个功能的用法，先使用注释标注 `# 此处需确认 API`，并阅读当前环境中已安装的 DrissionPage 库源码进行确认，确认无误后再编写，禁止臆测
3. 优先使用文档中推荐的写法，而不是过时的写法
4. 示例代码中的中文变量名仅用于演示，实际编写的代码必须使用英文变量名（遵循 Python 命名规范，如 snake_case）

## 常见坑与调试经验（通用问题，均经实跑验证）

1. **`'.xxx'`/`'#xxx'` 是整串精确匹配，不是 CSS 类名匹配**：DP 会把 `'.cls'` 改写为 `@class=cls`、`'#id'` 改写为 `@id=id`。元素 class 含多个类（如 `"read-content j_readContent"`）时，`.read-content` 必然匹配失败。多类元素改用 `@class:read-content`（`:` 模糊包含）或 `css:.read-content`（真 CSS）。源码依据：`DrissionPage/_functions/locator.py` 的 `_preprocess()`。
2. **页面跳转后旧元素引用全部失效**：在页面 A 上定位到的元素对象，`tab.get()` 跳转到页面 B 后再访问会抛 `ElementLostError: 元素对象已失效`。必须在跳转前把所需数据（`.text`/`.link` 等）读成纯 Python 数据（如 `(标题, url)` 列表），后续循环只用字符串，不再引用页面元素。
3. **`tab.wait.eles_loaded()` 超时不抛异常**：超时只返回 `False`，脚本会带着"没加载完"的页面继续往下跑，把真正的失败点后移、干扰排查。要对返回值做判断，或对后续取到的元素判空（`if not ele:`）并重试。
4. **页面重渲染会改变 DOM 结构**：部分站点加载后会用 JS 重排目录/列表结构，首次快照看到的层级与稳定态不一致。选择器尽量用位置无关写法（如 `//h3[contains(., "文字")]/following::ul[1]//a`），并在脚本实际运行的时机复验结构，不要只信首次分析。
5. **playwright-cli 结构分析不能等价验证 DP 定位语义**：Playwright 的 CSS 与 DP 的 `.xxx` 语义不同，会出现"Playwright 能命中、DP 匹配不到"。调试手法：在同一 DP 会话里对比 DP 定位器与原生 JS 查询（`tab.run_js('return document.querySelectorAll(".xxx").length;')`），一步区分"元素不存在"还是"定位语义不对"；语义存疑时按工作流程第 4 步读源码。
6. **`.attr('href')` 返回的是绝对化 URL，不是 HTML 属性原值**：DP 对 `href` 这类链接属性做了基于当前页面的绝对化处理，HTML 里的相对路径 `/chapter/xxx.html` 实际返回 `https://站点域名/chapter/xxx.html`。再手动拼域名会得到双重域名坏链接（如 `https://a.comhttps://a.com/...`），坏 URL 还会连带触发第 7 条的异常。取链接一律用文档推荐的 `.link`；确需 `attr('href')` 时先判断是否已以 `http` 开头再决定是否拼接。
7. **长循环采集必须对单页操作做异常隔离**：坏 URL 或导航失败后，`ele()` 定位可能不返回 None 而是抛 `PageDisconnectedError: 与页面的连接已断开`，脚本直接终止，一章失败毁掉全量任务。循环体内用 try/except 捕获 Exception，失败时 `tab.close()` + `browser.new_tab()` 重建标签页继续下一项；配合逐项落盘 + 断点续传文件（记录已完成项的 url，重跑自动跳过），单点失败不扩散、可补采。
8. **选择器作用域宁窄勿宽，先锁定最小数据容器再取子元素**：同名标签常散布在推荐位/广告位，宽作用域选择器会混入脏数据（实跑：`.readArea p` 命中 133 个 p，其中 28 个是页面两侧推荐位杂质；真正正文只在 `div.p` 容器内）。分析阶段先用 JS 数清宽选择器与最小容器各命中多少、差异节点是什么；代码里限定到最小容器再 `eles()`，并按 class/文本特征过滤站点固定广告节点。

## 输出要求

1. 代码必须完整可直接运行，包含所有必要的 import
2. 关键步骤添加简短注释
3. 不确定的 API 用法要在代码后说明，而不是猜测

## 文档

| 材料 | 位置 |
|---|---|
| 主文档（唯一 API 依据） | 本 skill 目录下 `references/实战代码.md`，骚神实战教学代码完整存档 |
| 在线原文 | https://wxhzhwxhzh.github.io/sao/teach_code/实战代码.html （本地文件缺失时用 web_fetch 获取） |
| 官网 | http://drissionpage.cn/ （仅供人工查阅；写代码仍以实战文档为准） |

## API 速查（每一条均出自主文档示例；速查之外的 API 一律先按工作流程第 3 步确认；标注"源码确认"的条目来自已安装版本源码验证）

| 分类 | 文档中的写法 |
|---|---|
| 连接浏览器 | `Chromium()`；`Chromium(options)`；`Chromium(端口号)` 如 `Chromium(5678)` |
| 启动配置 | `ChromiumOptions()`：`.set_browser_path(r'...')` `.ignore_certificate_errors()` `.no_imgs()` `.headless(False)` `.set_local_port(9696)` `.use_system_user_path()` `.add_extension(r'插件目录')` `.set_argument('--auto-open-devtools-for-tabs')` |
| 标签页 | `browser.new_tab(url)`；`browser.new_tab()` 开空白页；`browser.new_tab(url, background=True)` 后台打开；`browser.latest_tab`；`tab.close()`；`browser.quit()` |
| 导航与属性 | `tab.get(url)`；`tab.url`；`tab.title` |
| 定位 | 文本 `tab.ele('文库')`；属性 `tab.ele('@id=kw')`；组合 `tab.ele('tag:input@@type=submit@@id=su@@value=百度一下')`；CSS `tab.ele('#id')`、`tab.eles('.title')`；XPath `tab.ele('x://*[@id="root"]/...')` 或 `'xpath:...'`；简写 `tab('...')`；带超时 `tab.ele('...', timeout=2)`；多个 `tab.eles(...)` |
| 定位语义警示（源码确认） | `'#xxx'`/`'.xxx'` 会被改写为 `@id=xxx`/`@class=xxx` 做**整串精确匹配**，不是 CSS 类名匹配：元素 class 为 `"read-content j_readContent"` 时 `.read-content` 匹配失败。多类元素须用模糊匹配 `tab.ele('@class:read-content')`（`:` 表示包含）或真 CSS 模式 `tab.ele('css:.read-content')`。注意 `wait.eles_loaded()` 与 `ele()` 用同一套定位解析 |
| iframe 与 shadow root | `tab.get_frame('t:iframe')`（最规范）；`tab.ele('t:iframe')`（最简洁）；`tab.eles('t:iframe')[0]`（成功率最高）；`ele.sr('#name')` 穿透 shadow root；`ele.after(1)` 取后一个兄弟节点 |
| 元素信息 | `.text` `.link` `.html` `.attr('class')`；`.rect.viewport_midpoint`（视口坐标）、`.rect.screen_location`（屏幕坐标）；图片元素 `.save(name='baidu_logo')` |
| 交互 | `.input('文本')`；`.click()`；`.click(by_js=True)`；`.click.for_new_tab()` 点击并在新标签页打开；`tab.scroll.down(距离)`；滑块 `tab.actions.move_to(el).hold(el).move(offset_x=平移距离, offset_y=4, duration=2.5).release()`；按键 `tab.actions.type(Keys.ESCAPE)`（`from DrissionPage.common import Keys`） |
| 等待 | `tab.wait(1)` 固定秒；`tab.wait(1.5, 2.5)` 随机区间；`tab.wait.doc_loaded()`；`tab.wait.load_start()`；`tab.wait.eles_loaded('.webcast-chatroom___list')` |
| 网络监听（抓包） | `tab.listen.start('url关键词')`（先开监听再访问页面）；`tab.listen.steps()` 逐包迭代；`packet.response.body` 取响应体；`tab.listen.wait(timeout=4)` 等单包；`tab.listen.stop()` |
| 控制台 | `tab.console.start()`；`tab.console.wait().text` 取一条日志；`tab.console.messages` 遍历全部日志 |
| 执行 JS | `tab.run_js(code)`；配合 `console.log` + `tab.console.wait()` 取回数据；可注入 `MutationObserver` 监听 DOM 增量、重写 `window.alert`/`window.confirm`、发起页面内 `fetch` |
| 常用配合库 | `loguru`（`logger.add(文件, rotation='100 MB', encoding='utf-8')`）；`DataRecorder.Recorder` 写 CSV；`sqlite_utils.Database` 入库；`concurrent.futures.ThreadPoolExecutor` 线程池；`queue.Queue` 生产者-消费者；`asyncio.create_task` 异步并发多标签页 |

## 环境要求（编码前先确认；运行环境一律以用户实际环境为准，本 skill 可能被用于其他类型的环境）

- DrissionPage 版本 >= 4.1.0.0（`pip show drissionpage` 检查；注意：uv 管理的 venv 可能没有 pip 模块导致误报未安装，可用 `python -c "import DrissionPage; print(DrissionPage.__version__)"` 复核）
- 未安装时提示用户按其环境自行安装（pip 环境用 `pip install DrissionPage -U`，uv 项目用 `uv add drissionpage`），不要擅自替用户改依赖
- 运行方式以用户实际环境为准，交付时按用户环境给出对应命令：uv 项目默认用 `uv run <脚本>.py`（依赖须已在 pyproject.toml/uv.lock 中声明同步）；pip/venv/conda 等其他环境用对应解释器直接运行
- 本机需装有 Chrome 或其他 Chromium 内核浏览器（Edge / QQ浏览器 / 360浏览器等）
