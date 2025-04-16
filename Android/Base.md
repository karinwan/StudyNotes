# Base

- Open sourse, Linux based

## Android Framework

  <img width="300" alt="Screenshot 2025-04-14 at 9 01 57 PM" src="https://github.com/user-attachments/assets/14262e24-ed85-4514-9dcd-c0e9e8be0563" />

- `System Apps`: core apps like email, SMS, calendars, etc
- `Java API Framework`: Expose the functionality of some of these native libraries to apps; handle things like view management, notification support, and access to non-code resources
- `Native C/C++ Libraries` | `Android Runtime (ART)`: To run multiple virtual machines on low memory devices (by executing DEX files)
- `Hardware Abstraction Layer`: Standard interfaces like audio, bluetooth, camera
- `Linux Kernel`: Threading and low-level memory management; use key security features

---

## Android 四大组件
- `Activity`：负责与用户交互的单个界面/屏幕；
- `Service`：后台任务处理（无界面）；
- `BroadcastReceiver`：接收全局广播事件；
- `ContentProvider`：提供不同 app 之间的数据共享接口；

---

### Activity

- `Activity` 是 Android 应用的入口，每一个`Activity`表示一个用户可以看到的界面。
- 它会和**操作系统**交互，例如**管理生命周期**、**响应后退键**等。

#### Fragment

- `Fragment` 是一种可以嵌套在`Activity`中的 **UI 模块**，它**不是独立存在**的，必须附着在某个`Activity`上；
- 可以在一个`Activity`中组合多个`Fragment`，适合在大屏幕或平板上实现复杂的 UI；
- `Fragment`可以被多个`Activity`重用，`Activity`一般不能嵌套在另一个`Activity`中；
- `Fragment`有额外的生命周期方法.

#### Activity 与 Fragment 通信
最常见的是 `Bundle` 初始化参数传递、接口回调以及`ViewModel`共享。其中`ViewModel`是目前官方推荐的架构方式，既解耦又利于测试。

| 通信方式         | 解耦性 | 推荐程度 | 说明                                  |
|------------------|--------|----------|---------------------------------------|
| `ViewModel`共享   |  高   | ⭐⭐⭐⭐  | 推荐做法，支持**双向通信**和**状态共享**  |
| `Bundle`参数传递   |  高   | ⭐⭐⭐  | Activity → Fragment **初始化参数传递**   |
| `LiveData` / `EventBus` |  高 | ⭐⭐⭐  | 灵活适合**跨页面传递**，注意生命周期管理 |
| 接口回调         |  中   | ⭐⭐   | Fragment 回调事件通知 Activity        |
| `FragmentManager`   |  低   | ⭐   | 强耦合、结构不清晰                   |

`ViewModel`共享：
- 基于`Jetpack`架构，`Activity`和**多个**`Fragment`共享同一个`ViewModel`：
  ```kotlin
  val viewModel = ViewModelProvider(requireActivity()).get(SharedViewModel::class.java)
  viewModel.selectedItem.observe(viewLifecycleOwner) {
      // Fragment 监听数据变化
  }

  ```
- 发送数据（Fragment/Activity 均可）：
  ```kotlin
  viewModel.selectedItem.value = note
  ```
- 优点：解耦、可测试、符合 MVVM 架构；
- 缺点：需要 Jetpack 架构支持；

`Bundle`参数传递（Activity → Fragment）：
- 适合初始化参数、一次性设置；
  ```kotlin
  val fragment = DetailFragment()
  val bundle = Bundle()
  bundle.putString("noteId", "123")
  fragment.arguments = bundle
  ```
- 在 Fragment 中接收：
  ```kotlin
  val noteId = arguments?.getString("noteId")
  ```

事件总线（如 `LiveEventBus`、`EventBus`、`RxBus`）：
- 通过发布/订阅模型`Publish/Subscribe Model`，在组件间传递事件。适合**跨页面**通知；
   ```kotlin
  LiveEventBus.get<String>("saveSuccess").observe(this) {
      // 收到事件
  }
  LiveEventBus.get<String>("saveSuccess").post("Note saved")
  ```
- 优点：灵活、**全局广播**；
- 缺点：**生命周期管理**复杂，不推荐滥用。

接口回调（Fragment → Activity）：
- Fragment定义接口 `interface`， Activity实现：
  ```kotlin
  interface OnNoteSavedListener {
      fun onNoteSaved(noteId: String)
  }
  ```
- Fragment 中调用：
  ```kotlin
  (activity as? OnNoteSavedListener)?.onNoteSaved("123")
  ```
- 优点：简单直观；
- 缺点：强耦合，不适合多 Fragment 场景。

#### 生命周期 `Life Cycle`

- Activity 生命周期是 Android 管理 UI 界面和资源的机制，从创建、显示、暂停到销毁，每个阶段都有对应的**回调方法**`Callback`。

| 生命周期阶段       | Activity 方法     | Fragment 方法（完整）       | 说明              |
|--------------------|--------------------|----------------------|--------------------|
| 附加到上下文       | -         | `onAttach()`       | Fragment 绑定到宿主 Activity       |
| 创建               | `onCreate()`       | `onCreate()`      | 第一次被创建时调用，初始化逻辑，不涉及 UI     |
| 创建视图           | -     | `onCreateView()`      | 初始化 UI（返回 View）   |
| 视图已创建         | -    | `onViewCreated()`      | UI 创建完成后触发   |
| Activity 创建完成  | `onStart()`     | `onStart()`      | UI 可见，但还不能交互 |
| 可交互状态         | `onResume()`    | `onResume()`    | Activity 获得焦点，进入前台，可与用户交互   |
| 暂停               | `onPause()`    | `onPause()`     | UI 失去焦点（有另一个Activity进入前台），常用于保存状态和临时数据  |
| 停止               | `onStop()`      | `onStop()`    | 不可见，例如跳转到其他界面、切到后台  |
| 销毁视图           | -   | `onDestroyView()`    | 销毁 UI，但逻辑仍在  |
| 销毁               | `onDestroy()` | `onDestroy()`   | 销毁 Fragment 实例，释放所有资源，可能是用户退出或系统回收  |
| 分离               | -     | `onDetach()`      | Fragment 与 Activity 分离    |
| 恢复状态           | `onRestoreInstanceState()` | `onViewStateRestored()` | 重建之后恢复 UI 状态 | 

#### 横竖屏切换

- 默认情况下，横竖屏切换会使配置`configuration`发生变化，导致`Activity`**重建**，即经历完整的销毁与重新创建过程；
- 可以用`onSaveInstanceState()`保存状态；
- 想避免重建，可用`configChanges`控制。不建议滥用，适用于全屏播放等场景。

#### Activity 切换流程
启动新Activity（正常，非透明）：
- `onPause()` → `onStop()`
- `onCreate()` → `onStart()` → `onResume()`

启动新Activity（透明或 `Dialog` 样式）：
- `onPause()`
- `onCreate()` → `onStart()` → `onResume()`

用户按 Back 键返回到 ActivityA：
- `onPause()` → `onStop()` → `onDestroy()`
- `onRestart()` → `onStart()` → `onResume()`

---

## Context

Android提供的全局环境接口`interface`，用于：
- 访问**资源文件**（如字符串、颜色、图片）
- 获取**系统服务**（如剪贴板、通知、布局服务等）
- 启动 `Activity`、`Service`、`Broadcast` 等**组件**
- 访问**本地文件**、**数据库**、SharedPreferences

### Application Context 
- 用于**全局**操作
- 获取方式：`getApplicationContext()`
- 生命周期：和整个应用`App`一样长
- 示例用途：初始化数据库、第三方SDK、全局单例对象等

### Activity Context
- 用于**界面**操作
- 获取方式：`Activity`本身就是`Context`
- 生命周期：跟随`Activity`
- 示例用途：弹`Toast`、显示`Dialog`、加载布局等 UI-related tasks
- 生命周期短，不应在**单例**`Singleton`或**长生命周期**对象中持有，否则容易导致**内存泄漏**

---

## Bundle
- 用于在组件之间传递数据的容器；
- 一个类似于`Map`的**数据结构**，用于存储一组`key-value`对；
- 支持的数据类型包括：
  - 基本类型（Int、String、Boolean 等）
  - 自定义对象（推荐），实现`Parcelable`
  - 序列化对象（不推荐，效率低），`Serializable`
  - 数组、集合等可序列化结构
- 使用场景：
  - Activity 之间传参
  - Fragment 设置参数
  - 在生命周期中保存**页面状态**

---

## `Parcelable` vs `Serializable`
Parcelable 和 Serializable 都是用于对象序列化的接口，但 Parcelable 是 Android 专属、性能更好、更推荐使用；Serializable 使用简单但效率较低。

| 对比维度 | Parcelable | Serializable |
|--------|---------|---------|
| 所属平台 | Android 特有 | Java 原生 |
| 性能 | 高效，使用内存块读写 | 慢，使用反射 |
| 是否易用 | 手动实现较复杂（但 Kotlin 有 `@Parcelize`） | 非常简单，直接实现接口即可 |
| 是否支持所有类型 | 需要手动处理嵌套对象、集合等 | 自动支持多种类型 |
| 使用场景 | 推荐用于 Bundle、Intent 传参 | 不推荐用于频繁传输或大对象 |

### 对象序列化
- 序列化是指将一个对象转换成**可存储**或**可传输**的格式（如写入`Bundle`、`Intent`、保存到磁盘等）；
- `Parcelable`和`Serializable`是在 Android 中常用的两种方式。

---

### Why does an Android App lag?

### What is Context? How is it used? 

### Tell all the Android application components. 

### What is the project structure of an Android Application? 

### What is AndroidManifest.xml? 

### What is the Application class?



