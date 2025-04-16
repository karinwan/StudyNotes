# Jetpack & Kotlin 协程`Coroutines`

**轻量级**线程，用于**异步**编程，避免回调地狱。

- `suspend` 关键字：声明可**挂起**函数

- `CoroutineScope`：管理协程的**生命周期**

- `launch` vs `async`: 前者不返回结果，后者返回 `Deferred`

- `withContext`: 在指定调度器`Dispatcher`中切换线程

```kotlin
suspend fun fetchData(): String {
    return withContext(Dispatchers.IO) {
        // 网络请求
        "data"
    }
}
```

## 协程基本概念

## 启动协程的方式	

| 函数	| 返回值	|	用途	|	常用场景	|
|---|---|---|---|
| `launch`	|	`Job`	|	启动新协程，不返回结果	|	异步任务（如发送日志、发通知、网络请求后更新 UI）	|
| `async`	|	`Deferred<T>`	|	启动协程并返回一个结果（调用 `await()` 获取结果）	|	启动并发计算并获取值（如获取多个 API 数据）	|
| `runBlocking`	|	`T`	|	阻塞当前线程直到完成，不能在 UI 线程使用	|	测试代码、main 函数、非协程环境中	|


## 协程作用域`Scope`	

### 作用域`CoroutineScope`
协程运行的上下文，协程必须运行在一个作用域中，它决定协程的**生命周期**和**父子协程**之间的关系。
- `GlobalScope`: 生命周期不受控，App 退出仍可能运行（不推荐）`GlobalScope.launch {  }`
- `lifecycleScope`: Activity/Fragment
- `viewModelScope`: ViewModel `viewModelScope.launch {  )`

### `Job`
协程的任务单位

### 调度器 `Dispatcher` 
控制协程运行的**线程**

| 调度器 | 运行线程 | 用途场景 | 示例 |
|---|---|---|---|
| `Dispatchers.Default` | 后台线程池（CPU） | **计算密集**型任务（排序、加密等） | launch(Dispatchers.Default) |
| `Dispatchers.IO` | 后台线程池（IO） | 适合**阻塞**任务，如网络、文件、数据库等 IO 操作 | withContext(Dispatchers.IO) |
| `Dispatchers.Main` | 主线程（UI） | 在协程中更新 UI（仅限 Android/JS 等平台） | launch(Dispatchers.Main) |
| `Dispatchers.Unconfined` | 当前线程起始，后续不固定 | 很少用，调试/特殊用途 | launch(Dispatchers.Unconfined) |

## 取消协程的方式
- `job.cancel()`
- **父作用域**取消时，所有**子协程**都会一并取消

## 结构化并发
所有子协程必须在父协程完成前结束。这就是 Kotlin 协程比传统线程更安全的原因之一。

## 挂起函数（suspend）
- 标记函数可以**挂起**
- 只能在**协程**或另一个`suspend`函数中调用
- 挂起时释放线程，**不阻塞**主线程
- 写出像同步的异步代码

## withContext
用于线程/调度器切换，常用于切换 IO → Main

## 异常处理	
-` try/catch`捕获异常
  - `async`: 异常被延迟，在`await()`时抛出，用 try/catch 包住 await();
  - `runBlocking`: 异常会立即在主线程抛出，用普通 try/catch 即可
- `CoroutineExceptionHandler`处理未捕获异常
  - `lauch`: 异常会被抛到协程作用域中
- `supervisorScope`是协程中处理**异常隔离`Exception Isolation`**的关键工具，常见于结构化并发场景
  - 会隔离子协程之间的异常，某个子协程抛出异常不会取消其他兄弟协程
  - 错误隔离，适合容错性高的场景
  - 某些任务失败无所谓，继续跑其他任务（如 UI 多请求; 加载多个图片，某些失败也没关系）


