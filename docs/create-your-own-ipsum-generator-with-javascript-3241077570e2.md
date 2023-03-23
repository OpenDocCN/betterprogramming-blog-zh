# 用 JavaScript 创建自己的 Ipsum 生成器

> 原文：<https://betterprogramming.pub/create-your-own-ipsum-generator-with-javascript-3241077570e2>

## 尽可能多的免费虚假内容

![](img/7e6d0582509c8bf9980bc613d78f2f95.png)

由[格伦·凯莉](https://unsplash.com/@glencarrie?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

用随机单词生成内容可以方便测试。这就是为什么今天，我们将创建一个这样的函数。

> 使用这段代码构建您自己的 Ipsum 生成器，这样您就可以随意生成内容了👍

如果你愿意的话，你可以使用一个库，但是因为这个库非常简单，并且可以用很少的代码来构建，所以你最好自己做一个。

为了用随机单词生成内容，我们需要三个函数和一个短语源。

1.  给我们一个随机数的函数
2.  一个为我们提供随机单词的函数
3.  将多个单词放入一个字符串的函数
4.  单词的源将是一个定义好的单词数组。([从我的要点](https://gist.github.com/devbyray/8dbac8a32c7c87f659d9b34137e25ba0)中获取)

# 1.生成随机数

因为我们想从我们的源中获得一个随机的单词，所以我们需要生成一个随机的索引号。使用这个函数，我们需要记住数组的最小值和最大值。

```
Math.random();
// Returns 0.534098468876492
```

使用`Math.random()`，我们得到一个介于 0 和小于 1 之间的随机浮点数。例如，当我们把它乘以 10，我们会得到一个介于 0 和小于 10 之间的数。但在这种情况下，我们希望有一个不小于 0 且小于或等于 10 的数字。

```
Math.random() * (10 - 0) + 0;
// Returns 8.448742196214798
```

但是现在，它仍然漂浮着。所以我们要用`Math.round`来获取一个整数。

```
Math.round(Math.random() * (10 - 0) + 0)
// Returns 6 or 5 or 9 or 10
```

通过这种计算，我们可以得到一个介于 0 和 10 之间或等于 10 的数。您可以快速测试它是否如您所愿。

```
let number = 0;
let steps = 0;
while(number != 10) {
    number = Math.round(Math.random() * (10 - 0) + 0);
    steps = steps + 1;
    console.log('steps', steps)
}
```

使用这段代码，您将运行一个循环，直到它变成 10。通过记录这些步骤，你可以知道它需要多少轮。每次你运行这个，你会知道它需要不同数量的回合。

```
function randomNumber(min, max) {
    return Math.round(Math.random() * (max - min) + min);
}
```

这是获取两个数字之间的随机数的最后一个函数。让我们继续从我们的源 Array 中得到一个随机词。

# 2.随便找个词

我发现了一个不错的随机单词集，可以用来写这篇文章。你可以在要点上找到它。但是在这篇文章中，我将使用一个更短的版本。

> 也可以在某个主题里定义自己的词。例如[开发商 Ipsum](https://developer-ipsum.netlify.app/) 或[纸杯蛋糕 Ipsum](http://www.cupcakeipsum.com/)

```
const word = [
    "Got",
    "ability",
    "shop",
    "recall",
    "fruit",
    "easy",
    "dirty",
    "giant",
    "shaking",
    "ground",
    "weather",
    "lesson",
    "almost",
    "square",
    "forward",
    "bend",
    "cold",
    "broken",
    "distant",
    "adjective."
]
```

我们需要使用我们在上一步中创建的`randomNumber`函数来获得一个随机单词。在这个函数中，我们需要给出一个最小值和一个最大值作为参数；通过这个我们可以很容易做到。

```
const word = words[randomNumber(0, words.length - 1)];
```

第一个参数是 0，因为数组从 0 开始。第二个参数是我们的最大值，所以我们选择了`words.length - 1`。我们这样做是因为，在这个例子中，我们的数组中有 20 个单词，因此 length 属性将返回这些单词。但是由于数组是从 0 开始计数的，所以数组中的最新位置是 19。这就是我们做`- 1`的原因。

```
function getRandomWord() {
    return words[randomNumber(0, words.length - 1)];
}
```

我们的第二个函数是从数组中获取一个随机单词。

# 3.获取一个包含随机单词的字符串

现在我们想得到多个单词并把它变成一个字符串，这样我们就可以把它作为我们想要的内容。我们能做的最好的事情就是生成一个有几个位置的数组。

```
[...Array(10)]
// Returns [undefined, undefined, ....] with 10 items
```

使用`.map`方法，我们可以迭代并用随机单词填充它。

```
[...Array(10)].map(() => getRandomWord())
// Returns ["hand", "close", "ship", "possibly", "metal", "myself", "everybody", "serious", "adult", "favorite"]
```

现在我们有了一个随机单词的数组，我们需要使用`.join('')`将它转换成一个字符串。

```
[...Array(10)].map(() => getRandomWord()).join('')
```

但是，我们想给字符串一些可读性的“感觉”。因此，数组中的每个第一个单词的首字母都应该大写。让我们更新一下我们的`getRandomWord`函数。

```
function getRandomWord(firstLetterToUppercase = false) {
    const word = words[randomNumber(0, words.length - 1)];
    return firstLetterToUppercase ? word.charAt(0).toUpperCase() + word.slice(1) : word;
}
```

当我们创建一个函数来生成一个函数来获取生成的字符串时，它看起来像这样。利用`getRandomWord(i === 0)`中的比较，我们通过函数给出一个布尔值。

```
function generateWords(length = 10) {
    return [...Array(length)].map((_, i) => getRandomWord(i === 0)).join(' ').trim() + '.';
}
```

# 4.完成

现在我们已经创建了所有的代码，是时候检查完整的代码示例了。

```
const word = [
    "Got",
    "ability",
    "shop",
    "recall",
    "fruit",
    "easy",
    "dirty",
    "giant",
    "shaking",
    "ground",
    "weather",
    "lesson",
    "almost",
    "square",
    "forward",
    "bend",
    "cold",
    "broken",
    "distant",
    "adjective."
]function getRandomWord(firstLetterToUppercase = false) {
    const word = words[randomNumber(0, words.length - 1)];
    return firstLetterToUppercase ? word.charAt(0).toUpperCase() + word.slice(1) : word;
}function generateWords(length = 10) {
    return [...Array(length)].map((_, i) => getRandomWord(i === 0)).join(' ').trim() + '.';
}function randomNumber(min, max) {
    return Math.round(Math.random() * (max - min) + min);
}
```

尝试[这个 runkit 示例](https://runkit.com/devbyrayray/how-to-generate-a-string-with-random-words)中的功能:

感谢阅读。

# 谢谢！

![](img/2bd571bb1ce9f466ae279fa98111b086.png)

读完这个故事后，我希望你学到了一些新的东西，或者受到启发去创造一些新的东西！🤗

如果我给你留下了问题或一些要说的话作为回应，向下滚动并给我键入一条消息。如果你想保密，请在 Twitter @DevByRayRay 上给我发一条 [DM。我的 DM 永远是开放的😁](https://twitter.com/@devbyrayray)

[**通过电子邮件获取我的文章点击这里**](https://byrayray.medium.com/subscribe) **|** [**购买 5 美元中等会员**](https://byrayray.medium.com/membership)