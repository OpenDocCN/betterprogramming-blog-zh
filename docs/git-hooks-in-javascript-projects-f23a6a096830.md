# 如何在 JavaScript 项目中使用 Git 挂钩

> 原文：<https://betterprogramming.pub/git-hooks-in-javascript-projects-f23a6a096830>

## 我后悔没有早点知道的钩子

![](img/0653da4fda8c581c3952785d3eea90ee.png)

照片由[罗曼·辛克维奇·🇺🇦](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

当 Git 存储库中发生特定事件时，Git 挂钩将自动触发定制脚本。有两组钩子，客户端和服务器端。

您可以查看这个[链接](https://git-scm.com/docs/githooks)以获得 Git 挂钩的完整文档，但是在本文中，我将只讨论`pre-commit`和`pre-push`。您可以从头开始编写自己的脚本，也可以查看这个 [GitHub repo](https://github.com/aitemr/awesome-git-hooks#tools) 以获得现成的工具。对我来说，我更喜欢[哈士奇](https://typicode.github.io/husky/#/)。汪汪！汪汪！

# 问题

你能想象许多开发人员在一个项目上工作，每个人都有自己的代码风格吗？是的，我知道有一些工具叫做 [ESLint](https://eslint.org/) 、[beautiful](https://prettier.io/)和 [Stylelint](https://stylelint.io/) ，但是你如何确保每个人都在他们的 IDE 中安装了它或者手动运行这个命令呢？

您的项目可能已经集成了[哨兵](https://sentry.io/)，您必须将发布版本从主要版本更新为次要版本。人类会犯错；我们不能保证我们永远不会忘记在提交之前更新哨兵版本。

# 解决方案

由于我们既需要`pre-commit`又需要`pre-push`吊钩，我们选择左[粗](https://typicode.github.io/husky/#/)和[左吊钩](https://github.com/evilmartians/lefthook)。与 Lefthook 相比，Husky 的配置更简单。然后，我们需要来自 lint-staged 的帮助，以迫使每个人在将他们的代码提交到存储库之前遵守标准；找到哨兵释放线，并在`pre-push`中将其替换为正确的版本

# 入门指南

装置

```
npm i -D husky lint-staged
// or
pnpm i -D husky lint-staged
// or
yarn add husky lint-staged -D
```

启用 Git 挂钩

```
npx husky install
```

确保项目中的每个开发人员都启用了 Git 挂钩。在您的`package.json`中这样做，每当您本地运行`npm install`时，它将自动启用`husky`。

```
// package.json
{   
  "scripts": {     
    "prepare": "husky install"   
  } 
}
```

# 预提交

将下面的片段添加到`package.json`:

```
// package.json
{
  "lint-staged": {
    "*.{js,vue}": "eslint --cache --fix"
  }
}
```

上面的代码片段可以这样理解:每当一个扩展名为`js`或`vue`的文件被登台时，它就会运行命令`eslint --cache --fix`。这将尽可能尝试修复 ESLint 问题。否则，它将返回错误，您必须在再次提交之前修复错误。

在`.husky/pre-commit`创建一个文件

```
*#!/bin/sh
.* "*$*(*dirname* "$0")/_/husky.sh"*npx* lint-staged
```

当您运行`git commit`时，命令`npx lint-staged`将在成功提交之前运行。

# 预推

我们需要一个自定义脚本来更新推前释放哨兵。

Sentry 发布在`vite.config.js`中，因此脚本将定位到`release:`的关键字所在的行，并用正确的发布名称替换它。

将下面的片段添加到`package.json`

```
// package.json
{
  "scripts": {
    "prepush": "node scripts/update-sentry-release.js"
  }
}
```

并使用下面的代码片段创建一个文件`.husky/pre-push`

```
*#!/bin/sh
.* "*$*(*dirname* "$0")/_/husky.sh"*npm* run prepush
```

就是这样！

# 已知问题

`lint-staged`当您尝试在 Windows 中提交大量暂存文件时，会抛出错误。在处检查未决问题[。](https://github.com/okonet/lint-staged/issues/420)

自定义脚本`update-sentry-release`只有在没有换行符时才起作用。

当有人绕过 Git 钩子，你合并他们的提交时，你必须修复他们的 linter 错误。

感谢阅读。敬请关注更多内容。