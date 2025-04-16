# Design Patterns

## Builder - Learn from here

---

## Singleton 单例
- 确保一个类在整个应用中只存在**一个实例**，并提供**全局访问点**；
- Kotlin 的`object`关键字天然支持单例；
- 常见使用场景：
  - 全局工具类：Log 工具类、图片加载工具 Glide 等；
  - 数据层：Retrofit 网络请求封装；
  - 管理类：用户信息管理器、设置管理器。
- 把 Activity 的 Context 存入单例中，就会导致这个 Activity 无法被 GC 回收，从而发生内存泄漏。

---

## Factory

---

## Observer 观察者模式
- 一种行为设计模式，定义了对象之间的**一对多**依赖关系，当一个对象**状态改变**时，其所有依赖者都会**收到通知并自动更新**。
- `Subject` **被观察者**状态发生变化时通知所有 `Observer`，比如 `LiveData`、`StateFlow`；
- `Observer` **观察者**接收状态变化通知，并做出响应，比如 `Activity`、`Fragment` 等。

## Pub/Sub 发布-订阅模式
- 通过中介（如事件总线`EventBus`）实现解耦通知，发布者与订阅者互不感知。

### 对比
| 对比维度              | 观察者模式（Observer）               | 发布-订阅模式（Pub/Sub）              |
|-----------------------|--------------------------------------|----------------------------------------|
| 通知方式              | 直接通知                             | 通过中介（事件中心）通知               |
| 是否解耦              | 强耦合（观察者被持有）             | 解耦（互不认识）                     |
| 是否有中介             | 无                                  | 有（事件总线）                       |
| 通常使用场景           | 局部 UI 数据绑定（LiveData）         | 模块间事件广播（EventBus）             |


---

## Repository

## Adapter

## Facade

## Dependency Injection
