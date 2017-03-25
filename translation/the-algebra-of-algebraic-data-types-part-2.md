# 代数数据类型的代数系统（二）

在上一篇文章里我们介绍了单位类型 `Unit`（或 `()`）和零类型 `Void`，我们还引入了类型的操作符 `Add` 和 `Mul`，以及函数类型 `a -> b`。

在这片文章里我们会同时用到 Haskell 和数学记号，下面的这个对照表可以帮助你完成它们之间的相互转化。
$$
\begin{align}

    {\rm Void}                      && \leftrightarrow && 0 \\
    () \,,\; {\rm Unit}             && \leftrightarrow && 1 \\
    {\rm Add} \; a \; b             && \leftrightarrow && a + b \\
    (a,b) \,,\; {\rm Mul} \; a \; b && \leftrightarrow && a \cdot b \\
    a \rightarrow b                 && \leftrightarrow && b ^ a

    \end{align}
$$
在这篇文章里我主要会讲述递归类型，同时展示如何使用它们完成一些有趣的化简操作。

## `Maybe` 类型

使用 `Maybe` 类型可以让我们轻松许多。

它是这样定义的：

```haskell
data Maybe a = Nothing | Just a
```

中间的竖线标明了这是一个「和类型」，所以我们可以把它

```haskell
data Nothing = Nothing
data Just a  = Just a

type Maybe a = Add Nothing (Just a)
```

注意到我们已经把声明中的 `data` 换成了 `type` 吗？这说明 `Maybe a` 已经不再是一个类型的——它是一个已知类型的别名。

## 递归类型

### 列表

在 Haskell 里，基础的列表都是链表。一个有着多个类型为 `a` 的元素的列表可以是空，用 `[]` 表示，也可以是把一个 `a` 连接到一个 `a` 的列表上，用 `a : as` 表示。如果我们想定义自己的列表类型，我们一般会这样写：

```haskell
data List a = Nil | Cons a (List a)
```

让我们来花费几秒钟仔细看一下这个声明，就像 `Maybe` 一样，`List` 类型是两个更简单类型的和。

第一个加数是 `Nil`，是一个空构造器，等价于 `()`。

第二个加数是 `Cons a (List a)`，是一个类型为 `a` 的对象和一个 `a` 的列表的积。

列表的代数形式，可以写成下面的形式：
$$
L(a)=1+a\cdot L(a)
$$
看上去我们似乎可以把列表类型写成下面的形式：

```haskell
type List a = Add () (a, (List a))
```

然而这样并不能通过编译，因为我们使用的类型别名会在编译时展开，在类型检查之后，但是在编译之前，这个声明将永远不会完成展开——它会像这样不停地生长下去：

```haskell
Add () (a, Add () (a, Add () (a, ...)))
```

### 树

考虑树节点上带有值的二叉树结构，在 Haskell 里一般写成下面的形式：

```haskell
data Tree a = Empty | Node a (Tree a) (Tree a)
```

## 七树合一

## 一个解释？

* 类型的减法、除法和平方根有什么意义呢？
* 为什么这些看起来没有必要的对类型的操作能产生有意义的结果？

最后一个问题我们会留一些悬念。

