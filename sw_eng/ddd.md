# 领域驱动设计
用演进式的领域模型将软件设计与实现连接起来，用来满足复杂领域的软件开发方法。

## 领域模型
- 面向复杂领域
- 模型能将设计和实现衔接
- 模型是演进的

一个模型达成共识；不再有分析模型、设计模型等；

## 领域驱动设计过程
1. 分析师、设计师、主要程序员参与模型设计；
2. 只针对核心资产使用统一的领域模型()；非核心资产可以差别的模型或无模型？

## 战略设计
- 识别业务核心域；按核心域、支撑域、通用域切分，核心域才是DDD应该关注的地方。
- 分离关注点；以Bounded Context为边界划分模型；？？？？

- 模型集成策略；

核心域--》BC，对BC建模--》Domain

## 战术设计
##### 建模元素
- Entity(实体)
- Value Object(值对象)
- Aggregate
- Factory
- Repository
- Service
- Event

## 参考
1. [领域驱动设计到底难在哪？](https://www.jianshu.com/p/ab80cb9f307c)
2. [“领域驱动设计”答疑（汇总）](https://www.jianshu.com/p/f876d19a9aa3)
3. [DCI in C++](https://www.jianshu.com/p/bb9c35606d29)
4. Domain-Driven Design, Trakling Complexity in the Heart of Software; by Eric Evans.

# 面向模型的实现模式(C语言的实现)
### 表达概念
##### 1 命名
命名同时表达领域语义与**模型角色**(如xxService，xxRepo)；

Modular C可采用类似C++类的方法，不要一个文件塞太多东西，避免xxFunc, xxCommon这种无意义的命名；
推荐每个概念按照C++类的实现，放一个专有.c和对应的.h。

##### 2 行为
- 突出对外API，最小化暴露；(头文件显式定义API，只放API依赖的元素)
- 让接口有领域含义，尽量不要有get/set([Tell Don't Ask原则](https://www.aqee.net/post/tell-dont-ask.html)，尽量不感知调用对象内部状态)；(不要为了私有变量做get/set)
- 区分set(update)和构造的服务上下文区别；？？
- **通过包含接口类型，显式化声明接口；(派生行为，即例中的setup_handle函数指针)**

公有行为：提供一个可直接调用的接口函数；
私有行为：隐藏在自己的.c中；
通过派生得到接口的行为：C语言结构体中嵌入函数指针；

##### 3 属性
- 明确区分**属性**和**依赖**

Value Object(值对象)建议用const修饰。

### 表达关系
可参考UML类图中的[6大关系](uml.md)
- is-a：泛化关系，派生或继承；C语言用函数指针表示is-a，区分有状态和无状态；
  > - 使用函数指针结构体表示抽象接口；
  > - 无状态和有状态，有状态的情况需传递状态；
  > - 使用`void*`指针传递不同的状态类型；
  > - 使用表描述一组共同变化的函数指针； 
- has-a：组合&聚合；
  > - Composition: 优先推荐值包含；
  > - Aggregation: 使用指针；用const表示不可更改；
- has-many：
  > - 根据目标需要，实现选择合适的依赖方向； 
- dependency：
  > - 参数依赖，use关系，传递指针类型，const修饰；
  > - 返回值依赖，create关系，一般返回指针类型
  > - 减少dependency的物理依赖，尽量前置声明，不要在头文件中包含依赖的头文件；

### 生命周期管理？？？（目的是什么？）

C语言用factory和repository隐藏内存预占的全局变量；

- 全局变量没有生命周期的语义，区分内存预占和生命周期管理；
- C语言没有构造和析构，生命周期管理显式的使用Factory(如示例中port_factory.c中的alloc和release函数)；
- 用Factory隐藏内存预占的全局变量，明确对外提供生命周期语义；
- 嵌入式场景下，Factory和Repository很多情况可以合一；
- 编译器拓展，实现全局自动注册的能力，区分类型与对象的生命周期；


### 物理设计
保持逻辑结构和物理结构的一致性；

##### 1 符号隐藏
- 使用static关键字隐藏私有方法（Modular C中的PRIVATE实际为static）；
- 使用编译器拓展隐藏包内接口；
##### 2 文件组织
- 头文件结构
- API访问级别：包内、包外、私有；
- 接口参数格式，显式的this，const；
- Modular C风格，一个.h和.c表示一个概念；
- **前置声明**，可避免将仅内部使用的结构体开放到头文件中！！参考C++的[PImpl](https://en.cppreference.com/w/cpp/language/pimpl)技术；
> - 编译时弱依赖的指针、返回值类型、函数参数类型等都可做前置声明；
##### 3 文件夹(包)组织
*问题： 一个组件中的so有没有数量限制，6~7个so是不是太多？ 1个so是否合适？*