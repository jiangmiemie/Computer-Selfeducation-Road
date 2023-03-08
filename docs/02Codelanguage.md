---
sidebar_position: 3
---

# 学习编程语言

C语言是所有编程语言中的经典，很多高级语言都是从C语言中衍生出来的，例如:C++、C#、Object-C、Java、Go 等等。

对于程序员来说不同的编程语言好比不同的工具。以下是一些开发人员的工作以及他们使用的主要编程语言：

- 游戏开发人员使用 C++ 或 C# 为 PC 和控制台制作视频游戏。
- Web 开发人员使用 HTML、CSS、JavaScript 和 PHP 来制作网站和 Web 应用程序。
- 移动应用程序开发人员使用 Java 和 Kotlin 制作 Android 应用程序或使用 Swift 制作 iOS 应用程序。
- 软件开发人员使用 C++、C# 和 Java 来制作桌面应用程序、业务应用程序和系统软件。
- 数据科学家使用 Python、R 和 MatLab 来分析用于科学研究和教育目的的数据。

你想构建什么，就选择什么可以帮助你实现目标的编程语言。


## Web

Web 编程语言包括 CSS、HTML 和 JavaScript。

- 超文本标记语言 (HTML) 是用于构建 Web 内容并赋予其意义和目的的代码。例如，我的内容是一组段落还是要点列表？
- 级联样式表 (CSS) 是您用来设置网站样式的代码。例如，您希望文本是黑色还是红色？应该在屏幕上的什么地方绘制内容？应该使用什么背景图像和颜色来装饰您的网站？
- JavaScript 是用于向网站添加交互功能的编程语言。一些示例可能是游戏、按下按钮或在表单中输入数据时发生的事情、动态样式效果、动画等等。

### 如何学习 Web

Web编程语言的历史悠久，产生的教程也许许多多，自身的编码标准先后有过几次改动，而并不是所有的教程都能及时跟进。因此，挑选Web教程的要点：
- 足够新、足够权威
- 提供可复制的代码
- 提供练习与认证


[freecodecamp](https://www.freecodecamp.org/)教程，课程配套练习，完成这个教程即可申请认证。

[Web MDN](https://developer.mozilla.org/en-US/)Web 编程语言官方文档，旨在指导初学者进行 Web 开发，并提供他们开始编写网站代码所需的一切。

## Python

Python 属于面向对象的编程语言，以下是面向对象编程与面向过程编程的区别：

- 面向对象编程是以对象为中心，面向过程编程则以过程为中心；
- 面向对象编程通常都以类的创建和调用为主，面向过程编程以函数的创建和调用为主
- 它们的程序组成分别是一组对象的集合和一系列过程的集合；

Python 的一些显着特性：
- 使用优雅的语法，使您编写的程序更易于阅读。
- 附带一个大型标准库，支持许多常见的编程任务，例如连接到 Web 服务器、使用正则表达式搜索文本、读取和修改文件。
- 通过添加用编译语言（如 C 或 C++）实现的新模块可以轻松扩展。

### 如何学习 Python

随着Python 使用人数的增加,越来越多的Python教程开始出现，这里我想教大家如何挑选教程的几个要点：
- 足够专业与权威
- 提供可复制的代码
- 提供练习与认证


[freecodecamp](https://www.freecodecamp.org/)教程，课程配套练习，完成这个教程即可申请认证。

[Python3 Doc](https://docs.python.org/zh-cn/3/)Python 官方文档，所有教程的起点和终点。

## 面试常问

### 进程

进程是系统独立调度核分配系统资源（CPU、内存）的基本单位
进程之间是相互独立的，Python中进程通信一般借助进程对列Queue完成。
进程绕过了 全局解释器锁。 因此，multiprocessing 模块允许程序员充分利用给定机器上的多个处理器。 它在 Unix 和 Windows 上均可运行。
进程数等于CPU核心个数效率最佳，如果多了则核心数利用不充分，如果少了会导致进程切换，增加程序运行时间。

#### 通信方式
![image-1663123215577](/upload/computerselfeducationroad/image-1663123215577.png)

[multiprocessing模块代码文档](https://docs.python.org/zh-cn/3.10/library/multiprocessing.html?highlight=multiprocessing#module-multiprocessing)

```Python
from multiprocessing import Pool # 导出multiprocessing模块

def f(vaule):
    x = vaule[0]
    y = vaule[1]
    return x*y

if __name__ == '__main__':
    p = Pool(16) # 新建16个进程的进程池，因为我是16核处理器
    print(p.map(f, [(1,1), (2,2), (3,3)])) # 传入需要执行的数据
    p.close() # 关闭进程池
  
## 运行结果[1, 4, 9]
```

### 线程

线程是系统最小的执行单位（CPU分配时间轮片是通过线程来实现的）
同一时刻只有一个线程可以执行 Python 代码。
但是，如果你想要同时运行多个 I/O 密集型任务，则多线程仍然是一个合适的模型。
线程数等于CPU核心个数的2倍效率最佳。
GIL是一个互斥锁，它防止多个线程同时执行Python字节码。这个锁是必要的，主要是因为CPython的内存管理不是线程安全的
GIL在此环境里就限制了解释器本身只有一个线程处于运行中，任意Python解释器级别操作都是串行的，使得任一时间都只能有最多一个语句抛出异常。于是异常相关的共享变量就得到了保护。

#### 通信方式
- 锁机制：包括互斥锁、条件变量、读写锁
  互斥锁提供了以排他方式防止数据结构被并发修改的方法。
  读写锁允许多个线程同时读共享数据，而对写操作是互斥的。
  条件变量可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
- 信号量机制(Semaphore)
  包括无名线程信号量和命名线程信号量。
- 信号机制(Signal)
  类似进程间的信号处理。

线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。

[threading模块代码文档](https://docs.python.org/zh-cn/3.10/library/threading.html?highlight=threading#module-threading)

```Python
"""
通过实例化threading.Thread类创建线程
"""
import time
import threading


def test_thread(para='hi', sleep=3):
    """线程运行函数"""
    time.sleep(sleep)
    print(para)


def main():
    # 创建线程
    thread_hi = threading.Thread(target=test_thread)
    thread_hello = threading.Thread(target=test_thread, args=('hello', 1))
    # 启动线程
    thread_hi.start()
    thread_hello.start()
    print('Main thread has ended!')


if __name__ == '__main__':
    main()
```

### 协程

协程是用来编写 并发 代码的库，是构建 IO 密集型和高层级 结构化 网络代码的最佳选择。
协程是通过代码主动切换状态，而等待处理来操作，因此效率更高，语法也更细致，loop对象需要主动：创建、设置、提交、等待运行、停止。
最佳的协程数取决于内存的使用情况。

[asyncio模块代码文档](https://docs.python.org/zh-cn/3.10/library/asyncio.html?highlight=asyncio#module-asyncio)

```Python
import asyncio # 导入协程模块

class TestA:  # 定义一个类
    def __init__(self,loop) -> None:
        self.loop = loop
        asyncio.set_event_loop(loop=self.loop) # 3.1类实例化时，加入协程loop中

    async def run_page(self,tid): # 7 运行工作代码
        print(tid)
        return tid

    async def close(self,):
        for i in asyncio.all_tasks(): # # 9.1关闭协程任务
            i.cancel()
        self.loop.stop() # 9.2 关闭协程循环


def test():
    get_async_loop = asyncio.new_event_loop() # 1定义一个loop对象
    asyncio.set_event_loop(get_async_loop) # 2设置为事件循环

    async def spider(task_obj):
        async_task =  [asyncio.ensure_future(task_obj.run_page(1)),
                    asyncio.ensure_future(task_obj.run_page(2)),] # 6 定义2个子协程函数
        await asyncio.wait(async_task) # 8等待协程成勋运行

        await task_obj.close() # 9关闭协程循环
  
    task_obj = TestA(get_async_loop) #3实例化类
    asyncio.run_coroutine_threadsafe(spider(task_obj), loop=get_async_loop) #4将协程提交给给定的事件循环。线程安全。
    get_async_loop.run_forever() # 5协程运行

test()
```
