# Python 列表方法–用代码示例解释 Python 中的 append()与 extend()

> 原文：<https://www.freecodecamp.org/news/python-list-methods-append-vs-extend/>

当您使用 Python 列表时，通常需要组合多个列表中的数据。那你是怎么做到的呢？

在本教程中，我们将看到合并多个列表中的数据的不同方法。我们还将通过简单的例子了解列表方法`append()`和`extend()`是如何工作的。让我们开始吧。

## 如何在 Python 中使用'+'运算符

在我们了解`append()`和`extend()`方法如何工作之前，让我们看看用于连接列表的`+`操作符是做什么的。

考虑下面的例子，我们有两个列表，`list_1`和`list_2`，我们想要连接(或端到端地连接在一起):

```
list_1 = [1,2,3,4,5]
list_2 = [6,7,8]

print(list_1 + list_2)
# Output
[1, 2, 3, 4, 5, 6, 7, 8]

print(list_1)
# Output
[1, 2, 3, 4, 5]

print(list_2)
# Output
[6, 7, 8]
```

如果仔细通读上面的代码片段，可以看到以下内容。

*   `list_1 + list_2`不添加从`list_2`到`list_1`的项目。
*   相反，它创建一个包含来自`list_1`和`list_2`的项目的*新列表*。
*   因此，`list_1`和`list_2`保持不变。

如果我们想修改`list_1`而不是创建一个新的列表呢？好吧，让我们在下一节中使用`append()`和`extend()`方法来做这件事。

## 如何在 Python 中使用 append()方法

在本节中，我们将使用列表方法添加从`list_2`到`list_1`的项目。考虑到上一节中的相同示例列表，现在让我们尝试通过添加来自`list_2`的元素来修改`list_1`。

下面的代码片段使用`list_2`作为参数调用`list_1`上的`append()`方法。

```
# Let's append list_2 to list_1
list_1.append(list_2)
print(list_1)

# Output
[1, 2, 3, 4, 5, [6, 7, 8]]

# print the length of list_1
print(len(list_1))

# Output
6
```

我们看到`list_2`已经作为*单列表项追加到了* `list_1`的末尾。
这意味着`list_1` *的长度在`append()`操作后增加 1 个*。

如果我们想在`list_1`的末尾添加从`list_2`到`list_1`的项目，而不是作为一个单一的列表，而是作为单独的项目，该怎么办？让我们在下一节做这件事。

## 如何在 Python 中使用 extend()方法

在我们开始使用`extend()`方法之前，让我们做以下事情。

*   循环通过`list_2`
*   *追加`list_2`到`list_1`的各项*

```
for item in list_2:
  list_1.append(item)

print(list_1)

# Output
[1, 2, 3, 4, 5, 6, 7, 8]
```

我们现在已经按照我们想要的方式添加了从`list_2`到`list_1`的所有项目。😀
我们可以用`list_2`作为参数调用`list_1`上的`extend()`方法来做同样的事情。您可以在下面的代码片段中看到这是如何工作的:

```
# Let's extend list_1 with items from list_2
list_1.extend(list_2)
print(list_1)

# Output
[1, 2, 3, 4, 5, 6, 7, 8]

# print the length of list_1
print(len(list_1))

# Output
8
```

所以如果你用一个长度为`len2`、
的列表来扩展一个长度为`len1`的列表，那么调用`extend()`方法的列表的长度就是`len1 + len2`。

### 有什么警告吗？

如果我们希望在现有列表中添加一个条目，而不是整个列表(或者任何可重复的条目),该怎么办？在下面的例子中，让我们使用如下所示的`append()`将布尔值`True`添加到`list_1`。

```
list_1 = [1,2,3,4,5]
list_1.append(True)
print(list_1)

# Output
[1, 2, 3, 4, 5, True]
```

如果我们试着用`extend()`做和上面一样的事情会怎么样？

```
list_1 = [1,2,3,4,5]
list_1.extend(True)
print(list_1)

# Output
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-16-9e4e0d6da67b> in <module>()
      1 list_1 = [1,2,3,4,5]
----> 2 list_1.extend(True)
      3 print(list_1)

TypeError: 'bool' object is not iterable
```

哦，这样做会抛出一个错误。😢让我们尝试解析返回的错误消息。`TypeError: 'bool' object is not iterable`。这意味着我们的论点是不正确的类型。

`extend()`方法要求参数是可迭代的。在内部，`extend()`方法通过遍历 iterable 并将 iterable 中的每一项添加到第一个列表中来工作。列表、集合、元组和字符串都是可迭代的例子。

任何可以循环访问单个项目的对象都是一个**可迭代的**。
注意使用`extend()`方法的一般语法是`list_name.extend(iterable)`。

## 让我们在字符串上尝试 extend()和 append()

字符串本质上是一个字符序列，因此是可迭代的。让我们试着用一个字符串作为参数对`list_1`使用`extend()`和`append()`方法。

让我们使用`append()`方法将字符串*‘快乐’*添加到`list_1`中。

```
list_1 = [1,2,3,4,5]
list_1.append('Happy')
print(list_1)

# Output
[1, 2, 3, 4, 5, 'Happy']
```

现在让我们尝试使用`extend()`方法添加相同的字符串。这一次我们不应该得到一个错误。相反，extend 方法应该遍历字符串并将每个字符添加到`list_1`中。

让我们看看这在下面的代码示例中是否有效。🙂

```
list_1 = [1,2,3,4,5]
list_1.extend('Happy')
print(list_1)

# Output
[1, 2, 3, 4, 5, 'H', 'a', 'p', 'p', 'y']
```

很直观地看到，`append()`方法在*常量时间*中运行，而`extend()`方法的运行时间应该与作为参数传递给该方法的 iterable 的长度成比例。

## 包扎

我希望这篇文章能阐明如何在 Python 中使用`append()`和`extend()`方法。感谢您的阅读，祝您学习和编码愉快！

### 相关职位

关于 Python 列表和常用列表方法的一般介绍，请查看这篇文章:[Python 中的列表——综合指南。](https://www.freecodecamp.org/news/lists-in-python-comprehensive-guide/)