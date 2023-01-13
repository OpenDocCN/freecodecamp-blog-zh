# 如何在 JavaScript 中使用闭包——初学者指南

> 原文：<https://www.freecodecamp.org/news/closures-in-javascript/>

闭包是一个很难学习的 JavaScript 概念，因为很难看出它们实际上是如何使用的。

与函数、变量和对象等其他概念不同，您并不总是自觉而直接地使用闭包。你不要说:哦！这里我将使用闭包作为解决方案。

但同时，你可能已经使用这个概念一百次了。学习闭包更多的是识别闭包何时被使用，而不是学习一个新概念。

## JavaScript 中的闭包是什么？

当一个函数读取或修改一个在它的上下文之外定义的变量的值时，你就有了一个闭包。

```
const value = 1
function doSomething() {
    let data = [1,2,3,4,5,6,7,8,9,10,11]
    return data.filter(item => item % value === 0)
} 
```

这里函数`doSomething`使用了变量`value`。但是函数`item => item % value === 0`也可以这样写:

```
function(item){
    return item % value === 0
} 
```

您使用在函数本身之外定义的变量`value`的值。

## 函数可以访问上下文之外的值

与前面的示例一样，函数可以访问和使用在其“主体”或上下文之外定义的值，例如:

```
let count = 1
function counter() {
    console.log(count)
}
counter() // print 1
count = 2
counter() // print 2 
```

这允许我们在模块的任何地方修改`count`变量的值。然后当计数器函数被调用时，它会知道如何使用当前值。

## 我们为什么要使用函数？

但是为什么我们要在程序中使用函数呢？当然，不使用我们定义的函数来编写程序是可能的——虽然困难，但也是可能的。那么我们为什么要创建适当的函数呢？

想象一段代码，它做了一些奇妙的事情，无论如何，它是由 X 行代码组成的。

```
/* My wonderful piece of code */ 
```

现在假设你必须在程序的不同部分使用这段**美妙的代码**，你会怎么做？。

“自然”的选择是将这段代码放入一个可重用的集合中，这个可重用的集合就是我们所说的函数。函数是在程序中重用和共享代码的最佳方式。

现在，您可以尽可能多地使用您的函数。并且，忽略一些特殊的情况，调用你的函数 N 次就等同于编写那段精彩的代码 N 次。这是一个简单的替换。

## 但是终结在哪里呢？

使用反例，让我们把它看作是**段精彩的代码。**

```
let count = 1
function counter() {
    console.log(count)
}
counter() // print 1 
```

现在，我们想在许多部分重用它，所以我们将它“包装”在一个函数中。

```
function wonderfulFunction() {
    let count = 1
    function counter() {
        console.log(count)
    }
    counter() // print 1
} 
```

现在我们有什么？一个函数:`counter`，它使用了一个在它之外声明的值`count`。和一个值:`count`，它在`wonderfulFunction`函数范围内声明，但在`counter`函数内使用。

也就是说，我们有一个函数，它使用了一个在其上下文之外声明的值:**闭包**。

很简单，不是吗？现在，当函数`wonderfulFunction`被执行时会发生什么？一旦执行了**父**函数，变量`count`和函数`counter`会发生什么？

其体中声明的变量和函数*“消失”*(垃圾收集器)。

现在，让我们稍微修改一下这个例子:

```
function wonderfulFunction() {
    let count = 1
    function counter() {
        count++
        console.log(count)
    }
   setInterval(counter, 2000)
}
wonderfulFunction() 
```

现在在`wonderfulFunction`中声明的变量和函数会发生什么？

在这个例子中，我们告诉浏览器每 2 秒钟运行一次`counter`。因此 JavaScript 引擎必须保存对函数的引用以及它所使用的变量的引用。即使在父函数`wonderfulFunction`完成其执行周期后，函数`counter`和值计数仍然会“*活”*。

拥有闭包的这种“效果”的出现是因为 JavaScript 支持函数的嵌套。或者换句话说，函数是语言中的第一类公民，你可以像使用任何其他对象一样使用它们:嵌套、作为参数传递、作为返回值等等。

## 在 JavaScript 中我可以用闭包做什么？

### **立即调用的函数表达式(life)**

这是一种在 ES5 时代被广泛使用的技术，用来实现“模块”设计模式(在它被本地支持之前)。这个想法是将你的模块“包装”在一个可以立即执行的函数中。

```
(function(arg1, arg2){
...
...
})(arg1, arg2) 
```

这允许您在函数中使用只能由模块本身使用的私有变量——也就是说，允许它模拟访问修饰符。

```
const module = (function(){
	function privateMethod () {
	}
	const privateValue = "something"
	return {
	  get: privateValue,
	  set: function(v) { privateValue = v }
	}
})()

var x = module()
x.get() // "something"
x.set("Another value")
x.get() // "Another Value"
x.privateValue //Error 
```

### **功能工厂**

得益于闭包实现的另一个设计模式是“函数工厂”。这是函数创建函数或对象的时候，例如，允许您创建用户对象的函数。

```
 const createUser = ({ userName, avatar }) => ({
      id: createID(),
      userName,
      avatar,
      changeUserName (userName) {
        this.userName = userName;
        return this;
      },
      changeAvatar (url) {
        // execute some logic to retrieve avatar image
        const newAvatar = fetchAvatarFromUrl(url)
        this.avatar = newAvatar
        return this
      }
    });

        console.log(createUser({ userName: 'Bender', avatar: 'bender.png' }));

    {
      "id":"17hakg9a7jas",
      "avatar": "bender.png",
      "userName": "Bender",
      "changeUsername": [Function changeUsername]
      "changeAvatar": [Function changeAvatar]

    }
    */c
```

使用这种模式，你可以实现一种叫做 **currying** 的函数式编程思想。

### **阿谀奉承**

Currying 是一种设计模式(也是一些语言的一个特征),在这种模式下，函数被立即求值并返回第二个函数。这种模式允许您执行专门化和组合。

您使用闭包来创建这些“定制的”函数，定义并返回闭包的内部函数。

```
function multiply(a) {

    return function (b) {
        return function (c)  {
            return a * b * c
        }
    }
}
let mc1 = multiply(1);
let mc2 = mc1(2);
let res = mc2(3);
console.log(res);

let res2 = multiply(1)(2)(3);
console.log(res2); 
```

这些类型的函数接受单个值或参数，并返回另一个也接受参数的函数。这是参数的部分应用。也可以使用 ES6 重写这个例子。

```
let multiply = (a) => (b) => (c) => {

    return a * b * c;
}

let mc1 = multiply(1);
let mc2 = mc1(2);
let res = mc2(3);
console.log(res);

let res2 = multiply(1)(2)(3);
console.log(res2); 
```

我们可以在哪里申请 currying？在 composition 中，假设您有一个创建 HTML 元素的函数。

```
function createElement(element){
    const el = document.createElement(element)
    return function(content) {
        return el.textNode = content
    }
}

const bold = crearElement('b')
const italic = createElement('i')
const content = 'My content'
const myElement  = bold(italic(content)) // <b><i>My content</i></b> 
```

### **事件监听器**

另一个可以使用和应用闭包的地方是在使用 React 的事件处理程序中。

假设您使用第三方库来呈现数据集合中的项目。这个库公开了一个名为`RenderItem`的组件，它只有一个可用的属性`onClick`。该属性不接收任何参数，也不返回值。

现在，在您的特定应用程序中，您要求当用户单击项目时，应用程序显示带有项目标题的警告。但是您可用的`onClick`事件不接受参数——那么您能做什么呢？**关闭以拯救**:

```
// Closure
// with es5
function onItemClick(title) {
    return function() {
      alert("Clicked " + title)
    }
}
// with es6
const onItemClick = title => () => alert(`Clcked ${title}`)

return (
  <Container>
{items.map(item => {
return (
   <RenderItem onClick={onItemClick(item.title)}>
    <Title>{item.title}</Title>
  </RenderItem>
)
})}
</Container>
) 
```

在这个简化的示例中，我们创建了一个函数，该函数接收您想要显示的标题，并返回另一个函数，该函数满足 RenderItem 作为道具接收的函数的定义。

## **结论**

你甚至可以在不知道你正在使用闭包的情况下开发一个应用。但是知道它们的存在以及它们是如何真正工作的，会在你创建解决方案时开启新的可能性。

闭包是刚开始时很难理解的概念之一。但是一旦你知道你在使用它们并理解它们，它会让你增加你的工具并推进你的事业。

![English-Footer-Social-Card-1](img/de1b9d3f8689c74679c8c8bdd672961b.png)

🐦[在推特上关注我](https://twitter.com/matiasfha) ✉️ [加入时事通讯](https://matiashernandez.ck.page) ❤️ [支持我的工作](https://buymeacoffee.com/matiasfha)