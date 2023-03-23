# 5 个 JavaScript WTFs

> 原文：<https://betterprogramming.pub/5-javascript-wtfs-62b6742ff3dd>

## JavaScript 的古怪之处会让开发人员紧张地大笑

![](img/635db975817741faa69e148b55d90a15.png)

作者照片。

我们都爱 JavaScript。这是一门伟大的语言。让它更受欢迎的是，它也有自己“有趣”的一面。今天，我将向您展示 JavaScript 中的五个 wtf，并解释其中的内容。

不，这些不是虫子。

# 1.香蕉

你会怎么写香蕉？在 JavaScript 中，这将完成:

```
'b' + 'a' + + 'a' + 'a' // -> baNaNa
```

## 它是如何工作的？

`'b' +'a' + + 'a' + 'a'`导致`baNaNa`，因为`'b' + 'a' + + 'a' + 'a'`读取`'b' + 'a' + (+'a') + 'a'`，而`(+'a')`被转换为*而不是数字*、`NaN`。这反过来会发生，因为在 JavaScript 中，二进制加法运算符`+`试图将操作数转换为数字，但`'a'`是*而不是数字*。

# 2.布尔数学

如果你在整天与数字打交道后感到沮丧，就换成布尔型吧。在 JavaScript 中，你真的可以用布尔来做数学运算。例如:

```
**true** + **true**; // -> 2(**true** + **true**) * (**true** + **true** + **true**) // -> 6
```

## 它是如何工作的？

在 JavaScript 中，您可以使用`Number`构造函数将*的值强制转换为数字。这将`true`转换为`1`并将`false`转换为`0`:*

```
Number(true);  // -> 1
Number(false); // -> 0
```

类似地，一元运算符`+`试图将任何操作数转换为数字。对于`true`和`false`也是如此。这意味着使用一元运算符`+`将`true`强制为`1`并将`false`强制为`0`:

```
+true;  // -> 1
+false; // -> 0
```

最后，当你做加法或乘法时，`ToNumber`方法也会被调用。根据[规格](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/#sec-tonumber):

> “如果`argument`是`true`，则返回`1`。如果`argument`是`false`，则返回`+0`。

JavaScript 中的布尔数学现在变得有意义了，不是吗？

# 3.NaN 实际上是一个数字

让我们问问 JavaScript 什么是*而不是数字*、`NaN`的意思:

```
typeof NaN; // -> 'number'
```

真的吗？

## 它是如何工作的？

`NaN` *听起来很神秘，实际上是一个数值类型。* `NaN`只是在数值型的限制内无法表示某个特定值时使用。从字面上看，它就像一个数字，而不是一个设计好的数字。

# 4.使用数组访问属性

我确信你已经用一个数字或一个字符串通过`[]`操作符访问过属性。但是你知道你也可以传入一个数组吗？例如:

```
var data = { number: 1 };
var array = ["number"];data[array]; // -> 1
```

## 它是如何工作的？

这是因为，在幕后，`[]`操作符对其参数运行`toString`方法。在 JavaScript 中，将一个*单元素数组*转换成 string 会产生一个字符串形式的单个元素。

例如:

```
["test"].toString() // -> test
```

# 5.整理

这个任务应该很简单:让我们使用`sort()`方法对一组数字进行排序:

```
[1, 5, 2, 10, 30].sort()
```

结果:

```
[1, 10, 2, 30, 5]
```

但是等一下 JavaScript 认为这些数字现在已经排序了吗？拿着我的啤酒。

## 它是如何工作的？

这不是 bug。使用默认的`sort()`方法实际上将数组的元素转换成*字符串*，然后比较它们的 *UTF-16 值*来对列表进行排序。因此，如果你真的想排序除字符串以外的任何东西，传递一个*比较函数*到`sort`方法中。

例如，要按升序对数字进行排序:

```
[1,5,2,10,30].sort((a, b) => a - b); // -> [1, 2, 5, 10, 30]
```

# 结论

感谢阅读。我希望你已经有了一些惊人的 WTF 时刻，也许也学到了一些东西！

我很想加入你的 LinkedIn 网络。随意连接 [Artturi Jalli](https://www.linkedin.com/in/artturi-jalli-29619413a) 。

# 资源

[](https://github.com/denysdovhan/wtfjs) [## 丹尼斯多夫汉/wtfjs

### 🤪有趣而棘手的 JavaScript 示例列表。通过创建帐户为 denysdovhan/wtfjs 的发展做出贡献…

github.com](https://github.com/denysdovhan/wtfjs) [](https://stackoverflow.com/questions/2801601/why-does-typeof-nan-return-number) [## 为什么 typeof NaN 返回' number '？

### 对 NaN 来说，一个更好的名字，更准确地描述它的意思，更少混淆，将是一个数字上的例外。它…

stackoverflow.com](https://stackoverflow.com/questions/2801601/why-does-typeof-nan-return-number)