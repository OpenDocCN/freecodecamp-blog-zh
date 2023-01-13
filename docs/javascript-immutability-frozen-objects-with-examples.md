# JavaScript 不变性——用例子解释 JS 中的冻结对象

> 原文：<https://www.freecodecamp.org/news/javascript-immutability-frozen-objects-with-examples/>

在 JavaScript 中，我们使用一个`Object`将多个值存储为一个复杂的数据结构。你用一对花括号`{}`创建一个对象。

一个对象可以有一个或多个属性。每个属性都是由`colon(:)`分隔的键值对。密钥必须是字符串或 JavaScript 符号类型。该值可以是任何类型，包括另一个对象。

有了对对象解释，让我们创建一个对象来看看它是如何工作的:

```
const user = {
 'name': 'Bob',
 'age': 27   
}
```

这里，我们创建了一个具有两个属性(姓名、年龄)及其各自值的对象。我们用关键字`const`创建了一个名为`user`的变量，并将对象作为值赋给它。

默认情况下，对象是`mutable`。这意味着一旦创建了它们，您就可以向它们添加新属性、修改现有属性的值或删除属性。

在我早年的编程生涯中，我发现术语`mutable`和`immutable`非常令人困惑。让我试着用简单的英语解释一下。

可变是你可以改变的。不可变正好相反。所以，`mutability`是随时间变化的能力。`Immutability`表示某物不随时间变化。

有些情况下，您可能不希望以编程方式更改对象。因此你会想让它成为不可变的。

当一个对象是不可变的，你不能添加一个新的属性，修改它，或者删除一个现有的属性。连延长都没有办法。

这就是 a `Frozen Object`是什么，我们将在本文中学习、练习和理解它。

我最近在一篇 Twitter 帖子中讨论了冻结的对象。请随便看看。本文将用更多的细节和例子来展开。

> 在 JavaScript 中使用冻结对象吗？它有一些实际用途。
> 
> 一根线
> 🧵👇[# dev community](https://twitter.com/hashtag/DEVCommunity?src=hash&ref_src=twsrc%5Etfw)[# 100 days ofcode](https://twitter.com/hashtag/100DaysOfCode?src=hash&ref_src=twsrc%5Etfw)[# dev community in](https://twitter.com/hashtag/DEVCommunityIN?src=hash&ref_src=twsrc%5Etfw)[# dev community ng](https://twitter.com/hashtag/DEVCommunityNG?src=hash&ref_src=twsrc%5Etfw)[# JavaScript](https://twitter.com/hashtag/javascript?src=hash&ref_src=twsrc%5Etfw)
> 
> — Tapas Adhikary (@tapasadhikary) [July 19, 2021](https://twitter.com/tapasadhikary/status/1416995389169971200?ref_src=twsrc%5Etfw)