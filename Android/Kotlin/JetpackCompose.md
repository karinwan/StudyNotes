# JetPack Compose

声明式 UI 框架

https://developer.android.com/codelabs/jetpack-compose-basics#0

    Compose
    State: remember, rememberSaveable, MutableState
    Recomposition
    State hoisting
    Side-effects
    Modifier
    Theme
    Layout, List
    Gestures, Animation
    CompositionLocal


### Jetpack Compose vs Android View System


### 声明式 `Declarative` UI vs 命令式`Imperative` UI

声明式
- 只需要“声明状态对应什么 UI”，系统**自动**响应状态变化
- 使用 State 或 ViewModel 控制 UI 渲染
- 使用 Kotlin 函数（`@Composable`），可以嵌套组合
- IDE 支持快速预览、调试 UI
- Composable 函数可单独测试，易于单元测试
- 省去大量 `findViewById` 等

- `mutableStateOf` 创建响应式变量，状态变化自动更新 UI
- `State` / `LiveData` / `Flow`	Compose 可观察它们并更新 UI
- `Navigation`	Jetpack Compose 的导航组件

命令式
- 一步步指令式地告诉系统做什么、怎么做
- 数据变了 → 必须手动调用 `textView.setText()`

What are Composable functions?

What is Recomposition?

What is State in Compose?
- State 是用来驱动 UI 的数据，当 State 发生变化时，相关的 UI 会自动重新组合（Recomposition）并更新显示。


How does state management work in Jetpack Compose?

Stateful composable vs Stateless composable
- Stateful Composable 包含并管理自己的状态，内部定义 `remember` / `mutableStateOf`
= Stateless Composable 不包含状态，由外部传入控制状态，便于**复用**。

What are the side effects?
- 会影响外部世界或依赖外部状态的操作

Difference between `LaunchedEffect` and `DisposableEffect`.
- 管理副作用

What is rememberCoroutineScope and its use cases?

How to observe Flows, and LiveData states in Compose UI?

How can we handle asynchronous operations in Jetpack Compose?

How can we convert a non-composed state into a Compose state?

Explain derivedStateOf.

Explain rememberUpdatedState.

Difference between remember and rememberSaveable. - Learn from here

Explain the Lifecycle of a Composable in Jetpack Compose.

How do you handle lifecycle events in Compose functions?

What are the best practices for performance optimization in Jetpack Compose?

Can we use both Jetpack Compose and Android View in a Single App?

What is State Hoisting?

Explain CompositionLocal

Explain Jetpack Compose Phases.

What is the role of the Modifier in Jetpack Compose?

What are Semantics?

How can you handle user input and events in Jetpack Compose?

How do you handle navigation in Jetpack Compose?

How do you handle orientation changes in Jetpack Compose?

Explain the concept of unidirectional data flow in Jetpack Compose.

How to create Custom Layouts in Compose?
