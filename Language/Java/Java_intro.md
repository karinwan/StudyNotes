# Java介绍

## 生态圈：

- 平台：Java虚拟机，语言被编译成**字节码**，依赖的语言包括Groovy, Scale, JRuby, Kotlin…
- 文化： 开源，共享
- 社区：桌面应用、嵌入式开发、企业级应用、后台服务器，中间件 都可以用java - 最适合**服务器端**开发

## 发展史

- 诞生于SUN (Stanford University Network)公司，09年被Oracle（甲骨文）收购
- java之父: 詹姆斯 高斯林
- 96年1.0版本
- 04年直接更新到5.0
- 14年8.0， 加入lambda
- 18年11，长期支持版本
- 21年17，加入很多新特性语法编写方式，如类型推断，长期支持版本
- 23年21，加入了纤程（虚拟线程）概念，提供更高的并发性和更低的资源消耗，长期支持版本

- **JavaSE** (Java Platform, Standard Edition标准版)：允许在桌面和服务器上开发和部署java应用程序
- **JavaEE** (Java Platform, Enterprise Edition企业版)：企业环境下的应用程序开发，主要针对web应用程序开发
- **JavaME** (Java Platform, Micro Edition小型版)：嵌入式和移动设备（打印机、机顶盒……）

## 环境

JDK (Java Development Kit): Java开发工具包，包含了**JRE**

- javac - 编译工具
- java - 运行工具
- jdb 调试工具
- jhat - 内存分析工具

JRE (Java Runtime Environment): Java运行环境，包含了**JVM**以及开发用到的**核心类库**

从jdk9开始jdk目录中就没有单独的jre目录了，因为jre作为一个运行时，里面不需要包含太多的东西浪费空间，降低运行效率，在jdk9时引用**模块化**技术，让开发者能按照自己的应用创建一个最小的运行时（runtime）

JVM （java虚拟机）：java运行程序的假想计算机

跨平台：java代码可以在不同操作系统上运行（一次编写，到处运行），通过安装针对不同系统版本的**JVM**

## 开发

1. 编写：创建文档，后缀.java
2. 编译：`javac {java文件名}.java`，javac会将java文件编译，生产一个.class文件（字节码文件），用来给jvm运行
3. 运行：`java {class文件名}`，不需要后缀

## 注释

- 单行注释：
    
    ```java
    // 注释内容
    ```
    
- 多行注释：
    
    ```java
    /* 
    	注释内容
    */
    ```
    
- **文档注释：**
    
    ```java
    /** 
    	注释内容
    */
    ```
    
    - 帮助别人快速了解开发好的类
    - 可以根据`javadoc -d {生成的文件夹名} -author -version {文件名.java}`命令生成一个文档（API文档）

## 关键字

- 具有特殊含义的, java提前定义好的小写单词
- 在高级记事本里有特殊颜色
- 如class, public, main, void, …

## 文件名与类名

- 通常需要一致
- 如需不同名称需要去掉class前面的`public`关键词
- 一个java文件可以有多个`class`类, 但是只能有一个类带`public`
    - javac会给每一个class生成一个单独的.class文件
    - 不建议在一个java文件中写多个class
- `main`方法必须在带`public`的类中
