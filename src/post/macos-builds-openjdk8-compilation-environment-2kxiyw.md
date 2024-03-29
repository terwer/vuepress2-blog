---
title: macOS搭建openjdk8编译环境
short_title: ''
description: 本文介绍了在macOS上编译OpenJDK的环境准备和步骤，包括安装必要的软件和工具，解决常见错误，以及提供了相关资源链接。文章详细讲解了如何配置Xcode、安装ccache、解决编译错误，并提供了编译OpenJDK的命令。适用于想要在macOS上编译OpenJDK的开发者。
date: 2023-09-19 10:20:28
category:
  - 后端开发
  - JDK8
  - JDK源码分析
tag:
  - 编译
  - macos
  - openjdk
  - 配置
  - 调试
article: true
timeline: false
---
本文介绍了在macOS上编译OpenJDK的环境准备和步骤，包括安装必要的软件和工具，解决常见错误，以及提供了相关资源链接。文章详细讲解了如何配置Xcode、安装ccache、解决编译错误，并提供了编译OpenJDK的命令。适用于想要在macOS上编译OpenJDK的开发者。

<!-- more -->




## 环境准备

* macOS Ventura 13.5.2
* freetype （可使用 `brew install freetype`​ 安装）
* jdk1.8.0_333
* [XQuartz](https://www.xquartz.org/) XQuartz 2.7.11（xorg-server 1.18.4）
* gawk（可使用 `brew install gawk`​ 安装）

## 拉取 openjdk8 源码

```bash
git clone https://github.com/openjdk/jdk8u jdk8u
```

## 安装 ccache 加速编译

必须大于 3，我目前是 `4.8.3`​

```bash
brew install ccache
```

## 检测

* 可以通过 `sh ./configure --help`​ 查看字段意义，也可能看官网的解释：[http://hg.openjdk.java.net/jdk8u/jdk8u/raw-file/tip/README-builds.html#configure](http://hg.openjdk.java.net/jdk8u/jdk8u/raw-file/tip/README-builds.html#configure)
* ```
  sh ./configure --help
  ```

最终的检测命令

```
sh ./configure --with-xcode-path=/Applications/Xcode_12.4.app \
--with-boot-jdk=/Library/Java/JavaVirtualMachines/jdk1.8.0_333.jdk/Contents/Home \
--with-freetype-include=/usr/local/include/freetype2 \
--with-freetype-lib=/usr/local/lib/ \
--with-debug-level=slowdebug \
--enable-ccache \
--enable-debug-symbols \
--with-target-bits=64 \
--with-jvm-variants=server \
OBJCOPY=gobjcopy
```

## 构建

输出结果

```plaintext
====================================================
A new configuration has been successfully created in
/Users/terwer/Documents/code/jdk/jdk8u/build/macosx-x86_64-normal-server-slowdebug
using configure arguments '--with-xcode-path=/Applications/Xcode_12.4.app --with-boot-jdk=/Library/Java/JavaVirtualMachines/jdk1.8.0_333.jdk/Contents/Home --with-freetype-include=/usr/local/include/freetype2 --with-freetype-lib=/usr/local/lib/ --with-debug-level=slowdebug --enable-ccache --enable-debug-symbols --with-target-bits=64 --with-jvm-variants=server OBJCOPY=gobjcopy'.

Configuration summary:
* Debug level:    slowdebug
* JDK variant:    normal
* JVM variants:   server
* OpenJDK target: OS: macosx, CPU architecture: x86, address length: 64

Tools summary:
* Boot JDK:       java version "1.8.0_333" Java(TM) SE Runtime Environment (build 1.8.0_333-b02) Java HotSpot(TM) 64-Bit Server VM (build 25.333-b02, mixed mode)  (at /Library/Java/JavaVirtualMachines/jdk1.8.0_333.jdk/Contents/Home)
* Toolchain:      clang (clang/LLVM)
* C Compiler:     Version 12.0.0 (at /usr/bin/clang)
* C++ Compiler:   Version 12.0.0 (at /usr/bin/clang++)

Build performance summary:
* Cores to use:   7
* Memory limit:   32768 MB
* ccache status:  disabled

➜  jdk8u git:(master)
```

### 编译

进入 jdk 目录 执行 make all 执行成功后会有如下输出，编译后 jdk 目录将近 3G 左右，编译的过程中存在很多的 Warning，直接忽略了。

默认构建

```plaintext
make
```

结果输出

```bash
----- Build times -------
Start 2023-09-19 18:36:44
End   2023-09-19 18:41:27
00:00:14 corba
00:01:35 hotspot
00:00:09 jaxp
00:00:18 jaxws
00:02:08 jdk
00:00:18 langtools
00:04:43 TOTAL
-------------------------
Finished building OpenJDK for target 'default'
➜  jdk8u git:(master) ✗ 
```

### 查看 java -version

编译之后的 jdk 在这个目录下 `macosx-x86_64-normal-server-slowdebug`​

```plaintext
➜  jdk8u git:(master) ✗ ./build/macosx-x86_64-normal-server-slowdebug/jdk/bin/java -version
openjdk version "1.8.0_392-internal-debug"
OpenJDK Runtime Environment (build 1.8.0_392-internal-debug-terer_2023_09_19_18_36-b00)
OpenJDK 64-Bit Server VM (build 25.392-b00-debug, mixed mode)
➜  jdk8u git:(master) ✗ 
```

## 问题解决

1. 具体错误：

   ```
   configure: error: Xcode 6, 9-12 is required to build JDK 8, the version found was 15.0. Use --with-xcode-path to specify the location of Xcode or make Xcode active by using xcode-select.
   ```

   解决办法：

   安装 Xcode12，并设置

   ```
   --with-xcode-path=/Applications/Xcode_12.4.app
   ```
2. 具体错误：

   ```
   checking if we should generate debug symbols... configure: error: Unable to find objcopy, cannot enable debug-symbols
   ```

   解决方案：需要安装 `binutils`​​，并且需要在 `configure`​ ​时添加参数 `OBJCOPY`​​

   ```bash
   # 安装 binutils
   brew install binutils
   ```

   ```bash
   # configure 添加参数 
   OBJCOPY=gobjcopy
   ```

3. 具体错误：

   [https://github.com/akinomyoga/ble.sh/issues/190](https://github.com/akinomyoga/ble.sh/issues/190)

   解决方案

   安装 gawk

   ```bash
   brew install gawk
   ```

## 参考

[https://github.com/openjdk/jdk8u](https://github.com/openjdk/jdk8u)

[Mac 上编译 OpenJDK 过程记录](https://www.cnblogs.com/micrari/p/7018474.html)

[https://github.com/manheiba/jdk8mhb](https://github.com/manheiba/jdk8mhb)​

[《HotSpot 实战》](https://item.jd.com/11414422.html)

[MacOS 编译 openjdk8 并导入 Clion 调试](https://www.cnblogs.com/dwtfukgv/p/14727290.html)

[Mac10.14 下使用 Xcode10 编译调试 OpenJDK8 最新教程](https://blog.csdn.net/hilaryfrank/article/details/112859423)​

‍