Title: 「Functional Programming Principles in Scala」学习笔记之「第一周」
Status: draft

上周开始学习[Functional Programming Principles in Scala](https://www.coursera.org/learn/progfun1/home/info)，在此记录，一方面通过梳理查漏补缺，也可以供后来者参考。

「Getting Started」部分比较简单，就不说了。课程分一下部分：
* 编程范式（Programming Paradigms）
* 程序的元素（Elements of Programming）
* 求值策略和终止（Evaluation Strategies and Termination）
* 条件和值定义（Conditionals and Value Definitions）
* 块和作用域（Block and Lexical Scope）
* 尾递归（Tail Recursion）

## 编程范式（Programming Paradigms）
函数式编程是一种编程范式，不同的编程范式是不同的概念和思考方式。
根据维基百科，我整理出如下的关系：
* [指令式编程](https://zh.wikipedia.org/wiki/%E6%8C%87%E4%BB%A4%E5%BC%8F%E7%B7%A8%E7%A8%8B)（C，C++，Java，JavaScript 等大部分编程语言都归于此类）
* [声明式编程](https://zh.wikipedia.org/wiki/%E5%AE%A3%E5%91%8A%E5%BC%8F%E7%B7%A8%E7%A8%8B)
    * 函数式编程（Haskell，LISP，Clojure，Scala 等）
    * 逻辑式编程（Prolog 等）
而「面向对象编程」是与以上几类正交的，比如：
* Java 既是指令式的又是面向对象的。
* Scala 既支持函数式又支持面向对象

**指令式编程**
现实世界中的「菜谱」和「使用说明书」都是指令式的，它要告诉机器具体要做什么。通常包括：
* 修改变量
* 赋值
* 控制语句（分支 if-then-else，循环 for/while，break，continue，return）

**函数式编程**
纯函数式编程没有变量，赋值以及控制结构。广义上说，函数式编程强调用函数来构造优雅的程序。
函数是一等公民，体现在：
* 可以在任何地方定义函数，包括函数内部
* 函数可以作为参数和返回这被传递
* 可以用一系列操作把函数组合成更强大的函数

## 程序的元素（Elements of Programming）


## 求值策略和终止（Evaluation Strategies and Termination）

## 条件和值定义（Conditionals and Value Definitions）
## 块和作用域（Block and Lexical Scope）
## 尾递归（Tail Recursion）