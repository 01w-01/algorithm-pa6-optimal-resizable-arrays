1 ：Introduction
- 可变数组:最广泛使用的数据结构之一
- 倍增法缺陷：使用 O(N) 空间存储 N 个元素，浪费可达 50%
- Sitarski (1996) 和 Brodnik et al. (1999)：N + O(√N) 空间，O(1) 时间
- Brodnik et al. 证明 √N 的额外空间是下界（无法改进）
- 本论文把空间分为：
  - 存储空间：正常运行时维护数组所需的空间
  - 临时空间：grow/shrink 操作期间短暂需要的额外空间
- 对任意 r ≥ 2，只需 N + O(N^{1/r}) 的存储空间，临时使用 N + O(N^{1-1/r}) 空间
  - 访问：O(1) worst-case
  - 扩容：O(r) amortized
  - 下界：任何只用 N + O(N^{1/r}) 存储的数据结构，扩容的 amortized cost 必须是 Ω(r)

2 ：Resizable Arrays
- 定义下列操作接口：Array(), Length(), Get(i), Set(i,a), Grow(a), Shrink()
- 操作类似栈在数组末尾进行，但扩展为双端
- 块分为存数据、存指针、存元信息三类
- 定义 2.1：(s(N), t(N))-implementation：存储 N 个元素用 ≤ N + s(N) 空间，grow/shrink 操作期间用 ≤ N + t(N) 空间
