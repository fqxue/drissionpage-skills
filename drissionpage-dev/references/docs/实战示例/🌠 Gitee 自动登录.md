# 🌠 Gitee 自动登录

本示例演示如何通过控制浏览器自动登录 Gitee 网站。

## ✅️️ 页面分析

URL：https://gitee.com/login

按 `F12` 查看代码，可以看到两个输入框都可以通过 `id` 属性定位。

---

## ✅️️ 编码思路

带有 `id` 属性的元素很容易定位，两个输入框都可以直接用 `id` 属性定位。
登录按钮没有 `id` 属性，但可以观察到它是第一个 `value` 属性为 `'登 录'` 的元素，因此也可以用中文文字定位，代码可读性更好。

由于是使用浏览器进行登录，所以使用 `ChromiumPage` 来控制浏览器。

---

## ✅️️ 示例代码

```python
from DrissionPage import ChromiumPage

# 创建页面对象（d 模式，默认模式）
page = ChromiumPage()
# 访问登录页面
page.get('https://gitee.com/login')

# 定位账号输入框并输入账号
page.ele('#user_login').input('你的账号')
# 定位密码输入框并输入密码
page.ele('#user_password').input('你的密码')

# 点击登录按钮
page.ele('@value=登 录').click()
```

---

## ✅️️ 代码逐行解析

```python
from DrissionPage import ChromiumPage
```

↑ 导入 `ChromiumPage` 类，用于控制浏览器。

```python
page = ChromiumPage()
```

↑ 创建 `ChromiumPage` 对象。

```python
page.get('https://gitee.com/login')
```

↑ `get()` 方法用于访问指定 URL，会等待页面加载完成后再执行后续代码。

```python
page.ele('#user_login').input('你的账号')
```

↑ `ele()` 方法用于查找元素，`#` 表示通过 `id` 属性定位。`input()` 方法用于向元素输入文字。`ele()` 内置等待功能，如果元素未加载，会等待直到元素出现或超时（默认 10 秒）。

```python
page.ele('@value=登 录').click()
```

↑ `@` 表示通过属性名搜索元素，`click()` 方法执行点击操作。

---

## ✅️️ 要点说明

- **元素定位**：`#` 定位 id，`@` 定位属性
- **链式调用**：`.ele().input()` 可直接链式操作
- **内置等待**：`ele()` 自带等待机制，无需额外添加等待
