# 代数数据类型的代数系统

我于 2012 年十一月在伦敦做了一个有关这个主题的报告。你可以在这里看到这个视频并且在这里下载幻灯片。

在这个系列的文章里我将会解释一下为什么 Haskell 的类型是「代数化」的，当然，我们不会讲什么范畴论和高等数学的那套东西。

你在高中里学到的代数课程都是从数字和运算（例如加减乘除）开始讲起的。运算让你可以从两个数字变出一个新的数字。举个例子，使用加运算把 1 和 2 结合在一起，你将会得到 3，我们一般会这么写：
$$
1+2=3
$$
当你大一点的时候你就会接触到变量的概念，变量可以用来表示数字。再往后，你会学到一些代数的性质，比如：
$$
\begin{align}
0+x&=x\\
1\cdot x&=x\\
\end{align}
$$
这些性质对所有的 $x$ 都成立。还有一些其他的性质，都是有关数字和运算的。

当我们

## Haskell 的代数类型

在 Haskell 的代数类型里，对象是类型，例如 `Bool` 和 `Int`。运这些运算把已有的类型结合在一起，生成新的类型。一个很好的例子就是 `Maybe` 类型构造器。它本身并不是一个类型，你需要用它来创建类型，例如`Maybe Bool` 和 `Maybe Int`，后者都是类型。另一个例子是 `Either`，它从两个已知类型创建出一个新类型，比如 `Either Int Bool`。

### 计数

用 `Bool` 举个例子，它可以用下面的方式定义：

```haskell
data Bool = False | True
```

每一个类型是`Bool` 的对象都有两种可能的取值——`False` 或者 `True`（理论上来讲它也可能是 `undefined`——我们后面将会忽略这个事实）。如果描述得不那么精确的话，类型 `Bool` 与 2 相对应。

如果 `Bool` 是 2，那什么是 1？它应当是一个只有一种取值的类型。在计算机科学中，这样的类型常常被叫做 `Unit`，它的定义如下：

```haskell
data Unit = Unit
```

### 加法

### 乘法

### 零

用加法和乘法可以生成

## Haskell 类型代数中的定律

在我们刚刚定义的类型里面，是否存在着一些定律呢？就想对数那样，一个可以断言两个对象相等的定律，在我们所讲的例子中，对象就是类型。

不过我们这里说的等价（equality）并不是 Haskell 的中的相等，即 `(==)` 函数，我们所说的是一种一一对应关系——即当我们说两个类型 `a` 和 `b` 是相等的时候，你可以写出下面两个函数：

```haskell
from :: a -> b
to   :: b -> a
```

这把类型为 `a` 的值和类型为 `b` 的值结对，所以下面的等式将会永远成立（`==` 是相等函数）

```haskell
to (from a) == a
from (to b) == b
```

举个例子，我认为 `Bool` 类型和 `Add () ()` 类型是等价的。我可以证明下面的函数是等价的：

```haskell
to :: Bool -> Add () ()
to False = AddL ()
to True  = AddR ()

from :: Add () () -> Bool
from (AddL _) = False
from (AddR _) = True
```

我将会使用是那个等号来表示这种类型之间的等价关系。

### 和类型的定律

这里有两条加法类型的定义：

```hakell
Add Void a === a
```

这说明了类型是 `Add Void a` 的值和类型是 `a` 的值的个数相等。第二条定律是

```haskell
Add a b === Add b a
```

它说明了加法运算的可交换性，就像数字那样：
$$
\begin{align}
0+x&=x\\
x+y&=y+x
\end{align}
$$
如果你想练习一下，你可以尝试证明一下 Haskell 代数中这些定律的正确性，可以使用我们之前讲过的计数方法，也可以用 `from` 和 `to` 函数来表达。

### 积类型的定律

有三条乘法类型的定律：

```haskell
Mul Void a === Void
```

它说明了如果你用 `Void` 类型和任何类型相乘，你得到的还是 `Void`。

```haskell
Mul () a === a
```

这定律说明了如果你用 `()` 类型和任何类型相乘，将不会有任何改变。

## 函数类型

在 Haskell 中我们不仅有像 `Int` 和 `Bool` 这样的具体类型，还有函数类型，例如 `Int -> Bool` 和 `Double -> String`。我们该如何把她们整合到我们的代数系统中呢？

我们还是从计数的想法开始，有多少类似 $a\rightarrow b$ 的类型呢？

说得具体一点，把 $a$ 和 $b$ 固定为 `Bool`，`False` 既可以映射到 `False`，也可以映射到 `True`，同样地对于 `True` 也有两种映射。这样的话一共有 $2\cdot 2=2^2=4$ 种可能的类型为 `Bool -> Bool` 的函数。为了再具体一点，我们可以把它们都写出来：

```haskell
f1 :: Bool -> Bool -- equivalent to 'id'
f1 True  = True
f1 False = False

f2 :: Bool -> Bool -- equivalent to 'const False'
f2 _     = False

f3 :: Bool -> Bool -- equivalent to 'const True'
f3 _     = True

f4 :: Bool -> Bool -- equivalent to 'not'
f4 True  = False
f4 False = True
```

当 $b$ 还是 `Bool`，但是 $a$ 的类型是一个有三种值的类型时，会怎样呢？

```haskell
data Trio = First | Second | Thrid
```

`First`、`Second` 和 `Third` 中的每一个值都可以映射到两种可能的取值，所以一共有 $2\cdot 2\cdot 2=2^3=8$ 种类型为 `Trio -> Bool` 的函数。

同样地，

### 函数的定律

## 下期预告

在下一篇文章中我们将会讨论递归类型，例如列表和二叉树，并展示一下你