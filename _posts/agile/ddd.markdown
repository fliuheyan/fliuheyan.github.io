## 没啥用的DDD

### DDD是什么
是指通过统一语言、业务抽象、领域划分和领域建模等一系列手段来控制软件复杂度的方法论


### Why DDD
分离业务逻辑和技术细节
DDD中的限界上下文可以用于指导微服务中的服务划分

### DDD的产出

### 如何做DDD
1. 建立领域知识
2. 通用语言(UML/文档/绘图等)
* DDD的四层结构

User Interface -> Application -> Domain -> Infrastructure

#### Entity
Object
#### Value Object
attributes of object

#### 聚合
聚合，它通过定义对象之间清晰的所属关系和边界来实现领域模型的内聚
聚合根就是一个facade，用来跟其他聚合交互

* feature
聚合是用来封装真正的不变性，而不是简单的将对象组合在一起；
聚合应尽量设计的小；
聚合之间的关联通过ID，而不是对象引用；
聚合内强一致性，聚合之间最终一致性；

#### 限界上下文
定义领域模型所应用的上下文


### DDD实践 - 事件风暴
DDD主要难点就是
1.发现系统中的聚合
2.划分Bounded Content
因为需要对业务有较深刻的理解
Event Storm 基于 DDD 概念的系统分析方法
事件风暴最大的作用是帮助开发人员，业务人员，UX，测试等项目参与者对于业务流程有一个统一的认识.


https://zhuanlan.zhihu.com/p/95001438

https://zhuanlan.zhihu.com/p/110979132

### 不用DDD的话我们怎么规划我们的微服务

规划微服务的时候持有functional programming的观点，将总体的业务看成是一个大的action.
然后将大的action分割成为一个个小的action，一个微服务就是一个action，最后则通过web api
或者是RPC将所有微服务compose起来.

基于“Service + 贫血模型”的实现

所以微服务的难点在于如何保证系统的可用性，以及如何有效的监控.


### 参考
https://www.cnblogs.com/netfocus/archive/2011/10/10/2204949.html

https://www.cnblogs.com/netfocus/p/3307971.html
