# 步骤 7：构建并运行 App

在这步中，你将使用 *pub build* 来生成 App 的资源并将他们放入一个名为 *build* 的新目录中。除了其它任务之外，构建过程中还会生成最精简的 JavaScript 代码，这些代码可以在现在任何主流的浏览器中运行。

需要注意的是 *one-hour-codelab* 目录包含几个目录，分别对应每个步骤，他们都被当做 one-hour-codelab 应用的一部分。构建过程为每个目录生成了资源。每个目录都可以被单独部署。

## 检出 pubspec.yaml

右击打开 *pubspec.yaml*。单击编辑窗口底部的 **Source** 标签。

```
name: avast_ye_pirates
description: Write a Dart web app code lab
dependencies:
  browser: any
```

### 关键信息

* 目录中的 *pubspec.yaml* 文件标识了这个目录和其中的内容是一个应用。
* *pubspec.yaml* 提供了应用的元信息，例如它的名字。
* *pubspec.yaml* 同时也列出了应用所依赖的库。 这个 App 需要的 *browser* 库被托管在了 *pub.dartlang.org* 上面，同时还托管了许多其它的库。
* *any* 代表选择符合你SDK的最新的软件包。

## 查看包目录

在 Dart 编辑器中，展开 *1-blankbadge* 下的 *packages* 目录。

![pic1](images/learn-dart-in-minutes-build-and-run-pic1.png)

### 关键信息

* *packages* 目录包含了 *pubspec.yaml* 文件中列出的所有依赖的代码。他们是 Dart 编辑器自动安装的。
* *browser* 包包含了检查本地 Dart 支持的 *dart.js* 脚本。
* 为了能让应用成功部署，这些包必须包含在构建的应用中。

## 运行 Pub 构建

选择 *pubspec.yaml*，然后选择 **Tools > Pub Build - Minified，将会构建 *one-hour-codelab* 目录下的所有东西。输入内容类似如下：

```
Loading source assets...
Building avast_ye_pirates...
[Info from Dart2JS]:
Compiling avast_ye_pirates|web/piratebadge.dart...
[Info from Dart2JS]:
Took 0:00:08.671410 to compile avast_ye_pirates|web/piratebadge.dart.
Built 6 files to "build".
```

### 关键信息

* *pub build* 命令创建了一个 *build* 目录
* 你可以选择 **Pub Build - Minified** 或者 **Pub Build - Debug**。 当构建一个最精简的 JavaScript 时，所有空格和无关的字符都会被移动，创建一个更紧凑的文件，但它的可读性更低。
* *build* 目录包含了部署项目所需要的一切东西。

## 查看 build 目录

在你工作的 *1-blankbadge* 目录下，展开 *build* 目录，然后展开 *web* 目录。

![pic2](images/learn-dart-in-minutes-build-and-run-pic2.png)

### 关键信息

* *build/web* 包含了 App 单独部署所需的所有文件。
* *piratebadge.dart.js* 是一个精简的 JavaScript 文件。 当部署时，这个文件会运行在浏览器中。

## 运行 App

### 关键信息

* App 运行在本机上。如果你想共享给他人，你需要将你的 App 部署到主机上。
