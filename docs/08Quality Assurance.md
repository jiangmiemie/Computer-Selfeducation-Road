---
sidebar_position: 9
---

# 质量控制

构建满足需求且没有缺陷的软件的关键是测试。软件测试帮助开发人员知道他们正在构建正确的软件。测试自动化金字塔:

- 单元测试
- 集成测试
- 端到端测试

**单元测试**

单元测试是测试软件的各个单元（模块、功能/方法、例程等）以确保其正确性的地方。这种低级别测试可确保较小的组件功能正常，同时减轻更高级别测试的负担。通常，开发人员在开发过程中编写这些测试，并将它们作为自动化测试运行。

单元测试构成了测试自动化金字塔的基础。他们测试单个组件或功能以验证其在隔离条件下是否按预期工作。在单元测试中运行几个场景是必不可少的——快乐路径、错误处理等。

- 由于这是最重要的子集，因此必须编写单元测试套件以尽快运行。
- 请记住，随着添加更多功能，单元测试的数量会增加。
- 每次添加新功能时都必须运行此测试套件。
- 因此，开发人员会立即收到有关各个功能是否正常工作的反馈。

**集成测试**

集成测试是一种广泛的测试类别，其中将多个软件模块作为一个整体进行集成和测试。它旨在测试多个服务、资源或模块之间的交互。例如，API 与后端服务的交互，或服务与数据库的交互。

单元测试验证一小部分代码库。但是，需要运行集成测试来测试此代码如何与其他代码（构成整个软件）交互。本质上，这些是验证一段代码与外部组件交互的测试。这些组件的范围从数据库到外部服务 (API)。

- 集成测试是测试自动化金字塔的第二层。这意味着它不应像单元测试那样频繁运行。
- 从根本上说，他们测试功能如何与外部依赖项通信。
- 无论是对数据库还是 Web 服务的调用，软件都必须有效通信并检索正确的信息才能按预期运行。

>请记住，集成测试涉及与外部服务的交互，因此它们的运行速度比单元测试慢。它们还需要一个预生产环境来运行。

**端到端测试**

功能测试是对软件进行测试以确保满足功能要求的地方。通常，它是一种黑盒测试形式，测试人员不了解源代码；通过提供输入并比较预期/实际输出来执行测试。它与非功能测试形成对比，非功能测试包括性能、负载、可扩展性和渗透测试。

测试金字塔的顶层是端到端测试。这些确保整个应用程序按要求运行。端到端测试正是顾名思义：测试应用程序是否从头到尾完美运行。

- 端到端测试位于测试金字塔的顶端，因为它们运行时间最长。
- 运行这些测试时，必须想象用户的视角。
- 实际用户将如何与应用程序交互/如何编写测试来复制该交互

## 接口测试

本章列举了一些直接发起请求的模块，这些模块在旧时代HTTP1.0时期，非Js渲染时代是我们获取网页数据的利器。

现在是[HTTP2.0](https://http2-explained.haxx.se/)（简称h2）以及以VUE为代表的Js渲染页面时代。因此这些模块现在主要用于获取直接的数据，譬如：图片、视频、纯静态的HTML网页或XHR(XMLHttpRequest)类型的请求。

请求的方法与HTTP协议对资源的操作对应

(1) GET：请求获取URL位置的资源
(2) HEAD：请求获取URL位置资源的响应消息报告，即获得该资源的头部信息
(3) POST：请求向URL位置的资源后附加新的数据
(4) PUT：请求向URL位置存储一个资源，覆盖原URL位置的资源
(5) PATCH：请求局部更新URL位置的资源，即更改该处资源的部分内容
(6) DELETE：请求删除URL位置存储的资源
(特殊)options ：向服务器获取一些服务器和客户端能够打交道的参数，但与获取资源并不直接相关

urllib模块：仅支持HTTP1.0 仅同步 ，但是解码和解析功能是真的很好用

request模块：仅支持HTTP1.0 仅同步  是基于urllib3的再次封装，因此语法更简单

aiohttp模块：支持HTTP2.0 仅异步

httpx模块：支持HTTP1.1/2.0 同步异步，双卡双待，是直接发起请求的模块中最全能的模块。

### urllib

urllib 是一个收集了多个涉及 URL 的模块的自带包：可以打开和读取 URL、 抛出异常、解析 URL、解析 robots.txt 文件是最底层的模块。

[urllib模块代码文档](https://docs.python.org/zh-cn/3/library/urllib.html?highlight=urllib#module-urllib)

#### urllib发送请求

```python
import urllib.request
 
url = 'https://www.python.org'
# 方式一
response = urllib.request.urlopen(url)
print(type(response))  # <class 'http.client.HTTPResponse'>
# 方式二
request = urllib.request.Request(url)
res = urllib.request.urlopen(url)
print(type(res))  # <class 'http.client.HTTPResponse'>
print(response.read())                  # 获取响应体 二进制字符串
print(response.getheaders())
## 结果为
[('Connection', 'close'), ('Content-Length', '50064'), ('Server', 'nginx'), ('Content-Type', 'text/html; charset=utf-8'), ('X-Frame-Options', 'DENY'), ('Via', '1.1 vegur, 1.1 varnish, 1.1 varnish'), ('Accept-Ranges', 'bytes'), ('Date', 'Tue, 17 Jan 2023 14:37:33 GMT'), ('Age', '1938'), ('X-Served-By', 'cache-iad-kiad7000025-IAD, cache-nrt-rjtf7700057-NRT'), ('X-Cache', 'HIT, HIT'), ('X-Cache-Hits', '263, 1190'), ('X-Timer', 'S1673966254.566369,VS0,VE0'), ('Vary', 'Cookie'), ('Strict-Transport-Security', 'max-age=63072000; includeSubDomains')]
```

#### urllib异常处理

URLError是OSError的一个子类，所有请求问题都会被捕获。

HTTPError是URLError的一个子类，服务器上HTTP的响应会返回一个状态码，根据这个HTTP状态码来决定是否捕获，比如常见的404错误等。

```python
from urllib import request
from urllib import error

if __name__ == "__main__":
    url = "http://www.iloveyou.com/"#一个不存在的连接
    req = request.Request(url)
    try:
        response = request.urlopen(req)
        print(response.read())
    except error.URLError as e:
        print(e) # <urlopen error [Errno 11002] getaddrinfo failed>
```

#### urllib解析 URL

你肯定经历过复制网址出现乱码，这是因为网址必须以通用码的形式传送，而且还要避免几个特殊字符，因此网址要经编码，汉字经过编码后自然就是不可辨认的乱码了。

那么浏览器的地址栏中，网址为什么看起来是中文呢？这大概是浏览器的“人性化”处理，将编码好的中文网址还原出来“暂时”显示在地址栏中。

知道原理就能清楚的解码啦，你可以通过encode和decode方法进行操作解码和转码，只不过要考虑汉字中有%等特殊字符和/x与%互转的情况，所以，直接用quote函数吧，别重复造轮子。

```python
from urllib.parse import unquote
from urllib.parse import quote

url = 'https://www.baidu.com/s?ie=UTF-8&wd=%E7%A7%91%E6%8A%80&%E6%8A%80%E6%9C%AF'
print(unquote(url))
# 结果为https://www.baidu.com/s?ie=UTF-8&wd=科技&技术


print( 'https://www.baidu.com/s?ie=UTF-8&wd='+quote('科技&技术'))
# 结果为'https://www.baidu.com/s?ie=UTF-8&wd=%E7%A7%91%E6%8A%80&%E6%8A%80%E6%9C%AF'
```

#### urllib解析robots.txt 文件

```python
import urllib.robotparser
rp = urllib.robotparser.RobotFileParser()
rp.set_url("http://www.musi-cal.com/robots.txt")
rp.read()

print(rp.can_fetch("*", "http://www.musi-cal.com/")) #判断网页是否可以抓取，'*'表示适用于所有爬虫
# True
```

### requests

requests是基于urllib3开发的一个优雅而简单的PythonHTTP库，为人类构建。

[requests模块官方文档](https://requests.readthedocs.io/en/latest/)

#### requests发送请求

```python
import requests
r = requests.get('https://api.github.com/events')
r = requests.post('https://httpbin.org/post', data={'key': 'value'})
r = requests.put('https://httpbin.org/put', data={'key': 'value'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.patch('http://httpbin.org/patch',data ={'key': 'value'})
r = requests.options('https://httpbin.org/get') 
```

#### requests异常处理

Requests库通过raise_for_status()引发支持六种常用的连接异常

(1) requests.ConnectionError：网络连接错误异常，如DNS查询失败、拒绝连接等
(2) requests.HTTPError：HTTP错误异常
(3) requests.URLRequired：URL缺失异常
(4) requests.TooManyRedirects：超过最大重定向次数，产生重定向异常
(5) requests.ConnectTimeout：连接远程服务器超时异常
(6) requests.Timeout：请求URL超时，产生超时异常（发出请求到获得内容的整个过程的超时异常）

```python
import requests
try:
    r=requests.get("https://www.baidu.com/error",timeout=30) #请求一个url链接
    r.raise_for_status() #如果状态不是200，引发HTTPError异常
except requests.HTTPError as e:
    print(e)# 404 Client Error: Not Found for url: https://www.baidu.com/error
```

### aiohttp

[aiohttp模块官方文档](https://docs.aiohttp.org/en/stable/)

#### aiohttp发起请求

```python
import aiohttp
import asyncio

async def main():
    async with aiohttp.ClientSession('http://httpbin.org') as session:
        async with session.post('/post', data=b'data'):
            pass
        async with session.put('/put', data=b'data'):
            pass
        async with session.get('/get') as resp:
            print(resp.status)
            print(await resp.text())

asyncio.run(main())
```

### httpx

HTTPX是Python 3的全功能HTTP客户端，它提供同步和异步API，并支持HTTP / 1.1和HTTP / 2。配置代理和请求头也很容易。httpx还有web测试模式、精细化配置代理IP、Event Hooks、SSL证书等语法，但是因为VUE时代的到来，这些实现都有了新的方案，所以不再过多介绍。

[httpx代码文档](https://www.python-httpx.org/)

### httpxH2检测

```python
import httpx
import asyncio

'''
检查 HTTP 版本
在客户端上启用 HTTP/2 支持并不一定意味着您的 请求和响应将通过 HTTP/2 传输，因为客户端和服务器都需要支持 HTTP/2。如果连接到仅 支持 HTTP/1.1 客户端将使用标准的 HTTP/1.1 连接。

您可以通过检查来确定使用了哪个版本的 HTTP 协议 响应上的属性。
'''
async def main():
        client = httpx.AsyncClient(http2=True)
        response = await client.get('https://www.python-httpx.org/')
        print(response.http_version)  # "HTTP/1.0", "HTTP/1.1", or "HTTP/2".
asyncio.run(main())
```

#### httpx发起请求

```python
import httpx

## HTTP / 1.1 同步请求模式A
r = httpx.post('https://httpbin.org/post', data={'key': 'value'})
r = httpx.put('https://httpbin.org/put', data={'key': 'value'})
r = httpx.delete('https://httpbin.org/delete')
r = httpx.head('https://httpbin.org/get')
r = httpx.options('https://httpbin.org/get')
r = httpx.patch('https://httpbin.org/get')
r = httpx.get('https://www.example.org/')

r # <Response [200 OK]>
r.status_code # 200
r.headers['content-type'] # r.headers['content-type']
r.text #'<!doctype html>...</html>'

## HTTP / 1.1 同步请求模式B
client = httpx.Client()
r = client.post('https://httpbin.org/post', data={'key': 'value'})
r = client.put('https://httpbin.org/put', data={'key': 'value'})
r = client.delete('https://httpbin.org/delete')
r = client.head('https://httpbin.org/get')
r = client.options('https://httpbin.org/get')
r = client.patch('https://httpbin.org/get')
r = client.get('https://www.example.org/')
client.close()


## 异步请求模式A
async def maina():
    async with httpx.AsyncClient() as client:
        r = await client.post('https://httpbin.org/post', data={'key': 'value'})
        r = await client.put('https://httpbin.org/put', data={'key': 'value'})
        r = await client.delete('https://httpbin.org/delete')
        r = await client.head('https://httpbin.org/get')
        r = await client.options('https://httpbin.org/get')
        r = await client.patch('https://httpbin.org/get')
        r = await client.get('https://www.example.com/')
    print(r)  #<Response [200 OK]>
asyncio.run(maina())

## 异步请求模式B
async def mainb():
    client = httpx.AsyncClient()
    r = await client.post('https://httpbin.org/post', data={'key': 'value'})
    r = await client.put('https://httpbin.org/put', data={'key': 'value'})
    r = await client.delete('https://httpbin.org/delete')
    r = await client.head('https://httpbin.org/get')
    r = await client.options('https://httpbin.org/get')
    r = await client.patch('https://httpbin.org/get')
    r = await client.get('https://www.example.com/')
    await client.aclose()
    print(r)  #<Response [200 OK]>
asyncio.run(mainb())

## H2模式下同步与异步请求
client = httpx.Client(http2=True)
client = httpx.AsyncClient(http2=True)

## 配置合并
headers = {'X-Auth': 'from-client'}
params = {'client_id': 'client1'}
with httpx.Client(headers=headers, params=params,http2=True,proxies="http://localhost:8030") as client:
    headers = {'X-Custom': 'from-request'}
    params = {'request_id': 'request1'}
    r = client.get('https://example.com', headers=headers, params=params)

r.request.url # URL('https://example.com?client_id=client1&request_id=request1')
r.request.headers['X-Auth'] # 'from-client'
r.request.headers['X-Custom'] # 'from-request'

```

#### httpx高级用法

如果需要监视大型响应的下载进度，可以使用响应流式处理并检查属性。response.num_bytes_downloaded

```python
import tempfile
import httpx
import rich.progress

with tempfile.NamedTemporaryFile() as download_file:
    url = "https://speed.hetzner.de/100MB.bin"
    with httpx.stream("GET", url) as response:
        total = int(response.headers["Content-Length"])

        with rich.progress.Progress(
            "[progress.percentage]{task.percentage:>3.0f}%",
            rich.progress.BarColumn(bar_width=None),
            rich.progress.DownloadColumn(),
            rich.progress.TransferSpeedColumn(),
        ) as progress:
            download_task = progress.add_task("Download", total=total)
            for chunk in response.iter_bytes():
                download_file.write(chunk)
                progress.update(download_task, completed=response.num_bytes_downloaded)
```

![rich-progress](/upload/computerselfeducationroad/rich-progress.gif)


## 自动化框架

从我们访问一个网址开始，到在浏览器看到漂亮的界面，这个过程经历了许多步骤。然后当你断开网络会发现，页面内容依然没有消失，他们在哪里呢？

我们知道网页分为三部分，互相协作：

- HTML 定义了网页的内容
- CSS 描述了网页的布局
- JavaScript 控制了网页的行为

其实还可以加上一项

- 资源(多媒体)在合适的时候被调用

当网络传输完毕，所有的数据被保存在缓存中，数据结构有很多种，常用的数据结构有：数组，栈，链表，队列，树，图，堆，散列表等。

其中散列表，也叫哈希表，是根据关键码和值 (key和value) 直接进行访问的数据结构，通过key和value来映射到集合中的一个位置，这样就可以很快找到集合中的对应元素。因此散列表非常适合用来存储网页这样加载顺序不固定的数据。所以浏览器把数据打上一个requestid，放入内存，然后执行网页的html、css以及javascript。当用到对应数据时，通过requestid去拿取就好了。

我们通过查询谷歌浏览器开发者协议(Chrome DevTools Protocol)可以找到这条命令，其实所有浏览器开发者协议都可以，这里只是以谷歌为例:
![image.png](/upload/computerselfeducationroad/image-3a9420394c8e4e2a8d2229b23d2376db.png)

那接着我们要做的事情就是：通过浏览器工具，获取完整的浏览器缓存，接着读取缓存就能找到对应的数据了，你可以抓取浏览器中的流数据，并根据流数据类型做分类保存；对下载好的页面做修改并截图实现高亮部分文字的效果；下载多媒体等。当你做到这一步时，你的爬虫认知就从定向爬虫转变为了通用型爬虫了。

现在市面上常见的模拟浏览器工具有以下几种，我建议熟读其中playwright的开发文档，再阅读其他框架的文档时，会发现大部分都是相通的。

selenium：老牌模拟浏览器  仅同步

Pyppeteer： Puppeteer的python实现，仅支持谷歌浏览器，异步

playwright：支持三种不同浏览器，异步，微软月更

接下来我们通过一个自动搜寻验证码图片并解析的例子来熟悉下整个流程。代码中所有路径部分请替换为你自己的地址，过程中会有极少的数据提取操作，可以参见后续数据提取教程。
### selenium

如果你使用的是selenium，可以参考这段代码

[selenium代码官方文档](https://www.selenium.dev/selenium/docs/api/py/api.html)

```python
from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
import time
import json
import base64
from PIL import Image
from urllib.parse import urlparse
from io import BytesIO

options = webdriver.ChromeOptions()
proxy = False # 代理设置：关闭
if proxy:
    options.add_argument('--proxy-server={}'.format(proxy))


d = DesiredCapabilities.CHROME      
d['loggingPrefs'] = {'performance': 'ALL'}
options.add_experimental_option('w3c', False) # 实验室选项，w3c协议 关闭

'''你应该经历过有部分网站证书过期,会跳出是否继续访问。这里我们忽视这些错误'''
options.add_argument('--ignore-certificate-errors') # 忽略证书错误
options.add_argument('--ignore-ssl-errors') #忽略ssl错误
options.add_argument('--headless') # 选择无头模式，加载更快
options.binary_location = 'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe'

driver = webdriver.Chrome('D:\\MyProgram File\\python\\chromedriver.exe',
                options=options, 
                desired_capabilities=d)

driver.execute_cdp_cmd("Network.enable", {}) # 启用网络跟踪，网络事件现在将传递给客户端。

driver.get('https://id.qq.com/vc.html')  # 打开一个网址
time.sleep(2)

for f in driver.find_elements_by_tag_name("form"): # 找到所有表单类信息
    urls = [i.get_attribute("src") for i in f.find_elements_by_tag_name("img")] #找到表单内所有的图片（大概率只有一个，且是验证码）
  
  
    for log in driver.get_log("performance"):
        log = json.loads(log["message"]) # 减小我们的检索范围
        if log["message"]["method"] == "Network.responseReceived": # 精准检索收到的数据

            for url in urls:
                url = urlparse(url).path #避免网页内的url中的https的有无导致匹配不上

                if url in log['message']['params']['response']['url']: # 特征匹配
                    request_id = log['message']['params']['requestId'] # 获取requestid
                    data = driver.execute_cdp_cmd("Network.getResponseBody", {"requestId": request_id})['body'] # 提取id对应的数据
                    data = base64.b64decode(data) # 从cdp开发文档可以得知返回的是base64编码的数据，我们他把转回二进制
                    source_img =Image.open(BytesIO(data))# 接着读取二进制数据
                    source_img.show()  #打开看看，获取部分到这里就结束了。
```

如果你遇到selenium元素无法点击，可以推测下原因：

- 窗口没有最大化导致目标元素不可见
- 没有等待页面渲染完成
- 没有等待AJX异步加载完成
- 元素在框架内，没有跳转到框架中

### Pyppeteer

Pyppeteer对流抓取支持的不是很好，但是对Js定位与执行做的非常不错，所以这段代码稍短一些。

[pyppeteer代码官方文档](https://pyppeteer.github.io/pyppeteer/)

```python
import asyncio
from pyppeteer import launch
from urllib import request

class Get_pic(): ## 自动获取验证码图片
    async def get_refer(self): # 定义主函数 
        browser = await launch(headless=False)
        page = await browser.newPage()
        await page.goto('https://id.qq.com/vc.html')          # 打开网址 
        image_src = await page.Jeval('#img', 'el => el.src') # 锁定图片，获取网络对象
        # 第一个参数代表定位方式【.】代表属性【#】代表id 【>】表示下属
        # 第二个参数代表要执行的js代码，  => 表示匿名函数，输出 src路径
        request.urlretrieve(image_src, 'image.png') # 把网络对象复制到本地
        await browser.close() # 关闭浏览器

new = Get_pic() #实例化
asyncio.run(new.get_refer()) # 调用方法
```

### playwright

如果你使用的playwright，可以参考下方的代码。我使用了2种方法来实现：

playwright的监听器整合了cdp协议中的接口，因此代码更加简洁。
playwright通用支持选中元素之后执行JS语法。

[playwright代码文档](https://playwright.dev/python/docs/intro)

```python
from PIL import Image
from urllib.parse import urlparse
from io import BytesIO
import asyncio
from urllib import request
from playwright.async_api import async_playwright

class Get_pic(): ## 自动获取验证码图片
    async def get_refer(self): # 定义主函数 
        async with async_playwright() as p:  # 创建一个playwright对象
            browser =await p.chromium.launch(headless=False)  # 启动浏览器，配置一下无头模式
            context = await browser.new_context(ignore_https_errors = True,bypass_csp=True) # 切换绕过页面的 Content-Security-Policy
            page = await context.new_page() # 创建页面
            self.img_data = {}
            page.on("response", self.on_response) # 注册一个监听器，读取已加载内容，并把读取到的response传递给on_response方法
            await page.goto('https://id.qq.com/vc.html')          # 打开网址 
            image_src = await page.eval_on_selector('#img','el => el.src')
            request.urlretrieve(image_src, 'image.png') # 把网络对象复制到本地
            image = page.locator("img")
            url = await image.get_attribute("src") # 获取页面内的img标签的src属性
            self.urls = urlparse(url).query #避免网页内的url中的https的有无导致匹配不上
            for u in self.img_data:
                if  self.urls in u:
                    source_img =Image.open(BytesIO(self.img_data[u]))
                    source_img.show()
            await browser.close() # 关闭浏览器

    async def on_response(self,response):
        '''
        response 为playwright 的 response对象
        '''
        self.img_data[response.url] = await response.body()

new = Get_pic() #实例化
asyncio.run(new.get_refer()) # 调用方法
```
<!-- 
## 移动自动化

### mitidump

### appnium

### Airtest

### adbutils

### Frida -->

## 数据提取
通过之前的学习，我们已经可以通过多种方式获取到数据了，可能是直接可以使用的HTML，也可能是JS、CSS代码、多媒体资源等。

接着我们需要从中提取出对我们有价值的数据，这一步是文本预处理。提取的方式有根据网页节点结构、JS语法、标签属性、纯文本规律等。

正则表达式：根据文字规律，解析最快

Xpath：根据网页节点路径，解析较快

CSS：根据网页的css项，可读性更强

Beatifulsoup：根据属性、节点、css ，解析最慢

parsel：根据正则、网页节点路径、属性、节点、css提取，解析较快

当然，除了对数据除了藏在网页之中，也有可能藏在json中。介绍2个处理json类型的模块。

json：python标准库，只兼容标准json，速度快。

execjs：兼容性最好，并且支持转换后执行js，但是速度较慢。



### 正则表达式

正则表达式作为多编程语言中的数据匹配工具，实用又简单，预计学习时长8小时。这里送上学习笔记和思维导图。

![编程笔记-正则表达式](/upload/computerselfeducationroad/re.jpg)

经典示例

```Python
import re

# findall
target = 'life is short, i learn python.'
result = re.findall('python', target)
result1 = re.findall('java', target)
# findall是re库的一个重要方法，第一个参数是匹配规则，第二个参数是要匹配的目标字符串，还有第三个参数，我们之后讲，findall返回的结果是一个列表。
# result这行代码的意思是从target中匹配'python',如果匹配到就返回，没有匹配到就返回空列表。
print(result)# 得到的结果是['python']
print(result1)# 得到的结果是[]


# 元字符
target = 'abc acc aec agc adc aic'
result = re.findall('a[de]c', target)
# 这一行中的[de]表示这个位置上的字符是d或者是e都可以匹配出来
print(result)# 得到的结果是['aec', 'adc']

result = re.findall('a[b‐z]c', target)
# 这一行中的[b‐z]表示这个位置上的字符在b‐z范围内都可以匹配出来
print(result)# 得到的结果是['abc', 'acc', 'aec', 'agc', 'adc', 'aic']

result = re.findall('a[^c‐z]c', target)
# 这一行中的[^c‐z]表示这个位置上的字符不在c‐z范围内都可以匹配出来，注意是不在
print(result)# 得到的结果是['abc']


# 示例
text = '我住在3号楼666,我的电话号码是17606000003你后面有事给我打电话，打不通就打17327567890。实在不行就打固定电话010-7788'
result = re.findall('\d{3}[\d-]\d*',text)
# \d{3}代表至少3个数字起匹配（区号和电话号码都满足）
# [\d-]代表后面跟着的可以是数字（电话号码），也可以是-
# \d*代表后面的数字我都要
print(result)#结果是['17606000003', '17327567890', '010-7788']

 
# 分组
line = "Cats are smarter than dogs"
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
#re.M表示多行匹配，影响 ^ 和 $
#re.I 使匹配对大小写不敏感
if matchObj:
   print ("matchObj.group() : ", matchObj.group())#返回所有组
   print ("matchObj.group(1) : ", matchObj.group(1)) # 返回组1【注意不是从0开始】
   print ("matchObj.group(2) : ", matchObj.groups())# 返回所有组的元组形式
else:
   print ("No match!!")


# 替换与检索sub
phone = "2004-959-559 # 这是一个国外电话号码"
# 删除字符串中的 Python注释 
num = re.sub(r'#.*$', "", phone)
print ("电话号码是: ", num)
# 删除非数字(-)的字符串 
num = re.sub(r'\D', "", phone)
print ("电话号码是 : ", num)

# 将匹配的数字乘以 2
def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)
s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)', double, s))


#贪婪与非贪婪
content = '发布于2018/12/23'
result = re.findall('.*?(\d.*\d)', content)
# 这里的?表示的就是非贪婪模式，第一个.*会尽可能少地去匹配内容，因为后面跟的是\d，所以碰见第一个数字就终止了。
print(result)

result = re.findall('.*(\d.*\d)', content)
# 这里的第一个.*后面没有添加问号，表示的就是贪婪模式，第一个.*会尽可能多地去匹配
#内容，后面跟的是\d，碰见第一个数字并不一定会终止，当它匹配到2018的2的时候，发现剩#下的内容依然满足(\d.*\d)，所以会一直匹配下去，直到匹配到12后面的/的时候，发现剩下
#的23依然满足(\d.*\d)，但是如果再匹配下去，匹配到23的2的话，剩下的3就不满足
#(\d.*\d)了，所以第一个.*就会停止匹配，(\d.*\d)最终匹配到的结果就只剩下23了。
print(result)

result = re.findall('.*?(\d.*?\d)', content)
# 这里的第一个.*?表示非贪婪模式(非贪婪模式就是尽可能少地去匹配字符)，匹配到2018前面的'于'之后就停止了
# 括号里的.*?也是表示非贪婪模式，括号里的内容从2018的2开始匹配，因为后面一个数字
#是0，那么也就满足了(\d.*?\d)，所以就直接返回结果了，同样的，接下来的18也是这样，一
#直匹配到23才结束
print(result)
```

### Xpath

Xpath的基本语法

更多实例可以查看菜鸟教程的[Xpath实例](https://www.runoob.com/xpath/xpath-syntax.html)

| 表达式       | 描述                                                                   |
| ------------ | ---------------------------------------------------------------------- |
| nodename     | 选取此节点的所有子节点                                                 |
| /            | 从根节点选取（取子节点）                                               |
| //           | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置（取子孙节点） |
| .            | 选取当前节点                                                           |
| ..           | 选取当前节点的父节点                                                   |
| @            | 选取属性                                                               |
| *            | 匹配任何元素节点                                                       |
| @*           | 匹配任何属性节点                                                       |
| node()       | 匹配任何类型的节点                                                     |
| /bookstore/* | 选取 bookstore 元素的所有子元素                                        |
| //*          | 选取文档中的所有元素                                                   |
| //title[@*]  | 选取所有带有属性的 title 元素                                          |
| text()       | 提取文本信息                                                           |

下面是一些具体的例子，另外 通过在路径表达式中使用"|"运算符，您可以选取若干个路径。譬如：

```
//title | //price	选取文档中的所有 title 和 price 元素。
```

| 表达式                              | 描述                                                                                    |
| ----------------------------------- | --------------------------------------------------------------------------------------- |
| /bookstore/book[1]                  | 选取属于 bookstore 子元素的第一个 book 元素                                             |
| /bookstore/book[last()]             | 选取属于 bookstore 子元素的最后一个 book 元素                                           |
| /bookstore/book[last()-1]           | 选取属于 bookstore 子元素的倒数第二个 book 元素                                         |
| /bookstore/book[position()< 3]      | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素                                 |
| //title[@lang]                      | 选取所有拥有名为 lang 的属性的 title 元素                                               |
| //title[@lang='eng']                | 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性                                |
| /bookstore/book[price>35.00]        | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00                |
| /bookstore/book[price>35.00]//title | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00 |

### CSS

CSS的基本语法

更多实例可以查看[CSS 选择器参考手册](https://www.w3school.com.cn/cssref/css_selectors.asp)

| 选择器                               | 例子                                       | 例子描述                                          |
| ------------------------------------ | ------------------------------------------ | ------------------------------------------------- |
| .class                               | .intro                                     | 选择 class="intro" 的所有元素                     |
| .class1.class2                       | .name1.name2                               | 选择 class 属性中同时有 name1 和 name2 的所有元素 |
| .class1 .class2                      | .name1 .name2                              | 选择作为类名 name1 元素后代的所有类名 name2 元素  |
| #id                                  | #firstname                                 | 选择 id="firstname" 的元素                        |
| *                                    | *                                          | 选择所有元素                                      |
| element                              | p                                          | 选择所有 p 元素                                   |
| element.class                        | p.intro                                    | 选择 class="intro" 的所有 p 元素                  |
| element,element                      | div, p                                     | 选择所有 div 元素和所有 p 元素                    |
| element element                      | div p                                      | 选择 div 元素内的所有 p 元素                      |
| element>element                      | div > p                                    | 选择父元素是 div 的所有 p 元素                    |
| element+element                      | div + p                                    | 选择紧跟 div 元素的首个 p 元素                    |
| element1~element2                    | p ~ ul                                     | 选择前面有 p 元素的每个 ul 元素                   |
| [attribute]                          | [target]                                   | 选择带有 target 属性的所有元素                    |
| [attribute=value]                    | [target=_blank]                            | 选择带有 target="_blank" 属性的所有元素           |
| [attribute~=value]                   | [title~=flower]                            | 选择 title 属性包含单词 "flower" 的所有元素       |
| [attribute                           | =value]                                    | [lang                                             |
| [attribute^=value]                   | a[href^="https"]                           | 选择其 src 属性值以 "https" 开头的每个 a 元素     |
| [attribute$=value]|a[href$=".pdf"] | 选择其 src 属性以 ".pdf" 结尾的所有 a 元素 |                                                   |
| [attribute*=value]                   | a[href*="w3schools"]                       | 选择其 href 属性值中包含 "abc" 子串的每个 a 元素  |
| :active                              | a:active                                   | 选择活动链接                                      |
| ::after                              | p::after                                   | 在每个 p 的内容之后插入内容                       |
| ::before                             | p::before                                  | 在每个 p 的内容之前插入内容                       |
| :checked                             | input:checked                              | 选择每个被选中的 input 元素                       |
| :default                             | input:default                              | 选择默认的 input 元素                             |
| :disabled                            | input:disabled                             | 选择每个被禁用的 input 元素                       |
| :empty                               | p:empty                                    | 选择没有子元素的每个 p 元素（包括文本节点）       |
| :enabled                             | input:enabled                              | 选择每个启用的 input 元素                         |
| :first-child                         | p:first-child                              | 选择属于父元素的第一个子元素的每个 p 元素         |
| ::first-letter                       | p::first-letter                            | 选择每个 p 元素的首字母                           |
| ::first-line                         | p::first-line                              | 选择每个 p 元素的首行                             |
| :first-of-type                       | p:first-of-type                            | 选择属于其父元素的首个 p 元素的每个 p 元素        |
| :focus                               | input:focus                                | 选择获得焦点的 input 元素                         |
| :fullscreen                          | :fullscreen                                | 选择处于全屏模式的元素                            |
| :hover                               | a:hover                                    | 选择鼠标指针位于其上的链接                        |
| :in-range                            | input:in-range                             | 选择其值在指定范围内的 input 元素                 |
| :indeterminate                       | input:indeterminate                        | 选择处于不确定状态的 input 元素                   |
| :invalid                             | input:invalid                              | 选择具有无效值的所有 input 元素                   |
| :lang(language)                      | p:lang(it)                                 | 选择 lang 属性等于 "it"（意大利）的每个 p 元素    |
| :last-child                          | p:last-child                               | 选择属于其父元素最后一个子元素每个 p 元素         |
| :last-of-type                        | p:last-of-type                             | 选择属于其父元素的最后 p 元素的每个 p 元素        |
| :link                                | a:link                                     | 选择所有未访问过的链接                            |
| :not(selector)                       | :not(p)                                    | 选择非 p 元素的每个元素                           |
| :nth-child(n)                        | p:nth-child(2)                             | 选择属于其父元素的第二个子元素的每个 p 元素       |
| :nth-last-child(n)                   | p:nth-last-child(2)                        | 同上，从最后一个子元素开始计数                    |
| :nth-of-type(n)                      | p:nth-of-type(2)                           | 选择属于其父元素第二个 p 元素的每个 p 元素        |
| :nth-last-of-type(n)                 | p:nth-last-of-type(2)                      | 同上，但是从最后一个子元素开始计数                |
| :only-of-type                        | p:only-of-type                             | 选择属于其父元素唯一的 p 元素的每个 p 元素        |
| :only-child                          | p:only-child                               | 选择属于其父元素的唯一子元素的每个 p 元素         |
| :optional                            | input:optional                             | 选择不带 "required" 属性的 input 元素             |
| :out-of-range                        | input:out-of-range                         | 选择值超出指定范围的 input 元素                   |
| ::placeholder                        | input::placeholder                         | 选择已规定 "placeholder" 属性的 input 元素        |
| :read-only                           | input:read-only                            | 选择已规定 "readonly" 属性的 input 元素           |
| :read-write                          | input:read-write                           | 选择未规定 "readonly" 属性的 input 元素           |
| :required                            | input:required                             | 选择已规定 "required" 属性的 input 元素           |
| :root                                | :root                                      | 选择文档的根元素                                  |
| ::selection                          | ::selection                                | 选择用户已选取的元素部分                          |
| :target                              | #news:target                               | 选择当前活动的 #news 元素                         |
| :valid                               | input:valid                                | 选择带有有效值的所有 input 元素                   |
| :visited                             | a:visited                                  | 选择所有已访问的链接                              |

### Beatifulsoup

[BS4解析库用法详解](http://c.biancheng.net/python_spider/bs4.html)

#### Beatifulsoup解析器

这个模块属于模块名与下载名不一致，安装时需要注意。另外需要安装几个网页解析器，推荐使用lxml作为解析器，因为效率更高

```python
pip install beautifulsoup4
pip install lxml
pip install html5lib
```

安装完成之后可以通过如下方式解析网页数据

```python
html_doc = """
<html><head><title>"c语言中文网"</title></head>
<body>
<p class="title"><b>c.biancheng.net</b></p>
<p class="website">一个学习编程的网站
<a href="http://c.biancheng.net/python/" id="link1">python教程</a>
<a href="http://c.biancheng.net/c/" id="link2">c语言教程</a>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc, 'lxml')

print(soup.prettify()) #prettify()用于格式化输出html/xml文档
```

#### Beatifulsoup对象

文档树中的每个节点都是 Python 对象，这些对象大致分为四类

- Tag：标签类，HTML 文档中所有的标签都可以看做 Tag 对象。
- NavigableString：字符串类，指的是标签中的文本内容，使用 text、string、strings 来获取文本内容。
- BeautifulSoup：表示一个 HTML 文档的全部内容，您可以把它当作一个人特殊的 Tag 对象。
- Comment：表示 HTML 文档中的注释内容以及特殊字符串，它是一个特殊的 NavigableString。

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p class="Web site url"><b>c.biancheng.net</b></p>', 'html.parser')
#获取整个p标签的html代码
print(soup.p)
#获取b标签
print(soup.p.b)
#获取p标签内容，使用NavigableString类中的string、text、get_text()
print(soup.p.text)
#返回一个字典，里面是多有属性和值
print(soup.p.attrs)
#查看返回的数据类型
print(type(soup.p))
#根据属性，获取标签的属性值，返回值为列表
print(soup.p['class'])
#给class属性赋值,此时属性值由列表转换为字符串
soup.p['class']=['Web','Site']
print(soup.p)
```

#### Beatifulsoup节点

Tag 对象提供了许多遍历 tag 节点的属性，比如 contents、children 用来遍历子节点；parent 与 parents 用来遍历父节点；而 next_sibling 与 previous_sibling 则用来遍历兄弟节点 。

```python
#coding:utf8
from bs4 import BeautifulSoup
html_doc = """
<html><head><title>"c语言中文网"</title></head>
<body>
<p class="title"><b>c.biancheng.net</b></p>
<p class="website">一个学习编程的网站</p>
<a href="http://c.biancheng.net/python/" id="link1">python教程</a>,
<a href="http://c.biancheng.net/c/" id="link2">c语言教程</a> and
"""
soup = BeautifulSoup(html_doc, 'html.parser')
body_tag=soup.body
print(body_tag)
#以列表的形式输出，所有子节点
print(body_tag.contents)
```

#### find_all()与find()

find_all() 方法用来搜索当前 tag 的所有子节点，并判断这些节点是否符合过滤条件，最后以列表形式将符合条件的内容返回，语法格式如下：

```
find_all( name , attrs , recursive , text , limit )
```

参数说明：
name：查找所有名字为 name 的 tag 标签，字符串对象会被自动忽略。
attrs：按照属性名和属性值搜索 tag 标签，注意由于 class 是 Python 的关键字吗，所以要使用 "class_"。
recursive：find_all() 会搜索 tag 的所有子孙节点，设置 recursive=False 可以只搜索 tag 的直接子节点。
text：用来搜文档中的字符串内容，该参数可以接受字符串 、正则表达式 、列表、True。
limit：由于 find_all() 会返回所有的搜索结果，这样会影响执行效率，通过 limit 参数可以限制返回结果的数量。

find() 方法与 find_all() 类似，不同之处在于 find_all() 会将文档中所有符合条件的结果返回，而 find() 仅返回一个符合条件的结果，所以 find() 方法没有limit参数。

使用 find() 时，如果没有找到查询标签会返回 None，而 find_all() 方法返回空列表。

```python
from bs4 import BeautifulSoup
import re
html_doc = """
<html><head><title>"c语言中文网"</title></head>
<body>
<p class="title"><b>c.biancheng.net</b></p>
<p class="website">一个学习编程的网站</p>
<a href="http://c.biancheng.net/python/" id="link1">python教程</a>
<a href="http://c.biancheng.net/c/" id="link2">c语言教程</a>
<a href="http://c.biancheng.net/django/" id="link3">django教程</a>
<p class="vip">加入我们阅读所有教程</p>
<a href="http://vip.biancheng.net/?from=index" id="link4">成为vip</a>
"""
#创建soup解析对象
soup = BeautifulSoup(html_doc, 'html.parser')

# find_all 语法
#查找所有a标签并返回
print(soup.find_all("a"))
#查找前两条a标签并返回
print(soup.find_all("a",limit=2))
#按照标签属性以及属性值查找 HTML 文档
print(soup.find_all("p",class_="website"))
print(soup.find_all(id="link4"))
#列表行书查找tag标签
print(soup.find_all(['b','a']))
#正则表达式匹配id属性值
print(soup.find_all('a',id=re.compile(r'.\d')))
print(soup.find_all(id=True))
#True可以匹配任何值，下面代码会查找所有tag，并返回相应的tag名称
for tag in soup.find_all(True):
    print(tag.name,end=" ")
#输出所有以b开始的tag标签
for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
  
# find 语法
#查找所有a标签并返回
print(soup.find("a"))
#查找前两条a标签并返回
print(soup.find("a",limit=2))
#按照标签属性以及属性值查找 HTML 文档
print(soup.find("p",class_="website"))
print(soup.find(id="link4"))
#列表行书查找tag标签
print(soup.find(['b','a']))
#正则表达式匹配id属性值
print(soup.find('a',id=re.compile(r'.\d')))
print(soup.find(id=True))
#True可以匹配任何值，下面代码会查找所有tag，并返回相应的tag名称
for tag in soup.find(True):
    print(tag.name,end=" ")
#输出所有以b开始的tag标签
for tag in soup.find(re.compile("^b")):
    print(tag.name)
```

#### CSS选择器

BS4 支持大部分的 CSS 选择器，比如常见的标签选择器、类选择器、id 选择器，以及层级选择器。Beautiful Soup 提供了一个 select() 方法，通过向该方法中添加选择器，就可以在 HTML 文档中搜索到与之对应的内容。

```python
html_doc = """
<html><head><title>"c语言中文网"</title></head>
<body>
<p class="title"><b>c.biancheng.net</b></p>
<p class="website">一个学习编程的网站</p>
<a href="http://c.biancheng.net/python/" id="link1">python教程</a>
<a href="http://c.biancheng.net/c/" id="link2">c语言教程</a>
<a href="http://c.biancheng.net/django/" id="link3">django教程</a>
<p class="vip">加入我们阅读所有教程</p>
<a href="http://vip.biancheng.net/?from=index" id="link4">成为vip</a>
<p class="introduce">介绍:
<a href="http://c.biancheng.net/view/8066.html" id="link5">关于网站</a>
<a href="http://c.biancheng.net/view/8092.html" id="link6">关于站长</a>
</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc, 'html.parser')
#根据元素标签查找
print(soup.select('title'))
#根据属性选择器查找
print(soup.select('a[href]'))
#根据类查找
print(soup.select('.vip'))
#后代节点查找
print(soup.select('html head title'))
#查找兄弟节点
print(soup.select('p + a'))
#根据id选择p标签的兄弟节点
print(soup.select('p ~ #link3'))
#nth-of-type(n)选择器，用于匹配同类型中的第n个同级兄弟元素
print(soup.select('p ~ a:nth-of-type(1)'))
#查找子节点
print(soup.select('p > a'))
print(soup.select('.introduce > #link5'))
```

### parsel

parsel相当于Beatifulsoup的升级版，速度更快，而且后期可以无缝对接Scrapy，因为经常需要结合css和xpath两种语法来提取，所以可以查看这个[对比](https://en.wikibooks.org/wiki/XPath/CSS_Equivalents)

### 基本用法

```python
from parsel import Selector

html = '''
<div>
    <ul>
         <li class="item-0">first item</li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-0 active"><a href="link3.html"><span class="bold">third item</span></a></li>
         <li class="item-1 active"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a></li>
     </ul>
 </div>
'''
# 基本用法
selector = Selector(text=html)
items = selector.css('.item-0')
items2 = selector.xpath('//li[contains(@class, "item-0")]')

## 提取文本
for item in selector.css('.item-0'):
    text = item.xpath('.//text()') # // 是所有子孙节点，text()是提取文本
    print(text.get())# get 方法的作用是从 SelectorList 里面提取第一个 Selector 对象，然后输出其中的结果。
    print(text.getall())# getall 方法的作用是从 SelectorList 里面提取所有 Selector 对象，然后输出其中的结果。

## 提取属性
'''
用 css 和 xpath 方法实现。我们根据同时包含 item-0 和 active 这两个 class 
来选取第三个 li 节点，然后进一步选取了里面的 a 节点
'''
result = selector.css('.item-0.active a::attr(href)').get() # ::attr()
print(result) # link3.html
result = selector.xpath('//li[contains(@class, "item-0") and contains(@class, "active")]/a/@href').get() # /@
print(result) # link3.html

## 正则提取
result = selector.css('.item-0').re('link.*') # 匹配包含 link 的所有结果。
print(result)
# ['link3.html"><span class="bold">third item</span></a></li>', 'link5.html">fifth item</a></li>']
```

#### parsel命名空间

多个相同的XML文档被加载时，计算机不能正确区分。所以需要给不同的XML加上命名空间来区分。

命名空间是在元素的开始标签的 xmlns 属性中定义的。命名空间声明的语法是

```
xmlns:前缀="URI"
xmlns:gd="http://schemas.google.com/g/2005"
```

命名空间 URI 不会被解析器用于查找信息。如果像想要提取命名空间的url，需要先移除命名空间。

```python
import requests
from parsel import Selector
text = requests.get('https://feeds.feedburner.com/PythonInsider').text
sel = Selector(text=text, type='xml') # 使用xml解析器
 
# 我们可以尝试选择所有对象，然后看到它不起作用 （因为 Atom XML 命名空间混淆了这些节点）
sel.xpath("//link") # [] 

# 但是一旦我们调用Selector.remove_namespaces方法，就可以访问所有节点 直接通过他们的名字
sel.remove_namespaces()  # 移除命名空间
sel.xpath("//link") 
# [<Selector xpath='//link' data='<link rel="alternate" type="text/html...'>,
#<Selector xpath='//link' data='<link rel="next" type="application/at...'>,
# ...]


# 为什么默认情况下不始终调用命名空间删除过程?
# 删除命名空间需要迭代和修改 文档，默认情况下执行的操作成本相当高 对于所有文档。
# 在某些情况下，实际上可能需要使用命名空间，在 某些元素名称在命名空间之间发生冲突的情况。虽然这些情况非常罕见

## 如果存在命名空间，也可以通过命名空间来选取对应的数据
sel.xpath("//a:entry/a:author/g:image/@src", namespaces={"a": "http://www.w3.org/2005/Atom","g": "http://schemas.google.com/g/2005"}).getall()
```

#### parsel将 CSS 转换为 XPath

```python
from parsel import css2xpath
css2xpath('h1.title')
"descendant-or-self::h1[@class and contains(concat(' ', normalize-space(@class), ' '), ' title ')]"
css2xpath('.profile-data') + '//h2'
"descendant-or-self::*[@class and contains(concat(' ', normalize-space(@class), ' '), ' profile-data ')]//h2"
```

#### parsel使用技巧

包括错误的XPath语法、XPath和css组合使用

```python
from scrapy import Selector
sel = Selector(text='<a href="#">Click here to go to the <strong>Next Page</strong></a>')
xp = lambda x: sel.xpath(x).extract() # let's type this only once
xp('//a//text()') # take a peek at the node-set
  # [u'Click here to go to the ', u'Next Page']
xp('string(//a//text())')  # convert it to a string
  # [u'Click here to go to the ']


xp('//a[1]') # selects the first a node
[u'<a href="#">Click here to go to the <strong>Next Page</strong></a>']
xp('string(//a[1])') # converts it to string
[u'Click here to go to the Next Page']

xp("//a[contains(., 'Next Page')]//text()") # good
#['Click here to go to the ', 'Next Page']
xp("//a[contains(.//text(), 'Next Page')]") # bad
# []

xp("substring-after(//a, 'Next ')")# good
# [u'Page']
xp("substring-after(//a//text(), 'Next ')")# bad
# [u'']

## 组合使用
sel = Selector(text='<p class="content-author">Someone</p><p class="content text-wrap">Some content</p>')
xp = lambda x: sel.xpath(x).getall()
sel.css('.content').xpath('@class').getall()
# ['content text-wrap']
```

#### parsel对比bs4

对比项包括：标签+class属性、标签+多个class属性、标签+非class的属性键值、提取指定属性、选取指定节点。

可以看到能用bs4实现的，parsel都可以实现，并且速度更快。
bs4代码更多，但是可读性较好。parsel语法更简洁，但是可读性较差。

```python
from parsel import Selector
from bs4 import BeautifulSoup
from timeit import timeit

 # https://www.kickstarter.com/projects/cloverpress/pixiv-presents-artists-in-taiwan-and-korea?ref=section-homepage-featured-project
with open('1.html','r')as f:
    html = f.read()

def test1():
    sel = Selector(text=html)
    Title = sel.css('h2.project-name').xpath('.//text()').get()
    First_Page_Picture = sel.css('img.aspect-ratio--object::attr(src)').get()
    Pledged_Amount = sel.css('span.money')[4].xpath('.//text()').get()
    Due_Time = sel.css('span[data-test-id=deadline-exists]').xpath('.//text()').get()
    Time_Left = sel.css('span.block.type-16.type-28-md.bold.dark-grey-500')[1].xpath('.//text()').get()
    return Due_Time ,Time_Left,Pledged_Amount,Title,First_Page_Picture


def test2():
    newSoup = BeautifulSoup(html, "html.parser")
    Title = newSoup.find('h2',class_ = 'project-name').text
    First_Page_Picture = newSoup.find('img',class_ = 'aspect-ratio--object')["src"]
    Pledged_Amount = newSoup.find_all('span',class_ = 'money')[4].text
    Due_Time = newSoup.find('span',{'data-test-id': 'deadline-exists'}).text
    Time_Left = newSoup.find_all('span',class_= 'block type-16 type-28-md bold dark-grey-500')[1].text
    return Due_Time ,Time_Left,Pledged_Amount,Title,First_Page_Picture
  
print(test1() == test2()) # True

t1 = timeit('test1()', 'from __main__ import test1', number=10)
print(t1) # 0.3

t2 = timeit('test2()', 'from __main__ import test2', number=10)
print(t2) # 2
```

### json

json模块提供了python->json以及json->python两种格式，转换规则如下

| JSON           | Python       |
| -------------- | ------------ |
| object -- 对象 | dict         |
| array          | list -- 列表 |
| string         | str          |
| number (int)   | int          |
| number (real)  | float        |
| TRUE           | true         |
| FALSE          | false        |
| null           | None         |

| Python                              | JSON           |
| ----------------------------------- | -------------- |
| dict                                | object -- 对象 |
| list, tuple                         | array          |
| str                                 | string         |
| int, float, int 和 float 派生的枚举 | number         |
| TRUE                                | true           |
| FALSE                               | false          |
| None                                | null           |

注意：JSON 中的键-值对中的键永远是 str 类型的。当一个对象被转化为
JSON 时，字典中所有的键都会被强制转换为字符串。这所造成的结果是字典被转换为 JSON 然后转换回字典时可能和原来的不相等。换句话说，如果 x 具有非字符串的键，则有 loads(dumps(x)) != x。

json模块还有一些其他参数可以控制：编码形式、格式化输出等，不过很少用到

[json官方模块文档](https://docs.python.org/zh-cn/3/library/json.html#module-json)

#### json.load与json.dump

json.load与json.dump 是基于文件的转换

```python
import json

data = {
    "name": "Satyam kumar",
    "place": "patna",
    "skills": [
        "Raspberry pi",
        "Machine Learning",
        "Web Development"
    ],
    "email": "xyz@gmail.com",
    "projects": [
        "Python Data Mining",
        "Python Data Science"
    ]
}
with open("data_file.json", "w") as write:
    json.dump(data, write)

with open("data_file.json", "r") as read_content:
    print(json.load(read_content))
```

#### json.loads与json.dumps

json.load与json.dump 是直接基于数据的转换

```python
import json

# JSON string:
# Multi-line string
data = """{ 
    "Name": "Jennifer Smith", 
    "Contact Number": 7867567898, 
    "Email": "jen123@gmail.com", 
    "Hobbies":["Reading", "Sketching", "Horse Riding"] 
    }"""

# parse data:
res_p = json.loads(data)
print(type(res_p)) # <class 'dict'>

res_j = json.dumps(res_p)
print(type(res_j)) # <class 'str'>
```

### execjs

execjs可以使用execjs.eval(data)的形式把数据转化为真正的json对象，并通过call()方法执行这段js代码。

```python
import execjs

data = """{ 
    "Name": "Jennifer Smith", 
    "Contact Number": 7867567898, 
    "Email": "jen123@gmail.com", 
    "Hobbies":["Reading", "Sketching", "Horse Riding"] 
    }"""

res_p = execjs.eval(data)
print(type(res_p)) #dict


res_j = execjs.compile(res_p)
print(type(res_j._source)) #dict

execjs.eval("'red yellow blue'.split(' ')")
#['red', 'yellow', 'blue']

ctx = execjs.compile("""
    function add(x, y) {
        return x + y;
    }
""")
ctx.call("add", 1, 2)
#3

```

## 数据处理

提取出来的数据多种多样，可能会有URL、网页文本信息、多媒体信息。

有一部分信息会被直接存为本地常用的**文档**（xlsx、html、csv等），譬如抓取好的网页文本信息。也有的信息会直接被保存到**数据库**，后期有需要时再调用，譬如多媒体文件。这些信息有的会存入**消息队列**和其他系统对接做成一个更大的系统，譬如URL。

pands：支持十多种常见文档格式的输入与输出，一站式搞定数据存取与处理。

> 数据库今年来格局逐渐明朗，关系型与非关系型数据库、图数据库、搜索引擎等等。

> 消息队列目前是kafka和RabbitMQ占据主导地位，随着分布式的进行，未来应该还会有更多可用的消息队列。



### pandas

Pandas 的主要数据结构是 Series （一维数据）与 DataFrame（二维数据），这两种数据结构足以处理金融、统计、社会科学、工程等领域里的大多数典型用例。

Series 是一种类似于一维数组的对象，它由一组数据（各种Numpy数据类型）以及一组与之相关的数据标签（即索引）组成。

DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）。DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）。

![image-1674890386065](https://www.jiangmiemie.com/upload/computerselfeducationroad/image-1674890386065.png)

[pandas官方文档](https://pandas.pydata.org/docs/reference/index.html)

#### pandas数据存取-csv/xlsx/xls

```python
import pandas as pd

data = [['Google',10],['Runoob',12],['Wiki',13]]

df = pd.DataFrame(data,columns=['Site','Age'],dtype=float)

df.to_csv('file1.csv',index=False)
df = pd.read_csv('file1.csv')

df.to_excel('file1.xlsx',index=False)
df = pd.read_excel('file1.xlsx')

df.to_excel('file1.xls',index=False)
df = pd.read_excel('file1.xls')

df
'''
Site	Age
0	Google	10
1	Runoob	12
2	Wiki	13
'''
```

#### pandas数据存取-json

```python

data =[
    {
      "id": "A001",
      "name": "菜鸟教程",
      "url": "www.runoob.com",
      "likes": 61
    },
    {
      "id": "A002",
      "name": "Google",
      "url": "www.google.com",
      "likes": 124
    },
    {
      "id": "A003",
      "name": "淘宝",
      "url": "www.taobao.com",
      "likes": 45
    }
]
df = pd.DataFrame(data)

df.to_json('sites.json')
df = pd.read_json('sites.json')
   
print(df.to_string())

'''
     id    name             url  likes
0  A001    菜鸟教程  www.runoob.com     61
1  A002  Google  www.google.com    124
2  A003      淘宝  www.taobao.com     45
'''
```

#### pandas数据存取-sql

```python
from sqlalchemy import create_engine,text
import pandas as pd

MYSQL_HOST = '*'
MYSQL_PORT = 3306
MYSQL_USER = 'employee_u'
MYSQL_PASSWORD = 'employee_s'
MYSQL_DB = 'employee'

engine = create_engine('mysql+pymysql://{}:{}@{}:{}/{}'.format(MYSQL_USER,MYSQL_PASSWORD,MYSQL_HOST,MYSQL_PORT,MYSQL_DB),
                       echo = False)
                     
dataset = pd.DataFrame({'Names':['Abhinav','Aryan',
                                 'Manthan'],
                        'DOB' : ['10/01/2009','24/03/2009',
                                '28/02/2009']})

dataset.to_sql('Employee_Data',con = engine,index=False)

#附加到以前创建的数据库中
dataset.to_sql('Employee_Data',con = engine,index=False,if_exists = 'append')


# 查看是否写入成功
with engine.connect() as conn:
    df1 = pd.read_sql('Employee_Data',con = conn,columns = ["Names"])
    df2 = pd.read_sql('Employee_Data',con = conn,index_col = 'Names',columns = ["Names"])
    df3 = pd.read_sql_table("Employee_Data", conn)
    df4 = pd.read_sql_query(text("select * from Employee_Data"),con = conn)
    df5 = conn.execute(text("select MAX(`index`) from Employee_Data")).fetchall()
    df6 = conn.execute(text("select `index` from Employee_Data WHERE `index` > :index"),{"index":1}).fetchall()
print(df1)
print(df2)
print(df3)
print(df4)
print(df5)
print(df6)
```

#### pandas数据分析

pandas的主要功能是数据分析，这是我早年学习的时候整理的思维导图，按需自取即可。

![编程笔记-数据分析](/upload/computerselfeducationroad/Pandas.jpg)


## 人机验证

事实上，一切你能总结出规律的验证方式都可以破解。

常见的验证码可以通过tesserocr 或者 ddddocr处理

tesserocr：部署复杂，识别传统数据效果较好

ddddocr：需联网的可靠识别模型

对于稀奇古怪的验证方式需要自己去写opencv结合pytorch

opencv：最流行的计算机视觉库之一

pytorch：最流行的机器学习库之一

### tesserocr

这个模块的安装步骤比较复杂：

- pip install tesseract
- 下载官方的库，通过运行[官方给的exe](https://github.com/UB-Mannheim/tesseract/wiki)获得
- 安装勾选(可以看最底部的安装教程)
- 环境变量配置说明(可以看最底部的安装教程)
- 配置完环境变量可以在cmd中输入tesseract -v检测看是否安装成功
- 重启vscode
- 最后在tesseract.py中第30行,设置tesseract_cmd 为你的tesseract.exe路径
  ![image.png](/upload/computerselfeducationroad/image-82a5e2bb1ce944e79a40e52ab1dba154.png)

接着因为这部分代码包含二值化、去噪等功能，代码比较长，所以我们把这部分代码封装为一个模块，然后结合起来。一个自动化抓取验证码并识别的小工具就做好了。

```python
from PIL import Image, ImageEnhance
from io import BytesIO
import pytesseract
import re


class VerifyAnalysis:
    def read_memory_img(self, buf: bytes):
        if buf == b'':
            return
        self.source_img = Image.open(BytesIO(buf))

    def _resize(self):
        """传入image对象进行尺寸调整"""
        w, h = self.source_img.size
        if w < 100:
            w, h = w * 2, h * 2
        self.img = self.source_img.resize((w, h))

    def _grey_graph(self):
        self.img = self.img.convert("L")

    def _contrast(self):
        enh_con = ImageEnhance.Contrast(self.source_img)
        self.img = enh_con.enhance(1.5)

    def _binarizing(self, threshold):
        """传入image对象进行灰度、二值处理"""
        self._grey_graph()
        pixdata = self.img.load()
        w, h = self.img.size
        # 遍历所有像素，大于阈值的为黑色
        for y in range(h):
            for x in range(w):
                if pixdata[x, y] < threshold:
                    pixdata[x, y] = 0
                else:
                    pixdata[x, y] = 255

    def _depoint(self):
        """传入二值化后的图片进行降噪"""
        pixdata = self.img.load()
        w, h = self.img.size
        for y in range(1, h - 1):
            for x in range(1, w - 1):
                count = 0
                if pixdata[x, y - 1] > 245:  # 上
                    count = count + 1
                if pixdata[x, y + 1] > 245:  # 下
                    count = count + 1
                if pixdata[x - 1, y] > 245:  # 左
                    count = count + 1
                if pixdata[x + 1, y] > 245:  # 右
                    count = count + 1
                if pixdata[x - 1, y - 1] > 245:  # 左上
                    count = count + 1
                if pixdata[x - 1, y + 1] > 245:  # 左下
                    count = count + 1
                if pixdata[x + 1, y - 1] > 245:  # 右上
                    count = count + 1
                if pixdata[x + 1, y + 1] > 245:  # 右下
                    count = count + 1
                if count > 4:
                    pixdata[x, y] = 255

    def identification_code(self) -> str:
        return pytesseract.image_to_string(self.img, config='-l eng --psm 6 --oem 1').strip()

    def _noise_delete(self, data):
        '''删除空格'''
        self.d_map = {
            'isdigit': re.compile(r'\D+'),
            'alnum': re.compile(r'\W+')}
        self.code_style = 'alnum'
        return self.d_map[self.code_style].sub('', data)

    def auto_identify(self):
        if self.source_img is None:
            return ""
        self._resize()
        self._binarizing(100)
        self._depoint()
        # self.img.show()
        c = self.identification_code()
        return self._noise_delete(c)
```

### ddddocr

### opencv

### pytorch

## 监控和日志

Instrumentation 是指对产品性能的测量，以便诊断错误和写入跟踪信息。检测可以有两种类型：源检测和二进制检测。

后端监控允许用户查看基础架构的性能，即运行 Web 应用程序的组件。其中包括 HTTP 服务器、中间件、数据库、第三方 API 服务等。

遥测是从应用程序的不同组件持续收集数据的过程。此数据可帮助工程团队跨服务解决问题并确定根本原因。换句话说，遥测数据支持分布式应用程序的可观察性。

### Grafana