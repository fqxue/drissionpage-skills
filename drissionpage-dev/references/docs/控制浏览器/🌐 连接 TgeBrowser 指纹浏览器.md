# 🌐 连接 TgeBrowser 指纹浏览器

> 来源：https://www.drissionpage.cn/tutorials/TgeBrowser

## 简介

TgeBrowser 提供 API 接口启动浏览器，可配合 DrissionPage 进行自动化控制。

[TgeBrowser 官网](https://www.tgebrowser.com)

**适用场景：** 批量操作、数据采集、自动化测试

---

## 安装依赖

```bash
pip install DrissionPage requests
```

---

## 快速开始

### 完整示例代码

```python
import requests
import time
from DrissionPage import ChromiumPage, ChromiumOptions

# ========== 配置 ==========
API_BASE_URL = "http://127.0.0.1:50326"
API_KEY = "your_api_key_here"  # 在 TgeBrowser 客户端获取

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

# ========== 步骤 1: 创建浏览器环境 ==========
print("1. 创建浏览器环境...")
response = requests.post(
    f"{API_BASE_URL}/api/browser/create",
    json={
        "browserName": "自动化测试",  # 环境名称
        "proxy": {
            "protocol": "socks5",
            "host": "proxy.example.com",
            "port": 8888,
            "username": "username",
            "password": "password",
        },
        "fingerprint": {
          "os": "Windows",
          "platformVersion": 11,
          "kernel": "135",
          "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.9.2537 Safari/537.36",
          "canvas": True,
          "speechVoices": True,
          "clientRects": True,
          "fonts": ["Arial", "Courier New"],
          "disableTLS": [],
          "resolution": "1920x1080",
          "ram": 8,
          "cpu": 4,
          "language": "en-US",
           "languageBaseIp": True,
          "timezone": "Europe/Amsterdam",
          "timezoneBaseIp": True,
          "hardwareAcceleration": True,
          "disableSandbox": False,
          "startupParams": "",
          "deviceName": "DESKTOP-ABCD",
          "portScanProtection": ""
        },
        "startInfo": {
          "startPage": {
            "mode": "custom",
            "value": [
                "https://www.baidu.com"
            ]
          },
          "otherConfig": {
            "openConfigPage": False,
            "checkPage": True,
            "extensionTab": True
          }
        }
    },
    headers=headers
)

env_id = response.json()["data"]["envId"]
print(f"   环境创建成功，ID: {env_id}")

# ========== 步骤 2: 启动浏览器 ==========
print("2. 启动浏览器...")
response = requests.post(
    f"{API_BASE_URL}/api/browser/start",
    json={"envId": env_id},
    headers=headers
)

debug_port = response.json()["data"]["port"]
print(f"   浏览器已启动，调试端口: {debug_port}")

# ========== 步骤 3: 连接 DrissionPage ==========
print("3. 连接 DrissionPage...")
time.sleep(3)  # 等待浏览器完全启动

# 方式1: 通过端口连接（推荐）
co = ChromiumOptions().set_local_port(debug_port)
page = ChromiumPage(addr_or_opts=co)

# 方式2: 直接使用端口号
# from DrissionPage import Chromium
# page = Chromium(debug_port)

# 方式3: 使用地址:端口
# page = Chromium(f'127.0.0.1:{debug_port}')

# 方式4: 使用 WebSocket URL（需要先获取 ws_url）
# ws_url = response.json()["data"]["ws"]
# page = Chromium(ws_url)

print("   DrissionPage 连接成功")

# ========== 步骤 4: 执行自动化操作 ==========
print("4. 执行自动化操作...")

# 访问网页
page.get('https://www.example.com')
print(f"   当前页面: {page.title}")

# 更多操作示例
page.get_screenshot('screenshot.png')  # 截图
print("   已保存截图")

print("\n完成！")
```

### 代码说明

#### 步骤 1: 创建浏览器环境

```python
response = requests.post(
    f"{API_BASE_URL}/api/browser/create",
    json={
        "browserName": "自动化测试",  # 环境名称
        "proxy": {
            "protocol": "socks5",
            "host": "proxy.example.com",
            "port": 8888,
            "username": "username",
            "password": "password",
        },
        "fingerprint": {
          "os": "Windows",
          "platformVersion": 11,
          "kernel": "135",
          "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.9.2537 Safari/537.36",
          "canvas": True,
          "speechVoices": True,
          "clientRects": True,
          "fonts": ["Arial", "Courier New"],
          "disableTLS": [],
          "resolution": "1920x1080",
          "ram": 8,
          "cpu": 4,
          "language": "en-US",
           "languageBaseIp": True,
          "timezone": "Europe/Amsterdam",
          "timezoneBaseIp": True,
          "hardwareAcceleration": True,
          "disableSandbox": False,
          "startupParams": "",
          "deviceName": "DESKTOP-ABCD",
          "portScanProtection": ""
        },
        "startInfo": {
          "startPage": {
            "mode": "custom",
            "value": [
                "https://www.baidu.com"
            ]
          },
          "otherConfig": {
            "openConfigPage": False,
            "checkPage": True,
            "extensionTab": True
          }
        }
    },
    headers=headers
)
env_id = response.json()["data"]["envId"]
```

**说明：**

- 调用 `/api/browser/create` 接口创建浏览器环境
- `browserName` - 给环境起个名字，方便识别
- `proxy` - 代理
- 返回 `envId` - 环境ID，后续操作都需要用到

#### 步骤 2: 启动浏览器

```python
response = requests.post(
    f"{API_BASE_URL}/api/browser/start",
    json={"envId": env_id},
    headers=headers
)
debug_port = response.json()["data"]["port"]
```

**说明：**

- 调用 `/api/browser/start` 接口启动浏览器
- 传入刚才创建的 `envId`
- 返回 `port` - 调试端口，DrissionPage 需要用这个端口连接

#### 步骤 3: 连接 DrissionPage（多种方式）

**方式 1: 使用 ChromiumOptions（推荐）**

```python
from DrissionPage import ChromiumPage, ChromiumOptions

time.sleep(3)  # 等待浏览器完全启动
co = ChromiumOptions().set_local_port(debug_port)
page = ChromiumPage(addr_or_opts=co)
```

**方式 2: 直接使用端口号**

```python
from DrissionPage import Chromium

time.sleep(3)
page = Chromium(debug_port)
```

**方式 3: 使用地址:端口**

```python
from DrissionPage import Chromium

time.sleep(3)
page = Chromium(f'127.0.0.1:{debug_port}')
```

**方式 4: 使用 WebSocket URL**

```python
from DrissionPage import Chromium

# 先获取 WebSocket URL
ws_url = response.json()["data"]["ws"]

time.sleep(3)
page = Chromium(ws_url)
```

**说明：**

- `time.sleep(3)` - 等待 3 秒，确保浏览器完全启动
- `debug_port` - 从 API 返回的调试端口
- `ws_url` - 从 API 返回的 WebSocket 连接地址
- 所有方式效果相同，选择最适合的即可

#### 步骤 4: 使用 DrissionPage 操作

```python
page.get('https://www.example.com')  # 访问网页
page.title  # 获取页面标题
page.get_screenshot('screenshot.png')  # 截图
```

**说明：**

- 现在可以使用 DrissionPage 的所有功能进行自动化操作

---

## 连接浏览器的多种方式

TgeBrowser API 返回的浏览器信息包含多个连接方式，可以根据需要选择：

```python
# API 返回的数据
{
  "success": true,
  "data": {
    "envId": 900,
    "browserName": "测试环境",
    "port": 34721,           # 调试端口
    "ws": "ws://127.0.0.1:34721/devtools/browser/xxx",  # WebSocket URL
    "http": "http://127.0.0.1:34721/json/version",
    "pid": 32512
  }
}
```

### 方式对比

| 方式 | 代码 | 说明 |
| --- | --- | --- |
| ChromiumOptions | `ChromiumPage(ChromiumOptions().set_local_port(port))` | 最灵活，可设置更多选项 |
| 端口号 | `Chromium(34721)` | 最简洁 |
| 地址:端口 | `Chromium('127.0.0.1:34721')` | 支持远程连接 |
| WebSocket | `Chromium('ws://127.0.0.1:34721/devtools/browser/xxx')` | 直接使用完整地址 |

### 完整示例

```python
import requests
import time
from DrissionPage import Chromium, ChromiumPage, ChromiumOptions

API_BASE_URL = "http://127.0.0.1:50326"
API_KEY = "your_api_key"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

# 创建并启动浏览器
response = requests.post(
    f"{API_BASE_URL}/api/browser/create",
    json={"browserName": "连接测试", "proxy": {"protocol": "direct"}},
    headers=headers
)
env_id = response.json()["data"]["envId"]

response = requests.post(
    f"{API_BASE_URL}/api/browser/start",
    json={"envId": env_id},
    headers=headers
)

# 获取连接信息
data = response.json()["data"]
port = data["port"]
ws_url = data["ws"]

print(f"调试端口: {port}")
print(f"WebSocket: {ws_url}")

time.sleep(3)

# ========== 方式 1: ChromiumOptions ==========
co = ChromiumOptions().set_local_port(port)
page1 = ChromiumPage(addr_or_opts=co)
print(f"方式1 连接成功: {page1.title}")

# ========== 方式 2: 端口号 ==========
page2 = Chromium(port)
print(f"方式2 连接成功: {page2.title}")

# ========== 方式 3: 地址:端口 ==========
page3 = Chromium(f'127.0.0.1:{port}')
print(f"方式3 连接成功: {page3.title}")

# ========== 方式 4: WebSocket URL ==========
page4 = Chromium(ws_url)
print(f"方式4 连接成功: {page4.title}")

# 所有方式都可以正常使用
page1.get('https://www.baidu.com')
```

### 选择建议

- **开发调试** → 使用方式 2（最简洁）： `Chromium(port)`
- **需要配置** → 使用方式 1： `ChromiumOptions()` 可以设置更多选项
- **远程连接** → 使用方式 3：支持指定 IP 地址
- **已有 URL** → 使用方式 4：直接使用完整 WebSocket 地址

---

---

## DrissionPage 常用操作

### 页面操作

```python
# 访问网页
page.get('https://www.example.com')

# 获取页面信息
title = page.title
url = page.url

# 截图
page.get_screenshot('screenshot.png')

# 执行 JavaScript
result = page.run_js('return document.title')

# 刷新
page.refresh()
```

### 元素定位

```python
# 通过 ID
element = page.ele('#username')

# 通过 Class
element = page.ele('.button')

# 通过文本
element = page.ele('text:登录')

# 通过属性
element = page.ele('@name=email')

# 获取多个元素
elements = page.eles('tag:li')
```

### 元素操作

```python
# 输入文本
page.ele('#username').input('user123')

# 点击
page.ele('#login-btn').click()

# 获取文本
text = page.ele('.title').text

# 获取属性
href = page.ele('tag:a').attr('href')
```

### 等待

```python
# 等待元素出现
page.wait.ele_displayed('#result', timeout=10)

# 等待页面加载
page.wait.doc_loaded()

# 固定等待
page.wait(3)
```

## 参考资料

- [DrissionPage 官方文档](https://drissionpage.cn/)
- [TgeBrowser API 文档](https://docs.tgebrowser.com/docs.html)
- [快速开始](https://docs.tgebrowser.com/start.html)
