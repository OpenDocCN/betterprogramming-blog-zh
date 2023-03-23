# 如何用 JavaScript 创建随机字符串

> 原文：<https://betterprogramming.pub/how-to-create-a-random-string-with-javascript-9f130ae2227e>

## 方便测试和生成测试数据

![](img/843761274ad2582c2fa5475e5bc9f6f7.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Tai Bui](https://unsplash.com/@agforlclassic?utm_source=medium&utm_medium=referral) 拍摄的照片

出于许多目的，您可能需要一个随机字符串。有时长，有时短。很多时候，我在单元或端到端测试中使用这些功能来实现更多的自动化。

在这篇文章中，我将向你展示如何自己创造它；我会解释函数的每一部分来扩展它。

享受旅程！

# 得到一个随机数

在 JavaScript 中，我们有一个包含各种数学常数和函数的数学对象。当你想创造随机的东西时，你需要`Math.random()`。这将返回一个随机浮点数。(*在你的主机*上试试吧)。这个函数的结果总是大于 0 小于 1。

```
Math.random()
```

但是因为我们想要生成一个短的随机字符串，所以我们首先不需要一个数字。将结果`Math.random() * 10`相乘，你将得到一个介于 1 和 10 之间的数。我们的结果可能是类似于`5.698829761336681`的东西。

```
Math.random() * 10 // returns 5.698829761336681
```

那么我们如何把它变成一个字符串呢？很简单，如果你把`.toString(36)`放在后面，你会得到一个由数字和字母组成的字符串，中间有一个点。使用数字 36 作为`.toString()`方法中的参数，对字符串应用 base 36 编码。

```
(Math.random() * 10).toString(36); // returns '9.ja773x85wr'
```

每次运行这段代码，都会有所不同。如果你想删除点，然后像这样替换它。

```
(Math.random() * 10).toString(36).replace('.', ''); 
// returns '1cq54mxwg9hl'
```

现在您已经在每次运行时生成了一个随机字符串，您可以将它转换成一个函数。

```
function randomString() {
    return (Math.random() * 1000000).toString(36).replace('.', '');
}
```

你想要一个更长的字符串吗？你可以创建一个循环，再把它变成一个字符串。

```
function randomString() {
    return [...Array(5)].map((value) => (Math.random() * 1000000).toString(36).replace('.', '')).join('');
}// returns '8mtmvtuzzfnau0bf0ecy668tzrc3ztc57a7b87vehyu51yb8gj35t7'
```

我建议尝试一下这个函数，并把它用在一些实际的事情上。

下一篇文章将是另一个 JavaScript 练习。

# 谢谢！

![](img/2bd571bb1ce9f466ae279fa98111b086.png)

读完这个故事后，我希望你学到了一些新的东西，或者受到启发去创造一些新的东西！🤗

如果我给你留下了问题或一些要说的话作为回应，向下滚动并给我键入一条消息。如果你想保密，请在 Twitter @DevByRayRay 上给我发一条 [DM。我的 DM 永远是开放的😁](https://twitter.com/@devbyrayray)

[**通过电子邮件获取我的文章点击这里**](https://byrayray.medium.com/subscribe) **|** [**购买 5 美元中等会员资格**](https://byrayray.medium.com/membership)

# 阅读更多

![RayRay](img/992af170033696163d6cc0269218aedd.png)

[射线射线](https://byrayray.medium.com/?source=post_page-----9f130ae2227e--------------------------------)

## 最新的 JavaScript 和 TypeScript 故事

[View list](https://byrayray.medium.com/list/latest-javascript-typescript-stories-0358ad941491?source=post_page-----9f130ae2227e--------------------------------)14 stories![](img/c93ca03b33796c40dcc47873de2697c2.png)![](img/86f37efa11855f6f0f0f62984c37f696.png)![](img/ddbaa6d0bea676316247e82043d60b63.png)