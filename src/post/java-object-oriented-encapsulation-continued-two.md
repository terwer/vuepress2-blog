---
title: Java_SE_第十一讲：面向对象之封装续二
short_title: ''
description: 如果一个类包含了属性与方法那么该类的每一个对象都具有自己的属性但无论一个类有多少个对象这些对象共享同一个方法。关于方法参数传递的总结_对于java中的方法参数传递无论传递的是原生数据类型还是引用类型统一是传值（passbyvalue​）。什么类型的引用就能指向什么类型的对象比如people类型的引用就能指向people类型的对象但不能指向student类型的对象。比如_valpeople=people()正确valpeople=student()错误peoplepeople=newpeople()_正确p
date: 2022-10-10 21:53:44
category:
  - JavaSE
  - 后端开发
tag:
  - 构造方法
  - 对象
  - 参数
  - 类型
  - 一个
  - 面向对象
  - oop
  - object
article: true
timeline: false
---
1. 如果一个类包含了属性与方法，那么该类的每一个对象都具有自己的属性，但无论一个类有多少个对象，这些对象共享同一个方法。
2. 关于方法参数传递的总结：对于 Java 中的方法参数传递，无论传递的是原生数据类型还是引用类型，统一是传值（`pass by value`​）。
3. 什么类型的引用就能指向什么类型的对象，比如 People 类型的引用就能指向 People 类型的对象，但不能指向 Student 类型的对象。比如：

   ```kotlin
   val people = People() // 正确
   val people = Student() //错误
   ```

   ```java
   People people = new People(); // 正确
   People people = new Student(); //错误
   ```
4. 构造方法（`Constructor`​）：构造方法用于完成对象属性的初始化工作，构造方法的特点：  

   a) 构造方法的名字必须与类名完全一致（包含大小写）

   b) 构造方法没有返回值，连 `void` 也不能出现。

   c) 如果在定义一个类的时候，没有为类声明构造方法，那么 Java 编译器会自动为类添加一个没有参数且方法体为空的构造方法（默认的构造方法）

   d) 如果在定义一个类的时候，为类声明了构造方法，那么 Java 编译器就不会再为类添加构造方法了。

   e) 不能显式调用类的构造方法，构造方法通常是通过 new 关键字隐式调用。
5. new 关键字在生成对象时完成了三件事情：

   a) 为对象开辟内存空间。

   b) 调用类的构造方法。

   c) 将生成的对象的地址返回。
6. 默认的构造方法：构造方法没有参数且方法体为空。
7. 使用 `new`​ 来生成对象的时候，后面的小括号()表示构造方法的参数列表，如果构造方法不接收参数，那么小括号中的内容为空；如果构造方法接收参数，那么小括号中的实际参数就需要与构造方法定义中的形式参数保持一致（参数数量一致、参数类型一致、按照顺序逐一赋值）。

> 文章更新历史
>
> 2022/05/08 feat:新增 Kotlin 语法支持。
>
> 2022/05/08 fix:修改备注。