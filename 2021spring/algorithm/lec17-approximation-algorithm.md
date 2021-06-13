## Approximation Algorithm

主要内容：

- 近似算法 (Approximation Algorithms)
- Tight Example
- Lower Bounding

2 个实例：

- Vertex Cover ( 2 个近似算法 )
  - 求一个 Maximal Matching ，把匹配选中的所有点作为点覆盖的结果，$2 \cdot OPT$
  - Layering Algorithm: $2 \cdot OPT$
- Set Cover ( 2 个近似算法 )
  - Set Cover 近似算法的近似因子要么是 $\log{n}$ ，要么是 $f$ ， $f$ 是某个元素在给定子集中出现的最大次数。
  - 朴素贪心算法： $\ln{n} \cdot OPT$ 
  - cost-effective 算法：$H_n \cdot OPT$

实际上，VC 是 SC 问题的一个特例。

> The vertex cover problem is a special case of set cover with the highest element occurence frequency $f = 2$ .
>
> <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210601203018.png" style="width:67%;" />









## Vertex Cover

问题描述：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210528204600.png" style="width:80%;" />



### Greedy

对于 Cardinality Vertex Cover ，一个近似算法为：

> Find a maximal matching in $G$ and output the set of matched vertices.
>
> <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210530194449.png" style="width:67%;" />

这是一个近似因子 (Approximation Factor) 为 2 的近似算法：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210530194707.png" style="width:67%;" />

对于近似因子，我们总是希望它越小越好的，如果能找到一个确切的下界 $k \cdot OPT$，这就是所谓的近似算法的 Lower Bounding .

对于一个完全二分图 $K_{n,n}$ ：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210530195836.png" />

上述算法得到的 $|M| = n$ ，即点覆盖的近似结果为 $2n$ （但点覆盖的最优结果是 $n$），这就是所谓的 Tight Example .

PS: 在任意二分图中，最大匹配的基数等于最小点覆盖的基数。



### Layering

这是一个近似因子为 2 的算法。

假设存在一个常数 $c$ 使得点覆盖中，每个点的权值 $w(v)$ 都满足 $w(v) = c \cdot deg(v)$  。

Lemma - Let $w: V \rightarrow \textbf{Q}^+ $ be a degree-weight function. Then $w(V) \le 2 \cdot OPT$ .

> **Proof**
>
> - Let $c$ be the constant such that $w(v) = c \cdot deg(v)$  and let $U$ be an optimal vertex cover in $G$. 
> - Since $U$ covers all the edges, then we have:
>
> $$
> \sum_{v \in V}deg(v) = 2|E| \ge |E|
> $$
>
> - Therefore $w(U) \ge c |E|$ where $w(U) = OPT$ .
> - Since $w(V) = 2c|E|$, thus
>
> $$
> OPT \ge \frac{1}{2} w(V) \Rightarrow w(V) \le 2 \cdot OPT
> $$



---

这算法长这样：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210602194904.png"  style="width:67%;" />

另一种表述为（来自 Refs[1] ，这种写法可以很容易改写为 SC 的 Layering 算法）：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210607153114.png" style="width:67%;" />

一个例子（顶点的数字表示权重）：

|                              1                               |                              2                               |                              3                               |                              4                               |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210602195855.png) | ![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210602195905.png) | ![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210602195914.png) | ![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210602195923.png) |

---

Theorem - The layer algorithm achieves an approximation guarantee of factor 2 for the vertex cover problem, assuming arbitrary vertex weights.

> **Proof**
>
> We need to show that set $C$ is a vertex cover for $G$ and $w(C) \le 2 · OPT $ .
>
> Prove that $C$ is a vertex cover:
>
> - Assume, for contradiction, that $C$ is not a vertex cover for $G$.
> - There must be an edge $(u,v)$ with $u \in D_i$ and $v \in D_j$ .
> - Assume that $i \le j$, then $(u, v)$ is in $G_i$, contradicting the fact that $u$ is a degree zero vertex.
>
> Prove that $w(C) \le 2 \cdot OPT$, let $C^*$ be the optimal vertex cover.
>
> - For $v \in C$, if $v \in W_j$, then
>
> $$
> w(v) = \sum_{i \le j}t_i(v)
> $$
>
> - For $v \in V-C$, if $v \in D_j$, then
>
> $$
> w(v) \ge \sum_{i<j}t_i(v)
> $$
>
> - In each layer $i$, $C^* \cap G_i$ is a vertex for $G_i$.
> - Thus by previous lemma:
>
> $$
> t_i(C \cap G_i) \le 2 \cdot t_i(C^* \cap G_i)
> $$
>
> - Therefore
>
> $$
> w(C) = \sum_{i=0}^{k-1}t_i(C \cap G) \le \sum_{i=0}^{k-1} 2 \cdot t_i(C^* \cap G_i) \le 2w(C^*) = 2OPT
> $$

----

这算法的一个 Tight Example 是：

![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210602201533.png)



## Set Cover

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210528210132.png" style="width:80%;" />

一个最简单的近似算法是**贪心**。每次选取花费最小的 $S_i$ ，然后从 $U$ 中去除 $S_i$ 的所有元素，重复这个过程直到 $U$ 是空集。

如果每个 $S_i$ 的花费都是 1 ，那么问题就称为 “Cardinality Set Cover” :

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210602184451.png" style="width:67%;" />

比如：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210530202511.png" style="width:67%;" />

- 对于每个点 $x$ ，在它 30 miles 内的点都和它相邻，用集合 $S_x$ 表示与 $x$ 相邻的点。
- 那么问题转换为：怎么选择 $S_x$ 使得所有的点都被覆盖（要求选取的 $S_x$ 尽可能少）？



### Performance Ratio

Lemma - Suppose $B$ contains $n$ elements and that the optimal cover consists of $OPT$ sets. Then the greedy algorithm will use at most  $\ln{n} \cdot OPT$  sets.

> **Proof**
>
> - Let $n_t$ be the number of elements still **not** covered after $t$ iterations of the greedy algorithm (so $n_0 = n$).
> - Since these remaining elements are covered by the optimal $OPT$ sets, there must exist be some sets with at least $n_t / OPT$ of them. (因为还剩下有 $n_t$ 个点需要覆盖，最多只能使用 $OPT$ 个集合，因此每个集合至少有 $n_t / OPT$ 个元素。)
> - Therefore, the greedy strategy will ensure that
>
> $$
> n_{t+1} \le n_t - \frac{n_t}{OPT} = n_t(1 - \frac{1}{OPT})
> $$
> - And by repeated application:
>
> $$
> n_t \le n_0(1-\frac{1}{OPT})^t
> $$
>
> - We have the inequality:
>
> $$
> 1-x \le e^{-x}, \quad \forall{x \in R}
> $$
>
> - Since $1/OPT \ne 0$, we have:
>
> $$
> n_t \le n_0(1-\frac{1}{OPT})^T < n_0e^{-\frac{t}{OPT}} = ne^{-\frac{t}{OPT}}
> $$
>
> - $n_t$ denotes the number of elements still **not** covered after $t$ iterations, thus $n_t < 1$ means **ALL** elements are covered. Thus we let $ne^{-\frac{t}{OPT}} = 1$, the we get $t = \ln{n} \cdot OPT $ .



### Greedy Algorithm

这个算法中的 Set Cover 是指带权值的 Cardinality Set Cover 。

Set Cover 有 2 种近似因子的算法，一种是 $O(\log{n})$，一种是 $O(f)$ . 在这里只介绍 $O(\log{n})$ 的 2 种算法，$O(f)$ 的算法请参考 Ex-5.1 。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210602190922.png" style="width:70%;" />

- $C$ 表示已经覆盖的元素。
- 根据该算法覆盖元素的顺序，依次标记这些元素为 $e_1, \dots, e_n$ ，$price(e_k)$ 表示覆盖某个元素的代价。
- $cost(S)$ 是根据所有的 $price(e), e\in S$ 来定义的。

---

Lemma - For each $k \in [1, n]$, $price(e_k) \le OPT/(n-k+1)$ .

> **Proof**
>
> - In any iteration, the leftover sets of the optimal solution can cover the remaining elements at a cost of at most $OPT$ .
> - Therefore, among these sets, there must be one having cost-effectiveness of at most $OPT/|\bar{C}|$ . And:
>
> $$
> price(e_k) \le \frac{OPT}{|\bar{C}|}
> $$
>
> - In the iteration in which element $e_k$ was covered, $\bar{C}$ contains at least $n-k+1$ elements. (前面 $k-1$ 次迭代，每次只覆盖了一个元素。)
>
> $$
> |\bar{C}| \ge n-k+1 \Rightarrow price(e_k) \le \frac{OPT}{|\bar{C}|} \le \frac{OPT}{n-k+1}
> $$



Thereom - The greedy algorithm is an $H_n$ factor approximation algorithm for the minimum set cover problem, where $H_n = 1 + \frac{1}{2}+ \dots +\frac{1}{n}$ .

> **Proof**
>
> Since the cost of each set picked is distributed among the new elements covered, the total cost of the set cover picked is equal to
> $$
> \sum_{k=1}^{n} price(e_k) \le OPT \cdot (\frac{1}{n} + \frac{1}{n-1} + \dots + 1)
> $$
> $H_n$ 是一个调和级数，$H_n = \ln{n} + R$ ，$R$ 是欧拉常数。

---

对于这一贪心算法，一个 Tight Example 如下：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210602193438.png" style="width:70%;" />



### Layering

参考 Ex-5.1 。SC 问题也是可以通过 Layering 来近似的，SC-Layering 的近似因子为 $f$ ， $f$ 是某个元素在给定子集中出现的最大次数。

初始的 $S_0$ 为 $S_0 = \mathcal{S}$ 。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210607151624.png" style="width:67%;" />








## References

- [1] http://www.tcs.hut.fi/Studies/T-79.7001/2008SPR/slides/wieringa_080207.pdf
- [2] Vaz04 - Approximation Algorithms (Chapter 2)
