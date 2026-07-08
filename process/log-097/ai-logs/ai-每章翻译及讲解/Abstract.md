```
Abstract
A resizable array is an array that can grow and shrink by the addition or removal of items
from its end, or both its ends, while still supporting constant-time access to each item stored in
the array given its index. Since the size of an array, i.e., the number of items in it, varies over
time, space-efficient maintenance of a resizable array requires dynamic memory management.
A standard doubling technique allows the maintenance of an array of size N using only O(N)
space, with O(1) amortized time, or even O(1) worst-case time, per operation. Sitarski and
Brodnik et al. describe much better solutions that maintain a resizable array of size N using
only N + O(

√

N) space, still with O(1) time per operation. Brodnik et al. give a simple proof
that this is best possible.
We distinguish between the space needed for storing a resizable array, and accessing its items, and
the temporary space that may be needed while growing or shrinking the array. For every integer
r ≥ 2, we show that N + O(N1/r) space is sufficient for storing and accessing an array of size N,
if N + O(N1−1/r) space can be used briefly during grow and shrink operations. Accessing an
item by index takes O(1) worst-case time while grow and shrink operations take O(r) amortized
time. Using an exact analysis of a growth game, we show that for any data structure from a
wide class of data structures that uses only N + O(N1/r) space to store the array, the amortized
cost of grow is Ω(r), even if only grow and access operations are allowed. The time for grow and
shrink operations cannot be made worst-case, unless r = 2.
```

## 1. 中文翻译

**摘要**

可伸缩数组是一种数组。它可以通过在末端添加或删除元素来增长或收缩，也可以支持在两个端点添加或删除元素来增长或收缩；同时，只要给定数组中某个元素的下标，它仍然支持在常数时间内访问该元素。

由于数组的大小，也就是数组中元素的数量，会随着时间变化，要以节省空间的方式维护一个可伸缩数组，就需要动态内存管理。

一种标准的“倍增技术”可以用 `O(N)` 空间维护一个大小为 `N` 的数组，并且每次操作的时间为 `O(1)` 摊还时间，甚至可以达到每次操作 `O(1)` 最坏情况时间。Sitarski 和 Brodnik 等人描述了更好的解法：它们可以只用 `N + O(√N)` 空间维护一个大小为 `N` 的可伸缩数组，同时仍然保证每次操作 `O(1)` 时间。Brodnik 等人还给出了一个简单证明，说明这个结果已经是最好的可能结果。

我们区分两类空间：一类是用于存储可伸缩数组以及访问其中元素所需的空间；另一类是在数组增长或收缩过程中可能临时需要的空间。

对于任意整数 `r ≥ 2`，我们证明：如果在增长和收缩操作期间可以短暂使用 `N + O(N^{1−1/r})` 空间，那么只需要 `N + O(N^{1/r})` 空间，就足以存储并访问一个大小为 `N` 的数组。按下标访问一个元素需要 `O(1)` 最坏情况时间，而增长和收缩操作需要 `O(r)` 摊还时间。

通过对一个“增长游戏”进行精确分析，我们证明：对于一个很宽泛的数据结构类别中的任意数据结构，只要它只使用 `N + O(N^{1/r})` 空间来存储数组，那么 `grow` 操作的摊还代价就是 `Ω(r)`；即使只允许 `grow` 和 `access` 操作，这个下界也成立。除非 `r = 2`，否则增长和收缩操作的时间无法做到最坏情况界限。

---

## 2. 逐句解释与逻辑拆解

这段是论文摘要，主要交代问题、已有结果、本文贡献和下界结论。

### 第一部分：什么是 resizable array

原文：

> A resizable array is an array that can grow and shrink by the addition or removal of items from its end, or both its ends, while still supporting constant-time access to each item stored in the array given its index.

意思是：

可伸缩数组既要像数组一样支持：

```text
access(i)  ->  在 O(1) 时间访问第 i 个元素
```

又要像栈或双端队列一样支持在端点改变大小：

```text
grow / push      在末端加入元素
shrink / pop     从末端删除元素
```

有些版本还支持两端操作：

```text
push_front / pop_front
push_back  / pop_back
```

关键要求是：**数组大小可以变，但按下标访问仍然要快。**

---

### 第二部分：为什么需要动态内存管理

原文：

> Since the size of an array, i.e., the number of items in it, varies over time, space-efficient maintenance of a resizable array requires dynamic memory management.

普通数组通常是一整块连续内存，大小固定。可伸缩数组的元素数量 `N` 会变化，所以数据结构需要在增长或收缩时重新安排内存。

这里的 `N` 表示：

```text
N = 当前数组中存储的元素数量
```

作者关心的是：维护这样一个数组时，除了 `N` 个元素本身，还需要多少额外空间。

---

### 第三部分：标准倍增技术

原文：

> A standard doubling technique allows the maintenance of an array of size N using only O(N) space, with O(1) amortized time, or even O(1) worst-case time, per operation.

**standard doubling technique** 指标准倍增法。

常见做法是：

```text
当前容量满了，就申请一个 2 倍大小的新数组；
然后把旧数组元素复制过去。
```

比如容量变化：

```text
1 -> 2 -> 4 -> 8 -> 16 -> ...
```

它有两个结论：

```text
空间：O(N)
摊还时间：O(1)
```

`O(N)` 空间的意思是：总容量和当前元素数 `N` 同阶，可能有接近一半空间闲置，但数量级仍是线性的。

`O(1) amortized time` 是**常数摊还时间**。它表示某次扩容可能很贵，例如复制 `N` 个元素；但把很多次操作平均起来，每次操作的平均代价是常数。

---

### 第四部分：已有更优空间结果

原文：

> Sitarski and Brodnik et al. describe much better solutions that maintain a resizable array of size N using only N + O(√N) space, still with O(1) time per operation.

已有工作能做到：

```text
空间：N + O(√N)
时间：每次操作 O(1)
```

这比 `O(N)` 空间更精细。

对比一下：

```text
标准倍增法：容量可能是 2N 级别
更好方案：只比 N 多 O(√N)
```

这里的 `N + O(√N)` 表示：

* `N` 是存储实际元素必须要用的空间；
* `O(√N)` 是额外浪费或辅助空间。

当 `N` 很大时，`√N` 远小于 `N`，所以空间利用率更高。

---

### 第五部分：已有下界

原文：

> Brodnik et al. give a simple proof that this is best possible.

意思是 Brodnik 等人证明了：

```text
N + O(√N)
```

在他们考虑的模型或条件下已经是最优的。

这里的 “best possible” 通常指存在某种下界：想要同时满足那些操作时间要求，就不能把额外空间压得比 `O(√N)` 更低。

---

### 第六部分：本文的关键区分

原文：

> We distinguish between the space needed for storing a resizable array, and accessing its items, and the temporary space that may be needed while growing or shrinking the array.

作者开始提出本文的新视角：区分两种空间。

第一类是**常驻空间**：

```text
存储数组 + 支持 access 操作所需的空间
```

第二类是**临时空间**：

```text
grow / shrink 过程中短暂使用的空间
```

这个区分很重要。以前可能把所有空间统一算在一起；本文允许在扩容或缩容的短暂过程中使用更多空间，但操作完成后常驻空间很小。

---

### 第七部分：本文的上界结果

原文：

> For every integer r ≥ 2, we show that N + O(N1/r) space is sufficient for storing and accessing an array of size N, if N + O(N1−1/r) space can be used briefly during grow and shrink operations.

这里排版里的 `N1/r` 和 `N1−1/r` 应读作：

```text
N^{1/r}
N^{1−1/r}
```

结论是，对于任意整数：

```text
r ≥ 2
```

可以做到：

```text
常驻空间：N + O(N^{1/r})
临时空间：N + O(N^{1−1/r})
```

`r` 是一个可调参数。

举几个例子：

| `r` | 常驻额外空间 `O(N^{1/r})` | 临时额外空间 `O(N^{1−1/r})` |
| --: | ------------------: | --------------------: |
| `2` |             `O(√N)` |               `O(√N)` |
| `3` |        `O(N^{1/3})` |          `O(N^{2/3})` |
| `4` |        `O(N^{1/4})` |          `O(N^{3/4})` |

所以 `r` 越大：

```text
常驻额外空间越小；
临时额外空间越大；
操作摊还时间也越大。
```

这是一个空间与时间之间的权衡。

---

### 第八部分：时间复杂度

原文：

> Accessing an item by index takes O(1) worst-case time while grow and shrink operations take O(r) amortized time.

作者的结构保证：

```text
access(i)：O(1) 最坏情况时间
grow / shrink：O(r) 摊还时间
```

这里区分了两种时间保证：

* `O(1) worst-case time`：每一次访问都一定是常数时间；
* `O(r) amortized time`：增长和收缩操作平均下来每次是 `O(r)`，单次可能更贵。

---

### 第九部分：本文的下界结果

原文：

> Using an exact analysis of a growth game, we show that for any data structure from a wide class of data structures that uses only N + O(N1/r) space to store the array, the amortized cost of grow is Ω(r), even if only grow and access operations are allowed.

这里作者说他们分析了一个 **growth game**，也就是“增长游戏”。这应该是论文后面定义的一种抽象模型，用来刻画数组增长时不可避免的代价。

他们证明：

```text
如果常驻空间只有 N + O(N^{1/r})
那么 grow 的摊还代价至少是 Ω(r)
```

这里的 `Ω(r)` 是下界，意思是至少需要和 `r` 成正比的代价。

这个结论说明前面的上界：

```text
grow / shrink：O(r) amortized time
```

在数量级上是最优的，因为下界已经是：

```text
grow：Ω(r) amortized cost
```

也就是：

```text
上界 O(r)
下界 Ω(r)
```

两者匹配。

---

### 第十部分：最坏情况时间的限制

原文：

> The time for grow and shrink operations cannot be made worst-case, unless r = 2.

意思是：

对于 `r > 2` 的情况，`grow` 和 `shrink` 的 `O(r)` 时间只能做到摊还意义上的保证，无法保证每一次操作都在 `O(r)` 最坏情况时间内完成。

只有特殊情况：

```text
r = 2
```

可以把增长和收缩操作做成最坏情况时间界限。

---

## 3. 这段和论文上下文的关系

这是摘要，作用是把论文的核心故事提前说明：

1. 研究对象是 **可伸缩数组**；
2. 目标是在支持 `O(1)` 下标访问的同时，尽量减少额外空间；
3. 已有结果做到 `N + O(√N)` 空间和 `O(1)` 时间；
4. 本文提出更细的空间分类：常驻空间和临时空间；
5. 本文给出一个参数化结果：

```text
常驻空间：N + O(N^{1/r})
临时空间：N + O(N^{1−1/r})
access：O(1) worst-case
grow / shrink：O(r) amortized
```

6. 本文还证明这个 `O(r)` 摊还时间是最优数量级。

所以摘要已经给出了全文主线：**构造一个更省常驻空间的可伸缩数组，并证明其时间代价无法进一步改进。**

---

## 4. 专有名词与记号

| 术语 / 记号                       | 含义                          |
| ----------------------------- | --------------------------- |
| `resizable array`             | 可伸缩数组，大小可增长或收缩，同时支持按下标访问    |
| `grow`                        | 增长操作，通常是在数组末端加入一个元素         |
| `shrink`                      | 收缩操作，通常是从数组末端删除一个元素         |
| `access`                      | 按下标访问元素                     |
| `N`                           | 当前数组中的元素数量                  |
| `O(f(N))`                     | 渐进上界，表示数量级最多和 `f(N)` 同阶     |
| `Ω(f(N))`                     | 渐进下界，表示数量级至少和 `f(N)` 同阶     |
| `O(1)`                        | 常数时间或常数空间，与 `N` 无关          |
| `amortized time`              | 摊还时间，把一串操作的总代价平均到每次操作上      |
| `worst-case time`             | 最坏情况时间，每一次单独操作都满足的上界        |
| `standard doubling technique` | 标准倍增法，容量满时申请约两倍容量的新数组       |
| `N + O(√N)`                   | 实际元素需要 `N` 空间，额外空间为 `O(√N)` |
| `r ≥ 2`                       | 本文中的可调整数参数，控制空间和时间的权衡       |
| `growth game`                 | 增长游戏，论文中用于证明下界的抽象分析工具       |

这一段最重要的公式是：

```text
常驻空间：N + O(N^{1/r})
临时空间：N + O(N^{1−1/r})
grow / shrink 时间：O(r) amortized
grow 下界：Ω(r)
```

读后文时建议抓住 `r` 这个参数：它控制“空间省多少”和“操作慢多少”之间的交换关系。
