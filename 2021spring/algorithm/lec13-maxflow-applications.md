## Max Flow Problems: Applications

本节的主要内容是介绍 Max Flow 的主要应用场景（即把其他问题规约到 Max Flow 问题来解决）：

- Edge-Disjoint Paths
- Network Connectivity (Menger’s Theorem)
- Disjoint Paths in Undirected Graphs
- Multiple Sources and Sinks in Max-flow
- Circulation with Supplies and Demands
- Survey Desgin
- Airline Scheduling
- Project Selection





## Edge-Disjoint Paths

**Definition**

> - **Edge-disjoint** - Two paths are edge-disjoint if they have no edge in common.
> - **Edge-disjoint paths problem** - Given a diagraph $G=(V,E)$ and two vertices $s$ and $t$, find the max number of edge-disjoint $s \rightarrow t$ paths.
> - Max-flow formulation - Assign **unit capacity** to every edge.

**Theorem** 1-1 correspondence between $k$ edge-disjoint $s \rightarrow t$ paths in $G$ and integral flows of value $k$ in $G'$.

> **Proof** $\Rightarrow$
>
> - Let $P_1, \dots, P_k$ be $k$ edge-disjoint $s \rightarrow t$ paths in $G$ .
> - Set $f(e) = 1$ if edge $e$ participates in some paths $P_j$ (at least one path) , otherwise 0 .
> - Since paths are disjoint, $f$ is a flow of value $k$ (but not max-flow yet) .
>
> **Proof** $\Leftarrow$
>
> - Let $f$ be an integral flow in $G'$ with value $k$.
> - Consider edge $(s, u)$ with $f(s, u) = 1$ . 
>   - by flow conservation, there exists an edge $(u, v)$ with $f(u, v) = 1$ . 
>   - continue until reach $t$, always choosing a new edge.
>   - 大白话：对于 $f(e) = 1$ 这样的边，总是存在后继边 $e'$，且 $f(e')=1$ ，直到路径延伸到汇点 $t$ .
> - Produces $k$ (not necessarily simple) edge-disjoint paths.
>   - 由于每个边的容量都是 1 ，因此能够保证存在 $k$ 个上述的路径。

**Corollary** We can solve edge-disjoint paths problem via max-flow formulation.

> **Proof**
>
> - Integrality theorem $\Rightarrow$ there exists a max-flow $f^*$ in $G'$ that is integral.
> - 1-1 correspondence $\Rightarrow$ $f^*$ corresponds to max number of edge-disjoint $s \rightarrow t$ paths in $G$.



## Network Connectivity

**Definition**

> - **Dis-connectivity** - A set of edges $F \sube E$ disconnects $t$ from $s$ if every $s \rightarrow t$ path uses at least one edge in $F$.
> - **Network connectivity** - Given a digraph $G = (V,E)$ and two nodes $s$ and $t$, find minimal number of edges whose removal disconnects $t$ from $s$.

我感觉，这个 $F$ 集合就是有向图的最小割集（之一）。



**Menger's Theorem**

> The max number of edge-disjoint $s \rightarrow t$ paths equals the min number of edges whose removal disconnects $t$ from $s$.
>
> **Proof** $\le$ 
>
> - Suppose the removal of $F \sube E$ disconnects $t$ from $s$, and $|F| = k$.
> - Every $s \rightarrow t$ path use at least one edge in $F$ (according to its definition) .
> - Hence the number of edge-disjoint paths is $\le k$ .
>
> **Proof** $\ge$
>
> - Suppose max number of edge-disjoint $s \rightarrow t$ paths is $k$.
> - Then value of max-flow is $k$ .
> - **Max-flow Mini-cut Theorem** $\Rightarrow$ There exists a cut $(A, B)$ of capacity $k$ .
> - Let $F$ be the set of edges going from $A$ to $B$.
> - We can see that $|F| = k$ and it disconnects $t$ from $s$ .

通过证明 Menger's Theorem ，可以通过 Max-Flow 求解最大不相交路径的数量，该数量即为 Network Connectivity 问题的解。



## Disjoint Paths in Undirected Graphs

**Definition**

> - **Edge-disjoint paths problem in undirected graphs** - Given a graph $G = (V,E)$ and two nodes $s$ and $t$, find the max number of edge-disjoint $s \rightarrow t$ paths.
> - **Max-flow formulation** - Replace each edge with two antiparallel edges and assign unit capacity to every edge.
> - **Observation** - Two paths $P_1$ and $P_2$ may be edge-disjoint in the digraph but not edge-disjoint in the undirected graph (because of the anti-parallel edges) .

**Lemma** In any flow network, there exists a maximum flow $f$ in which for each pair of anti-parallel edge $e$ and $e'$: either $f(e) = 0$ or $f(e') = 0$ or both. Moreover, integrality theorem still holds.

> **Proof** [by induction on number of such pairs]
>
> - Suppose $f(e) > 0$ and $f(e') > 0$ for a pair of anti-parallel edges $e$ and $e'$ .
> - Set $f(e) = f(e) - \delta$ and $f(e') = f(e') - \delta$, where $\delta = \min \{f(e), f(e')\}$ .
> - $f$ is still a flow of the same value but has one fewer such pair.
>
> 这里没看明白，为什么流量减少之后，流函数 $f$ 的流量依旧不变？

**Theorem** Max number of edge-disjoint $s \rightarrow t$ paths = value of max-flow.

> **Proof** Similar to proof in digraphs, use the lemma above.





## Multiple Sources and Sinks

**Definition**

> Given a digraph $G = (V,E)$ with edge capacities $c(e) \ge 0$ and multiple source nodes and multiple sink nodes, find max flow that can be sent from the source nodes to the sink nodes.

**Max-flow formulation**

- Add a new source node $s$ and sink node $t$.
- For each original source node $s_i$ , add edge $s \rightarrow s_i$ with capacity $\infin$.
- For each original sink node $t_j$ , add edge $t_j \rightarrow t$ with capacity $\infin$ .



## Circulation with Supplies and Demands

这一节就要吐槽 PPT 做的不好了，总是从教材里面「断章取义」，截取一个片段，看得让人云里雾里的。

下面用大白话来讲一下 Circulation Problem ：给定一个网络流图 $G=(V,E)$ 和它的源点集合 sources ，汇点集合 sinks，假设 sources 中节点均可以生产商品 (Supplies) ，sinks 中的节点需要得到某个数量的商品 (Demands) ，求出一个网络流方案（即一个流函数 $f$ ），从 sources 出发，到达并满足 sinks 的商品数量要求。

形式化描述：

> Assume that all capacities and demands are integers. Given flow network $G=(V,E)$ with capacities on edges. Now associated with each node $v \in V$ is a demand $d_v$.
>
> - If $d_v > 0$, this indicates that node $v$ has a demand $d_v$ for flow.
>   - If this node is a sink, and it wishes to receive $d_v$ units more flow than it sends outs
> - If $d_v < 0$, this indicates that node $v$ has a supply $-d_v$.
>   - If this node is a source, and it wished to send out $-d_v$ units more flow than it receives.
> - If $d_v = 0$, then the node neither a source nor a sink.
>
> We use $S$ to denote the set of all nodes with negative demand and $T$ to denote the set of all nodes with positive demand. 

一般来说，$d_v > 0$ 的节点是 sinks 节点，$d_v < 0$ 的节点是 sources 节点。

$\forall{v \in S}$，$v$ 虽然是提供流量的，但也允许流入流量，流入的流量需要从 $v \rightarrow x$ 这样的边流出去，在这样的情况下，从 $v$ 出去的流量是 $-d_v+\sum_{\text{into }v} f(v)$ 。对于 $T$ 中的节点也是同理。

Circulation 的数学定义：

> Given a digraph $G = (V,E)$ with edge capacities $c(e) \ge 0$ and node demands $d(v)$, a circulation is a function $f(e)$ that satisfies:
>
> - For each $e \in E$, $0 \le f(e) \le c(e)$ .
>
> - For each $v \in V$, 
>
>   $$
>   \sum_{e \text{ into } v} f(e) - \sum_{e \text{ out of } v} f(e) = d(v)
>   $$

显然，这时候问题已经不是一个 Maximization Problem，而是一个 Feasibility Problem ，但仍然可以规约到最大流问题去解决。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210509140029.png" style="width:70%;" />

图 (a) 所示是一个 Circulation 问题的实例，方格中的数字代表一个解，边上的数值代表容量。

如图 (b) 所示，将 Circulation 问题规约到最大流问题：

- Add new source $s$ and sink $t$.
- For each $v$ with $d(v) < 0$, add edge $(s, v)$ with capacity $−d(v)$.
- For each $v$ with $d(v) > 0$, add edge $(v, t)$ with capacity $d(v)$.

最后在转换得到的 $G'$ 中做一遍最大流，即可得到原问题的解。



### Lower Bounds

如果我们给每个边都限定一个最低流量 $l(e)$ ，即每条边至少需要流过 $l(e)$ 的流量，那么该怎么解决呢？

> Given a digraph $G = (V,E, l, c, d)$ with edge capacities $c(e) \ge 0$ , lower bound $l(e) \ge 0$, and node demands $d(v)$, a circulation is a function $f(e)$ that satisfies:
>
> - For each $e \in E$, $l(e) \le f(e) \le c(e)$ .
>
> - For each $v \in V$, 
>
>   $$
>   \sum_{e \text{ into } v} f(e) - \sum_{e \text{ out of } v} f(e) = d(v)
>   $$
>
> **Circulation problem with lower bounds** - Given $G = (V,E,l,c,d)$, does there exist a feasible circulation?

对于这样的情况，我们可以对每个边 $e = v \rightarrow w$ 做如下的转换：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210509141131.png" style="width:67%;" />






## Survey Design

Consider a company that sells $k$ products and has a database containing the purchase histories of $n$ customers.

- A customer can only be asked about products that he/she has purchased.
- Each customer $i$ should be asked about a number of products between $c_i$ and $c_i'$.
  - 每个顾客回答的问题数目在 $[c_i, c_i']$ 这个区间内，每个问题与不同的产品相关。
- There must be between $p_j$ and $p_j'$ distinct customers asked about each product $j$.
  - 在调查结果中，每个产品必须有 $k (k \in [p_j, p_j'])$ 个不同的客户回答过相关问题。 

如果把产品和客户都看作是一个点，购买记录表示二者之间的一条边，那么可以得到一个二分图。如果 $c_i = c_i' = p_j = p_j' = 1$，那么问题就转换为求解**二分图的完美匹配**。

下面把 Survey Design Problem 规约到最大流问题（如下图所示）。

- $t \rightarrow s$ 这条边表示的是问题的总数。
- 令所有点的 $d_v = 0$ 。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210509143858.png" style="width:67%;" />

那么问题就转换为 Circulation with Lower Bounds 。





## Airline Scheduling

问题描述：

- 每个航班 (Flight) 由 4 个参数组成，起点、终点、出发时间、到达时间，即 $(origin_i, dest_i, s_i, t_i)$ .
- 每个航班飞行都需要一组工作人员 (Crews) ，一组工作人员可以无限次飞行（😅万恶的资本主义社会），但显然的是，一组人员无法同时在 2 个航班上。
- 给定 $n$ 个航班的参数，求完成这些航班需要的最少的工作人员的组数。

这种情况下，可以把问题构造成为一个 Circulation 。



## Project Selection

这里 PPT 就写得很好，一页之内把要点都总结了。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210509150144.png"  style="width:77%;" />

- 给定一个图 $G=(V,E)$，其中 $V$ 是 Project 集合，每个项目均有一个 $p_v$ 代表这个 Project 的收益（可正可负）。
- $E$ 代表 Project 的依赖关系，$e = v \rightarrow w$ 代表 $v$ 依赖于 $w$，要想选择 $v$ ，那么 $w$ 也必须被选择。
- 求解一个闭包集合 (Closure) $A$，使得 $A$ 的所有项目的收益之和最大。
  - $A$ 具有的性质：$\forall{i \in A}$ ，对于与 $i$ 关联的每个边 $(i, j) \in E$，均有 $j \in A$ 。

把 Project 集合分为正收益和负收益 2 个子集，可规约到最小割问题解决。

- Add source vertex $s$ and sink vertex $t$, and define $p_s = p_t = 0$ .
- Assign a capacity of $\infin$ to each prerequisite edge.
- Add edge $(s, v)$ with capacity $p_v$ if $p_v > 0$.
- Add edge $(v, t)$ with capacity $-p_v$ if $p_v < 0$.

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210509151355.png" style="width:67%;" />

令 $C = \sum_{i \in P \text{ and }p_i > 0} p_i$ ，可以发现上图的最大流最多为 $C$ ，因此可以通过最大流算法解决。

最后是解的对应关系：

> $(A, B)$ is min cut **iff** $A − {s}$ is an optimal set of projects.
>
> - Infinite capacity edges ensure $A − {s}$ is feasible.
>
> - Max revenue because:
>   $$
>   \begin{aligned}
>   \text{cap}(A,B) &= \sum_{v \in B: p_v > 0} p_v + \sum_{v \in A: p_v < 0} (-p_v) \\
>   &= \sum_{v:p_v>0} p_v - \sum_{v \in A} p_v
>   \end{aligned}
>   $$
>
> 😅 证明我也看不懂的，PPT 就放 2 个要点，还能看得懂个 🔨 噢。



## References

- [1] KT05 - Algorithm Design (Chapter 7)