# react . use effect Hook–常见问题及解决方法

> 原文：<https://www.freecodecamp.org/news/most-common-react-useeffect-problems-and-how-to-fix-them/>

React hooks 已经存在了一段时间了。大多数开发人员已经对他们的工作方式和常见用例相当熟悉了。但是有一个问题是我们很多人经常会上当的。

# 使用案例

让我们从一个简单的场景开始。我们正在构建一个 React 应用程序，我们希望在我们的一个组件中显示当前用户的用户名。但是首先，我们需要从 API 获取用户名。

因为我们知道我们还需要在应用程序的其他地方使用用户数据，所以我们还想在自定义的 React 钩子中抽象数据提取逻辑。

本质上，我们希望 React 组件看起来像这样:

```
const Component = () => {
  // useUser custom hook

  return <div>{user.name}</div>;
}; 
```

看起来够简单！

# 用户用户反应挂钩

第二步是创建我们的`useUser`定制钩子。

```
const useUser = (user) => {
  const [userData, setUserData] = useState();
  useEffect(() => {
    if (user) {
      fetch("users.json").then((response) =>
        response.json().then((users) => {
          return setUserData(users.find((item) => item.id === user.id));
        })
      );
    }
  }, []);

  return userData;
}; 
```

我们来分解一下。我们正在检查钩子是否正在接收一个用户对象。之后，我们从一个名为`users.json`的文件中获取一个用户列表，并对其进行过滤，以便找到具有我们需要的 id 的用户。

然后，一旦我们有了必要的数据，我们就把它保存到钩子的`userData`状态。最后都是返回`userData`。

***注**:这是一个人为的例子，仅供说明之用！现实世界中的数据获取要复杂得多。如果你对这个话题感兴趣，[可以看看我的文章](https://blog.whereisthemouse.com/graphql-requests-made-easy-with-react-query-and-typescript)关于如何用 ReactQuery、Typescript 和 GraphQL 创建一个伟大的数据获取设置。*

让我们将钩子插入 React 组件，看看会发生什么。

```
const Component = () => {
  const user = useUser({ id: 1 });
  return <div>{user?.name}</div>;
}; 
```

不错！一切看起来和工作起来都像预期的那样。但是等等...这是什么？

# 埃斯林特穷尽-德普斯规则

我们的钩子中有一个 ESLint 警告:

```
React Hook useEffect has a missing dependency: 'user'. Either include it or remove the dependency array. (react-hooks/exhaustive-deps) 
```

嗯，我们的`useEffect`好像少了一个依赖。哦，好吧！补充一下吧。最坏能发生什么？😂

```
const useUser = (user) => {
  const [userData, setUserData] = useState();
  useEffect(() => {
    if (user) {
      fetch("users.json").then((response) =>
        response.json().then((users) => {
          return setUserData(users.find((item) => item.id === user.id));
        })
      );
    }
  }, [user]);

  return userData;
}; 
```

啊哦！看起来我们的`Component`现在不会停止重新渲染。这是怎么回事？！

我们来解释一下。

# 无限再现问题

我们的组件被重新渲染的原因是因为我们的`useEffect`依赖关系在不断变化。但是为什么呢？我们总是将同一个对象传递给我们的钩子！

虽然我们确实传递了一个具有相同键和值的对象，但它并不是完全相同的对象。每次我们重新渲染我们的`Component`，我们实际上是在创建一个新的对象。然后我们将新对象作为参数传递给我们的`useUser`钩子。

在内部，`useEffect`比较这两个对象，由于它们有不同的引用，它再次获取用户并将新的用户对象设置为状态。状态更新然后触发组件中的重新呈现。等等，等等，等等...

那么我们能做什么呢？

# 怎么修

既然我们理解了这个问题，我们就可以开始寻找解决办法了。

第一个也可能是最明显的选择是从`useEffect`依赖数组中移除依赖，忽略 ESLint 规则，继续我们的生活。

但这是错误的方法。这可能(很可能)会导致我们的应用程序出现错误和意外行为。如果你想了解更多关于`useEffect`的工作原理，我强烈推荐丹·阿布拉莫夫的[完全指南](https://overreacted.io/a-complete-guide-to-useeffect/)。

那么下一步是什么？

在我们的例子中，最简单的解决方案是从组件中取出`{ id: 1 }`对象。这样会给对象一个稳定的参考，解决我们的问题。

```
const userObject = { id: 1 };

const Component = () => {
  const user = useUser(userObject);
  return <div>{user?.name}</div>;
};

export default Component; 
```

但这并不总是可能的。假设用户 id 在某种程度上依赖于组件属性或状态。

例如，我们可能使用 URL 参数来访问它。如果是这种情况，我们有一个方便的`useMemo`钩子来记忆对象并再次确保一个稳定的引用。

```
const Component = () => {
  const { userId } = useParams();

  const userObject = useMemo(() => {
    return { id: userId };
  }, [userId]); // Don't forget the dependencies here either!

  const user = useUser(userObject);
  return <div>{user?.name}</div>;
};

export default Component; 
```

最后，可以只传递用户 id 本身，这是一个原始值，而不是将对象变量传递给我们的`useUser`钩子。这将防止`useEffect`钩子中的引用相等问题。

```
const useUser = (userId) => {
  const [userData, setUserData] = useState();

  useEffect(() => {
    fetch("users.json").then((response) =>
      response.json().then((users) => {
        return setUserData(users.find((item) => item.id === userId));
      })
    );
  }, [userId]);

  return userData;
};

const Component = () => {
  const user = useUser(1);

  return <div>{user?.name}</div>;
}; 
```

问题解决了！

在此过程中，我们甚至不需要违反任何 ESLint 规则...

***注意:**如果我们传递给自定义钩子的参数是一个函数，而不是一个对象，我们将使用非常相似的技术来避免无限的重新渲染。一个显著的区别是，在上面的例子中，我们必须用`useCallback`替换`useMemo`。*

感谢您的阅读！

对代码很好奇？自己玩[这里](https://codesandbox.io/s/useeffect-gotcha-20jw9?file=/src/App.js)。

访问我的[博客](https://blog.whereisthemouse.com/)和 Twitter 上的[关注我](https://twitter.com/iva_kop)，了解更多与 React 相关的内容。

图片由 [vectorjuice](https://www.freepik.com/vectors/technology) 提供