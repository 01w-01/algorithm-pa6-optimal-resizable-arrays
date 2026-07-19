6 ：An (O(rN^{1/r}), O(N^{1-1/r}))-implementation
- 把上一节r=3的特例推广到任意 r ≥ 2
- 使用 B, B², ..., B^{r-1} 大小的数据块，其中 B ≈ N^{1/r}，每层最多 2B 个块
- 即类似模拟冗余 B 进制计数器，每种大小至多2B个
- 合并：2B个 B^i 大小的块 → 1 个 B^{i+1} 大小的块，每2B个向前进一
- 拆分：1 个 B^k 大小的块 → B 个 B^{k-1} 大小的块 + ... + B 个 B 大小的块


7 ：A Data Structuring Transformation
- 把 O(rN^{1/r}) 优化到 O(N^{1/r})
- 简单证明：如果某个大块长度 b > t(N)，不一次性分配它，而是分成 ⌈b/t(N)⌉ 个大小为 t(N) 的小块，但是需要额外的指针来指向这些块，但指针数量 ≤ (N + s(N))/t(N)，即小于总大小除以块大小


8 ：The Growth Game
- 定义一个单人博弈来抽象 standard 实现的 grow 操作
- (N, k, ℓ)-博弈：
  - k 个子数组，最多 ℓ 个空位
  - 三种操作：(a) 有空位 → 直接放入，(b) 有空子数组 → 分配新数组，(c) 所有 k 个子数组都满 → 合并 i 个子数组
  - 代价 = 1 + 被移动的元素数
- 先归约到 ℓ=0（引理 8.2：进行 (N, k, ℓ)-增长博弈等价于在大小为 ℓ + 1 的元素块上进行 (N/(ℓ+1), k, 0)-增长博弈）
- (N, k)-增长博弈精确解（定理 8.6）：
  - C_{N,k} = (N+1)n - C(n+k, k+1)，其中 C(n+k-1, k) - 1 ≤ N ≤ C(n+k, k) - 1
  - 最优状态满足 C(n+i-2, i) ≤ a_i ≤ C(n+i-1, i)
- 二项式计数器（binomial counter）
  - 状态值 a_i = C(b_i + i - 1, i)， b_i 是计数器上各位的值
  - 基数变化的 B 进制计数器

9 ：Lower Bound on Amortized Cost of Grow Operations
- 用8节的分析结果证明：对任何 standard 实现，若仅用 N + O(N^{1/r}) 存储，grow 的 amortized cost 为 Ω(r)

10 ：Concluding Remarks
- 论文的总结段落