# Pheonix

## MVP

## MVVM
- 强调数据驱动 UI，适合响应式框架（如 Jetpack Compose、DataBinding）

---

| 对比维度      | MVC（Model-View-Controller）     | MVP（Model-View-Presenter）     | MVVM（Model-View-ViewModel）     |
|-------------------|-------------------------|-------------------------|--------------------------------|
| 核心组成          | Model, View, Controller       | Model, View, Presenter          | Model, View, ViewModel              |
| 中间层职责名称    | Controller        | Presenter                | ViewModel                  |
| 业务逻辑位置      | Controller（通常是 Activity）          | Presenter（独立类）        | ViewModel             |
| View 与逻辑关系   | View 调用 Controller，Controller 直接更新 UI | View 调用 Presenter，Presenter 回调 View 接口 | View 观察 ViewModel 暴露的数据（如 LiveData）     |
| 数据驱动 UI       | ❌ 否，UI 需手动更新       | ❌ 否，需手动调用 View 接口      | ✅ 是，View 自动监听数据变化        |
| 代码耦合度        | 高（Activity 同时承担多角色）        | 中（接口多但结构清晰）        | 低（View 和 ViewModel 解耦）     |
| 单元测试友好性    | 差                   | 较好                          | 很好                            |
| 线程/异步支持     | 需自己管理线程             | 通常由 Presenter 控制线程         | 可用协程 + LiveData + Flow     |
| 生命周期感知      | ❌ 无                 | ❌ 无                    | ✅ ViewModel 生命周期感知         |
| Android 场景适配  | 早期传统项目          | 中期项目结构清晰                 | Jetpack、Compose、现代 Android 架构推荐          |
