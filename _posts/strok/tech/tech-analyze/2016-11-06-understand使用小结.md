---
layout: single
title: understand使用小结
description: 工作中用到的技术
tags: 分析技术
---

>understand是用来分析现有的代码结构，如果用来分析业务，那么就需要使用Rational+Rose结合进行分析。

1	Understand介绍
1.1	Understand是什么
Understand是一款代码分析软件，可以根据现在的代码去分析代码结构及方法中所体现出的代码逻辑。
对于程序员而言，希望能达成几个目的：
1.	查看一个业务操作的流程
2.	流程环节里面的业务逻辑判断
3.	对应的数据库表操作，最好能得到数据库的表结构。
其实这些就对应了进行UML分析的时序图、逻辑图、状态图和对象图。有了这几个图，那么，对于整个一套代码的了解也就足够。
1.2	Understand 是来解决什么的
2	Understand使用
2.1	快速搜索定位
2.2	项目视图
2.3	查看代码逻辑图
2.4	查看代码逻辑图

3	Understand 使用场景
说明，通过本次使用所需要的场景进行摸索。尽快服务于工作：
1.	时序图：
2.	UML图：
3.	代码逻辑图：
4.	状态图：这个用于跟踪关键字段、状态控制字段的状态值是如何变化的？
3.1	查看代码逻辑图
这个对应着一个类里面的方法，通过阅读方法体得出代码的逻辑。
3.2	查看时序图
时序图，其实对应的是一个用例，涉及到多个类之间的关联关系。
3.3	查看状态图
状态图，这个也对应的是一个用例。任何一个用例都应当有关键的字段。
4	Understand常见问答
4.1	打开的分析代码是乱码
设置成功了 project->configures->file option ->encode->utf-8
4.2	其他

5	其他补充说明
5.1	申明
