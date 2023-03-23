# 如何用 Angular NGX-Formly 格式化一个值

> 原文：<https://betterprogramming.pub/how-to-format-a-value-with-angular-ngx-formly-c18dcd7be647>

## 格式化邮政编码、电话号码、银行账户等等的简便方法

![](img/5201e65dfc289332077817d0a4cced9b.png)

作者图片

这就是为什么我喜欢用 [NGX-Formly](https://formly.dev/) ！它们使得书写小的、大的、甚至复杂的表格变得更加容易。但是最近，我想在用户填写输入值后改变它们的格式。因为有一定的方式，后端 API 期望它们。

> 如果你不知道如何使用 NGX-formly，我强烈推荐阅读下面的帖子(它们甚至包含在 NGX-formly 本身的文档中)

1.  [使用 Ngx Formly 在 Angular 上构建快速的 JSON 驱动的表单](/build-fast-json-powered-forms-on-angular-with-ngx-formly-b7a00733e66e)
2.  [如何使用 ngx-formly 在 Angular 上构建快速、高级的 JSON 表单](/build-fast-json-powered-forms-on-angular-with-ngx-formly-b7a00733e66e)

# NGX 格式格式化程序

例如，我们想创建一个邮政编码字段和一个首字母字段。在这些字段中，我们希望对值应用特定的格式。在这种情况下，这将是视觉格式化，没有什么非常复杂的。

方法背后的逻辑是这样的。

# 格式化程序

所以每个需要格式化程序的字段都需要一个属性`parser.`

```
{
    //...
    parsers: [(value: string) => value?.toUpperCase()],
    //...
}
```

解析器需要是一个函数数组。每个函数都有一个保存值的参数。因为解析器需要一个数组，所以可以在每个字段上应用多个格式化程序。

该函数需要返回一个新值。因此它可以在输入字段中更新。

确保字段 config 也有属性`expressionProperties`，在其中更新模型；否则，它将不起作用。

```
expressionProperties: {
    'model.postal': 'model.postal',
},
```

# 例子

我已经通过 [StackBlitz](https://stackblitz.com/edit/angular-vibhz9?file=src%2Fapp%2Fapp.component.ts) 为本教程创建了一个示例项目，您可以用它来试验解析器。

如果你想在 Github 上查看项目，[请在这里找到资源库](https://github.com/devbyray/angular-ngx-formly-value-formatter-parser)。

# 结论

希望本教程能帮助你用 NGX-Formly 更容易地格式化你的值。如果您没有使用 NGX-Formly，我希望它能让您更快地构建表单！

祝你好运，玩得开心。

# 谢谢！

![](img/2bd571bb1ce9f466ae279fa98111b086.png)

读完这个故事后，我希望你学到了一些新的东西，或者受到启发去创造一些新的东西！🤗

如果我给你留下了问题或一些要说的话作为回应，向下滚动并给我键入一条消息。如果你想保密，请在 Twitter @DevByRayRay 上给我发一条 [DM。我的 DM 永远是开放的😁](https://twitter.com/@devbyrayray)

[**通过电子邮件获取我的文章点击这里**](https://byrayray.medium.com/subscribe) **|** [**购买 5 美元中等会员资格**](https://byrayray.medium.com/membership)

# 阅读更多

![RayRay](img/992af170033696163d6cc0269218aedd.png)

[雷雷](https://byrayray.medium.com/?source=post_page-----c18dcd7be647--------------------------------)

## 荒诞的故事

[View list](https://byrayray.medium.com/list/angular-stories-24674407532a?source=post_page-----c18dcd7be647--------------------------------)6 stories![](img/b94f2b7d2929c90566cd2dd6f657a751.png)![](img/02b73423a62d73b113af9fdf9629c79f.png)![Stacked books](img/b02a2f57c0093e04ab1d11d3a55f35ea.png)