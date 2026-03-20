# ⤵️ download方法

> 来源：https://www.drissionpage.cn/download/DownloadKit

DrissionPage 每种页面对象都内置一个下载工具，提供任务管理、多线程并发、大文件分块、自动重连、文件名冲突处理等功能。

该工具现已独立打包成一个库，名为 DownloadKit，详细介绍见：[DownloadKit](http://drissionpage.cn/DownloadKitDocs)。

这里只介绍其主要功能，具体使用和设置方法请移步该文档。

## ✅️️ 功能简介

### 📌 支持该工具的对象

以下对象均支持

- `SessionPage`
- `ChromiumTab`
- `MixTab`
- `ChromiumFrame`
- `ChromiumPage`
- `WebPage`

---

### 📌 下载器功能

- 可下载指定 url 文件
- 支持多线程并发下载多个文件
- 大文件自动分块使用多线程下载
- 可对现有文件追加数据
- 自动创建目标路径
- 下载时支持文件重命名
- 自动处理文件名冲突
- 自动去除路径和文件名中非法字符
- 支持 post 方式
- 支持自定义连接参数
- 任务失败自动重试

> [!NOTE] 注意
> `DownloadKit`是对 requests 封装实现的，不是调用浏览器功能。
> 如果下载目标对 headers、data 等有要求，必需手动添加。

---

## ✅️️ 添加任务

### 📌 单线程任务

使用`download()`方法可添加单线程任务，该方法是阻塞式的，且只使用一个线程。

**示例：**

```python
from DrissionPage import SessionPage

page = SessionPage()
url = 'https://www.baidu.com/img/flexible/logo/pc/result.png'
save_path = r'C:\download'

res = page.download(url, save_path)
print(res)
```

显示：

```shell
url：https://www.baidu.com/img/flexible/logo/pc/result.png
文件名：result.png
目标路径：C:\download
100% 下载完成 C:\download\result.png

('success', 'C:\\download\\result.png')
```

---

### 📌 并发任务

使用`download.add()`添加并发任务。

**示例：**

```python
url1 = 'https://dldir1.qq.com/qqfile/qq/TIM3.4.8/TIM3.4.8.22092.exe'
url2 = 'https://dldir1.qq.com/qqfile/qq/PCQQ9.7.16/QQ9.7.16.29187.exe'
save_path = 'files'

page = SessionPage()
page.download.add(url1, save_path)
page.download.add(url2, save_path)
```

---

### 📌 文件分块并行下载

使用`download.add()`方法的`split`参数可设置大文件是否分块下载。

使用`download.set.block_size()`方法可设置分块大小。

默认情况下载，超过 50M 的文件会自动分块下载。

**示例：**

```python
page = SessionPage()
page.download.set.block_size('30m')  # 设置分块大小
page.download.add('http://****/demo.zip')  # 默认分块下载
page.download.add('http://****/demo.zip', split=False)  # 不使用分块下载
```

---

### 📌 阻塞式多线程任务

使用并行分块下载时，也可以使任务逐个下载，在`add()`后使用`wait()`即可。

**示例：**

```python
page = SessionPage()
page.download.add('http://****/demo.zip').wait()
page.download.add('http://****/demo.zip').wait()
```

---

### 📌 详细使用文档

以上仅是普通示例，详细功能请查阅：[DownloadKit 添加任务](http://drissionpage.cn/DownloadKitDocs/usage/add_missions/)

---

## ✅️️ 下载设置

### 📌 全局设置

使用`download.set.****()`方法，可对默认下载行为进行设置。

包括以下设置：

- 保存路径
- 允许使用的线程总数
- 是否启用分块下载
- 分块大小
- 连接失败重试次数
- 重试间隔
- 连接超时时间
- 文件名冲突时的处理方式
- 日志和显示相关设置

---

### 📌 每个任务单独设置

新建任务时，`download()`和`add()`方法的参数可对当前任务进行参数设置，覆盖全局设置。

详见上文添加参数的文档。

---

### 📌 详细使用文档

详细设置功能请查阅：[DownloadKit 运行设置](http://drissionpage.cn/DownloadKitDocs/usage/settings/)

---

## ✅️️ 任务管理

### 📌 任务对象

对象`Mission`用于管理任务，有以下功能：

- 查看任务状态、信息、进度
- 保存任务参数，如 url、连接参数等
- 取消进行中的任务
- 删除已下载的文件

---

### 📌 获取单个任务对象

使用`download.add()`添加任务时，会返回一个任务对象。

**示例：**

```python
mission = page.download.add('http://****.pdf')
print(mission.id)  # 获取任务id
print(mission.rate)  # 打印下载进度（百分比）
print(mission.state)  # 打印任务状态
print(mission.info)  # 打印任务信息
print(mission.result)  # 打印任务结果
```

除添加任务时获取对象，也可以使用`download.get_mission()`获取。在上一个示例中可以看到，任务对象有`id`属性，把任务的`id`传入此方法，会返回该任务对象。

**示例：**

```python
mission_id = mission.id
mission = page.download.get_mission(mission_id)
```

---

### 📌 获取全部任务对象

使用页面对象的`download.missions`属性，可以获取所有下载任务。该属性返回一个`dict`，保存了所有下载任务。以任务对象的`id`为 key。

```python
page.download_set.save_path(r'D:\download')
page.download('http://****/****1.pdf')
page.download('http://****/****1.pdf')
print(page.download.missions)
```

**输出：**

```text
{
    1: <Mission 1 D:\download\xxx1.pdf xxx1.pdf>
    2: <Mission 2 D:\download\xxx1_1.pdf xxx1_1.pdf>
    ...
}
```

---

### 📌 获取下载失败的任务

使用`download.get_failed_missions()`方法，可以获取下载失败的任务列表。

```python
page.download_set.save_path(r'D:\download')
page.download('http://****/****1.pdf')
page.download('http://****/****1.pdf')
print(page.download.get_failed_missions()
```

**输出：**

```text
[
    <Mission 1 状态码：404 None>,
    <Mission 2 状态码：404 None>
    ...
]
```

> [!NOTE] Tips
> 获取失败任务对象后，可从其`data`属性读取任务内容，以便记录日志或择机重试。

---

### 📌 详细使用文档

详细设置功能请查阅：[DownloadKit 任务管理](http://drissionpage.cn/DownloadKitDocs/usage/misssions/)
