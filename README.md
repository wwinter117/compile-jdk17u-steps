# compile-jdk17u-steps
记录下在m1pro芯片的macOS上编译jdk17u的过程

## 为什么编译jdk17u这个版本

`JDK 8` 最早发布时，主要支持的是 `x86` 和 `x64` 架构的系统。`Apple M1 Pro` 芯片使用的是 `AArch64` 架构，而 `JDK 8` 并没有对 `macOS/AArch64` 提供原生支持，需要使用`Rosetta 2` 转译工具在 `M1 Pro `芯片上运行基于 `x86_64` 架构的 `JDK 8`。

`JDK 17` 已经为 `macOS/AArch64` 提供了原生支持，性能和兼容性都更好。

- [JEP 306: Restore Always-Strict Floating-Point Semantics](https://openjdk.java.net/jeps/306)
- [JEP 356: Enhanced Pseudo-Random Number Generators](https://openjdk.java.net/jeps/356)
- [JEP 382: New macOS Rendering Pipeline](https://openjdk.java.net/jeps/382)
- [JEP 391: macOS/AArch64 Port](https://openjdk.java.net/jeps/391)
- [JEP 398: Deprecate the Applet API for Removal](https://openjdk.java.net/jeps/398)
- [JEP 403: Strongly Encapsulate JDK Internals](https://openjdk.java.net/jeps/403)
- [JEP 406: Pattern Matching for switch (Preview)](https://openjdk.java.net/jeps/406)
- [JEP 407: Remove RMI Activation](https://openjdk.java.net/jeps/407)
- [JEP 409: Sealed Classes](https://openjdk.java.net/jeps/409)
- [JEP 410: Remove the Experimental AOT and JIT Compiler](https://openjdk.java.net/jeps/410)
- [JEP 411: Deprecate the Security Manager for Removal](https://openjdk.java.net/jeps/411)
- [JEP 412: Foreign Function & Memory API (Incubator)](https://openjdk.java.net/jeps/412)
- [JEP 414: Vector API (Second Incubator)](https://openjdk.java.net/jeps/414)
- [JEP 415: Context-Specific Deserialization Filters](https://openjdk.java.net/jeps/415)

其中 `JEP 391`的增强是专门为了支持 `macOS/AArch64`（即 `M` 系列芯片）的，因此从 `JDK 17` 开始，编译 `JDK` 会充分利用 `M1` 芯片的架构优势，而不需要依赖 `Rosetta 2` 来进行` x86` 到 `ARM` 的转译。

详细的构建流程查看`openjdk`官方： https://github.com/openjdk/jdk17u/blob/master/doc/building.md

接下来是在 `m1pro` 芯片的 `macos` 上的编译过程

## 编译环境准备

**bootjdk**

遵循`N-1`，使用`jdk16`编译`jdk17u`

```
# 下载jdk16来编译jdk17u
brew install --cask adoptopenjdk/openjdk/adoptopenjdk16

# 获取 JDK 16 的安装路径（无需将其加入环境变量）
/usr/libexec/java_home -v 16

# 一般以上命令会输出以下路径，记录下这个路径，后面编译时指定这个路径
/Library/Java/JavaVirtualMachines/adoptopenjdk-16.jdk/Contents/Home
```

**xcode**

重要：使用推荐的`13.1`的这个版本

如果你的`Xcode`不是这个版本可以按照下面流程进行下载：

```
# apple官网下载版本13.1版本的Xcode（以下链接下载，需要登录）
https://developer.apple.com/download/all/?q=13.1

# 解压（或者双击解压）
xip --expand Xcode_13.1.xip

# 记录下解压后的路径，一般以下是用户目录下的Download路径：
～/Downloads/Xcode.app
```

**freetype**

```
brew install freetype
```

**autoconf**

```
brew install autoconf
```
### 获取源码

**ssh:**

```
git clone git@github.com:openjdk/jdk17.git
```

**https:**

```
git clone https://github.com/openjdk/jdk17.git
```
## 开始编译

进入源码目录，输入：

```
sh configure --with-boot-jdk=/Library/Java/JavaVirtualMachines/adoptopenjdk-16.jdk/Contents/Home/ --with-xcode-path=/Users/zhangdongdong/Downloads/Xcode.app
```

其中

`--with-boot-jdk=` 后面指定刚才下载的jdk16的路径
`--with-xcode-path=` 后面指定刚才下载的Xcode路径

## idea中验证

投稿视频中演示
