---
title: Java异常体系结构
---

### Java异常体系结构

![image](https://note.youdao.com/yws/public/resource/7fc235c525305c0a08973cbf52212bc5/xmlnote/F8F814C1853543ECB84F66407E511C99/7121)

#### Throwable
Thorwable类所有异常和错误的超类，有两个子类Error和Exception，分别表示错误和异常

#### Exception 
程序本身可以处理的异常，这种异常分两大类运行时异常和非运行时异常。 
    程序中应当尽可能去处理这些异常。

* 运行时异常

    > 运行时异常都是RuntimeException类及其子类异常，如NullPointerException、IndexOutOfBoundsException等， 
    > 这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的， 
    > 程序应该从逻辑角度尽可能避免这类异常的发生。

* 非运行时异常

    > 非运行时异常是RuntimeException以外的异常，类型上都属于Exception类及其子类。 
    > 从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。 
    > 如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

#### Error
Error是程序无法处理的错误，比如OutOfMemoryError、ThreadDeath等。这些异常发生时， 
    Java虚拟机（JVM）一般会选择线程终止。