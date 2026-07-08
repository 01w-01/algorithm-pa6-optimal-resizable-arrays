```
接下来是论文的section 1

1 Introduction
Arrays and resizable arrays are perhaps the most widely used data structures. Surprisingly, we show
that apparently not everything was already said about them. We describe simple new implemen-
tations of resizable arrays that in many cases are more memory-efficient than all implementations
proposed so far. Our perspective is theoretical, but our proposed implementations may have some
practical implications.
An array of size N is a data structure that holds a sequence of items a0, a1, . . . , aN−1. The i-th
item in the sequence, for any 0 ≤ i < n, can be retrieved or modified in constant time. An array
is naturally implemented as a contiguous block of N words of memory, where each word holds an
item, or a pointer to an item. Items are retrieved or modified in constant time using the random
access capabilities of the machine used. Arrays as defined here are inherently of a fixed size.
A resizable array, also known as a dynamic array or dynamic table, is an array whose size can
increase or decrease, while still allowing the retrieval or the modification of the i-th item for any i
in constant time. Decreasing the size of an array is easy. We simply do not use some of the memory
allocated to the array. This, however, is not memory-efficient. Increasing the size of an array is
∗A preliminary version of this paper appeared in SOSA 2023.
†Department of Computer Science, Princeton University, NJ, USA. E-mail: ret@princeton.edu. Research at
Princeton University partially supported by an innovation research grant from Princeton and a gift from Microsoft.
‡Blavatnik School of Computer Science, Tel Aviv University, Tel Aviv, Israel. E-mail: zwick@tau.ac.il. Research
supported by ISF grant no. 2854/20.
harder, since the word just beyond the contiguous block allocated to the array may be in use for a
different purpose. Thus, maintaining a resizable array requires dynamic memory management.
For simplicity, we assume throughout most of the paper that the resizable arrays grow and shrink
only at their ‘far end’. Thus, when the size of the array is increased from N to N + 1 a new
item aN is added to the array. Similarly, when the size is decreased from N to N −1, the item aN−1
disappears. Such resizable arrays form a generalization of a stack. All our results extend to resizable
arrays that can grow and shrink at both ends, however, and thus they give a generalization of a
double-ended queue (deque). This follows easily: one can easily implement a double-ended resizable
array using two single-ended resizable arrays.
The standard textbook solution for resizable arrays (see, e.g., Cormen et al. [4, Section 17.4]) is the
doubling, or geometric expansion/shrinking technique. Start by allocating an array of some initial
size. If the array is full, allocate a new array of, say, twice the size and copy all the items into
it. When the array is, say, less than a quarter full, allocate a new array of, say, half the size and
again copy all items into it. In both cases, the memory used by the old array is released and can
be used for other purposes. A simple amortization argument shows that the amortized cost of each
size increase or decrease is O(1). Each item can still be accessed and modified in O(1) worst-case
time. Furthermore, the time for an increase or decrease operation can be made O(1) worst-case by
a standard background-rebuilding technique.
According to Wikipedia [16], the geometric expansion/shrinking technique, with growth factors
ranging from 1.25 to 2, is used to implement Java’s ArrayList, Python’s PyListObject, C++ STL’s
Vector and more.
The doubling technique allows storing a resizable array of size N using O(N) space. This is nice,
and may be sufficient in many applications. But it also means that at various times as much as
50% of the memory allocated for an array is not used, even if only grow operations are considered.
More generally, if a growth factor of 1 + α is used, a 1+1α fraction of memory may be wasted. By
choosing a small value of α the fraction of wasted storage can be made arbitrarily small, but the
amortized cost for a size increase becomes O(1+αα ), i.e., larger and larger.
Is it possible to store a resizable array of size N using N + o(N) space while still maintaining O(1)
time per operation? Sitarski [15] and Brodnik et al. [2] answered this question affirmatively by
describing simple implementations that use only N +O(

√

N) space. (See more details in Section 3.)
Brodnik et al. [2] also show that in a certain sense this is optimal. More specifically, they show that
any resizable array implementation must sometime use N + Ω(

√

N) space. (We review their simple

argument in Section 4.)
We propose distinguishing between the amount of space needed to store a resizable array currently
of size N, supporting retrieving and modifying items in O(1) worst-case time, and the space that
may be needed temporarily to increase or decrease the size of the array, in O(1) amortized time.
In many settings it is preferable to have a more compact representation of the array, even if more
storage is required, temporarily, for resizing the array. For example, an application may use many
resizable arrays, only one of which is resized at any given moment.
Our first implementation to ‘break’ the lower bound of Brodnik et al. [2] is a simple implementation
that uses only N + O(N1/3) space to store an array, while temporarily using N + O(N2/3) space
to resize it. Items can be accessed in O(1) worst-case time, and increasing or decreasing the size of
the array takes O(1) amortized time. Furthermore, we show that this is essentially optimal. Any
implementation that uses only N + O(N1/3) space for storing an array must, at certain times, use
N + Ω(N2/3) space during resizing.
More generally we show that, for every r ≥ 2, it is possible to store a resizable array using N +
O(N1/r) space, while needing only N +O(N1−1/r) space to resize it. Retrieving and modifying items
by index takes O(1) worst-case time, while resizing takes O(r) amortized time. Since background
rebuilding cannot be used, the O(r) amortized time for resizing, which we show is optimal, cannot
be made worst-case, unless r = 2, in which case the implementation becomes very similar to the
implementations of Sitarski [15] and Brodnik et al. [2].
The rest of the paper is organized as follows. In Section 2 we give a more precise definition of
the problem and the computational model used. In Section 3 we describe previous work, mostly
that of Sitarski [15] and Brodnik et al. [2]. In Section 4 we describe the simple lower bound of
Brodnik et al. [2] and our extension of it. In Section 5 we give our simple r = 3 solution, i.e.,
N + O(N1/3) space for storing, N + O(N2/3) space for resizing. In Section 6 we describe, for
any r ≥ 2, an implementation that uses N + O(rN1/r) space to store the array, while using only
N + O(N1−1/r) space during resize operations. The amortized cost of grow and shrink operations
is O(r). In Section 7 we give a simple transformation that can be used to tune the implementation
of Section 6 and reduce the space needed for storing the array to N + O(N1/r) while maintaining
constant time access and O(r) amortized time for grow and shrink operations. (Note that this is
significant only if r = r(N) is considered to grow with N.) To prove that the O(r) amortized time
of resizing operations is optimal, at least for a wide class of data structures that we call standard,
we introduce in Section 8 an abstract growth game and analyze it completely. In Section 9 we rely
on the analysis of the growth game to obtain an Ω(r) lower bound on the amortized cost of grow
operations, if only N + O(N1/r) space can be used to store the array. We end in Section 10 with
some concluding remarks.
```

## 1. 中文翻译

## 1 引言

数组和可伸缩数组也许是使用最广泛的数据结构。令人意外的是，我们将说明，关于它们似乎还有一些内容没有被充分讨论。我们描述了一些新的、简单的可伸缩数组实现；在许多情况下，它们比目前提出过的所有实现都更节省内存。我们的视角是理论性的，但我们提出的实现也可能具有一些实际意义。

大小为 `N` 的数组是一种数据结构，它保存一个元素序列：

```text
a_0, a_1, ..., a_{N-1}
```

对于任意 `0 ≤ i < N`，序列中的第 `i` 个元素都可以在常数时间内被取出或修改。数组很自然地可以实现为一段连续的、包含 `N` 个 word 的内存块；每个 word 存放一个元素，或者存放指向某个元素的指针。利用机器的随机访问能力，可以在常数时间内取出或修改数组元素。按照这里的定义，数组本质上具有固定大小。

可伸缩数组，也叫动态数组或动态表，是一种大小可以增加或减少的数组；同时，对于任意 `i`，它仍然允许在常数时间内取出或修改第 `i` 个元素。减少数组大小很容易。我们只要停止使用分配给数组的一部分内存即可。不过，这样做并不节省内存。增加数组大小更困难，因为紧接在数组连续内存块之后的那个 word 可能已经被用于其他目的。因此，维护一个可伸缩数组需要动态内存管理。

为了简单起见，在论文的大部分内容中，我们假设可伸缩数组只在它的“远端”增长和收缩。因此，当数组大小从 `N` 增加到 `N + 1` 时，一个新元素 `a_N` 被加入数组。类似地，当数组大小从 `N` 减少到 `N − 1` 时，元素 `a_{N−1}` 消失。这样的可伸缩数组是栈的一种推广。不过，我们的所有结果都可以扩展到两端都能增长和收缩的可伸缩数组，因此它们也给出了双端队列，也就是 `deque`，的一种推广。这个结论很容易得到：可以用两个单端可伸缩数组来实现一个双端可伸缩数组。

可伸缩数组的标准教材解法，例如 Cormen 等人的书中介绍的，是倍增技术，也叫几何扩张/收缩技术。开始时，分配一个具有某个初始大小的数组。如果数组满了，就分配一个新的数组，比如大小是原来的两倍，然后把所有元素复制过去。当数组比如不到四分之一满时，就分配一个新的数组，比如大小是原来的一半，然后再次把所有元素复制过去。在这两种情况下，旧数组使用的内存都会被释放，并且可以用于其他目的。一个简单的摊还分析可以说明，每次大小增加或减少的摊还代价是 `O(1)`。每个元素仍然可以在 `O(1)` 最坏情况时间内被访问和修改。此外，通过一种标准的后台重建技术，增加或减少操作的时间也可以做到 `O(1)` 最坏情况时间。

根据 Wikipedia，几何扩张/收缩技术被用于实现 Java 的 `ArrayList`、Python 的 `PyListObject`、C++ STL 的 `Vector` 等，其中增长因子范围从 `1.25` 到 `2`。

倍增技术允许用 `O(N)` 空间存储一个大小为 `N` 的可伸缩数组。这很好，在许多应用中也可能足够。但这也意味着，在某些时刻，分配给数组的内存中最多可能有 `50%` 没有被使用；即使只考虑增长操作，也会这样。更一般地，如果使用 `1 + α` 作为增长因子，那么可能有 `1 / (1 + α)` 比例的内存被浪费。通过选择很小的 `α`，浪费空间的比例可以变得任意小；但一次大小增加操作的摊还代价会变成：

```text
O((1 + α) / α)
```

也就是越来越大。

是否可能用 `N + o(N)` 空间存储一个大小为 `N` 的可伸缩数组，同时仍然保持每次操作 `O(1)` 时间？Sitarski 和 Brodnik 等人给出了肯定回答：他们描述了一些简单实现，只使用：

```text
N + O(√N)
```

空间。更多细节见第 3 节。Brodnik 等人还说明，在某种意义上，这是最优的。更具体地说，他们证明，任何可伸缩数组实现都必须在某些时候使用：

```text
N + Ω(√N)
```

空间。我们会在第 4 节回顾他们的简单论证。

我们提出一种区分方式：区分“当前大小为 `N` 的可伸缩数组，为了存储自身并支持 `O(1)` 最坏情况时间的取出和修改所需的空间”，以及“为了在 `O(1)` 摊还时间内增加或减少数组大小而可能临时需要的空间”。在许多场景中，更紧凑地表示数组是更可取的，即使调整数组大小时需要临时使用更多存储空间。例如，一个应用可能使用很多个可伸缩数组，但在任意给定时刻只有其中一个正在被调整大小。

我们第一个“突破” Brodnik 等人下界的实现是一个简单实现：它只使用：

```text
N + O(N^{1/3})
```

空间来存储数组，同时在调整数组大小时临时使用：

```text
N + O(N^{2/3})
```

空间。元素可以在 `O(1)` 最坏情况时间内被访问，增加或减少数组大小需要 `O(1)` 摊还时间。此外，我们还证明，这个结果本质上是最优的。任何只使用：

```text
N + O(N^{1/3})
```

空间来存储数组的实现，都必须在某些调整大小的时刻使用：

```text
N + Ω(N^{2/3})
```

空间。

更一般地，我们证明，对于每个 `r ≥ 2`，都可以用：

```text
N + O(N^{1/r})
```

空间存储一个可伸缩数组，同时只需要：

```text
N + O(N^{1−1/r})
```

空间来调整它的大小。通过下标取出和修改元素需要 `O(1)` 最坏情况时间，而调整大小需要 `O(r)` 摊还时间。由于不能使用后台重建，所以调整大小的 `O(r)` 摊还时间无法变成最坏情况时间；我们还会证明这个 `O(r)` 是最优的。只有当 `r = 2` 时例外，此时该实现会变得非常类似于 Sitarski 和 Brodnik 等人的实现。

论文其余部分安排如下。第 2 节更精确地定义问题以及所使用的计算模型。第 3 节描述已有工作，主要是 Sitarski 和 Brodnik 等人的工作。第 4 节描述 Brodnik 等人的简单下界，以及我们对它的扩展。第 5 节给出简单的 `r = 3` 解法，也就是用：

```text
N + O(N^{1/3})
```

空间来存储，用：

```text
N + O(N^{2/3})
```

空间来调整大小。第 6 节描述对于任意 `r ≥ 2` 的实现，它使用：

```text
N + O(rN^{1/r})
```

空间来存储数组，同时在调整大小操作期间只使用：

```text
N + O(N^{1−1/r})
```

空间。`grow` 和 `shrink` 操作的摊还代价是 `O(r)`。第 7 节给出一个简单变换，可以用于调整第 6 节的实现，把存储数组所需空间降低到：

```text
N + O(N^{1/r})
```

同时保持常数时间访问，以及 `grow` 和 `shrink` 操作的 `O(r)` 摊还时间。注意，只有当考虑 `r = r(N)` 随 `N` 增长时，这一点才有显著意义。为了证明调整大小操作的 `O(r)` 摊还时间是最优的，至少对于一大类我们称为标准的数据结构是如此，我们在第 8 节引入一个抽象的增长游戏，并对它进行完整分析。第 9 节依靠增长游戏的分析得到一个 `Ω(r)` 下界：如果只能使用：

```text
N + O(N^{1/r})
```

空间来存储数组，那么 `grow` 操作的摊还代价至少是 `Ω(r)`。最后，第 10 节给出一些结论性讨论。

---

## 2. 逻辑拆解

### 2.1 论文从最基础的数据结构切入

作者先说数组和可伸缩数组非常常见，但还有理论问题没有讲完。

这里的重点是：

```text
数组很常见
可伸缩数组很常见
但空间效率仍然可以继续改进
```

这为论文动机服务：研究对象很基础，但结果还有新东西。

---

### 2.2 固定数组的定义

数组大小为 `N`，元素是：

```text
a_0, a_1, ..., a_{N-1}
```

第 `i` 个元素可以在常数时间访问：

```text
0 ≤ i < N
access(i) = O(1)
modify(i) = O(1)
```

这里的 `word` 可以直观理解为机器内存中的一个基本存储单元，足够存一个元素或一个指针。

固定数组依赖一块连续内存：

```text
[ a_0 ][ a_1 ][ a_2 ] ... [ a_{N-1} ]
```

这样机器可以通过地址计算直接访问：

```text
base_address + i
```

所以访问是 `O(1)`。

---

### 2.3 可伸缩数组为什么困难

可伸缩数组要同时满足两件事：

```text
大小可以变
按下标访问仍然 O(1)
```

缩小数组比较简单，因为可以直接少用一部分空间：

```text
[ used ][ used ][ used ][ unused ][ unused ]
```

但这样会浪费内存。

增长更麻烦，因为原数组后面的内存可能已经被别人占用：

```text
[ array block ][ other data ]
```

所以不能直接在末尾追加一个 word。

这就是为什么需要动态内存管理：可能要申请新空间、复制元素、释放旧空间。

---

### 2.4 本文先讨论单端增长/收缩

论文大部分只考虑在数组“远端”操作：

```text
grow:   N -> N + 1, 加入 a_N
shrink: N -> N - 1, 删除 a_{N-1}
```

这类似栈：

```text
push / pop
```

但它仍然支持按下标访问，所以比普通栈更强。

作者还说，两端版本也能处理。方法是用两个单端可伸缩数组实现一个双端可伸缩数组：

```text
front side  用一个数组
back side   用一个数组
```

所以论文只分析单端情况，已经足够说明核心问题。

---

### 2.5 标准倍增法

标准方法是：

```text
满了：申请 2 倍大小的新数组，复制全部元素
太空了：申请 1/2 大小的新数组，复制全部元素
```

例如：

```text
容量 4 满了：
[ a0 a1 a2 a3 ]

申请容量 8：
[ a0 a1 a2 a3 _ _ _ _ ]
```

单次扩容可能要复制 `N` 个元素，所以最坏单次很贵。

但摊还来看，每个元素不会频繁被复制，所以每次操作平均是：

```text
O(1) amortized time
```

访问仍然是：

```text
O(1) worst-case time
```

因为最终还是数组式访问。

---

### 2.6 标准倍增法的空间问题

倍增法使用 `O(N)` 空间，但常数可能大。

如果容量是 `2N` 左右，实际只存 `N` 个元素，那么可能浪费接近一半空间：

```text
[ used N ][ unused almost N ]
```

如果增长因子是：

```text
1 + α
```

那么 `α` 越小，空间越紧凑；但扩容越频繁，摊还代价越大。

论文给出的代价是：

```text
O((1 + α) / α)
```

当 `α` 很小时，比如 `α = 0.01`，这个值大约是 `101`，操作会变慢。

所以这里出现第一个核心权衡：

```text
增长因子大：操作快，浪费空间多
增长因子小：空间省，操作变慢
```

---

### 2.7 已有结果：`N + O(√N)`

作者接着问：

```text
能不能只用 N + o(N) 空间，同时保持每次操作 O(1)？
```

这里：

```text
o(N)
```

表示比 `N` 小得多的低阶项。

例如：

```text
√N = o(N)
```

因为当 `N` 很大时，`√N / N -> 0`。

Sitarski 和 Brodnik 等人已经做到：

```text
N + O(√N)
```

这比倍增法的 `O(N)` 更精确、更省空间。

Brodnik 等人还证明，在某种意义下必须使用：

```text
N + Ω(√N)
```

也就是额外空间至少要有 `√N` 级别。

---

### 2.8 本文的关键新视角：区分常驻空间和临时空间

作者提出：

```text
存储和访问数组所需空间
调整大小时临时所需空间
```

这两者分开算。

直观例子：

一个程序中有很多个动态数组：

```text
A1, A2, A3, ..., Ak
```

大多数时候它们只是被访问，很少同时扩容。

所以我们更关心每个数组平时占多少空间；扩容时临时多用一点空间可以接受，因为同一时刻可能只有一个数组在扩容。

---

### 2.9 第一个新结果：`r = 3`

论文先给一个简单版本：

```text
存储空间：N + O(N^{1/3})
临时 resize 空间：N + O(N^{2/3})
访问：O(1) worst-case
增长/收缩：O(1) amortized
```

这个结果“突破” Brodnik 等人的下界，是因为 Brodnik 的下界把所有空间混在一起看。

本文分开看以后：

```text
平时存储空间可以少于 N + O(√N)
```

比如：

```text
N^{1/3} < √N
```

但代价是 resize 时临时空间变大：

```text
N^{2/3}
```

作者还证明这个搭配本质最优：

```text
如果平时只用 N + O(N^{1/3})
resize 时就必须有 N + Ω(N^{2/3})
```

---

### 2.10 一般化结果：任意 `r ≥ 2`

一般形式是：

```text
存储空间：N + O(N^{1/r})
resize 临时空间：N + O(N^{1−1/r})
访问/修改：O(1) worst-case
resize：O(r) amortized
```

几个例子：

```text
r = 2:
存储额外空间 O(N^{1/2})
临时额外空间 O(N^{1/2})
resize 摊还时间 O(1)

r = 3:
存储额外空间 O(N^{1/3})
临时额外空间 O(N^{2/3})
resize 摊还时间 O(1)，因为 O(3) 也是 O(1)

r = 4:
存储额外空间 O(N^{1/4})
临时额外空间 O(N^{3/4})
resize 摊还时间 O(4)
```

注意：当 `r` 被当成固定常数时，`O(r)` 也可以看作 `O(1)`。但如果允许 `r` 随 `N` 增长，例如 `r = log N`，那 `O(r)` 就不能看作常数。

---

### 2.11 为什么不能用后台重建

标准动态数组可以通过后台重建，把扩容复制工作拆散到后续多个操作里，从而做到最坏情况 `O(1)`。

本文说对于他们的一般方案：

```text
background rebuilding cannot be used
```

所以 resize 的 `O(r)` 只能是摊还时间。

只有：

```text
r = 2
```

时可以做到类似已有方案，也能有最坏情况保证。

---

### 2.12 后文结构

这段最后是论文路线图：

```text
Section 2: 定义问题和计算模型
Section 3: 回顾已有工作
Section 4: 讲 Brodnik 下界和本文扩展
Section 5: 给 r = 3 的简单方案
Section 6: 给任意 r ≥ 2 的通用方案
Section 7: 调整方案，把空间从 N + O(rN^{1/r}) 降到 N + O(N^{1/r})
Section 8: 定义并分析 growth game
Section 9: 用 growth game 证明 Ω(r) 下界
Section 10: 结论
```

重点是：

```text
Section 5-7 给上界，也就是构造算法
Section 8-9 给下界，也就是证明做不到更好
```

---

## 3. 这段和上下文的关系

这是论文第 1 节，引言。它的作用是：

1. 定义本文研究的对象：`resizable array`。
2. 回顾标准倍增法及其空间浪费。
3. 引出已有最优结果：`N + O(√N)`。
4. 提出本文的新区分：常驻空间 vs 临时 resize 空间。
5. 给出本文主要结果：

   ```text
   N + O(N^{1/r}) 存储空间
   N + O(N^{1−1/r}) resize 空间
   O(1) access
   O(r) amortized resize
   ```
6. 预告后文结构：先构造，再证明下界。

这一节暂时没有真正展开证明，主要是在搭建问题框架。

---

## 4. 专有名词与记号

| 术语 / 记号                         | 含义                            |
| ------------------------------- | ----------------------------- |
| `array`                         | 数组，固定大小，支持按下标常数时间访问           |
| `resizable array`               | 可伸缩数组，大小可以增减，同时支持按下标访问        |
| `dynamic array`                 | 动态数组，`resizable array` 的另一个名字 |
| `dynamic table`                 | 动态表，也是可伸缩数组的别称                |
| `N`                             | 当前数组大小，也就是当前元素数量              |
| `a_i`                           | 数组中的第 `i` 个元素                 |
| `word`                          | 一个机器字，内存中的基本存储单元              |
| `random access`                 | 随机访问，能通过地址计算直接访问任意位置          |
| `contiguous block`              | 连续内存块                         |
| `grow`                          | 增长操作，数组大小从 `N` 变成 `N + 1`     |
| `shrink`                        | 收缩操作，数组大小从 `N` 变成 `N − 1`     |
| `far end`                       | 远端，指数组末尾，也就是 `a_{N-1}` 后面     |
| `stack`                         | 栈，只在一端插入和删除                   |
| `deque`                         | 双端队列，两端都能插入和删除                |
| `doubling technique`            | 倍增技术，满了就申请约两倍容量               |
| `geometric expansion/shrinking` | 几何扩张/收缩，每次按固定比例扩大或缩小          |
| `growth factor`                 | 增长因子，例如 `2` 或 `1.25`          |
| `α`                             | 增长因子 `1 + α` 中的参数             |
| `amortized cost`                | 摊还代价，多次操作平均后的代价               |
| `worst-case time`               | 最坏情况时间，每一次操作都满足的时间上界          |
| `background rebuilding`         | 后台重建，把一次大规模搬迁拆散到多次操作里完成       |
| `N + o(N)`                      | 只比 `N` 多低阶空间，额外空间相对 `N` 趋近于 0 |
| `N + O(√N)`                     | 实际元素 `N` 加上 `√N` 级别额外空间       |
| `N + Ω(√N)`                     | 至少需要 `√N` 级别额外空间              |
| `resize`                        | 调整大小，包括 `grow` 和 `shrink`     |
| `r ≥ 2`                         | 本文引入的可调算法参数                   |
| `growth game`                   | 增长游戏，用来证明下界的抽象模型              |
| `standard data structures`      | 论文后面定义的一类“标准”数据结构             |

这节要抓住一个主线：作者把“平时存数组的空间”和“扩缩容时临时用的空间”分开算，于是得到了比 `N + O(√N)` 更小的常驻空间。
