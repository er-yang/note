# Babel Plugin

[原文](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md)

Babel 是一个通用的多功能的 JavaScript 编译器。此外它还拥有众多模块可用于不同形式的静态分析。

Babel 的三个主要处理步骤分别是： **解析（parse）**，**转换（transform）**，**生成（generate）**。.

### 解析

**解析**步骤接收代码并输出 AST。 这个步骤分为两个阶段：[**词法分析（Lexical Analysis） **](https://en.wikipedia.org/wiki/Lexical_analysis)和 [**语法分析（Syntactic Analysis）**](https://en.wikipedia.org/wiki/Parsing)。.

#### 词法分析

词法分析阶段把字符串形式的代码转换为 **令牌（tokens）** 流。.

你可以把令牌看作是一个扁平的语法片段数组：

#### 语法分析

语法分析阶段会把一个令牌流转换成 AST 的形式。 这个阶段会使用令牌中的信息把它们转换成一个 AST 的表述结构，这样更易于后续的操作。

### 转换

[转换](https://en.wikipedia.org/wiki/Program_transformation)步骤接收 AST 并对其进行遍历，在此过程中对节点进行添加、更新及移除等操作。 这是 Babel 或是其他编译器中最复杂的过程 同时也是插件将要介入工作的部分，这将是本手册的主要内容， 因此让我们慢慢来。

### 生成

[代码生成](https://en.wikipedia.org/wiki/Code_generation_(compiler))步骤把最终（经过一系列转换之后）的 AST 转换成字符串形式的代码，同时还会创建[源码映射（source maps）](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)。.

代码生成其实很简单：深度优先遍历整个 AST，然后构建可以表示转换后代码的字符串。

### Visitors（访问者）

当我们谈及“进入”一个节点，实际上是说我们在**访问**它们， 之所以使用这样的术语是因为有一个[**访问者模式（visitor）**](https://en.wikipedia.org/wiki/Visitor_pattern)的概念。.