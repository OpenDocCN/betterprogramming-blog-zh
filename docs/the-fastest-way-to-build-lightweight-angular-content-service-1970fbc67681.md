# 构建轻量级角度内容服务的最快方式

> 原文：<https://betterprogramming.pub/the-fastest-way-to-build-lightweight-angular-content-service-1970fbc67681>

## 最好的解决方案很简单！

![](img/07c33c2f132aa7e7180c6bdcafff3b54.png)

照片由[韦斯·廷德尔](https://unsplash.com/@lonestarexotic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/racecar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在典型的角度应用程序中，我们使用大量的标题、标签、标题和更多的内容。但是如果你需要一种方法把这些内容元素放在一个地方，你需要一个类似字典的东西。它是小单词和更广泛的句子的组合。

一些应用程序需要多种语言，但其他应用程序需要一种语言，但有大量内容可以重用。在本文中，我想向您展示为您的 Angular 应用程序创建一个`ContentService`(有人称之为字典)的最快方法。

> 如果你需要多语言的内容服务，我强烈推荐 [NGX-Translate](http://www.ngx-translate.com/) 。这个包提供了一个简单的 API 和管道机制来重用内容，但也支持多种语言。

# 创建内容服务

创建 Angular 服务最简单快捷的方法是使用 Angular CLI。

```
ng generate service services/content
```

通过运行这个命令，您将生成一个 Angular 服务，自动添加到`app.module.ts`文件中。如果您的项目具有不同的 CLI 设置，它可能会出现在另一个 Angular 模块中。

现在内容服务看起来是这样的。

# 创建一个 JSON 字典文件

您需要创建一个字典文件来放入您所有的标题、标签和其他内容。请将它放在最适合您的应用的地方。

我创建了一个`dictionary`文件夹，在那里我创建了`general.dictionary.json`文件，但是我可以在那里有更多的字典文件。

我放进去的内容是这样的。

你可以创建任何你喜欢的结构；这取决于你怎么做最好。

# 为可重用性准备内容服务

我们从创建私有财产`cache$`开始，在那里我们制作一个`BehaviourSubject`。我们这样做是因为我们可以从应用程序的任何地方订阅这个`BehaviourSubject`。最棒的是，当一个内容项被更新时，它将自动在任何地方被更新。

> 我创建了一个 [StackBlitz 示例](https://stackblitz.com/edit/angular-content-dictionary-service?file=README.md)，来展示为什么`BehaviourSubject`是服务的一个重要部分。

> 如果你想知道不同主题之间的区别，请查看这个帖子"[当在 Angular](https://hasnode.byrayray.dev/rxjs-subject-behavioursubject-replaysubject-asyncsubject-void-subject-angular) 中使用 RxJS 主题、behaviour 主题、replay 主题、async 主题或 Void 主题时"

下一步是在顶部导入字典文件。

在服务的构造函数中，我们希望确保如果`BehaviourSubject`是`null`，我们需要从字典文件中添加内容。

现在我们需要一个方法将`BehaviourSubject`及其内容公开给订阅者。我们通过返回`cache$`属性来做到这一点。在这种情况下，方法的返回类型是`any`,因为您不必键入字典的结构。但是如果你想，你可以做到。

为了使服务更适合在 HTML 模板中使用，我们可以公开链接了`.getValue()`方法的`content()`方法。

现在，我们的服务中有了一切可以使用的东西。服务的完整代码如下所示。简单右😉。

# 使用内容中的内容服务

我猜你知道怎么做一个有角度的组件。CLI 是我最喜欢的方式。例如，您生成一个`HomepageComponent`。

```
ng generate component components/homepage
```

如果你自己有个角的成分，那也没问题。

首先，我们需要将`ContentService`导入到我们的组件中，并通过`constructor`公开它。

现在我们想将来自`ContentService`的内容暴露给 HTML 模板。我们在组件中创建一个`content`属性，并通过`constructor`添加值。

通过`console.log`，您可以测试是否一切正常。

现在使用`ContentService`将字典文件中的标题添加到 HTML 模板中。

在示例中，您可以看到我们向模板添加了一个表达式。在这个表达式中，我们使用了无效碰撞技术。我们这样做，所以当属性不在字典文件中时，我们不会得到错误。在这种情况下，它只是向你显示“标题”。如果该值可用，它将交付该值。

# 资源

**StackBlitz** 中的代码示例👇

[](https://stackblitz.com/edit/angular-content-dictionary-service?ctl=1&embed=1&file=src/app/app.component.ts&hideExplorer=1&hideNavigation=1) [## 角度内容/字典服务(RxJS) - StackBlitz

### 编辑描述

stackblitz.com](https://stackblitz.com/edit/angular-content-dictionary-service?ctl=1&embed=1&file=src/app/app.component.ts&hideExplorer=1&hideNavigation=1) 

**Github** 示例👇

[](https://github.com/devbyray/angular-content-dictionary-service/tree/master) [## github-dev byray/angular-content-dictionary-service:用 StackBlitz ⚡️创建

### 由斯塔克布里兹·⚡️.创作通过创建一个应用程序，为 devbyray/angular-content-dictionary-service 开发做出贡献

github.com](https://github.com/devbyray/angular-content-dictionary-service/tree/master) 

# 结论

现在，您在 Angular 中有了一个简单的内容服务，而不需要使用外部包。所以它重量轻，速度超快，这是它最大的优点。我们可能经常认为它太复杂，但我们需要的只是简单的东西。

希望这有助于您构建易于维护的出色的角度应用程序。

# 谢谢！

![](img/2bd571bb1ce9f466ae279fa98111b086.png)

读完这个故事后，我希望你学到了一些新的东西，或者受到启发去创造一些新的东西！🤗

如果我给你留下了问题或一些要说的话作为回应，向下滚动并给我键入一条消息。如果你想保密，请在 Twitter @DevByRayRay 上给我发一条 [DM。我的 DM 永远是开放的😁](https://twitter.com/@devbyrayray)

[**通过电子邮件获取我的文章点击这里**](https://byrayray.medium.com/subscribe) **|** [**购买 5 美元中等会员资格**](https://byrayray.medium.com/membership)

# **阅读更多**

![RayRay](img/992af170033696163d6cc0269218aedd.png)

[雷雷](https://byrayray.medium.com/?source=post_page-----1970fbc67681--------------------------------)

## 荒诞的故事

[View list](https://byrayray.medium.com/list/angular-stories-24674407532a?source=post_page-----1970fbc67681--------------------------------)6 stories![](img/b94f2b7d2929c90566cd2dd6f657a751.png)![](img/02b73423a62d73b113af9fdf9629c79f.png)![Stacked books](img/b02a2f57c0093e04ab1d11d3a55f35ea.png)