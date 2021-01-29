# Ract Fiber

**React Fiber**是经过两年研究，最终面世的新的架构；React Fiber 的目标是提高其对动画，布局和手势等领域的适用性。它的主体特征是增量渲染：能够将渲染工作分割成块，并将其分散到多个帧中。其他主要功能包括在进行更新时暂停，中止或重新使用工作的能力，为不同类型的更新分配优先权的能力和新的并发原语。

**在新的react架构中，会将代码到页面分成3个阶段**

* **schedule** 阶段，负责调度任务的优先级（requestIdleCallback，不过react使用的是自己实现的ployfill）
* **reconcile** /**render** 阶段，判断组件任务是否需要更新，是可暂停的
* **commit** 阶段，将变化的组件渲染到页面上

在其中fiber作为reconcile阶段的最小任务单元

## Fiber的含义

`Fiber`包含三层含义：

1. 作为架构来说，之前`React15`的`Reconciler`采用递归的方式执行，数据保存在递归调用栈中，所以被称为`stack Reconciler`。`React16`的`Reconciler`基于`Fiber节点`实现，被称为`Fiber Reconciler`。
2. 作为静态的数据结构来说，每个`Fiber节点`对应一个`React element`，保存了该组件的类型（函数组件/类组件/原生组件...）、对应的DOM节点等信息。
3. 作为动态的工作单元来说，每个`Fiber节点`保存了本次更新中该组件改变的状态、要执行的工作（需要被删除/被插入页面中/被更新...）。

#### beginWork

> 向下遍历JSX，为每个JSX节点的子JSX节点生成对应的Fiber，并设置effectTag

#### completeWork

> 为每个Fiber生成对应的DOM节点

#### workLoopSync

调用**workLoopSync**方法，他内部会循环调用**performUnitOfWork**方法。

##### performUnitOfWork

performUnitOfWork每次接收一个Fiber**，调用**beginWork**或**CompleteWork**，处理完该Fiber后返回下一个需要处理的Fiber。

整个流程虽然看起来繁琐，但就做了2件事：

1. 采用深度优先遍历，从上往下调用beginwork生成子Fiber，生成后继续向子Fiber遍历
2. 当遍历到底没有子Fiber时，开始从底往上遍历，调用completeWork为每个步骤1中已经创建的Fiber创建对应的DOM节点

#### 渲染阶段

### effectList

在我们的设计中，**commit**阶段会遍历找到所有含有**effectTag**的Fiber节点。如果Fiber树很庞大的话，这个遍历会很耗时。

但其实在**render**阶段我们已经知道哪些Fiber会被设置Fiber.effectTag， 所以我们可以在**render**阶段就提前标记好他们，将他们组织成链表的形式。