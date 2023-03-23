# 用 Github 动作自动发布一个 JavaScript 包到 NPM

> 原文：<https://betterprogramming.pub/how-to-publish-a-javascript-package-to-npm-automatically-with-github-actions-1acde7b908d6>

## 维护开源包可能是一项耗时的任务

![](img/4de36066a8460fe61215920728c79060.png)

阿里夫·瓦希德在 [Unsplash](https://unsplash.com/s/photos/future?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

维护开源包可能是一项耗时的任务。有问题要分类，有拉请求要审查，有变更日志要写。发布新版本的代码通常是手工完成的，自动化通常被放在维护者的待办事项列表的次要位置。

一个坚如磐石的发布过程有几个关键特征，即[变更日志](https://www.techopedia.com/definition/13934/changelog)、 [Git 标签](https://git-scm.com/book/en/v2/Git-Basics-Tagging)、 [NPM 版本](https://stackoverflow.com/questions/10972176/find-the-version-of-an-installed-npm-package)，以及强制[语义版本](https://semver.org/)。使所有这些保持同步，这样用户就能了解一个版本中的变化，并了解如何保持最新。未能执行所有这些步骤的维护人员将很难对问题进行分类，这导致花费更多的时间进行调试，而花费更少的时间进行编码。

我最近遇到了一个工具组合，[语义发布](https://github.com/semantic-release/semantic-release)和 [Github 动作](https://github.com/features/actions)，它们使得整个发布过程自动化、透明、简单易懂。

# 它是如何工作的

在我们讨论实现之前，理解我们的工具将执行什么工作是很重要的。这样，如果有问题或修改，我们可以修复它们。将完成这里的大部分工作——他们在自述文件中说得最清楚。

> 它自动化了整个包发布工作流，包括确定下一个版本号、生成发布说明和发布包。

## 下一个版本号

在发布时，为了确定下一个版本号，该工具读取自上一个发布以来的提交，它通过查看您的 Git 标记来定位。基于提交的类型，它可以决定如何提升包的版本。要做到这一点，提交需要以某种方式编写。

默认情况下，语义发布使用[角度提交消息约定](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)。这一点很关键，因为软件包的消费者需要知道新版本是否发布了新特性，是否引入了突破性的变化，或者两者都有。例如，如果有人提交了`fix(pencil): stop graphite breaking when too much pressure applied`，semantic-release 知道这包含一个修复，它必须创建一个补丁发布。这将增加次要版本范围(0.0.x)中的版本。

以前从未见过这种版本控制？ [*检出语义版本*](https://semver.org/) *。*

在分析了所有的提交之后，它会选择优先级最高的变更类型，并确保应用的是那个类型。例如，如果自上一个版本以来引入了两个提交，一个是中断(x.0.0)，一个是次要的(0.0.x)，它会知道通过中断范围来升级版本。

## 生成发行说明

一旦发现下一个版本是什么类型的版本，就会基于提交生成 changelog 注释。`semantic-release`不使用传统的`CHANGELOG.md`文件来通知用户发生了什么变化，而是使用附加在 Git 标签上的 [Github 版本](https://help.github.com/en/github/administering-a-repository/about-releases)来通知用户。

这里是一个 Github 版本的例子，语义发布生成并推进构建。

## Github 动作自动化

`semantic-release`是执行大部分工作的工具，但是我们仍然需要一个服务来运行这个工具。这就是 [Github Actions](https://github.com/features/actions) 发挥作用的地方。当 pull-requests 合并到 master(或您配置的任何基本分支)中时，Github Actions 将运行一个作业，该作业只运行您配置的语义发布。我们之前描述的所有工作都将被执行。

这里有一个 Github 操作的例子，使用语义发布来发布一个新版本。

# 要采取的步骤

我们将使用尽可能多的默认值来使配置变得非常简单。使用插件系统来增强功能。[以下是语义发布使用的默认插件。](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/plugins.md#default-plugins)

让我们来回顾一下这些步骤，以使这一切顺利进行。

**1。**在 package 的 package.json 中添加一个虚拟版本属性。发布的代码将由`semantic-release`将正确的版本写入该文件。

新 package.json 中的 version 属性

**2。**添加一个新的属性到 package.json，`publishConfig`——这将是我们语义发布配置的家。

新 package.json 中的 publishConfig 属性

**3。**最后一步是创建一个 Github 动作 YAML 文件。这将告诉 Github Actions 在提交存储库时做什么。

Github 操作配置文件

**4。**将`NPM_TOKEN`添加到 Github repos 设置页面的秘密中。您可以从您的 NPM 账户:`https://www.npmjs.com/settings/<username>/tokens`生成其中一个

![](img/15997cd829a60572632a6fde7e43fbdf.png)

秘密藏在哪里

就是这样！您现在拥有了一个完全自动化的包发布过程！

# 奖金

我在我们最近在 Yolk AI 开源的一个回购上实现了这个。它被命名为 [next-utils](https://github.com/Yolk-HQ/next-utils) ，这里描述的一切都可以在那里找到。

将 semantic-release 与 Github Actions 结合使用的另一个好处是，它支持即时可用的 bot 注释。它将深入到自上次发布和评论以来关闭的每个问题和拉动式请求，以确保每个人都知道。这里有一个例子。

![](img/169853f38958fb47687379fd798e4d07.png)

github-actions bot 对 github 问题的评论示例