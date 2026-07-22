# TODO — Optimal Resizable Arrays Review

> **仓库现状**：`process/log-074/` 有 4 份全文章节笔记，`process/log-097/` 有 1 份综合笔记 + 5 个 AI 翻译/背景文件。**没有统一的 review 正文**。
> **老师判定**：材料很多但无正文的组 → 下次检查时默认分支必须有可阅读、可归属、逐次形成的统一 review。
> **禁令**：不再往 `ai-logs/` 新增翻译或资料文件。

---

## 最终交付物

在仓库根目录下新建 `review/`，包含：

| 文件 | 内容 |
|------|------|
| `review/main.md` | 统一 review 正文（~9 节） |
| `review/references.md` | 正式引用列表 |
| `review/contributions.md` | 成员贡献说明 |

---

## 分工

### 成员 A（074）— 基于已有笔记撰写技术核心

| # | 任务 | 素材来源 | 产出（写入 `review/main.md` 对应节） |
|---|------|----------|--------------------------------------|
| ✅A1 | 撰写 §4 下界证明：Theorem 4.1 抽屉原理、Theorem 4.2 s(N)·t(N) ≥ N 完整推导、Theorem 4.3 代入 r 的推论 | `read 3-5.md` | §4 |
| ✅A2 | 撰写 §5 r=3 特例构造：两级数据块结构、Grow/Shrink 操作流程、不变量与正确性论证 | `read 3-5.md` | §5 |
| ✅A3 | 撰写 §6 一般 r 级构造：B 进制计数器模拟、Combine/Split/Rebuild 操作的完整描述、Credit 分析的每一步（Combine 消耗元素自身 credit、Split 消耗 level 积累 credit、Rebuild 消耗全局 credit） | `read 6-10.md` | §6 |
| A4 | 撰写 §7 空间优化变换：将 O(rN^{1/r}) 压到 O(N^{1/r}) 的分块技巧 | `read 6-10.md` | §7 |
| A5 | 撰写 §8 Growth Game 精确解：博弈定义 → ℓ=0 归约 → 递推公式 → 定理 8.6 闭式解 → 二项式计数器的构造 | `read 6-10.md` | §8 |
| A6 | 撰写 §9 下界应用：从 Growth Game 到数据结构 Ω(r) amortized 下界（Theorem 9.1）的完整归约链条 | `read 6-10.md` | §9 |
| A7 | 撰写技术直觉与技术评价：为什么区分存储/临时空间是突破点、为什么多级 B 进制、为什么 2B 冗余、Credit 分析的妙处 | 自行总结 | §10-11 |

### 成员 B（097）— 基于已有笔记撰写框架 + 统稿

| # | 任务 | 素材来源 | 产出（写入对应文件） |
|---|------|----------|----------------------|
| ✅B1 | 撰写 §1 摘要与总体评价 | `笔记.md` §1-2 | `review/main.md` §1 |
| ✅B2 | 撰写 §2 问题定义与重要性：ADT 定义、内存模型、(s(N),t(N))-实现的形式化定义、为什么这个问题重要 | `笔记.md` §2 + `ai-logs/Resizable Arrays 背景知识.md` | `review/main.md` §2 |
| ✅B3 | 撰写 §3 相关工作：整理倍增法→HAT→Brodnik→Bille et al.→Goodrich & Kloss 的发展脉络，用对比表格呈现各方案的 空间/时间/临时空间/worst-case 特性 | `笔记.md` §3 + `ai-logs/Resizable Arrays 背景知识.md` | `review/main.md` §3 |
| B4 | 补充论文发表后的后续进展：`tzarrays` crate、`optarray` crate、OCaml `varray`，附实际 benchmark 数据 | 自行调研 | `review/main.md` §3.4 |
| B5 | 整理正式引用列表：所有引用的论文、crate、教材，统一格式 | 从全部笔记中提取 | `review/references.md` |
| B6 | 撰写局限性与开放问题、总结 | `笔记.md` §10 | `review/main.md` §12-13 |
| B7 | 撰写成员贡献说明：具体列出每人做了什么 | — | `review/contributions.md` |
| B8 | 统稿：合并 A1-A7 和 B1-B6 的全部章节 → 统一术语 → 修正笔误 → 确保可读性 | — | `review/main.md` 终稿 |

---

## 协同规则

1. **所有产出直接写入 `review/` 目录，提交到 `main` 分支**——不允许留在个人文件夹
2. **每完成一个任务就 commit**，message 格式：`review: <节号> <简述> (<作者>)`，确保 `git blame` 可归属
3. **统一术语**（开工前两人对齐）：
   - resize → 调整大小
   - amortized → 均摊
   - worst-case → 最坏情况
   - grow/shrink → 扩容/缩容
   - storage/temporary space → 存储空间/临时空间
4. **A 写完后 B 统稿**：B 的 B8 依赖 A1-A7 全部完成
5. **停止往 `ai-logs/` 和 `process/` 新增任何翻译/资料文件**

---

## 时间线

| 阶段 | 内容 | 时限 |
|------|------|------|
| 第一天 | B1-B3（引言+问题+相关工作）、A1-A2（下界+r=3） | 24h |
| 第二天 | A3-A6（一般构造+空间优化+Growth Game+下界应用）、B4-B6（后续进展+局限+总结） | 48h |
| 第三天 | A7（技术直觉与评价）、B5（引用列表）、B7（成员贡献） | 72h |
| 第四天 | B8 统稿 + 交叉校对 + 最终提交 | 96h |

---

## 完成验证

- [ ] `main` 分支 `review/main.md` 存在且完整（覆盖摘要→总结全部章节）
- [ ] 每个定理（4.1, 4.2, 6.1, 7.3, 8.6, 9.1）有证明框架或关键推导步骤
- [ ] §3 含相关工作对比表格
- [ ] §3.4 含论文发表后后续进展（tzarrays 等）
- [ ] 有独立的技术评价（非单纯复述论文，有自己的分析）
- [ ] `review/references.md` 引用列表格式统一
- [ ] `review/contributions.md` 具体列出每位成员的贡献
- [ ] 全文术语一致（按协同规则第 3 条）
- [ ] `git blame` 可追溯每节作者
- [ ] `process/log-097/ai-logs/` 不再有新文件
- [ ] `process/` 不再有新的翻译/资料文件
