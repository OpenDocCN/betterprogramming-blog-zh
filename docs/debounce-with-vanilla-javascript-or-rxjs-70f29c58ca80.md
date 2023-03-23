# 用普通 JavaScript 或 RxJS 去抖

> 原文：<https://betterprogramming.pub/debounce-with-vanilla-javascript-or-rxjs-70f29c58ca80>

## 去抖动去神秘化

![](img/c2994aae4c8eda0f226c26624c435ed5.png)

照片由[拜尔娜·巴蒂斯](https://unsplash.com/@barnabartis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

我最近不得不清理我们在 [DeckDeckGo](https://deckdeckgo.com) 中使用的代码。值得注意的是，我不得不将单例方法重构为无状态函数。其中一个给了我一段更艰难的时间，这就是我写这篇文章的想法！

# 什么是去抖？

假设您已经在应用程序中实现了一个`<input/>`,它会在每次数据库内容改变时触发数据库更新。出于性能原因，甚至可能出于成本原因(例如，如果您正在使用 [Google Firestore](https://cloud.google.com/firestore/pricing) )，您可能不希望每次按下键盘按键时都触发数据库更新，而是仅在需要时执行保存。例如，您可能希望仅当用户标记暂停时，或者当她/他已经完成了与组件的交互时，才执行保存。

同样，您的应用程序中可能有一个函数，它可能会被连续调用多次，您更愿意只考虑最后一次调用。

这就是去抖对我的意义——确保一个方法不会被调用得太频繁。

# 去抖时间

通常，为了检测哪些功能应该被有效地触发，观察调用之间的延迟。例如，如果我们对一个函数进行去抖，去抖时间为 300 毫秒，一旦观察到两次调用之间有 300 毫秒或更长时间，该函数就会被触发。

# 普通 Javascript

目前没有跨浏览器支持的标准“去抖动功能”的平台实现(当然，如果我错了，请纠正我！).幸运的是，Javascript 提供了使用`setTimeout`延迟函数调用和使用`clearTimeout`取消函数调用的能力。我们可以将这些结合起来实施我们自己的解决方案:

```
export function debounce(func: Function, timeout?: number) {
    let timer: number | undefined; return (...args: any[]) => {
        const next = () => func(...args); if (timer) {
            clearTimeout(timer);
        } timer = setTimeout(next, timeout > 0 ? timeout : 300);
    };
}
```

在上面的代码中，我们的函数(我们实际上想要执行的函数，作为参数`func`传递)将被延迟(`setTimeout`)。在这样做之前，我们首先检查它之前是否已经被调用过(使用`timer`引用之前的调用)。如果之前已经调用过，我们在有效延迟我们的目标之前取消之前的调用(`clearTimeout`)。

我们可以用一个简单的测试来验证这个实现。我们可以调用一个函数，将一个字符串连续多次记录到控制台。如果一切正常，输出应该只出现一次:

```
const myFunction: Function = debounce(() => {
  console.log('Triggered only once');
});myFunction(); // Cleared
myFunction(); // Cleared
myFunction(); // Cleared
myFunction(); // Cleared
myFunction(); // Performed and will output: Triggered only once
```

如果你想观察和测试这一点，试试这个[笔](https://codepen.io/peterpeterparker/pen/WNegLNb)。

# RxJS

上述使用普通 Javascript 的解决方案很酷，但是使用[RxJS](https://rxjs-dev.firebaseapp.com)(Javascript 的反应式扩展库)达到同样的结果怎么样呢？那会很巧妙，不是吗？幸运的是，RxJS 提供了一个开箱即用的解决方案，使用可观测量轻松实现去抖功能。此外，这个解决方案更简洁，可读性更好。

我们要用的 RxJS 函数是`[debounceTime](https://rxjs-dev.firebaseapp.com/api/operators/debounceTime)`。如文档中所解释的，它延迟由源可观测值发出的值，但是如果新的值到达源可观测值，则丢弃先前未决的延迟发射。为了重现上面的例子并创建一个可观察的，我们可以使用一个`Subject`并连续多次触发`next()`。如果一切都按计划进行，我们应该在控制台中只看到一个输出。

```
const mySubject: Subject<void> = new Subject();subject.pipe(debounceTime(300)).subscribe(() => {
  console.log('Triggered only once');
});mySubject.next(); // Cleared
mySubject.next(); // Cleared
mySubject.next(); // Cleared
mySubject.next(); // Cleared
mySubject.next(); // Performed and will output: Triggered only once
```

就是这样。不用写自定义函数，RxJS 只是为我们解决了去抖。

如果你也想在行动中尝试一下，看看另一个 [Codepen](https://codepen.io/peterpeterparker/pen/ZEzqXPw) 。

*注意:在上面的例子中，为了简单起见，我没有考虑取消订阅可观察对象。显然，如果您想在实际应用中使用这种解决方案，请小心谨慎。*

# 蛋糕上的樱桃🍒🎂

在我们的开源项目 [DeckDeckGo](https://deckdeckgo.com) 中，我们在我们的应用程序和组件中使用了一个名为`deckdeckgo/utils`的小 utils 包(发布给 [npm](https://www.npmjs.com/package/@deckdeckgo/utils) )，它提供了各种实用程序，其中一个是普通的 Javascript `debounce`函数。因此，如果您需要一个快速和肮脏的解决方案，请成为我们的客人，并尝试一下！

 [## deckgo/deckdeckgo

### 在 DeckDeckGo 的应用程序和组件中开发和使用的 utils 方法和函数的集合。如果你愿意…

github.com](https://github.com/deckgo/deckdeckgo/tree/master/webcomponents/utils) 

到无限和更远🚀

大卫