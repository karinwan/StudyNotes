## Process

## Thread

## 线程间通信 Inter-Thread Communication - ITC

### Handler
- 线程之间通信的工具
- 负责把任务（消息`Message`或`Runnable`）从一个线程发送到另一个线程的消息队列`MessageQueue`，由 Looper 接收并执行。
- 定义接收时要做什么

```kotlin
Thread A              →         Thread B (Looper线程)
-------------                   -----------------------
Handler.sendMessage()   →       MessageQueue.enqueue()
                                Looper.loop() 读取消息
                                Handler.handleMessage()
```

- 常见使用流程（主线程 Handler）
  - 获取主线程 `Looper` : Android 主线程天生就有 `Looper`，所以可以直接用 `Looper.getMainLooper()`
  - 创建 `Handler` : `val handler = Handler(Looper.getMainLooper())`
  - 发送消息或任务 : `handler.sendMessage`发送消息 / `handler.post()` 放入主线程消息队列 /`handler.postDelayed` 延时执行任务
  - Looper所在线程处理消息或任务
  - 处理回调: `override fun handleMessage()`
  
Handler 能不能在子线程中使用？
- 可以，但必须先调用 Looper.prepare() 和 Looper.loop()
  
Handler 是怎么避免 ANR 的？	
- 不一定能避免，如果主线程阻塞，还是会 ANR

如果创建了两个handler，如何知道哪个消息对应哪个handler？
- 每条消息`Message`都有一个字段 `target`，它记录了该消息是由哪个 `Handler` 发送的

### Looper 消息循环器
- 用于循环处理消息队列`MessageQueue`并分发给 `Handler` 的组件
- 一个线程只能有一个 Looper
- 主线程默认有 Looper，子线程需要手动创建
  ```kotlin
  Thread {
      Looper.prepare() // 1. 创建 Looper
      val handler = Handler(Looper.myLooper()!!) // 2. 绑定 Handler 到此 Looper
      handler.post {
          println("执行任务")
      }
      Looper.loop() // 3. 启动消息循环（永远运行）
  }.start()
  ```
  
线程休眠：
Looper 的**睡眠机制**底层依赖的是`epoll_wait()`，它通过`nativePollOnce`等方法调用 Linux 的 `epoll`，实现了**消息队列为空**时线程进入休眠、等待新事件到来。这种机制高效且节能，是 Android 主线程**空闲时不占用 CPU** 的关键设计。

### MessageQueue	
消息队列，先进先出处理待执行的消息

### ANR `Application Not Responding`
- 是Android 系统在检测到应用**主线程****长时间未响应**用户操作或系统事件时抛出的异常提示。
- 不是 Java 异常（不会被 try/catch 捕获），而是**系统弹出的对话框**提示用户“这个应用卡死了，要不要关闭”
- 常见场景:
  - **主线程**执行**耗时**操作，如在主线程中做网络请求、文件读写、数据库操作等；
  - **广播中**执行**耗时**逻辑	BroadcastReceiver 中做大文件处理；
  - 主线程**死循环** / **无限递归**，导致主线程一直在干一件事，无法响应事件；
  - 线程间**死锁**，主线程与某线程互相等待。
- 如何避免：异步执行耗时任务
- 如何排查：查看 `/data/anr/traces.txt`、使用 systrace 工具

### RxJava
- 基于事件流和链式调用的**响应式**编程框架，擅长处理异步数据流、线程切换、组合操作，可以大大简化回调地狱。
  ```kotlin
  Observable.just("Hello")
    .subscribeOn(Schedulers.io())           // 在 IO 线程发射数据
    .observeOn(AndroidSchedulers.mainThread()) // 在主线程接收数据
    .subscribe(
        { data -> textView.text = data },   // onNext
        { error -> error.printStackTrace() } // onError
    )
  ```
- `subscribeOn()` 设置 **数据源** 执行线程（只能设置**一次**）
- `observeOn()` 设置 **观察** 线程（可多次切换）
- 数据流: 被观察的数据/任务	`Observable` / `Flowable`（大数据）
- 操作符: 对数据流做转换、过滤、组合等操作	`map`、`flatMap`、`filter`、`zip`、`debounce` 等
- 错误处理: 捕获并处理异常	`onErrorResumeNext()`、`retry()`
