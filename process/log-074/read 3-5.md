3 ：Previous Work
- 主要讲的是当前比较优的可变数组的结构
- 3.1讲了最省空间的是每次等比扩容，但时间 O(N)
- 3.2 讲了Sitarski 的 HAT：√B × B 的二维结构，N + O(√N) 空间，O(1) amortized
- 3.3 Brodnik et al.：数据块大小为 1,2,3,... 或 2 的幂次，N + O(√N) 空间，O(1) worst-case

4 ：The Lower Bound of Brodnik et al. and Its Extension
- 定理 4.1：任何可变数组在某些时刻必须使用 N + Ω(√N) 空间
  - 证明思路：如果块数 k ≥ √N，则指针本身占用 √N 空间；如果 k < √N，则最大块必 > √N，分配时造成浪费
- 定理 4.2（扩展下界）：s(N) · t(N) ≥ N ，即使只支持 grow 和 access 操作
- 推论：若 s(N) = O(N^{1/r})，则 t(N) = Ω(N^{1-1/r})

5 ：Simple (O(N^{1/3}), O(N^{2/3}))-implementations（r=3 的特例）
- 先构造块大小为 N^{2/3} 的 HAT，但最后一块可能几乎为空，浪费 N^{2/3} 空间
- 于是对最后一块再套一层 HAT（块大小为 N^{1/3}），把浪费降到 O(N^{1/3})（递归思想）
- 大的数据块总是满，只有最小的那一层有不完全满的块