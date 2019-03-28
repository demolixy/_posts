---
title: Maven scope的几种类型
---

## 1 问题背景

* 在处理单元测试的时候，多个模块需要处理相同的单元测试逻辑，所以就将重复的逻辑给抽象出来，组成一个base，让各个基类实现各自的内容。

## 2 问题现象
* 但是集成的过程中发现始终import不进去base jar里面的内容

## 3 问题定位
* 最终定位是```pom.xml```依赖这个jar，他的scope是test，也就是说只有```test-source```才会import这个jar，正常的src是依赖不上的。

## 4 问题总结
* 本次问题还是对mvn的scope不太熟悉导致的。所以就将maven的几种scope给列出来，复习一下。

* scope
    > 依赖范围控制哪些依赖在哪些classpath 中可用，哪些依赖包含在一个应用中。

* compile (编译范围)
    > compile是默认的范围；如果没有提供一个范围，那该依赖的范围就是编译范围。编译范围依赖在所有的classpath 中可用，同时它们也会被打包。

* provided （已提供范围）
    > provided 依赖只有在当JDK 或者一个容器已提供该依赖之后才使用。例如， 如果你开发了一个web 应用，你可能在编译 classpath 中需要可用的Servlet API 来编译一个servlet，但是你不会想要在打包好的WAR 中包含这个Servlet API；这个Servlet API JAR 由你的应用服务器或者servlet 容器提供。已提供范围的依赖在编译classpath （不是运行时）可用。它们不是传递性的，也不会被打包。

* runtime （运行时范围）
    > runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你可能在编译的时候只需要JDBC API JAR，而只有在运行的时候才需要JDBC驱动实现。

* test （测试范围）
    > test范围依赖 在一般的编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用。

* system （系统范围）
    > system范围依赖与provided 类似，但是你必须显式的提供一个对于本地系统中JAR 文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分。这样的构件应该是一直可用的，Maven 也不会在仓库中去寻找它。如果你将一个依赖范围设置成系统范围，你必须同时提供一个 systemPath 元素。注意该范围是不推荐使用的（你应该一直尽量去从公共或定制的 Maven 仓库中引用依赖）。

* scope的依赖传递
    > A–>B–>C。当前项目为A，A依赖于B，B依赖于C。知道B在A项目中的scope，那么怎么知道C在A中的scope呢？答案是： 当C是test或者provided时，C直接被丢弃，A不依赖C； 否则A依赖C，C的scope继承于B的scope。