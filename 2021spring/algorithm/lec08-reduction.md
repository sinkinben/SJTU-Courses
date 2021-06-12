## Lec07 - Reduction

Refs:
- [1] [Algorithms - DPV](http://algorithmics.lsi.upc.edu/docs/Dasgupta-Papadimitriou-Vazirani.pdf) 

**Notes** - 我不知道 "reduce / reduction" 的中文翻译是「规约」还是「归约」，但行文如果一直用 "reduce" 这个词，文风就很奇怪，所以这里使用「规约」一词作为中文翻译，因为我的苹果输入法已经记住这个词了。

在前面的 Lecture 中提到，SAT 已被证明为是一个 NPC 问题，这就意味着：任何一个 NP 问题都能规约为 SAT 问题。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210416192117.png" style="width:70%;" />

这节课的主要内容就是上图的几个例子，「简单」介绍一下规约的 trick:

- Rudrata path => Rudrata cycle
- 3-SAT => Independent Set
- SAT => 3-SAT
- Independent Set => Vertex Cover
- Independent Set => Clique
- 3D-Matching => ZOE
- ZOE => Subset Sum
- ZOE => ILP

TODO:

- 3-SAT => 3D-Matching
- ZOE => Rudrata Cycle
- Any Problem => SAT





## Rudrata path => Rudrata cycle

关于 Rudrata cycle 问题，可以参考 `Lec05.md` .

如图所示，假设问题是要在 $G(V,E)$ 中寻找一个 Rudrata path ，那么可以构造 $G'(V', E')$ ，并且令 $V'=V \cup \{x\}$ ，$ E' = E \cup \{(x,s), (x,t)\}$  . 那么，如果 $G'$ 存在 Rudrata cycle ，我们把 $x$ 从 $G'$ 中去除，即可解决在 $G$ 中寻找 Rudrata path 的问题。当然，把 Rudrata cycle 规约为 Rudrata path 也是可以的。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210416192757.png" style="width:80%;" />



## 3-SAT => Independent Set

SAT 和独立集的相关概念参考 `Lec05.md` 。

**3-SAT **

给定一个 3-SAT 的 CNF 表达式，寻找一组变量 $x_i$ 的赋值，使得该 CNF 为真。以下为一个 3-SAT 表达式例子：
$$
(\overline{x} \lor y \lor \overline{z}) \land
({x} \lor \overline{y} \lor {z}) \land
({x} \lor {y} \lor {z}) \land
(\overline{x} \lor \overline{y})
$$
**Independent Set**

给定图 $G(V,E)$ 和整数 $g$ ，是否存在一个包含 $g$ 个顶点的独立集 $S$ 。

下面阐述 3-SAT 规约为 Independent Set 的方法。

- 对于 3-SAT 的 CNF 表达式，其每个子句 (Clause) 的长度都小于等于 3 ，即最多只包含 3 个文字 (Literal) 。只需要从 3 个 Literal 中任选一个（赋值为 True），那么该子句即可为 True。每个子句用图来表示，就是一个三角形。
- 如果某个子句中选择了 $x$ ，那么在其他子句中就不能选择 $\overline{x}$ 。因此，把 $x$ 与位于其他三角形的 $\overline{x}$ 连接一条边。

以上述 CNF 为例，最终得到的图 $G$ 如下：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210416201951.png" style="width:80%;" />

显然，3-SAT 的解就是在图 $G$ 中寻找一个大小为 $g$ 的独立集 $S$ 。$g$ 是 3-SAT 的 CNF 表达式的子句个数，此处 $g=4$ 。最终：

- 如果 $S$ 包含了 $x$ ，说明 CNF 中 $x$ 应为 True ；
- 如果 $S$ 包含了 $\overline{x}$ ，说明 CNF 中 $x$ 应为 False ；
- 如果 $S$ 二者均不包含，说明 CNF 的真值与 $x$ 无关，$x$ 可任意赋值。

此处的逆命题也是成立的，即：如果已知一组变量 $X$ 的赋值，它能够使得 3-SAT 的 CNF 表达式为真，那么 CNF 对应的图 $G$ 具有独立集 $S$ 。$S$ 的大小是 $X$ 中**有唯一赋值**的变量个数（因为存在某些变量，赋值 T/F 均不影响 CNF 的真值）。



## SAT => 3-SAT

正如 Refs [1] 里面所述：

> This is an interesting and common kind of reduction, from a problem to a *special case* of itself.

显然，合取符号 $\land$ 是具有分配律的，即：
$$
k \land (a \lor b) = (k \land a) \lor (k \land b)
$$
通过真值表易证。

对于任意 SAT 的一个子句，我们设为：$C = (a_1 \lor a_2 \lor \dots \lor a_k)$ ，其中 $k \ge 3 $  。我们有以下的等价变换：
$$
C' = (a_1 \lor a_2 \lor y_1) \land
(\overline{y_1} \lor a_3 \lor y_2) \land
(\overline{y_2} \lor a_4 \lor y_3) \land \dots \land
(\overline{y}_{k-3} \or a_{k-1} \lor a_k)
$$
 其中，$y_i$ 是新增的变量（下面说明其作用），在给定一组变量 $a_i$ 的条件下，可以证明存在一组 $y_i$ 使得 $C = C'$ : 

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210416212600.png" style="width:70%;" />

- 证明 1 ：若 $C'$ 为真 ，那么至少有一个 $a_i$ 为真（即 $C$ 为真）。
  - 反证法证明。如果 $a_i$ 均为假，那么从 $C'$ 第 1 项看， $y_1$ 必须为真；那么可推出第 2 项中 $y_2$ 为真；以此类推，在倒数第二项中，$y_{k-3}$ 为真，最后一项 $(\overline{y}_{k-3} \lor a_{k-1} \lor a_{k})$ 必然为假，导致 $C'$ 为假，与已知条件矛盾。
- 证明 2 ：若 $C$ 为真，那么 $C'$ 为真。
  - $C$ 为真，说明至少有一个 $a_i$ 为真。在 $C'$ 中，令与这样的 $a_i$ 搭配的那些 $y$ 为假（不带帽子的 $y$），即可使得 $C'$ 为真。

显然，对于任意的 SAT 表达式，对于它的**长度大于 3 的子句**，都进行如上的变换，最终可以得到一个 3-SAT 表达式。时间复杂度为 $\sum_{i=1}^{n}k_i$ ，其中 $n$ 是 SAT 的子句个数，这显然是一个多项式时间。



## Independent Set => Vertex Cover

**Vertex Cover**: 给定图 $G$ 和整数 $b$ ，问：是否存在一个 $b$ 个顶点的集合，覆盖 $G$ 所有的边。

只从问题的定义上看，**最大独立集问题**与**最小点覆盖问题**就已经很相似了。

> A set of nodes $S$ is a vertex cover of graph $G = (V,E)$ **iff** the remaining nodes, $V − S$, are an independent set of $G$.

这意味着：最小点覆盖集合 $S$ ，那么 $V-S$ 是最大独立集。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210417132857.png"  style="width:70%;" />





## Independent Set => Clique

团 (Clique) 问题：给定一个有 $n$ 顶点的图 $G=(V,E)$，以及一个非负整数 $k$ ，在 $G$ 中寻找一个点数等于 $k$ 的完全图（当然也是 $G$ 的子图）。

假设 $S$ 是给定的图 $G=(V,E)$ 的一个独立集，构造 $G$ 的补图 $\overline{G} = (V, \overline{E})$ ，其中 $\overline{E}$ 是 $V$ 对应的完全图的边（任意 2 点均有一边），减去 $E$ 所得的集合。根据独立集和团的定义，那么有：

> A set of nodes $S$ is an independent set of $G$ **iff** $S$ is a clique of $\overline{G}$.



## 3-SAT => 3D Matching

TODO: It's too long to learn.





## 3D Matching => ZOE

**ZOE (Zero-One Equations)**

给定一个 $m \times n$ 的 01 矩阵 $\textbf{A}$，找到一个 $n \times 1$ 维的矩阵 (aka, colume vector) 的 $\textbf{x}$ ，使得 $\textbf{Ax} = \textbf{1}$ .

下面阐述 3D Matching 转换为 ZOE 的规约方法。

> Assume that: In 3D-Matching, there are $m$ boys, $m$ girls, $m$ pets, and $n$ `<bog, gril, pet>` triples.

定义一组 01 变量 $\textbf{x} = (x_1, \dots, x_n)$  ，如果 $x_i = 1$ ，说明第 $i$ 个三元组被选择到匹配集当中，否则该三元组不被选择。

对于每一个男生 $b_i$ ，包含该 $b_i$ 三元组的三元组分别标号为 $j_1, j_2, \dots, j_k$ ，由于一个匹配当中，只能且必须包含 $b_i$ 一次，那么有：
$$
x_{j_1} + x_{j_2} + \dots + x_{j_k} = 1
$$
对于女生和宠物而言，也是如此。

那么对于上述的一组变量 $\textbf{x}$ ，可以得到 $3m$ 个这样的方程，我们把这 $3m$ 个方程按照男生，女生，宠物的顺序（三者顺序可任意）排列，再写成矩阵的形式，其实就可以得到：求出解向量 $\textbf{x}$ ，使得 $\textbf{Ax} = \textbf{1}$ .

下图是 3D-Matching 转换为 ZOE 的一个例子。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210417142121.png" style="width:67%;" />

对于左边的 3D-Matching 的例子，一共给定 9 个待匹配对象（用圆表示），待选择的三元组一共 5 个（用三角形表示）。

对于矩阵 $\textbf{A}$ 而言，一共 9 行，分别代表 3D-Matching 中的 9 个对象；一共 5 列，代表给出的三元组集合中有 5 个元素。



## ZOE => Subset Sum

**Subset Sum** - Find a subset of a given set of integers that adds up to exactly $W$.

这 2 个问题都是 ILP 问题的一个特殊情形。

- ZOE 的解向量 $\textbf{x}$ 是 $n$ 维的，其中的每一个元素只能取 0 或者 1 ，解空间为 $2^n$ 。
- Subset Sum 中，给定集合的每一个元素，要么取，要么不取，集合大小为 $n$ ，解空间也是 $2^n$ 。

从直觉上而言，二者似乎是等价的（实际上也是如此），下面阐述规约的过程。

在 ZOE 问题中，给定一个 $m \times n$ 的矩阵 $\textbf{A}$ ，需要求解 $\textbf{x}$ 使得 $\textbf{Ax} = \textbf{1}$ ：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210417143632.png" />

以上述的矩阵为例，它的一个解是 $\textbf{x} = [1 \ 1 \ 0 \ 1]$ .

实际上解向量 $\textbf{x}$ 的中某个 $x_i$ 的含义为：是否选择 $\textbf{A}$ 的第 $i$ 列（根据矩阵乘法的定义）。

如果把 $\textbf{A}$ 的每一列看作一个二进制数，即：
$$
\begin{aligned}
(10010)_2 &= 18 \\
(00101)_2 &= 5  \\
(00100)_2 &= 4  \\
(01000)_2 &= 8
\end{aligned}
$$
而我们的目标是得到一个 5 维的全 1 向量 $\textbf{1} = (11111)_2 = 31$ ，那么问题就可以转换为：从集合 $\{18, 5, 4, 8\}$ 中选取若干元素，这些元素的和是 31 ，这就是 Subset Sum 问题。



## ZOE => ILP

回顾 ILP 问题：

> In ILP we are looking for an integer vector $\textbf{x}$ that satisfies $\textbf{Ax} \le \textbf{b}$, for given matrix $\textbf{A}$ and vector $\textbf{b}$.

那么，怎么把 ZOE 中的等式规约为 ILP 中的不等式呢？

ZOE 实际上是在求解一组 $\textbf{x} = \{x_1, \dots, x_n\}$ ，其中，每个 $x_i$ 为 0 或者 1 ，那么显然，可以得到一组约束条件：
$$
\begin{aligned}
-x_i &\le 0 \\
x_1 &\le 1
\end{aligned}
$$
此外，对于 ZOE 的任意一个方程 $\sum{x_i} = 1$ ，比如 $x_1 + x_3 + x_4 = 1$ , 同样可以写成：
$$
\begin{aligned}
-(x_1 + x_3 + x_4) &\le -1 \\
x_1 + x_3 + x_4 &\le 1
\end{aligned}
$$
对于 ZOE 的每一个变量 $x_i$ 和所有的等式方程，都进行上述的改写操作，最终的问题就「泛化」为一个 ILP 问题。



## ZOE => Rudrata Cycle

Rudrata Cycle 问题是在一个**非完全图**中，找到一个哈密顿环路。

TODO: It's too long to learn.



## Rudrata cycle => TSP

Rudrata cycle 是 TSP 的一个变种，它到 TSP 到规约其实是很简单的：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210417155916.png" style="width:67%;" />

下面讨论 $\alpha$ 的取值：

- 如果 $\alpha = 1$ ，那么 TSP 中所有的边均为 1 或者 2 。对于不同的城市 $i,j,k$ ，必然有 $d_{ij} + d_{jk} \ge d_{ik}$ 。因此，贪心策略得到的路径必然是最优的，我们只需要找到一个长度为 $n$ 环路即可。

- 如果 $\alpha$ 是一个很大的值，那么上述 $d_{ij} + d_{jk} \ge d_{ik}$ 的性质不成立。所以 TSP 的解只能是以下的 2 种情况之一：

  - TSP 环路的权值为 $n$ 。

  - TSP 环路的权值至少为 $n + \alpha$ 。

注意到，如果 $\alpha > 1$ ，这里 $[n+1, n+\alpha)$ 这一区间的值是不可能为 TSP 的解。根据 Refs [1] 的 Chapter 9，这一 "Gap" 意味着：除非 $P=NP$ 得到证明，否则不可能通过贪心或者近似算法去求解 TSP 问题。



## Summary

Reduce 这样一个过程，从英文单词的含义上理解，理应是把一个问题 $P$ 简化为 $P'$ ，使得它更容易解决。

然而往往并非如此，比如

- NPC 问题是所有 NP 问题的 Reduction 的结果，这里的 "Reduction" 试图把多个问题「汇总」为一个问题 $P'$ ，这样的本意是：只要我们能解决 $P'$ ，那么我们就能解决 Reduction 前的所有 NP 问题。
- ZOE 从 01 **等式**方程规约为一般的、更具普遍性的**不等式**方程。

因此，从这个角度来看，Reduction 并没有把问题的难度简化，反而有可能增加其困难程度。也就是说，$P$ 经过 Reduction 得到的 $P'$，其难度至少是**「大于等于」**$P$ 的。

