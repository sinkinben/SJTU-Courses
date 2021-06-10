## Summary

**NP 问题与 Reduction**

- 如何证明一个问题 $L$ 是 NP 问题？
  - 根据定义证明：给出一个多项式时间的验证算法（注意不需要证明 L 无法在多项式内解决，这并不是 NP 问题的要求）。
  - 证明 L 属于 P 问题：给出一个多项式的求解算法。
  - 证明 L 是 NPC 问题：把一个已证明是 NPC 的问题 $L'$ 规约到 $L$。

**近似算法**

- Tight Example 需要对于所有的 $n$ 都成立，不能是一个具体的 example 。
- 证明近似因子为 $k$：
  - 最小化问题，证明 $OPT \le \mathcal{A} \le k \cdot OPT$ .
  - 最大化问题，证明 $OPT \ge \mathcal{A} \ge \frac{1}{k} OPT$ .
  - 随机算法， $\textbf{E}[\mathcal{A}]$ 替代 $\mathcal{A}$ 去证明。



**LP 问题**

- 写 Dual 时，如果 Primal LP 的目标函数有常数项，那么在 Dual 同样保留这个常数项（并且符号也不需要改变）。
  - Dual 记得要：`max -> min`，并且 $\le \quad \rightarrow \quad \ge$ .

