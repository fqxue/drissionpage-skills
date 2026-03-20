# 🥦 设置 cookies

> 来源：https://www.drissionpage.cn/tutorials/functions/set_cookies

## ✅️️ 设置 cookies

### 📌 页面对象中设置

任意页面对象都有`set.cookies()`方法，用于设置 cookies。

该方法接收多种格式的 cookies 信息，可设置一个或多个 cookies。

使用浏览器时，任意页面对象设置的 cookies 是所有标签页共用的（由`new_tab(new_context=True)`创建的标签页除外）。

**示例：**

```python
from DrissionPage import Chromium

tab = Chromium().latest_tab
cookies = 'name1=value1; name2=value2; path=/; domain=.example.com;'

tab.set.cookies(cookies)
```

---

### 📌 `SessionOptions`中设置

`SessionOptions`对象有`set_cookies()`方法，可接收一个或多个 cookies，用于`SessionPage`初始化时设置 cookies。

每次设置会覆盖之前所有 cookies 信息。

**示例：**

```python
from DrissionPage import SessionOptions

cookies = 'name1=value1; name2=value2; path=/; domain=.example.com;'

co = SessionOptions()
co.set_cookies(cookies)
```

---

### 📌 删除 cookies

页面对象用`set.cookies.remove()`和`set.cookies.clear()`删除和清空 cookies。

`SessionOptions`对象用`set_cookies(None)`清空 cookies。

具体用法详见使用文档有关章节。

---

## ✅️️ cookies 格式

### 📌 设置一个 cookie

设置一个 cookie 时，可传入`Cookie`、`dict`或`str`类型。

`dict`和`str`需要有`name`和`value`字段。

`str`多个字段间用`';'`或`','`分隔，但不能两种同时出现。

**格式：**

```python
# dict类型
{'name': 'abc', 'value': '123', 'domain': '.example.com', ...}

# str类型
'name=abc; value=123; domain=.example.com; ...'
```

---

### 📌 设置多个 cookies

设置多个时，可传入`CookieJar`、`list`、`tuple`、`str`、`dict`类型。

列表里面可以放`Cookie`、`str`或`dict`类型，多个 cookies 格式可以是不同的。

> [!NOTE] 注意
> 列表中如果放`str`或`dict`，每个项都只能是一个 cookie。

**格式：**

```python
# dict类型
{'abc': '123', 'def': '456', 'domain': '.example.com', ...}

# str类型
'abc=123; def=456; domain=.example.com; ...'

# list或tuple类型
['name=123; domain=.example.com; ...', 'name=abc; value=123; domain=.example.com; ...']
```

---

### 📌 说明

cookies 中只有`name`和`value`字段是必须的，但如果没有`domain`字段，添加到浏览器时会自动添加。

添加的内容根据调用`set.cookies()`方法的对象 url 而定。

比如一个 Tab 对象当前 url 为`'https://www.baidu.com'`，添加无指定域名的 cookies 时，会自动添加该字段，内容为`'www.baidu.com'`。
