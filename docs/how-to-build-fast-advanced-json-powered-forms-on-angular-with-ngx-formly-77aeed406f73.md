# 如何使用 ngx-formly 在 Angular 上构建快速、高级的 JSON 表单

> 原文：<https://betterprogramming.pub/how-to-build-fast-advanced-json-powered-forms-on-angular-with-ngx-formly-77aeed406f73>

## 验证、可重复部分、条件字段，以及将表单提交给 API

![](img/213df91a6b76f63a03d0c787a3c5e76a.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文是名为“Angular 中的 JSON Powered Forms”的两部分系列的第二部分如果你没有检查第一部分，我建议你先检查。

在第一篇文章中，我讨论了如何用 UI 框架 [Angular Material](https://material.angular.io/) 在 Angular 项目中实现 [ngx-formly](https://github.com/ngx-formly/ngx-formly) 。

在本文中，我们将继续同一个项目。我们将实现基本验证、自定义验证、可重复部分、条件字段、如何重置表单以及如何将表单提交给 API 端点。

坐下来，给自己弄点喝的，因为我们要带着 ngx-formly 在 Angular 里潜入高级形态的兔子洞。

```
**Part 1** 1\. [What’s Ngx Formly?](https://medium.com/@devbyrayray/b7a00733e66e#3d0d)
2\. [Install Dependencies](https://medium.com/@devbyrayray/b7a00733e66e#8175)
3\. [Set Up an Angular Project](https://medium.com/@devbyrayray/b7a00733e66e#0e6c)
4\. [Add Ngx Formly and Your UI Framework](https://medium.com/@devbyrayray/b7a00733e66e#8521)
5\. [Create a Simple Form](https://medium.com/@devbyrayray/b7a00733e66e#1778)
6\. [Check the Data From the Form](https://medium.com/@devbyrayray/b7a00733e66e#ded9)**Part 2**
**1\.** [**Validation**](https://medium.com/@devbyrayray/77aeed406f73) **2\.** [**Repeating Sections**](https://medium.com/@devbyrayray/77aeed406f73) **3\.** [**Conditional Fields**](https://medium.com/@devbyrayray/77aeed406f73) **4\.** [**Reset Form**](https://medium.com/@devbyrayray/77aeed406f73) **5\.** [**Submit Form**](https://medium.com/@devbyrayray/77aeed406f73)
```

# 1.确认

先说验证。每个表单都应该有一些验证来帮助用户填写正确的信息。

添加基本验证非常简单，只需向对象添加一些属性。如果您想要自定义验证，您只需创建两个函数并将它们添加到 formly 模块中。

## 1.1 基本验证

您可以通过向输入对象添加属性来添加一些验证。

*   `required`:如果是必填字段。
*   `min`:仅用于数字输入栏定义一个最小数字。
*   `max`:这只是为数字输入域定义一个最大数字。
*   `minlength`:通过该属性，您可以定义最小字符数。
*   `maxlength`:该属性可以定义最大字符数。

在每个输入对象中，您可以添加这些属性。如果不添加该字段，将不会对其进行验证。

如果您想要所需属性的验证消息，您必须将其添加到`angular.module.ts`中。

使用参数`field`，您可以访问已经填充到验证属性中的值。这将有助于使它更有活力。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/d8a1f15ffd82f3505a06fe947f643e588fc0a276)

## 1.2 自定义验证

如果您想要对您的字段进行自定义验证，您需要添加两个函数。一个函数(`IpValidator`)进行验证，另一个函数(`IpValidatorMessage`)处理验证消息。这些功能需要包含在`FormlyModule`中。

在`FormModule.forRoot()`中，你给出一个配置对象。对于验证函数，您添加了属性`validators`，其中添加了一个带有一个对象(`{ name: 'ip', validation: IpValidator }`)的数组。要添加验证消息，您需要添加属性`validationMessages`，在其中添加一个带有对象(`{ name: 'ip', message: IpValidatorMessage }`)的数组，如下所示。

你的`app.module.ts`应该是这样的:

在您想要进行自定义验证的字段的字段配置中，您必须添加属性`validators`，该属性包含一个值为`["ip"]`的`validation`属性。该值与您在`app.module.ts`中添加的值相同。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/0571ff48b5e396f9622d14ab7ff8269de9b83c92)

# 2.添加可重复的部分

不是所有的应用程序都需要这样。但是当你需要的时候，这个功能就变得非常好用了。

您可以对字段数组进行分组，使其可重复。但是首先，我们必须创建一个新的 formly 类型(`RepeatTypeComponent`)，它包括一个`<formly-field>`和一个按钮，用于创建一组新的输入类型。

我们必须将这个表单类型添加到`declaration`属性和`FormlyModule`中，这样我们就可以在表单配置中使用它。

现在我们可以将它添加到表单配置数组中。

如果您在`<pre>{{model | json }}</pre>`中输出您的模型，点击+按钮并填写输入字段。现在，您将看到一个包含所有信息的数组。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/100723445ab5308651e9dde890f7fd02efa13039)

# 3.添加条件字段

如果你正在用 Angular 构建一个大的应用程序，你可能需要条件字段。有了 formly，我们可以非常简单地做到这一点。

您只需向您的字段配置添加一个`hideExpression`属性。该值将引用模型。如果之前的输入名称为空，则`email`字段将被隐藏。当它包含一个值时，它将被显示。

最棒的是,`email`属性在模型中也是不可见的，所以您不会发送它。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/575b4febae9758ace7894ba657edfbaeab048822)

# **4。重置表单**

使用 formly，添加一个重置按钮来清除表单中的所有值也非常容易。

只需添加按钮，在类型为`FormlyFormOptions`的组件类中创建一个`options`属性，并将按钮添加到表单中。然后将`[options]=”options”`添加到表单组件，将`(click)=”options.resetModel()”`添加到按钮。

重要的是要知道，如果您的模型已经填充了值，该功能不会删除这些信息。当您更改该值或添加新值时，它将重置这些值。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/10852832404b515b216ac4fceb9374c2b6335eb4)

# **5。提交表格**

使用 formly 提交我们的表单也很容易。

向您的`<form>`标签添加一个`(ngSubmit)=“submit()”`，它将调用一个方法来提交应该在该组件中声明的内容。

```
onSubmit() {
  if (this.form.valid) {
    console.log(JSON.stringify(this.model));
  }
}
```

现在，这将在控制台中显示模型数据。但是我猜想如果你正在使用 Angular，你应该熟悉如何在其中发送请求。如果不是，请用这个例子。

确保您已经在您的`app.module.ts`中加载了 [HttpClientModule](https://angular.io/api/common/http/HttpClientModule) 。

在您的组件中，确保您在构造函数中包含了`HttpClient`，这将确保您可以在您的组件中使用这个`.http`。

```
constructor(private http: HttpClient) { }
```

在`submit`方法中，我们将使用`HttpClient`来执行 post 请求。

我们知道这个 URL 是无效的，所以它会导致一个错误。但是如果你用一个有效的替换它，它就能工作。

更好的做法是通过场外的角度服务来请求，但我会把这留给你。

在 GitHub 上检查示例项目中的[提交。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example/tree/15e0f3800da0825c36585d8da37e2c1dcb548a7b)

# **结论**

我希望本教程的第二部分能够帮助您用 ngx-formly 构建更高级的表单。如果你还有关于如何做某些事情的问题，请添加到评论中。

你还不确定 ngx-formly 对企业级应用是否有效吗？我可以向您保证，我和我的团队自己使用它来构建具有多个输入字段的所有表单。

我们甚至在基于开放 API 规范 3.0 的 API 中让它变得如此通用。对于每个模型，它都会自动构建表单。

如果你需要帮助设置你的表单，请查看 GitHub 上的[示例报告。](https://github.com/raymonschouwenaar/angular-ngx-formly-material-example)

# 谢谢！

![](img/2bd571bb1ce9f466ae279fa98111b086.png)

读完这个故事后，我希望你学到了一些新的东西，或者受到启发去创造一些新的东西！🤗

如果我给你留下了问题或一些要说的话作为回应，向下滚动并给我键入一条消息。如果你想保密，请在 Twitter @DevByRayRay 上给我发一条 [DM。我的 DM 永远是开放的😁](https://twitter.com/@devbyrayray)

[**通过电子邮件获取我的文章点击这里**](https://byrayray.medium.com/subscribe) **|** [**购买 5 美元中等会员**](https://byrayray.medium.com/membership)

# 阅读更多

![RayRay](img/992af170033696163d6cc0269218aedd.png)

雷雷

## 荒诞的故事

[View list](https://byrayray.medium.com/list/angular-stories-24674407532a?source=post_page-----77aeed406f73--------------------------------)6 stories![](img/b94f2b7d2929c90566cd2dd6f657a751.png)![](img/02b73423a62d73b113af9fdf9629c79f.png)![Stacked books](img/b02a2f57c0093e04ab1d11d3a55f35ea.png)![RayRay](img/992af170033696163d6cc0269218aedd.png)

[雷雷](https://byrayray.medium.com/?source=post_page-----77aeed406f73--------------------------------)

## 最新的 JavaScript 和 TypeScript 故事

[View list](https://byrayray.medium.com/list/latest-javascript-typescript-stories-0358ad941491?source=post_page-----77aeed406f73--------------------------------)14 stories![](img/c93ca03b33796c40dcc47873de2697c2.png)![](img/86f37efa11855f6f0f0f62984c37f696.png)![](img/ddbaa6d0bea676316247e82043d60b63.png)