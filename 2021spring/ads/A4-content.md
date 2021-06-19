考试 A4 纸内容。



## Consistency

- Strict consistency
  - 每个指令/操作都有一个全局的时间戳，按照时间戳的先后次序执行。
- Sequential consistency
  - 每个 Core 都有一个指令序列，在总的执行序列中，每个 Core 的指令相对顺序不变
  - 每个 Core 的执行结果，与总序列的执行结果一致。
  - Ivy: Distributed
    - Provides a shared-memory system across  a group of workstations
    - Easier to write parallel/distributed programs (compared with using message passing)
    - Each processor’s local memory keeps a subset of all pages
    - If page not found in local memory, request from remote node
- Release Consistency：跟互斥锁 (Mutex) 类似。
- Eventual consistency