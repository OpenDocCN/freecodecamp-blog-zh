# JSON 初学者——用简单的英语解释 JavaScript 对象符号

> 原文：<https://www.freecodecamp.org/news/what-is-json-a-json-file-example/>

许多软件应用程序需要在客户机和服务器之间交换数据。

很长一段时间，当涉及到两点之间的信息交换时，XML 是首选的数据格式。然后在 2000 年初，JSON 作为信息交换的替代数据格式被引入。

在本文中，您将了解关于 JSON 的所有内容。你会明白它是什么，如何使用它，我们会澄清一些误解。所以，不再耽搁，让我们来了解一下 JSON。

## JSON 是什么？

JSON(**J**ava**S**script**O**object**N**rotation)是`text-based`数据交换格式。它是键-值对的集合，其中键必须是字符串类型，值可以是以下任何类型:

*   数字
*   线
*   布尔代数学体系的
*   排列
*   目标
*   空

需要注意几个重要的规则:

*   在 JSON 数据格式中，键必须用双引号括起来。
*   键和值必须用冒号(:)符号分隔。
*   可以有多个键值对。两个键值对必须用逗号(，)分隔。
*   JSON 数据中不允许有注释(//或/* */)。(但是如果你好奇的话，你可以[绕过那个](https://www.freecodecamp.org/news/json-comment-example-how-to-comment-in-json-files/)。)

下面是一些简单的 JSON 数据:

```
{
    "name": "Alex C",
    "age": 2,
    "city": "Houston"
}
```

Simple JSON Data

有效的 JSON 数据可以有两种不同的格式:

*   用一对花括号`{...}`括起来的键值对集合。你在上面看到了这个例子。
*   键-值对的有序列表的集合，由逗号(，)分隔，并用一对方括号`[...]`括起来。请参见下面的示例:

```
[
	{
        "name": "Alex C",
        "age": 2,
        "city": "Houston"
	},
    {
        "name": "John G",
        "age": 40,
        "city": "Washington"
	},
    {
        "name": "Bala T",
        "age": 22,
        "city": "Bangalore"
	}
]
```

JSON Data with a Collection of Ordered Records

假设你有 JavaScript 开发背景。在这种情况下，您可能会觉得 JSON 格式和 JavaScript 对象(以及对象数组)非常相似。但事实并非如此。我们很快就会看到细节上的不同。

JSON 格式的结构源自 JavaScript 对象语法。这是 JSON 数据格式和 JavaScript 对象之间唯一的关系。

JSON 是一种独立于编程语言的格式。我们可以在 Python、Java、PHP 和许多其他编程语言中使用 JSON 数据格式。

## JSON 数据格式示例

可以将 JSON 数据保存在扩展名为`.json`的文件中。让我们用雇员的属性(由键和值表示)创建一个`employee.json`文件。

```
{
	"name": "Aleix Melon",
	"id": "E00245",
	"role": ["Dev", "DBA"],
	"age": 23,
	"doj": "11-12-2019",
	"married": false,
	"address": {
		"street": "32, Laham St.",
		"city": "Innsbruck",
		"country": "Austria"
	},
	"referred-by": "E0012"
}
```

Employee Data in JSON Format

上面的 JSON 数据显示了一个员工的属性。这些属性是:

*   `name`:员工姓名。该值属于`String`类型。所以，它用双引号括起来。
*   `id`:员工的唯一标识。又是一个`String`型。
*   员工在组织中扮演的角色。一个员工可能扮演多个角色。所以`Array`是首选的数据类型。
*   `age`:员工当前的年龄。是一辆`Number`。
*   `doj`:员工加入公司的日期。因为它是一个日期，所以必须用双引号括起来，并像对待`String`一样对待。
*   员工结婚了吗？如果是，对还是错。因此该值属于`Boolean`类型。
*   `address`:员工的地址。一个地址可以有多个部分，如街道、城市、国家、邮政编码等等。因此，我们可以将地址值视为一个`Object`表示(带有键值对)。
*   `referred-by`:在组织中推荐该员工的员工 id。如果该员工是通过推荐加入的，则该属性将具有值。否则，它将把`null`作为一个值。

现在让我们创建一个雇员集合作为 JSON 数据。为此，我们需要在方括号内保存多个员工记录[...].

```
[
	{
        "name": "Aleix Melon",
        "id": "E00245",
        "role": ["Dev", "DBA"],
        "age": 23,
        "doj": "11-12-2019",
        "married": false,
        "address": {
            "street": "32, Laham St.",
            "city": "Innsbruck",
            "country": "Austria"
            },
        "referred-by": "E0012"
	},
    {
        "name": "Bob Washington",
        "id": "E01245",
        "role": ["HR"],
        "age": 43,
        "doj": "10-06-2010",
        "married": true,
        "address": {
            "street": "45, Abraham Lane.",
            "city": "Washington",
            "country": "USA"
            },
        "referred-by": null
	}
]
```

An Array of Employee Data in JSON Format

您注意到第二个雇员 Bob Washington 的`referred-by`属性值了吗？就是`null`。意思是他没有被任何员工推荐。

## 如何使用 JSON 数据作为字符串值

我们已经看到了如何在 JSON 文件中格式化 JSON 数据。或者，我们可以使用 JSON 数据作为字符串值，并将其赋给一个变量。因为 JSON 是一种基于文本的格式，所以在大多数编程语言中可以作为字符串来处理。

让我们举一个例子来理解如何在 JavaScript 中实现它。您可以将整个 JSON 数据作为一个字符串包含在单引号`'...'`中。

```
const user = '{"name": "Alex C", "age": 2, "city": "Houston"}';
```

JSON Data as a String Value - Using quotes

如果希望保持 JSON 格式不变，可以借助模板文字创建 JSON 数据。

```
const user = `{
    "name": "Alex C",
    "age": 2,
    "city": "Houston"
}`; 
```

JSON Data as a String Value - Using Template Literals

当您希望使用动态值构建 JSON 数据时，它也很有用。

```
const age = 2;

const user = `{
    "name": "Alex C",
    "age": ${age},
    "city": "Houston"
}`;

console.log(user);

// Output
{
    "name": "Alex C",
    "age": 2,
    "city": "Houston"
}
```

Build JSON Data using Dynamic Value

## JavaScript 对象和 JSON 不一样

JSON 数据格式源自 JavaScript 对象结构。但相似之处仅此而已。

JavaScript 中的对象:

*   可以有方法，JSON 没有。
*   密钥可以不带引号。
*   允许评论。
*   是 JavaScript 自己的实体。

这里有一个 Twitter 帖子，通过几个例子解释了这种差异。

> JavaScript 对象和 JSON(JavaScript 对象表示法)是不一样的。
> 
> 我们常常认为它们是相似的。不是这样的👀
> 让我们了解一下
> 🔥
> 
> 一根线
> 
> 🧵👇
> 
> — Tapas Adhikary (@tapasadhikary) [November 24, 2021](https://twitter.com/tapasadhikary/status/1463493300225204225?ref_src=twsrc%5Etfw)