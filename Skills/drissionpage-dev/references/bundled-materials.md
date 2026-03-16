# 已打包资料清单

这些文件已经从当前仓库复制到 skill 内，保证 `Skills/drissionpage-dev` 单独上传后仍可使用。

## 已复制的 docs_en 资料（英文文档）

- `references/docs_en/usage_introduction.md`
- `references/docs_en/demos/`（5 个示例）
  - `references/docs_en/demos/douban_book_pics.md`
  - `references/docs_en/demos/login_gitee.md`
  - `references/docs_en/demos/maoyan_TOP100.md`
  - `references/docs_en/demos/multithreading_with_tabs.md`
  - `references/docs_en/demos/starbucks_pics.md`
- `references/docs_en/get_start/`（6 个文件）
  - `references/docs_en/get_start/basic_concept.md`
  - `references/docs_en/get_start/before_start.md`
  - `references/docs_en/get_start/import.md`
  - `references/docs_en/get_start/examples/control_browser.md`
  - `references/docs_en/get_start/examples/data_packets.md`
  - `references/docs_en/get_start/examples/switch_mode.md`
- `references/docs_en/get_elements/`（3 个文件）
  - `references/docs_en/get_elements/introduction.md`
  - `references/docs_en/get_elements/usage.md`
  - `references/docs_en/get_elements/not_found.md`
- `references/docs_en/features/features_demos/switch_mode.md`

## 已复制的 docs_zh 资料（中文文档）

- `references/docs_zh/README.md`
- `references/docs_zh/index.json`
- `references/docs_zh/入门指南/`（9 个文件）— 安装、导入、基本概念、准备工作、常见问题、收发数据包、模式切换、自动登录等
- `references/docs_zh/控制浏览器/`（32 个文件）— 连接浏览器、Page 对象、元素交互、定位语法、标签页管理、截图录像、监听网络数据、动作链、等待、iframe 操作等
- `references/docs_zh/SessionPage/`（8 个文件）— 创建页面对象、启动配置、查找元素、获取元素/页面信息、访问网页、页面设置等
- `references/docs_zh/下载文件/`（3 个文件）— download 方法、浏览器下载等
- `references/docs_zh/进阶使用/`（8 个文件）— 全局设置、命令行、实用工具、异常处理、打包程序、数据读取加速等
- `references/docs_zh/特性与示例/`（9 个文件）— 特性介绍、与 selenium/requests 对比、模式切换、下载文件、获取元素属性等

## 使用顺序

1. 生成代码时，默认**优先**看 `references/docs_en/demos/` 和 `references/docs_en/get_start/examples/`（示例/demo 最优先）。
2. 示例不足时，**其次**查阅 `references/docs_zh/` 中文文档获取详细用法和参数说明。
3. 中文文档未覆盖或需要对照时，**再其次**看 `references/docs_en/` 其余英文文档和源码。
