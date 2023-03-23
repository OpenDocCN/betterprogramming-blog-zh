# 解决 JavaScript 数组和对象重复的两种方法

> 原文：<https://betterprogramming.pub/2-ways-to-resolve-duplication-in-javascript-arrays-and-objects-e377e1bdc5e1>

## 你知道如何处理重复吗？

![](img/256fbb50f48481068feb0860d1c23cf4.png)

照片由[Jaanus jagomgi](https://unsplash.com/@jaanus?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在本文中，我想向您展示消除重复数据是多么简单。有几个地方可能会因为某种原因出现重复数据。

没有什么比 JavaScript 中的重复数据更烦人的了。因此，让我们来实际演练一下如何与之对抗。

我强烈建议不要简单地复制和粘贴我的代码。当然可以——但是请按照我的解释去理解它是如何工作的。

# 1.具有重复字符串或数字的数组

```
const dupStrings = ['Angular', 'React', 'VueJS', 'Svelte', 'React']
const dupNumbers = [1,2,3,4,5,6,2,4,6,7,8,9,0]
```

第一个数组中有一些重复的字符串，另一个数组中有重复的数字。

为了去掉重复的数据，我们将使用`Set`。`Set`只接受唯一值。

> `Set`对象是值的集合。您可以按插入顺序循环访问集合中的元素。`Set` **中的一个值只能出现一次**；这在`Set`的收藏中是独一无二的。”— [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```
console.log(new Set(dupStrings))// Set(5) {"Angular", "React", "VueJS", "Svelte", ""}console.log(new Set(dupNumbers))// Set(10) {1, 2, 3, 4, 5, 6, 7, 8, 9, 0}
```

当我们将数组放入`Set`并将其输出到控制台时，您会看到重复的值被从数组中移除。

在下面的 CodeSandbox 中，你可以看到它是如何工作的。

查看[代码沙箱](https://codesandbox.io/s/array-with-duplicated-strings-ornumbers-fu3d2)

但是现在，我们把它变成了一个`Set`，我们需要清理后的数组，这样我们就可以进一步施展我们的 JavaScript 魔法了。

最简单的方法是在数组括号`[...]`中使用`...` [展开语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)。让我们把它放到一个函数中，这样我们就可以在应用程序的任何地方重用它。

## 创建一个函数

现在我们需要从它创建一个函数，这样我们就可以在其他地方重用它。

## 该功能是如何工作的？

在我们的函数中，我们定义了一个参数`array`，我们期望它是一个数组。我们在`if`语句中检查这一点。

*   `!array`:我们简单的检查数组是`null`还是`undefined`
*   `!Array.isArray(array)`:我们使用 array helper 方法来检查`array`的值是否确实是一个数组。如果你把`!`放在它前面，我们检查输出是否是`false`。
*   `array.length === 0`:我们用它来检查数组的长度是否为`0` ——这表示一个空数组

如果满足其中一个条件，我们让函数返回函数中给定的数组。

如果条件不满足，它会把数组放到`new Set()`中，我们使用`...` spread 语法把它变成一个数组。

最后，我们需要调用函数。

```
unDuplicateArraySingleValues(array)
```

## 测试功能

检查下面的例子是否一切正常。

检查[代码沙箱](https://codesandbox.io/s/array-with-duplicated-strings-ornumbers-with-function-jwrj9)

# 2.具有重复对象的数组

让我们定义一个包含重复信息的示例对象数组

```
const dupObjects = [
   { id: 1, name: "a", value: "c" },
   { id: 2, name: "b", value: "d" },
   { id: 2, name: "c", value: "e" },
   { id: 3, name: "d", value: "c" }
];
```

您可以看到有几个重复的属性值。

我认为这听起来很明显，你不想在你的对象数组中有任何重复的信息。

## 创建一个函数

因此，我们需要一个函数来检查我们的数组，并根据给定的属性，检查是否有其他对象具有相同信息的属性。

此`function`的启动与上一示例相同。因此，为此制作一个新的`function`是个好主意。在这个例子中，我们保持不变。

## 该功能是如何工作的？

在我们的`function`中，我们定义了一个参数`array,`，我们期望它是一个数组。我们在`if`语句中检查这一点。

*   `!array`:我们简单的检查数组是`null`还是`undefined`
*   `!Array.isArray(array)`:我们使用 array helper 方法来检查`array`的值是否确实是一个数组。如果你把`!`放在它前面，我们检查输出是否是`false`。
*   `array.length === 0`:我们用这个来检查数组的长度是否为`0` ——这表示一个空数组。
*   `!property`:通过这个，我们检查属性值是否为`false`——这意味着我们检查`null`、`undefined`或`0`。因为如果是这种情况，我们就不能检查对象中的重复信息。

然后我们创建一个变量`objectArrayKeys,`，在这个变量中我们存储了在`propertyName`参数中给定的属性值数组。

在`uniqueKeys`中，我们删除重复的值。但是我们利用了之前的函数。这个函数返回一个只有唯一值的新数组。

最后，我们基于`uniqueKeys`数组返回一个只包含唯一对象的数组。

最后，我们需要调用函数。

```
unDuplicateArrayObjects(dupObjects, "id")
```

## 测试功能

在下面的例子中，检查更改函数中的属性名，这样我们就可以验证一切工作正常。

查看[代码沙箱](https://codesandbox.io/s/array-with-duplicated-objects-with-function-44pdz)

# 结论

我希望您已经学会了如何消除单值数组或对象数组中的重复数据。

这个问题我个人已经 googled 了上千次了。这就是我为你写这篇教程的原因。

![](img/a72eb48c81bbe92d2070b77f9724eb94.png)

大家好，我是来自荷兰🇳🇱的 JavaScript 开发人员 **Ray** ，我喜欢分享我从 2009 年开始做开发人员以来所获得的知识。我写关于 JavaScript、TypeScript、Angular 和任何与开发人员生活相关的东西的故事。

你可以在 [Twitter](https://twitter.com/devbyrayray) 和 [Instagram](https://www.instagram.com/devbyrayray/) 或[上关注我，订阅我发布新故事时发送的简讯](https://buttondown.email/devbyrayray)。

*快乐编码🚀*

# 从我这里读更多

[](https://medium.com/better-programming/build-fast-json-powered-forms-on-angular-with-ngx-formly-b7a00733e66e) [## 使用 NGX Formly 在 Angular 上构建快速的、基于 JSON 的表单

### 表格可能是一场噩梦——让我们把它们变得更好

medium.com](https://medium.com/better-programming/build-fast-json-powered-forms-on-angular-with-ngx-formly-b7a00733e66e) [](https://medium.com/better-programming/you-dont-need-a-javascript-framework-df2a36c2dd0a) [## 你不需要 JavaScript 框架

### 有时反应，角，或 Vue.js 可能太多了

medium.com](https://medium.com/better-programming/you-dont-need-a-javascript-framework-df2a36c2dd0a) [](https://medium.com/better-programming/2-ways-to-resolve-duplication-in-javascript-arrays-and-objects-e377e1bdc5e1) [## 解决 JavaScript 数组和对象重复的两种方法

### 你知道如何处理重复吗？

medium.com](https://medium.com/better-programming/2-ways-to-resolve-duplication-in-javascript-arrays-and-objects-e377e1bdc5e1) [](https://medium.com/better-programming/7-steps-to-dockerize-your-angular-9-app-with-nginx-915f0f5acac) [## 使用 Nginx 对 Angular 9 应用程序进行分类的 7 个步骤

### 在 Docker 环境中设置 Angular 9 应用程序，并立即进行部署

medium.com](https://medium.com/better-programming/7-steps-to-dockerize-your-angular-9-app-with-nginx-915f0f5acac)