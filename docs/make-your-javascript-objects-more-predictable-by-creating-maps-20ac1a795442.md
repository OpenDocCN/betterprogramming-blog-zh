# 将地图中的 JavaScript 对象转变为可预测性

> 原文：<https://betterprogramming.pub/make-your-javascript-objects-more-predictable-by-creating-maps-20ac1a795442>

## 不再有未定义属性的错误

![](img/4f2bf5ebae0fdbc2eef08feb22d8e712.png)

美国宇航局在 [Unsplash](https://unsplash.com/s/photos/earth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 物体的问题

您正在深入研究您的代码，突然您得到一个错误:“无法读取未定义的属性 *x* ”啊，老兄。

大多数时候，这意味着调试——用这里的`debugger`，那里的`console.log()`攻击我的代码。大多数时候，我很快就修好了。但是有些日子我会花上几个小时？

# 示例问题

我们有一个物体叫做`car`。

.

汽车对象有一组键值对。我们从我们的 API 中获得这些信息。通常，您会在包含属性`last_updated` 的对象中获得一个`events`属性——声明该信息最后一次被更改的时间。

运行这段代码时，我们得到一个错误。

```
Uncaught TypeError: Cannot read property 'last_updated' of undefined at <anonymous>:1:14
```

为了防止错误，我们必须检查属性`events`是否在对象中。

这样，如果`event`属性不在我们的对象中，我们就不会有错误。但是我发现这段代码可读性不是很好。

# 地图解决方案

当我们把我们的对象变成一个`Map()`时，我们从映射中得到所有的明确方法。

您可以使用`Object.entries()`方法将您的对象转换成地图，并将您的对象作为参数提供给它。

这将是地图的输出。

一个`Map()`有一系列方便的方法，我们可以用它们来使我们的代码更可读，更可预测，并且出错的机会更少。

*   `set('key', 'value')`存储一个键值对
*   `get('key')`返回键值(如果有的话)
*   `has('key')`根据密钥是否存在返回`true`或`false`
*   `delete('key')`从映射中删除一个键值对
*   `clear()`从映射中删除所有的键值对
*   `size()`从映射中返回键值对的数量

因此，如果我们想从属性`events`中获得信息`last_updated`，我们使用`.has('prop')`方法。此方法基于属性的存在返回一个布尔值。

在您的控制台中，您将看到控制台记录的`‘not available’`没有任何错误。

> 正如其他一些有帮助的开发者在评论中显示的那样。对于让你的对象更加可预测的每个问题来说，地图可能不是最好的解决方案。
> 
> 事实证明，[反射 API](https://ponyfoo.com/articles/es6-reflection-in-depth) ，[可选链接](https://v8.dev/features/optional-chaining)或者更好的是，[类型脚本](http://www.typescriptlang.org/)有助于编写更可预测的 JavaScript。
> 
> 不过，我认为这可能是一个有趣的解决方案😉

# 结论

我认为使用`Map()`可以让你的代码更具可读性、可预测性，并且对错误不那么敏感。使用正常的`Object`，当某个属性丢失时，您的代码会给出错误。这样会导致不好的用户体验，我们来预防一下。

如果您想查看更多关于`Map()`的详细信息，请查看 MDN web 文档的[详细页面。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

我希望你从这篇文章中学到了一些新东西。编码快乐！

# 阅读更多

[](https://medium.com/dev-together/frontend-development-on-a-budget-raspberry-pi-4-4c917124d348) [## 预算上的前端开发:树莓 Pi 4

medium.com](https://medium.com/dev-together/frontend-development-on-a-budget-raspberry-pi-4-4c917124d348) [](https://medium.com/dev-together/5-tips-for-healthier-developers-ea21d1e826f) [## 给健康开发者的 5 个建议

### 你的身体和大脑需要关注

medium.com](https://medium.com/dev-together/5-tips-for-healthier-developers-ea21d1e826f) [](https://medium.com/dev-together/what-tech-i-use-in-2020-as-developer-f3269f857f0e) [## 作为开发者，我将在 2020 年使用什么技术

### 我在工作中使用的软件、硬件和其他东西

medium.com](https://medium.com/dev-together/what-tech-i-use-in-2020-as-developer-f3269f857f0e) [](https://medium.com/dev-together/javascript-concepts-you-need-before-starting-w-frameworks-libraries-25a325312b5c) [## 开始使用框架和库之前需要的 JavaScript 概念

### 在你熟悉它们之前不要开始

medium.com](https://medium.com/dev-together/javascript-concepts-you-need-before-starting-w-frameworks-libraries-25a325312b5c)