# Design Patterns

## Builder - Learn from here

## Singleton 单例
- 确保一个类在整个应用中只存在**一个实例**，并提供**全局访问点**；
- Kotlin 的`object`关键字天然支持单例；
- 常见使用场景：
  - 全局工具类：Log 工具类、图片加载工具 Glide 等；
  - 数据层：Retrofit 网络请求封装；
  - 管理类：用户信息管理器、设置管理器。
- 把 Activity 的 Context 存入单例中，就会导致这个 Activity 无法被 GC 回收，从而发生内存泄漏。

## Factory

## Observer

## Repository

## Adapter

## Facade

## Dependency Injection
