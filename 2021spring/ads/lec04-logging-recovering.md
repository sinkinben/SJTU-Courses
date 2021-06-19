## Crash Recovering and Logging

ADS Course, lecture-04, at March-15, 2021.

**影子副本**

- 拷贝一份数据 $F_{shadow}$ ，真正的修改在 $F_{curr}$ 上执行
- 如果出错，那么用 $F_{shadow}$ 覆盖被写过的 $F_{curr}$ 恢复到原来的状态
- 如果没出错，那么用成功执行后的 $F_{curr}$ 覆盖 $F_{shadow}$

影子副本存在的问题：

- 如果存在两个事务 $T_1, T_2$ ，$T_1$ commit 之后可能保存了 $T_2$ 的中间状态



**Checkpoint**

- 如果存在很长的事务，出现 Crash 时，我们不希望恢复到最近的一次 commit，而是恢复到中间的状态，即 checkpoint 
- 出现 Crash 时，需要恢复到最近的 checkpoint ，如果事务已经 Commit ，那么需要 REDO；如果还没有 Commit，那么需要 UNDO 。
