# 为什么要使用异步生成器？

> 原文：<https://betterprogramming.pub/why-would-you-use-async-generators-eabbd24c7ae6>

## 讨论与异步生成器相关的概念和策略

![](img/76845eed1c4065744fd6aa28a93cd6dc.png)

今年 6 月，我参加了凯尔·辛普森在阿姆斯特丹举办的研讨会“ [JavaScript:最近的部分](https://frontendmasters.com/courses/js-recent-parts/)”。材料和表演都很棒，凯尔是表演工作坊的真正大师！

我最兴奋的一个话题是`async generators`，它在活动接近尾声时被提及。观众参与进来，并被问了几个关于使用`async generators`技术可以实现的不同策略和迭代顺序的问题。为了更好地理解它，我决定写一篇关于这些模式的短文。我将解释一些与`generators`话题相关的概念，并详细描述这些策略。

有很多很好的文章和教程详细解释了[迭代器](https://codeburst.io/a-simple-guide-to-es6-iterators-in-javascript-with-examples-189d052c3d8e)。所以，为了更接近这篇文章的目标，我会简单地说一下。

# 迭代器

[迭代器](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_generators)简而言之就是任何实现[迭代器协议](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterator_protocol)的对象。基本上，`Iterator`对象必须包含`next`函数，并返回一个具有任意类型`value`和布尔`done`属性的对象。`next`方法定义了迭代的顺序以及每一步产生的值。

要迭代这样的对象，可以使用不同的技术:

许多内置对象，如数组、字符串、映射和集合，已经实现了可迭代接口。通常，为了与`generators`兼容，迭代器在`Symbol.iterator`属性上声明。

您可能会注意到，在到达结束状态后，after done 看起来是`true`。对迭代器方法的`next()`调用返回相同的结果- `{value: undefined, done: true}`。

生成器是一个函数，它简化了迭代器的声明。它可以**停止执行**和**返回多个值**。

一个生成器以`function*`操作符开始，它的主体就像一个普通的函数，扩展了一些特性。

# 异步迭代器

异步迭代器允许延迟包含异步逻辑的迭代过程。异步迭代器`next()`方法的每个结果返回一个`Promise`，解析成一个对象，包含`value`和`done`属性。

**异步函数**允许我们使用`await`语法并返回`Promise`。从逻辑上讲，**异步发电机**允许`await`并产生`Promises`。

> 那么，为什么有人会使用异步发电机呢？

好问题！`Async iterators`自带酷炫*循环构造:*

```
for await (const item of asyncIterator) {
  // ...
}
```

这就是原因！😉这意味着开发人员可以定义任何他或她喜欢的异步迭代顺序。例如，您可以迭代[节点读取流](https://nodejs.org/api/stream.html#stream_readable_symbol_asynciterator)。

```
const fs = require('fs')
const readStream = fs.createReadStream('./test.txt', { encoding: 'utf8' })async function print() { 
  for await (const chunk of readStream) {
    console.log(chunk)
  }
}

print()
```

不要忘记切换到更新版本的`Node`:

```
nvm use 12
Now using node v12.4.0 (npm v6.9.0)
```

请注意，`for-await`只能在`async`函数内部使用。从实现的角度来看，`async iterators`应该用`Symbol.asyncIterator`属性来声明。

# **异步策略**

假设您想要请求多个异步源:

```
async function* inOrderRequests(urls) {
  for (const url of urls) {
    const response = await fetch(url)
    const text = await response.text()
    yield text
  }
}
```

现在，可以在异步函数中迭代`urls`:

```
async function pageText(urls) {
  for await (const responseText of inOrderRequests(urls)) {
    console.log(responseText) 
  }
}pageText([location.href])
```

`pageText([location.href])`函数调用输出当前浏览器页面的初始文本。

在研讨会期间，Kyle 确定了可用于请求异步资源的三种不同策略:

*   `in order - in order`当一个请求在另一个请求的响应返回后执行。上面的`pageText`就是这种策略的一个例子。
*   `asap - in order`当请求被一起执行并且响应以初始顺序返回时。我们将用一个`asapInOrderRequests`函数来修改前面的例子。

现在，所有的请求都会立即执行。该函数等待每个请求解决，但是请求所花费的总时间受限于它最大的延迟`Promise`。

*   最后，当请求和结果都被执行并尽快返回时，应用`asap - asap`策略。

在研讨会上，[凯尔](https://me.getify.com/)展示了所有三种可能性，其中`asap - asap`是最难的，因为他正在即兴创作。我确实也发现后者最有趣

在这个例子中，JavaScript 闭包特性被用来表示一个已解决的`Promise`。每当它被满足，发电机`yields`的结果，所以整个比赛的`Promises`正在发生。完成这个练习花了我一些时间和努力。希望你会觉得有用。

我对你的反馈很感兴趣。请让我知道你对那段代码的看法。你会在你的项目中使用类似的结构吗？评论中再见🙂

并有一个不错的编码！