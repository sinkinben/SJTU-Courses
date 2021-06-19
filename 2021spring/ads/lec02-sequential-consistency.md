## Sequential Consistency

ADS Course note, lecture-2.

## What is Consistency

> Consistency model defines rules for the apparent order and visibility of updates, and it is a continuum with tradeoffs.
>
> --Todd Lipcon

Consistency 和 Coherence 的直观认识：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210316141501.png" style="width:60%;" />

Consistency Model 就是说，在分布式系统应对高并发的场景时，每个节点对于数据的读和写，应该采取些什么样的策略，而不同的策略会导致不同的强度的 Consistency 。

对于 Consistency Model 来说：

- 没有绝对正确或者错误的 Model，只能在 ease of programmability 和 efficiency 之间取舍。
- 在分布式场景下，需要解决的问题：Data replication (Caching), Concurrency (No shared lock), Failures (machine or network)

## Naive Distributed Shared Memory

|                          Naive DSM                           |                           Example                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210316142320.png"  /> | <img src = "https://gitee.com/sinkinben/pic-go/raw/master/img/20210316142411.png" /> |



## Consistency Models

强一致性模型：

- **Strict Consistency**: Each operation under strict consistency is stamped with a global wall-lock time.
- **Sequential Consistency**: Global wall-clock -> *total order of ops*

弱一致性模型：

- **Eventual Consistency**
- **Causal Consistency**
- **Client-centric Consistency**



### Strict Consistency

Strict Consistency: each operation under strict consistency is stamped with a global wall-lock time.

Consistency rules:

- For all nodes, each `read` gets the **latest** write value.
- All operations at one CPU have timestamp in **execution order**.

**Strict consistency can not be achieved.** It is not practical because no **global wall-clock** is available.



### Sequential Consistency

Sequential Consistency is weaker than strict consistency.

Rules:

- Global wall-clock -> *total order of ops*
- Each CPUs' ops appear in order
- **ALL** CPUs see results according to total order

Properties:

- All read/write ops follow total ordering
- Read must see result latest write

据说是 [Lesile Lamport](http://www.lamport.org) 提出的概念，最早用于描述 Multi-Core CPU 的并发行为。大意是说，如果可以找到一个所有 Core 执行指令的总序列，该序列中每个 Core 要执行指令的顺序得以保持，且实际的 Core 执行结果与该序列的执行结果一致，则称该次执行达到了顺序一致性。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210316153526.png" style="width:80%;" />

上图中的 Reordered 序列就是所谓的「总序列」，对于 Sequential Consistency，需要满足 2 点要求：

- 对于每个 Core 的操作序列，它们在 Reordered 中的相对顺序不变。
- 每个 Core 的执行结果，与 Reordered 的执行结果一致。

Sequential Consistency 属于强一致性，也是一种不可能实现的一致性模型（可参考 Refs [2] 给出的例子）。



## Ivy: Shared Virtual Memory

这一概念出自一篇论文：[Memory Coherence in Shared Virtual Memory Systems](https://www.cs.utexas.edu/~dahlin/Classes/GradOS/papers/p321-li.pdf)

真没耐心看，饶了我吧 😅 。

还提出了一种内存一致性模型：

> Release Consistency: Introduce a special type of variables, synchronization variables or locks.
>
> Release Consistency delay changes visible to other processors until certain synchronization access occurs.

该不会锁这种机制就出自这篇论文吧，蒸馏啊 😅 。

Ivy 这个系统的 Contribution:

- Provides a shared-memory system across  a group of workstations
- Easier to write parallel/distributed programs (compared with using message passing)



## Q&A

1. What are the differences between *consistency, consensus* and *coherence* ?

> Single object consistency is called **coherence**, and consistency across multiple objects.

Consistency 一般是指分布式存储系统中的「一致性」，更具体地讲是指 Consisteny Model . 所谓的 Consistency Model 就是说，在某个数据项 X 发生变化后，不同的 Client A 和 B，在其后的同一时刻，看到的 X 是分别是什么样的。个人理解，Consistency Model 更像是一种分布式数据库的同步策略。

Coherence 一般是指 Cache Coherence 。分布式系统有两种通信模型，消息通信模型（message-passing system）和共享内存模型（shared-memory model）。consistency model 来自于前者， cache coherence 来自于后者。

Cache Coherence 解决的是 Multi-Core CPU 访问内存的同步问题。由于现代计算机采用的都是 3 级 Cache 的架构，其中，CPU 的每个 Core 的 L1-Cache 都是独立的，那么在访存的时候，就会出现常见的并发同步问题。

譬如，Core-1 在它的 L1-Cache 中更新了内存地址 `x` 的内容，并且完成写回操作。但对于 CPU 的其他 Core 来说，它们并不知晓 `x` 已被修改，依然认为自己的 L1-Cache 中的 `x` 是最新的。那么，这些 Core 访问内存 `x` 的时候，得到的应该是新值还是旧值，这就是 Cache Coherence Model 解决的问题。更进一步地，如果 Core-1 的 L1-Cache 的写回操作还没完成，对于其他 Core 访问 `x` 的时候，又应该是什么样的策略呢？这些问题属于 Cache Coherence 范畴的。

Consensus 在某些场合也翻译为「一致性」，其单词的意思是「共识」，Consensus 一般用于形容 Paxos, Raft 这些算法的性质。





## References

- [1] [分布式系统：Coherence，Consistency and Serializable](https://zhuanlan.zhihu.com/p/342470321)
- [2] [什么是顺序一致性](https://lotabout.me/2019/QQA-What-is-Sequential-Consistency/)

