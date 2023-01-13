# 如何用 Python 一行程序解决 Leetcode 问题

> 原文：<https://www.freecodecamp.org/news/solve-leetcode-problems-using-python-list-comprehension/>

Python 是最强大的编程语言之一。它为我们提供了各种独特的特性和功能，使我们能够轻松地编写代码。

在本文中，我们将使用 Python 最有趣的特性之一——**列表理解**，在一行中解决 Leetcode 数组问题。

## 什么是列表理解？

在进入问题之前，让我们确定我们理解了什么是列表理解。

> 列表理解是一些编程语言中可用的语法结构，用于基于现有列表创建列表

让我们用一个例子来看看列表理解是如何工作的。

考虑一组数字。我们的任务是在奇数索引的数字上加 1，在偶数索引的数字上加 2。

现在我们将看看如何使用 for 循环和列表理解来解决上述问题。

### 如何用 for 循环解决问题

```
def addOneAndTwo(nums, n):
    for i in range(n):
        if i % 2 == 1:
            nums[i] += 1 
        else:
            nums[i] += 2 
    return nums 
```

### 如何用列表理解解决

```
def addOneAndTwo(nums, n):
    return [nums[i] + 1 if i % 2 == 1 else nums[i] + 2 for i in range(n)] 
```

您可以看到使用列表理解的解决方案是如何从 6 行简化为 1 行的。这就是列表理解的力量。

## 如何用列表理解解决 Leetcode 问题

现在让我们用列表理解的方法用一行代码解决下面的问题。

### 1.[洗牌](https://leetcode.com/problems/shuffle-the-array/)

Leetcode 的问题如下:

假设数组`nums`由`2n`个元素组成，形式为`[x[1],x[2],...,x[n],y[1],y[2],...,y[n]]`。*以* `[x[1],y[1],x[2],y[2],...,x[n],y[n]]`的形式返回数组。

#### 例子

输入:nums = [2，5，1，3，4，7]，n = 3
输出:[2，3，5，4，1，7]

说明:由于 x1=2，x2=5，x3=1，y1=3，y2=4，y3=7 那么答案就是[2，3，5，4，1，7]。

#### 解决办法

```
def shuffle(self, nums, n):
    return reduce(lambda a, b: a + b, [[nums[i], nums[j]] for i, j in zip(range(0, n), range(n, 2 * n))]) 
```

### 2.[良好对的数量](https://leetcode.com/problems/number-of-good-pairs/)

给定一个整数数组`nums`。一对`(i,j)`叫做*好的*如果`nums[i]` == `nums[j]`和`i` < `j`。返回*好*对的数量。

#### 例子

输入:nums = [1，2，3，1，1，3]
输出:4

说明:有 4 个好的对(0，3)，(0，4)，(3，4)，(2，5)0-索引。

#### 解决办法

```
def numIdenticalPairs(self, nums):
    return sum([int(i != j and nums[i] == nums[j]) for i in range(0, len(nums)) for j in range(i + 1, len(nums))]) 
```

### 3.[拥有最多糖果的孩子](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)

给定数组`candies`和整数`extraCandies`，其中`candies[i]`代表第 ***个*** 小孩拥有的糖果数量。

对于每个孩子，检查是否有办法将`extraCandies`分发给他们，这样他们就可以得到**最大**数量的糖果。请注意，多个孩子可以拥有**最大**数量的糖果。

#### 例子

输入:candies = [2，3，5，1，3]，extraCandies = 3
输出:[真，真，真，假，真]

解释:孩子 1 有 2 颗糖果，如果他们收到所有额外的糖果(3)，他们将有 5 颗糖果——这是孩子们中糖果数量最多的。

孩子 2 有 3 颗糖果，如果他们收到至少 2 颗额外的糖果，那么他们将拥有孩子中最多的糖果。

小孩 3 有 5 颗糖果，这已经是小孩中糖果数量最多的了。

孩子 4 有 1 颗糖果，即使他们收到所有额外的糖果，他们也只有 4 颗。

孩子 5 有 3 颗糖果，如果他们收到至少 2 颗额外的糖果，那么他们将拥有孩子中最多的糖果。

#### 解决办法

```
def kidsWithCandies(self, candies, extraCandies):
    return [candy + extraCandies >= max(candies) for candy in candies] 
```

### 4.[解压缩游程编码列表](https://leetcode.com/problems/decompress-run-length-encoded-list/)

我们得到一个整数列表`nums`,它代表一个用游程编码压缩的列表。

考虑每对相邻的元素`[freq, val] = [nums[2*i], nums[2*i+1]]`(带`i >= 0`)。对于每一个这样的对，子列表中都有值为`val`的`freq`元素。从左到右连接所有子列表，生成解压缩列表。

返回解压后的列表。

#### 例子

输入:nums = [1，2，3，4]
输出:[2，4，4，4]

解释:第一对[1，2]意味着我们有 freq = 1 和 val = 2，所以我们生成数组[2]。

第二对[3，4]意味着我们有 freq = 3 和 val = 4，所以我们产生[4，4，4]。最后，串联[2] + [4，4，4]是[2，4，4，4]。

#### 解决办法

```
def decompressRLElist(self, nums):
    return reduce(lambda a, b: a + b, [[nums[i + 1]] * nums[i] for i in range(0, len(nums), 2)]) 
```

### 5.[最富有客户的财富](https://leetcode.com/problems/richest-customer-wealth/)

给你一个整数网格`accounts`，其中`accounts[i][j]`是`i​​​​​^(​​​​​​th)​​​​`客户在`j​​​​​^(​​​​​​th)`银行的存款金额。归还最富有的顾客拥有的财富。

客户的**财富**是他们所有银行账户中的钱数。最富有的客户是拥有最大**财富**的客户。

#### 例子

输入:帐户= [[1，2，3]，[3，2，1]]
输出:6

解释:`1st customer has wealth = 1 + 2 + 3 = 6 2nd customer has wealth = 3 + 2 + 1 = 6` 两个客户都被认为是最富有的，每人拥有 6 英镑的财富，所以返回 6 英镑。

#### 解决办法

```
def maximumWealth(self, accounts):
    return max([sum(row) for row in accounts]) 
```

## 结论

我希望上述解决方案是有用的。您可以将 [**列表理解**](https://data-flair.training/blogs/python-list-comprehension/) 与 [**映射**、**过滤**、**归约**](https://www.freecodecamp.org/news/15-useful-javascript-examples-of-map-reduce-and-filter-74cbbb5e0a1f/) 等其他功能结合使用，使解决方案更加简单有效。

## 谢谢你🤘

[Linkedin](https://www.linkedin.com/in/ganeshkumarm1) | [Github](https://github.com/ganeshkumarm1)