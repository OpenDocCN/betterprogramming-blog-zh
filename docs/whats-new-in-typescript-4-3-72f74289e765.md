# TypeScript 4.3 有什么新功能？

> 原文：<https://betterprogramming.pub/whats-new-in-typescript-4-3-72f74289e765>

## TypeScript 的惊人增量升级

![](img/fa079f3cc87ef974fa7ac54de952690d.png)

凯文·Ku 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片。

TypeScript 4.3 于 5 月 26 日发布🎉。发布的顶级功能有哪些？它们会提高我们的生产率吗？是否应该立即更新？

在这篇文章中，我将经历所有最令人兴奋的。以下是总结:

*   `import`报表助手
*   属性上的独立写入类型
*   重写方法
*   模板字符串类型改进
*   ECMAScript `#private`类元素
*   条件句中缺少`await`的错误
*   静态索引签名
*   改进了对知名符号的支持

# 导入报表助手

这个版本中最酷的特性之一实际上将会内置到您的编辑器中:支持`import`语句。

现在，当使用`import`语法时，TypeScript 将为您提供一个导入候选项的列表。当您选择一个时，`import`语句将自动为您生成。

让我们看一个例子:

![](img/70310a38670546ca201d3444f302d2a0.png)

导入自动完成一直是个难题。问题是在 ES 导入中，导入的实体在导入位置之前。如果导入语法颠倒过来，对开发人员来说会更容易:

```
// an example of a more intuitive import statementfrom 'react' import { useState }
```

有一些`VS Code`插件对此有所帮助，但没有什么像内置功能一样。

# 属性上的独立写入类型

如果您经常使用 TypeScript 的公共访问器，您很可能会看到这个错误:`'get' and 'set' accessors must have the same type`。当`get`和`set`公共访问器没有完全相同的类型时，就会发生这种情况。

让我们用以前的版本来看一个例子:

升级到 TypeScript 4.3 后，此代码无需任何更改即可运行。

我们可以更好地控制如何定义我们的 getters 和 setters 以及它们的签名方法，这总是很棒的。

# 重写方法

在这个版本中，增加了一个众所周知的关于 OOP 的关键字:`override`。以前，显式重写父方法是不可能的。它必须以一种含蓄的方式完成:

看上面的例子，不清楚这是开发人员有意为之的行为还是一个错误。例如，如果我们从我们的`Foo`对象中移除`bar`方法会发生什么？没什么。代码仍然有效。这是开发商的意图吗？看编译器很难知道。

在 4.3 中，增加了一个新的旗帜:`--noImplicitOverride`。这将使上面的代码失败，并出现以下错误:

```
This member must have an 'override' modifier because it overrides a member in the base class 'Foo'
```

这将防止任何隐式覆盖。你可以向编译器暗示你的编码意图。

启用标志`--noImplicitOverride`意味着创建覆盖方法的唯一方式是使用新添加的`override`关键字。

让我们看一个例子:

现在，如果我们从`Foo`类中移除`bar`方法，我们将面临以下错误:

```
This member cannot have an 'override' modifier because it is not declared in the base class 'Foo'
```

这很有用，因为感觉上我们更能控制方向盘。

# 模板字符串类型改进

模板文字类型是在 4.1 中引入的。从那以后，每个 TypeScript 版本都包含了一些改进。

当使用与 string 组合的模板类型时，TypeScript 现在将尝试将这些类型推断为模板类型。

让我们看一个例子:

这同样适用于类型推断。让我们比较一下以前版本和新版本的输出:

由于 TypeScript 试图将这些字符串推断为模板类型，所以一切正常。TypeScript 现在可以更好地理解模板类型，所以我们可以这样做:

# 改进的#private 类访问器

从 3.8 版本开始，`private`访问器就是 TypeScript 中的一个特性。

概括一下:

> "默认情况下，类属性是公共的，可以在类外检查或修改。然而[有一个阶段 3 提议](https://github.com/tc39/proposal-private-methods)允许使用散列前缀`#`定义私有类字段。
> 
> …
> 
> 私有字段可以在类构造函数上从类声明本身内部访问。”— [MDN 网络文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)

然而，直到现在，有些事情你还不能做:

*   用`private`修饰符声明一个方法。
*   用`private`修饰符声明一个 getter。
*   用`private`修饰符声明一个静态属性。

有了这个新版本，以上所有都成为可能。这个特性现在几乎完全符合 ECMAScript `private`修饰符的定义。

# 条件中缺少 await 的错误

从现在开始，当启用`strictNullChecks`配置时，TypeScript 在断言一个承诺时会给出一个错误。

那个条件总是`true`，所以编译器要求你改变`if`语句。

# 静态索引签名

索引签名是我们习惯直观使用的东西。它以隐式而不是显式的方式声明属性。

只要符合索引签名，就可以声明显式变量。

既然我们已经了解了什么是索引签名，那么在这个新版本中有什么变化呢？变化其实很简单:`static`值现在支持索引签名。

在 4.3 之前，将显示消息`'static' modifier cannot appear on an index signature.`。

# 改进了对知名符号的支持

添加了对众所周知的 TypeScript 符号的支持。那些是什么？内置在 TypeScript 中的`Symbol.*`。

# 最后的想法

这是另一个很好的版本，包含了许多小的、有用的东西。很高兴看到模板文字功能越来越成熟。

展望未来，我很期待看到`Generalized index signatures`是否能进入下一个版本。他们已经计划了几个版本，但从未实现。这是什么特点？简而言之，它是在一个类型上定义多个类型键的能力:

```
interface PropertyMap {
  [x: string | number | symbol]: string;
}
```

目前，我们得到以下错误消息:

```
An index signature parameter type must be either 'string' or 'number'
```

可以在 GitHub 上跟进 PR [。](https://github.com/microsoft/TypeScript/pull/26797)

# **相关文章**

[](/top-5-typescript-features-you-should-master-2358db9ab3d5) [## 您应该掌握的五大打字稿功能

### 使用这些必须知道的特性来提高您的打字稿知识

better 编程. pub](/top-5-typescript-features-you-should-master-2358db9ab3d5) [](/typescript-a-gentle-introduction-to-mapped-types-f65e45fa2598) [## TypeScript:映射类型的简明介绍

### 学习构建自己的一套 TypeScript 工具

better 编程. pub](/typescript-a-gentle-introduction-to-mapped-types-f65e45fa2598)