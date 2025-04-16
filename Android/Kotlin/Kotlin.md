## Type System

- default non-nullable, null to non-nullable cases compilar error

 ## Callbacks and Listeners

 - much more concise
 - use of lambdas with Jetpack Compose makes even more concise

## Extented properties

---

## `LiveData`, `StateFlow`, and `EventBus`

| 名称 | 本质是什么？ | 属于哪类工具？ | 官方/第三方 |
|------|------------|------------|------------|
| LiveData | 类（LiveData<T>） | Jetpack 架构组件（架构库） |  官方（Google）|
| StateFlow | 类（StateFlow<T> 接口） | Kotlin 协程的一部分（状态流） |  官方（JetBrains）|
| EventBus | 第三方库中的类（EventBus） | 事件总线框架（通信工具） |  第三方（greenrobot）|

### `LiveData`
- `LiveData<T>` 是 Jetpack 提供的一个**数据持有类**`Data Holder Class`；
- 支持**观察者模式**；
- 能够根据组件（如 Activity、Fragment）生命周期**自动注册/反注册**，只有在 UI 可见时才接收数据，避免内存泄漏；
- 持有最新值，新观察者可立即接收；
- `setValue()` 需主线程；`postValue()` 可后台线程使用。

使用场景：
- `ViewModel` 向UI层**传递状态**（如加载中、数据列表、错误提示）
- 配合 `LiveData` + `ViewModel` 实现响应式 UI
- **数据变化**驱动**视图自动更新**，无需手动触发

示例代码：
- `ViewModel`:
  ```kotlin
  class MyViewModel : ViewModel() {
      private val _message = MutableLiveData<String>()
      val message: LiveData<String> = _message
  
      fun updateMessage(text: String) {
          _message.value = text
      }
  }
  ```
- UI层收集状态（`XML`）：
  ```kotlin
  viewModel.message.observe(viewLifecycleOwner) { text ->
      textView.text = text
  }
  ```

### `StateFlow`
- `StateFlow<T>` 是一个 热流`Hot Flow`；
- 始终持有一个最新值，并在值变化时通知所有观察者；
- 属于`kotlinx.coroutines.flow`包，是`Flow`的子接口；
- 线程安全、生命周期无感知。

使用场景：
- `ViewModel`中持有 UI 状态（`Loading` / `Success` / `Error`）
- 多个组件共享某个状态值（如用户登录状态）
- 替代`LiveData`用于`Compose、Jetpack View 的 UI 状态绑定

示例代码：
- `ViewModel`:
  ```kotlin
  class MyViewModel : ViewModel() {
      private val _uiState = MutableStateFlow("Initial")
      val uiState: StateFlow<String> = _uiState
  
      fun updateState(newText: String) {
          _uiState.value = newText
      }
  }
  ```
- UI层收集状态（`Compose` 或 `XML`）：
  ```kotlin
  lifecycleScope.launchWhenStarted {
      viewModel.uiState.collect { state ->
          textView.text = state
      }
  }
  ```
- `MutableStateFlow` 允许修改 .value，但我们只希望 ViewModel 内部能修改；
- `StateFlow` 是**只读**接口，UI 层只能收集它，不能 .value = xxx。


