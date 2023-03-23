# 到底什么才是好的拉式请求呢？

> 原文：<https://betterprogramming.pub/what-makes-a-good-pull-request-anyway-2a013a39aebe>

## 像专家一样了解创建拉式请求的工具和组件

![](img/b85ee4c4896aab4516967f10b344f621.png)

照片由[卡勒姆·肖](https://unsplash.com/@callumshaw?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在工程师的生活中，拉动式请求(PR)应该是一切的中心。这是你如何发布你的作品，以及其他人如何评价你的作品。

一段时间以前，开发工作是以 JIRA 为中心的，现在已经证明 PRs 是保存所有开发信息的最佳位置。为什么？因为它们被用作真理的单一来源。PRs 是所有开发人员讨论的地方。

当你希望你的代码被审查，并最终被合并时，知道如何创建一个好的 PR 是必须的。这是高级开发人员和初级开发人员之间的关键区别。

PRs 的重要性在开源社区中尤其明显，它们应该以一种清晰易懂的方式呈现。

# 那么，怎样才能成为一个好的公关呢？

## 1.语境

提供 PR 目的的概要说明。你的同事必须能够一眼就看出这是一个 bug、特性、集成测试等等。

*   如果是一个 bug，解释你的发现并简要总结你是如何修复的。

包括任何有助于其他人更好地理解此任务的视觉信息。截图总是受欢迎的，尤其是关于新功能的。他们帮助其他人描绘已经建成的东西。非技术人员欣赏这些。

⚠️避免在开发新功能时修复错误；这使得测试和审查过程更加复杂。为 bug 创建一个特殊的优先任务总是最好的。发现的 bug 和特性可能有不同的优先级——而决定这些优先级是项目经理的工作。

## 2.一致性

有许多构建代码的方法和许多你可以遵循的风格指南。最重要的是，一旦团队同意了某种风格，就要保持一致。打破一致性将使 PR 更难审查，代码更难阅读。

为一个新项目创建 PR 时，重要的是首先理解他们的工作方式，然后开始编码。匆忙的代码可能会降低你的速度。

幸运的是，我们有棉绒和像[漂亮](https://prettier.io/)这样的工具，可以帮助我们更容易保持这种一致性。

💁你采取不同的方法是好的，但是在使用它们之前最好先交流一下。规则是用来打破的，但它们需要正确的沟通。

## **3。尺寸**

将你的工作分成可管理的部分是非常重要的。你不应该做出太多的改变。如果你发现你的公关变得太大，试着把你的任务分成小块。

不要陷入重构陷阱。定期进行代码重构是个好主意，只是不要把它们和特性/错误混在一起；这会增加你的公关规模。相反，创建一个新的任务并在那里完成它。

Gif:“是陷阱！”

*   PR 越小，审核越快
*   在更小的 PRs 上更容易发现打字错误和小的回归错误
*   你交付的代码越少，你与其他同事的冲突就越少

公关推文图片

## 4.测试说明

如果你的任务将由 QA 工程师测试，确保你给他测试你的工作所需的所有细节。

💁包括您用来模拟特定场景的任何技巧可能有助于 QA 团队。

即使你没有 QA，描述如何测试你的代码也是值得的。任何人都可以加入进来并尝试一下。这对未来的你可能是个很好的提醒。

## 5.标题

确保你给公关一个精确和准确的标题。如果您使用其他工具，如 JIRA，请确保包括票号。

## 6.试验

添加测试总是有益的。它将展示您的代码，并使其更加健壮。其他同行将确信您的代码正在按预期运行。

确保为正确的工作使用正确的测试工具。测试应该是增值的，而不是样板。

Tweet:编写测试。不太多。主要是整合。

## 7.承诺

您的提交消息应该非常简单，不言自明。如果你不喜欢，你不必多次提交。

💁你可以在提交时使用表情符号，因为它们非常易读。他们通过包含相关表情符号来帮助更快地进入上下文。

让我们来看一个 bug 修复提交:

`git commit -m "JGM-3 🐛 preventing user from getting logged out"`

让我们来分解一下:

*   `JGM-3`项目+票号
*   `🐛`这个表情符号表明这是一个 bug
*   `preventing user from getting logged out`对这里所讨论内容的清晰总结

让我们看看当每个人都使用这种技术时，有多少提交被一起读取。

![](img/e49207727cfcfbce74ebe6612e714fa6.png)

使用图标的提交日志示例

只要看一眼，我们就能明白`master`发生了什么。

这个表情符号指南可以作为一个开始的参考。

[](https://gitmoji.dev) [## 吉特莫吉

### Gitmoji 是你提交信息的表情符号指南。旨在成为在…上使用表情符号的标准化骗子

gitmoji.dev](https://gitmoji.dev) 

## 8.通知

确保你通知了合适的人来检查。这很大程度上取决于你的团队结构。如果你的团队分为后端和前端，确保你通知了正确的开发人员。

## 9.何时创建您的公关

作为一个团队，你们制定一个策略是非常方便的。这里有一个例子:你可以同意将一天的工作分成几部分，并同意在早上检查 PRs。然后你可以设置一个每天早上出门的提醒。

然后，作为一个好习惯，你把一天的目标设定为以公关结束。这将保证你的工作在第二天早上第一件事就是被审查。

和你的团队一起定义策略会带来一致性和效率。一点也不像团队合作！

# 供您使用的工具

我们将探索 GitHub 为您提供的所有工具，以确保 PRs 尽可能的高质量。其他供应商也有非常相似的工具，所以它应该也适用于他们。

## 1.GitHub 模板

您可以为所有存储库 pr 创建一个模板。现在，所有创建 PR 的同事都会看到该模板。他们只需要适当地填充它。该模板旨在指导你如何展示你的作品。以下是如何在 GitHub 中创建模板:

[](https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository) [## 为存储库创建拉请求模板

### 当您将“拉”请求模板添加到存储库中时，项目参与者将自动看到模板的…

docs.github.com](https://docs.github.com/en/free-pro-team@latest/github/building-a-strong-community/creating-a-pull-request-template-for-your-repository) 

让我们来看一个理想的模板:

原始的`md`标记如下所示(供参考):

```
## What have I changedDeveloper point of view to help code review.
Please descibe the changes made and give reason for any non-obvious piece of code.## What have I fixedWhat was going wrong?
How is that fixed?
What can we do to prevent it from happening on other places?## Testing instructionsAdd here all the detailed feature/bugs to check.
Are there any corner case to look at?## Layout changesAre there any changes on the theme/layout?## Approval status-   [ ] QA approved
-   [ ] Designer approved
-   [ ] Product approved
```

## 2.Github 动作的自动化

> GitHub Actions 现在拥有世界一流的 CI/CD，可以轻松实现所有软件工作流程的自动化。直接从 GitHub 构建、测试和部署您的代码。按照您想要的方式进行代码审查、分支管理和问题分类。
> 
> [github.com](https://github.com/features/actions)

有了`Github Actions`,你可以整合任何你能想到的工作流，以确保代码安全地合并到 master 中。最常添加的内容是:

*   棉绒/漂亮的格子
*   单元测试/集成测试
*   捆尺寸检查器
*   缺少翻译检查器
*   使用[网页测试](https://www.webpagetest.org/)测量应用性能

查看`awesome-actions` GitHub 项目，这应该有助于开始。

[](https://github.com/sdras/awesome-actions) [## sdras/awesome-操作

### 与 GitHub 操作相关的令人敬畏的事物的精选列表。GitHub 平台事件直接触发动作…

github.com](https://github.com/sdras/awesome-actions) 

## 3.GitHub 超级棉绒机

这个可怕的[工具](https://github.com/github/super-linter)将减轻你的棉绒地狱。它应该有助于:

*   建立编码实践
*   布局和格式指南
*   防止代码上载到默认分支

# 结论

![](img/2f6c03cc9d1a29737cdc15bf31d305b4.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们探讨了如何提高你的公关创造技能。PRs 是你向他人展示你的代码运行良好的方式。完美的公关会因团队而异，但基本原则应该是相同的。

正如另一篇文章所说，公关代表你的品牌，所以你应该好好照顾他们。

干杯！