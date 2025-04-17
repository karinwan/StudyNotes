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

#### 任务栈 `Task Stack`
- Android 系统为管理 Activity 启动和切换而维护的“**后进先出**（LIFO）”结构，每个任务栈代表一组**关联**的 Activity 页面。
- 每次启动一个新的 Activity，系统会将它压入当前**栈顶**
- 用户按**返回键**，系统会将它从栈顶弹出
- 整体结构类似 栈`Stack` 数据结构 → 后进先出

#### Activity 启动模式 `Launch Modes`

| 启动模式         | 是否复用实例 | 是否清栈 | 特点 / 场景                        |
|------------------|--------------|----------|------------------------------------|
| `standard`         |  X         |  X     | 默认，每次启动新实例                |
| `singleTop`        |  栈顶复用   |  X     | 栈顶存在时复用，常用于通知栏跳转     |
| `singleTask`       |  栈中复用   |  清除上方，并调用`onNewIntent()`传递新参数 | 全局唯一入口页面，如主页、登录页    |
| `singleInstance`   |  独立栈     |  清除上方 | 独立任务栈，常用于浮窗或独立模块    |

标准 `standard`：
- 默认模式，每次启动都会创建新的实例，即使它已经在栈中存在。
- 易出现重复页面 → “Activity 多次叠加”
  ```xml
  <activity android:name=".DetailActivity" />
  ```

栈顶复用 `singleTop`：
- 如果 Activity 已经在**栈顶**，不会重新创建，只会调用`onNewIntent()`。
- 适用：通知栏点击跳转、页面反复刷新
  ```xml
  <activity android:name=".NewsActivity" android:launchMode="singleTop" />
  ```

全栈唯一 `singleTask`：
- 如果栈中已有该 Activity 实例，系统会复用，并清除其上的所有 Activity。
- 适用：主页、登录页等“唯一入口”页面
- `android:launchMode="singleTask"`

单例 + 独立任务栈 `singleInstance`：
- 该 Activity 会被放入一个独立的任务栈中，其他 Activity 无法与之共存
- 适用：浮窗、插件系统
- `android:launchMode="singleInstance"`


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

#### View
- The fundamental building block of Android UI (a single UI element).
- It's responsible for:
  - Measuring itself (`onMeasure`)
  - Laying out (`onLayout`)
  - Drawing content (`onDraw`)
  - Handling user input (click, touch, etc.)
- Examples: `Button`, `TextView`, `ImageView` all extend View.

#### 绘制流程(三大阶段)
1. `measure()`
   - 确定每个 View 的**大小** (宽度和高度)
   - 重写 `onMeasure()`，从顶层**父 View **到**子 View ****递归调用**`measure()`方法，并**回调**每个 View 的`onMeasure()`方法
2. `layout()`
   - 确定每个 View 在**父容器**中的**位置**（左、上、右、下坐标），进行页面的布局；
   - 重写 `onLayout()`，从**根 View **向**子 View **递归调用`layout()`方法，并回调每个 View 的`onLayout()`方法。
3. `draw()`
   - 将 View 实际绘制到屏幕上（包括背景、内容、子 View）
   - `draw()` 
     - `drawBackground()` 
     - `onDraw()` 绘制自身内容，如文本、图形
     - `dispatchDraw()` 绘制子 View
  - 自定义 View 时可重写：`onDraw(canvas: Canvas)`

#### ViewGroup
- a container that holds multiple Views.
- Measure & layout child Views
- Organize UI elements into a structure
- Control drawing order & event dispatch
- Examples: 
  - `LinearLayout`
  - `ConstraintLayout`
  - `FrameLayout`
  - `RecyclerView`

#### 层级关系
- `Activity` → `Window` → `DecorView`（根`root ViewGroup`） → 你的布局 `ViewGroup` →
  - 普通 `View`
  - `Fragment` 的容器 → Fragment 返回的 `View`
- `Activity`是整个 UI 系统的控制入口；
- `Window` 是 Activity 的**窗口**，**承载** UI；
- `View`是实际显示在屏幕上的**元素**；
- `Fragment`是“嵌套的可重用页面”，**插入到View树**中。

#### 触摸事件 `Touch Event`
- 用户在屏幕上操作时，Android 系统生成的`MotionEvent`对象，用于驱动 UI 响应**点击**、**滑动**、**缩放**等交互。
- 由`Activity`**向下传递**，经过`ViewGroup`到子`View`
  - `Activity`调用`dispatchTouchEvent()`开始事件分发
  - `onInterceptorTouchEvent`为true表示**拦截**事件，进行处理（只`ViewGroup`有）；
  - `onTouchEvent`为true表示事件**消费**(实际处理事件），如果到最后都不处理就会返回到`Activity`的`onTouchEvent`处理；
    - 一旦某个 View 返回了 true，就不会再继续向下或横向传递

上下和左右滑动事件冲突
- 核心是通过手势方向判断，在 `onInterceptTouchEvent()` 或 `dispatchTouchEvent()` 中根据滑动方向决定**是否拦截**事件。


---

### Service
- **无界面**后台组件
- 可执行**长期任务**
- 启动方式：直接启动`startService()` / 绑定`bindService()`
- 生命周期**独立**于UI（Activity 被销毁，它仍可运行），适合持续任务（音乐播放、后台下载、定位、计步器或运动记录）

#### 类型
- 前台服务：通过**通知**让用户感知，优先级高（`startForeground`）。常见有消息通知。
- 后台服务：8.0 后受限制，推荐`JobScheduler`。

#### 启动方式
- startService(): 启动后在后台**独立无限期运行**，启动组件被销毁也不影响；
- bindService(): 绑定组件运行，**解绑**即停止并销毁；
- startForeground(): 前台服务，必须展示通知

---

### BroadcastReceiver
- 响应广播事件`Broadcast`的组件
- 可接收系统广播（如网络变化、电量变化）或自定义广播（如登录成功、订单支付成功）
- 注册方式：静态`Manifest`（系统托管、常驻，无需app启动即可接收广播）或动态`registerReceiver()`（应用内与注册组件绑定，仅前台进程可以接收）
- 生命周期**极短**，仅执行`onReceive()`

---

### ContentProvide

---

## Context

Android提供的全局环境/通用接口`interface`，抽象类，用于：
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
- 获取方式：`Activity`**本身就是**`Context`
- 生命周期：跟随`Activity`
- 示例用途：弹`Toast`、显示`Dialog`、加载布局等 UI-related tasks
- 生命周期短，会持有视图引用，不应在**单例**`Singleton`或**长生命周期**对象中持有，否则容易导致**内存泄漏**

### 对比
- `Application`是`Context`的**子类**，属于整个app生命周期的**全局**`Context`
- `Activity` 是 Android 的 UI 组件之一，本身**继承**自 `Context`，因此当我们说`Activity Context`时，实际上就是指当前`Activity`**作为上下文环境**来使用。这两者在对象上是同一个，但概念不同：一个代表**界面组件**，一个代表**访问系统资源的能力**。

---
## Intent 
- Android 中用于在**组件之间传递消息**的对象，可以用于启动 Activity、Service、发送广播等。
- 可携带数据（`putExtra`），如携带`Bundle`：`intent.putExtras(bundle)`

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
| 是否易用 | 手动实现较复杂（但Kotlin有`@Parcelize`注解`Annotations`） | 非常简单，直接实现接口即可 |
| 是否支持所有类型 | 需要手动处理嵌套对象、集合等 | 自动支持多种类型 |
| 使用场景 | 推荐用于 Bundle、Intent 传参 | 不推荐用于频繁传输或大对象 |

### 对象序列化
- 序列化是指将一个对象转换成**可存储**或**可传输**的格式（如写入`Bundle`、`Intent`、保存到磁盘等）；
- `Parcelable`和`Serializable`是在 Android 中常用的两种方式。

---

### 可以在子线程里面刷新UI吗？如果我非要在子线程里刷新了UI呢？会怎么样？会抛出什么异常？

- 不可以在子线程中刷新 UI
- 如果强行操作 UI 会抛出 `CalledFromWrongThreadException` 异常
- Android 的 UI 控件（如 TextView、Button）只能在 **主线程**`Main Thread / UI Thread` 中访问，是因为：
  - Android 的 UI 系统 不是线程安全的
  - 子线程更新 UI 可能导致 **界面状态不一致**、崩溃
- 切回主线程更新 UI
  1. 使用 `runOnUiThread`
     ```kotlin
     runOnUiThread { textView.text = "Hello" }
     ```
  2. 使用 `Handler(Looper.getMainLooper())`
     ```kotlin
     val handler = Handler(Looper.getMainLooper())
     handler.post {
         textView.text = "Hello"
     }
     ```
  3. 使用协程（推荐）
     ```kotlin
     lifecycleScope.launch {
          val data = withContext(Dispatchers.IO) {
              // 子线程做网络请求
              "Network Result"
          }
          textView.text = data  // 回到主线程更新 UI
     }
     ```

---

### Why does an Android App lag?

### Tell all the Android application components. 

### What is the project structure of an Android Application? 

### What is AndroidManifest.xml? 

### What is the Application class?



