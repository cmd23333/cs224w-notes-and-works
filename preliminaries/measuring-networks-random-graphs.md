---
layout: post
title: Measuring Networks and Random Graphs
header-includes:
   - \usepackage{amsmath}
---

## Measuring Networks via Network Properties 用网络的属性来度量这个网络
In this section, we study four key network properties to characterize a graph: **degree distribution, path length, clustering coefficient**, and **connected components**. Definitions will be presented for undirected graphs, but can be easily extended to directed graphs.

在这一节，我们将介绍网络的四个重要特征：度分布、路径长度、聚类系数、连通分量。由无向图来引入这些定义，可以很容易的延申到有向图。

### Degree Distribution 度分布
The **degree distribution** $$P(k)$$ measures the probability that a randomly chosen node has degree $$k$$. The degree distribution of a graph $$G$$ can be summarized by a normalized histogram, where we normalize the histogram by the total number of nodes.

We can compute the degree distribution of a graph by $$P(k) = N_k / N$$. Here, $$N_k$$ is the number of nodes with degree $$k$$ and $$N$$ is the number of nodes. One can think of degree distribution as the probability that a randomly chosen node has degree $$k$$.

To extend these definitions to a directed graph, compute separately both in-degree and out-degre distribution.

**度分布**  可以将度分布视为随机选择的节点具有度$ k $的概率。使用$$ P(k)= N_k / N $$计算图的度分布，其中$$ N_k $$是度数为$ k $的节点数，而$ N $是总节点数。要将这些定义扩展为有向图，则需要分别计算出度分布和入度分布。

### Paths in a Graph  图中的路径
A **path** is a sequence of nodes in which each node is linked to the next one:

路径是节点的序列，其中每个节点都与序列中的下一个节点连接：
$$
P_n = \{i_0, i_1, i_2, \dots, i_n\}
$$

such that $$\{(i_0, i_1), (i_1, i_2), (i_2, i_3), \dots, (i_{n-1}, i_n)\} \in E$$

The **distance (shortest path, geodesic)** between a pair of nodes is defined as the number of edges along the shortest path connecting the nodes. If two nodes are not connected, the distance is usually defined as infinite (or zero). One can also think of distance as the smallest number of nodes needed to traverse to get form one node to another. 

一对节点之间的**距离（最短路径）**被定义为沿着连接这对节点的最短路径所含有的边数。 如果两个节点未连通，则距离通常定义为无穷（或零）。 还可以认为距离是从一个节点到另一个节点所需要遍历的最小节点数。

In a directed graph, paths need to follow the direction of the arrows. Thus, distance is not symmetric for directed graphs. For a graph with weighted edges, the distance is the minimum number of edge weight needed to traverse to get from one node to another.

在有向图中，路径需要遵循箭头的方向。 因此，有向图的距离不是对称的。 对于边带有权重的图，距离是从一个节点到另一个节点所需要经过的权重和的最小值。

The **average path length** of a graph is the average shortest path between all connected nodes. We compute the average path length as

图的“平均路径长度”是所有连接的节点之间的平均距离。 计算公式为
$$
\hat h = \frac{1}{2 E_{max}} \sum_{i, j \neq i} h_{ij}
$$

where $$E_{max}$$ is the max number of edges or node pairs; that is, $$E_{max} = n (n-1) / 2$$ and $$h_{ij}$$ is the distance from node $$i$$ to node $$j$$. Note that we only compute the average path length over connected pairs of nodes, and thus ignore infinite length paths.

其中$$ E_ {max} $$是边或节点对的最大数量，即$$ E_ {max} = n（n-1）/ 2 $$，$$ h_ {ij} $$是节点$$ i $$到节点$$ j $$的距离。 注意，我们仅计算连接的节点对上的平均路径长度，因此忽略无穷长度的路径，即忽略不连通的节点对。

### Clustering Coefficient 聚类系数
The **clustering coefficient** (for undirected graphs) measures what proportion of node $$i$$'s neighbors are connected. For node $$i$$ with degree $$k_i$$, we compute the clustering coefficient as

聚类系数（针对无向图）衡量节点$$ i $$邻居们的连接程度。 对于度数为$ k_i $的节点$ i $，我们将该节点的聚类系数计算为
$$
C_i = \frac{2 e_i}{k_i (k_i - 1)}
$$

where $$e_i$$ is the number of edges between the neighbors of node $$i$$. Note that $$C_i \in [0,1]$$. Also, the clustering coefficient is undefined for nodes with degree 0 or 1.

其中$$ e_i $$是节点$$ i $$的邻居们之间的边数，$$ \frac{k_i (k_i - 1)}{2}$$表示度为$k_i$的节点的邻居之间最多可以拥有的边数。 $$ C_i \ ∈ [0,1] $$。 同样，对于度数为0或1的节点，聚类系数是未定义的。

We can also compute the **average clustering coefficent** as

同样我们可以将平均聚类系数定义为：
$$
C = \frac{1}{N} \sum_{i}^N C_i.
$$

The average clustering coefficient allows us to see if edges appear more densely in parts of the network. In social networks, the average clustering coefficient tends to be very high indicating that, as we expect, friends of friends tend to know each other.

平均聚类系数可以帮助我们识别出网络的哪些部分连接更密集。 在社交网络中，平均聚类系数趋于较高的值，这表明，如我们所期望的，朋友的朋友倾向于彼此认识。

### Connectivity 连通性

The **connectivity** of a graph measures the size of the largest connected component. The **largest connected component** is the largest set where any two vertices can be joined by a path. 

图的“连通性”用于度量最大连通分量的大小。 **最大连通分量**是分量中任意两个节点有路径连通的最大子图。

To find connected components:

若要寻找连通分量：

1. Start from a random node and perform breadth first search (BFS)
2. Label the nodes that BFS visits
3. If all the nodes are visited, the netowrk is connected
4. Otherwise find an unvisited node and repeat BFS
5. 从随机节点开始并执行广度优先搜索（BFS）
6. 标记BFS访问的节点
7. 如果访问了所有节点，则表明网络已连通
8. 否则，找到一个未访问的节点并重复BFS

## The Erdös-Rényi Random Graph Model 

## Erdös-Rényi随机图模型

The **Erdös-Rényi Random Graph Model** is the simplest model of graphs. This simple model has proven networks properties and is a good baseline to compare real-world graph properties with.

**Erdös-Rényi随机图模型**是最简单的图模型。 这个简单的模型可用于验证网络的属性，并且是真实的图用于比较属性的良好基线。

This random graph model comes in two variants:

这个随机图模型有两类：

1. **$$G_{np}$$** : undirected graph on $$n$$ nodes where each edge $$(u,v)$$ appears IID with probability $$p$$
2. **$$G_{nm}$$**: undirected graph with $$n$$ nodes, and $$m$$ edges picked uniformly at random
3. $$ G_ {np} $$ ：有$$ n $$个节点的无向图，其中每个边$$(u，v)$$出现的概率为$$ p $$，互相独立且同分布。
4. $$ G_ {nm} $$：具有$ n $个节点和随机选取的$ m $条边

Note that both the $$G_{np}$$ and $$G_{nm}$$ graph are not uniquely determined, but rather the result of a random procedure. Generating each graph multiple times results in different graphs.

注意到，$$ G_ {np} $$和$$ G_ {nm} $$所生成的图不是确定的，而是随机过程的结果， 多次生成会产生不同的图。

### Some Network Properties of $$G_{np}$$

###  $$G_{np}$$的一些网络性质

The degree distribution of $$G_{np}$$ is binomial. Let $$P(k)$$ denotes the fraction of nodes with degree $$k$$, then
$$ G_ {np} $$的度分布是二项分布。 令$$ P(k)$$表示度为$$ k $$的节点的比例，则
$$
P(k) = \binom{n-1}{k} p^k (1-p)^{n-1-k}
$$

The mean and variance of a binomial distribution respectively are $$\bar k = p(n-1)$$ and $$\sigma^2 = p(1-p)(n-1)$$. Below we include an image of binomial distributions for different paramters. Note that a binomial distribution is a discrete analogue of a Gaussian and has a bell-shape.

该二项分布的均值和方差分别为$$ \bar k = p(n-1)$$和$$ \sigma ^ 2 = p(1-p)(n-1)$$。 下面，我们展示了不同参数的二项式分布图。 注意，二项分布是高斯分布的离散模拟，并且概率分布曲线为钟形。

![binom-dist](../assets/img/binom_dist_graph.png?style=centerme)

One property of binomial distributions is that by the law of numbers, as the network size increases, the distribution becomes increasingly narrow. Thus, we are increasingly confidence that the degree of a ndoe is in the vicinity of $$k$$. If the graph has an infinite number of nodes, all nodes will have the same degree.

二项分布的一个特性是，随着网络规模的增加（n增大），分布变得越来越狭窄（因为$$\frac{\sigma}{\bar k}→0$$）。 因此，我们越来越有信心相信节点的度在$$ k $$附近。 所以如果模拟图具有无限个节点，则所有节点将具有相同的度数。

### The Clustering Coefficient of $$G_{np}$$

### $$G_{np}$$的聚类系数

Recall that the clustering coefficient is computed as $$C_i = 2 \frac{e_i} {k_i (k_i -1)}$$ where $$e_i$$ is the number of edges between $$i$$'s neighbors. Edges in $$G_{np}$$ appear IID with probability $$p$$, so the expected $$e_i$$ for $$G_{np}$$ is

回想一下，聚类系数的计算公式为$$ C_i = 2 \frac {e_i} {k_i(k_i -1)} $$，其中$$ e_i $$是$$ i $$的邻居之间的边数。 $$ G_ {np} $$中的边出现的概率为$$ p $$，因此$$ G_ {np} $$的$$ e_i $$的期望为

$$\mathbb{E}[e_i] = p \frac{k_i(k_i - 1)}{2}$$

This is because $$\frac{k_i(k_i - 1)}{2}$$ is the number of distinct pairs of neighbors of node $$i$$ of degree $$k_i$$, and each pair is connected with probability $$p$$.

这是因为$$ \frac {k_i(k_i-1)} {2} $$是度为$ k_i $的节点$$ i $$的邻居间的边的最大值，并且每个边出现的概率都为 $$ p $$。

Thus, the expected clustering coefficient is

因此，聚类系数的期望为
$$
\mathbb{E}[C_i] = \frac{p \cdot k_i (k_i - 1)}{k_i (k_i - 1)} = p = \frac{\bar k}{n-1} \approx \frac{\bar k}{n}.
$$

where $$\bar k$$ is the average degree. From this, we can see that the clustering coefficient of $$G_{np}$$ is very small. If we generate bigger and bigger graphs with fixed average degree $$\bar k$$, then $$C$$ decreases with graph size $$n$$. $$\mathbb{E}[C_i] \to 0$$ as $$n \to \infty$$.

其中$$ \bar k $$是平均度数。 由此可见，$$ G_ {np} $$的聚类系数非常小。 如果我们生成具有固定平均度数$$ \bar k $$的越来越大的图（$p$越来越小），则$$ C $$将随着图大小$$ n $$增大而减小。 $$ \mathbb {E} [C_i] \to 0 $$ 当 $$ n \to \infty $$。

### The Path Length of $$G_{np}$$ 路径长度
To discuss the path length of $$G_{np}$$, we fist introduce the concept of **expansion.** Graph $$G(V, E)$$ has expansion $$\alpha$$ if $$\forall S \subset V$$, the number of edges leaving $$S \geq \alpha \cdot \min (|S|, | V \setminus S|)$$. Expansion answers the question ''if we pick a random set of nodes, how many edges are going to leave the set?'' Expansion is a measure of robustness: to disconnect $$\ell$$ nodes, one must cut $$\geq \alpha \cdot \ell$$ edges.

讨论$$ G_ {np} $$的路径长度，首先介绍**expansion**的概念。我们说图**$$ G(V,E)$$**具有扩展$$ \alpha $$，如果对于$$ \forall S \subset V $$，从该子点集$$ S $$出去的边数 $$ \geq \alpha \cdot \min(| S |,| V \setminus S |)$$。 扩展回答了以下问题：“如果我们选择一个随机的节点集，那么有多少条边会从该集出去？”扩展是一种鲁棒性的度量：要断开$$ \ell $$个节点，必须切断$$ \geq \alpha \cdot \ell $$个边。

Equivalently, we can say a graph $$G(V,E)$$ has an expansion $$\alpha$$ such that

等价的，我们可以说图$G(V,E)$有扩展$$\alpha$$，使得
$$
\alpha = \min_{S \subset V} \frac{\# \text{ edges leaving $S$}}{\min(|S|, |V \setminus S|)}
$$

An important fact about expansion is that in a graph with $$n$$ nodes with expansion $$\alpha$$, for all pairs of nodes, there is a path of $$O((\log n) / \alpha)$$ connecting them. For a random $$G_{np}$$ graph, $$\log n > np > c$$, so $$\text{diam}(G_{np}) = O(\log n / \log (np))$$. Thus, we can see that random graphs have good expansion so it takes as logarithmic number of steps for BFS to visit all nodes.

关于扩展的一个重要事实是，在具有$$ n $$个节点且具有扩展$$ \alpha $$的图中，对于所有节点对，都存在$$ O((\log n)/ \alpha)$$的路径连接它们。 对于随机的$$ G_ {np} $$图，$$ \ log n> np> c $$，因此$$ \text {diam}(G_ {np})= O(\log n / \log(np))$$。 我们可以看到随机图具有良好的扩展性，因此BFS访问所有节点的步数为对数级的。

![expansion](../assets/img/expansion.png?style=centerme)
Thus, the path length of $$G_{np}$$ is $$O(\log n)$$. From this result, we can see that $$G_{np}$$ can grow very large, but nodes will still remain a few hops apart.

因此，$$ G_ {np} $$的路径长度为$$ O(\log n)$$。 从这个结果中，我们可以看到$$ G_ {np} $$可以变得非常大，但是节点之间仍然只相距几跳。

![er-path](../assets/img/ER_path.png?style=centerme)

### The Connectivity of $$G_{np}$$ 连通性
The graphic below shows the evolution of a $$G_{np}$$ random graph. We can see that there is an emergence of a giant component when average degree $$\bar k = 2 E / n$$ or $$p = \bar k / (n-1)$$. If $$k = 1 - \epsilon$$, then all components are of size $$\Omega(\log n)$$. If $$\bar k = 1 + \epsilon$$, there exists 1 component of size $$\Omega(n)$$, and all other components have size $$\Omega(\log n)$$. In other words, if $$\bar k > 1$$, we expect a single large component. Additionally, in this case, each node has at least one edge in expectation.

下图显示了$$ G_ {np} $$随机图的演化过程。 可以看到，当平均度数$$ \bar k = 2 E / n $$或$$ p = \bar k /(n-1)$$时，就会出现一个大型的连通分量。 如果$$ k = 1-\epsilon $$，则所有分量的大小均为$$ \Omega(\log n)$$。 如果$$ \bar k = 1 + \epsilon $$，则存在1个大小为$$ \Omega(n)$$的分量，而所有其他分量的大小为$$ \Omega(\log n)$$。 换句话说，如果$$ \bar k> 1 $$，我们期望有一个大的分量，在这种情况下，每个节点在概率上至少具有一个边缘。

![er-path](../assets/img/evol_of_rand_graph.png?style=centerme)

### Analyzing the Properties of $$G_{np}$$  分析性质

In grid networks, we achieve triadic closures and high clustering, but long average path length. 

在网格网络中，我们实现了三角封闭和高程度的聚集，但是平均路径长度较长。

![grid-network](../assets/img/grid_network.png?style=centerme)

In random networks, we achieve short average path length, but low clustering. 

在随机网络重，我们实现了较短的平均路径长度，但是聚集程度较低。

![grid-network](../assets/img/random_network.png?style=centerme)

Given the two above graph structures, it may seem unintuitive that graphs can have short average path length while also having high clustering. However, most real-world networks have such properties as in the below table, where $$h$$ refers to the average shortest path length, $$c$$ refers to the average clustering coefficient, and random graphs were generated with the same average degree as actual networks for comparison. 

给定以上两个图结构，似乎不太能想到：图可以在平均路径长度较短的同时也具有较高的聚类度。 但是，大多数现实世界的网络具有如下表所示的属性，其中$$ h $$表示平均最短路径长度，$$ c $$表示平均聚类系数，并且使用相同的平均度生成随机图进行比较。

| Network|$$h_{actual}$$|$$h_{random}$$|$$c_{actual}$$|$$c_{random}$$|
|----|--------|------|----|-----|
|Film actors | 3.65 | 2.99 | 0.79 | 0.00027|
|Power Grid | 18.70 | 12.40 |0.080 | 0.005|
|C. elegans | 2.65 |2.25 |0.28 | 0.05|

Networks that meet the above criteria of both high clustering and small average path length (mathematically defined as $$L \propto \log N$$ where $$L$$ is average path length and $$N$$ is the total number of nodes in the network) are referred to as small world networks. 

满足上述标准的高聚类和小平均路径长度的网络（数学定义为$$ L \propto \log N $$，其中$$ L $$是平均路径长度，$$ N $$是网络中的节点）称为小世界网络。

## The Small World Random Graph Model 小世界网络随机图

In 1998, Duncan J. Watts and Steven Strogatz came up with a model for constructing a family of networks with both high clustering and short average path length. They termed this model the ''small world model''. To create such a model, we employ the following steps:

1998年，Duncan J. Watts和Steven Strogatz提出了一个模型，用于构建具有高聚类和较短平均路径长度的网络族。 他们称这种模型为“小世界模型”。 创建这样的模型，需要以下步骤：

1. Start with low-dimensional regular attic (ring) by connecting each node to $$k$$ neighbors on its right and $$k$$ neighbors on its left, with $$k \geq 2$$.
2. Rewire each edge with probability $$p$$ by moving its endpoint to a randomly chosen node.{% include sidenote.html id='note-graphnetwork' note='Several variants of rewiring exist. To learn more, see *M. E. J. Newman. Networks, Second Edition, Oxford University Press, Oxford (2018)*'%}
3. 从低维规则的环开始，将每个节点连接到其右侧的$ k $邻居和其左侧的$ k $邻居（$ k \geq 2 $）。
4. 以概率$$ p $$对每个边重新布线，将边的终点移动到随机选择的节点。

![Small World Model](../assets/img/small_world.png?style=centerme)

Then, we make the following observations:

这样，我们观察到以下一些现象：

- At $$p = 0$$ where no rewiring has occured, this remains a grid network with high clustering, high diameter.
- For $$0 < p < 1$$ some edges have been rewired, but most of the structure remains. This implies both **locality** and **shortcuts**. This allows for both high clustering and low diameter.
- At $$p = 1$$ where all edges have been randomly rewired, this is a Erdős–Rényi (ER) random graph with low clustering, low diameter.
- 在没有重新布线（$$ p = 0 $$）的情况下，仍然是具有高聚类，大直径的网格网络。
- $$ 0 <p <1 $$，重新改动了一些边，但大部分结构仍保留。 这意味着“局部”和“捷径”。 这允许高聚类和低直径。
- $$ p = 1 $$，所有边缘均已重新布线，这就是一个Erdős-Rényi（ER）随机图，具有低聚类，低直径。

![Clustering and Average Path Length](../assets/img/clustering_path.png?style=centerme)

Small world models are parameterized by the probability of rewiring $$p \in [0,1]$$. By examining how the clustering coefficient and the average path length vary with values of $$p$$, we see that average path length falls off much faster as $$p$$ increases, while the clustering coefficient remains relatively high. Rewiring introduces shortcuts, which allows for average path length to decrease even while the structure remains relatively strong (high clustering).

小世界模型的参数是重新布线的概率$$p\in[0,1] $$。 实验发现，平均路径长度随着$$ p $$的增加而下降得更快，而聚类系数仍然相对较高。 重新布线引入了捷径，即使在结构保持相对坚固（高度聚类）的情况下，平均路径长度也可以减小。

From a social network perspective, this phenomenon is intuitive. While most our friends are local, but we also have a few long distance friendships in different countries which is enough to collapse the diameter of the human social network, explaining the popular notion of "Six Degrees of Seperation".

从社交网络的角度来看，这种现象是直观的。 尽管我们的大多数朋友都是本地人，但我们在不同国家/地区也有一些长距离的友谊，这足以使人类社交网络的直径崩溃式下降，从而解释了流行的“六度分离”概念。

Two limitations of the Watts-Strogatz Small World Model are that its degree distribution does not match the power-law distributions of real world networks, and it cannot model network growth as the size of network is assumed. 

Watts-Strogatz小世界模型的两个局限性在于其度分布与现实网络的幂律分布不匹配，并且由于假定了网络的大小，因此无法对网络的增长进行建模。

## The Kronecker Random Graph Model 

## Kronecker随机图模型

Models of graph generation have been studied extensively. Such models allow us to generate graphs for simulations and hypothesis testing when collecting the real graph is difficult, and also forces us to examine the network properties that generative models should obey to be considered realistic. 

图的生成模型已被广泛研究。 这样的模型使我们能够在收集实际图数据遇到困难时，生成用于仿真和假设检验的图，并且还迫使我们检查生成模型应遵循的网络属性，使得这些属性和现实的图相似。

In formulating graph generation models, there are two important considerations. First, the ability to generate realistic networks, and second, the mathematical tractability of the models, which allows for the rigorous analysis of network properties. 

在制定图生成模型时，有两个重要的考虑因素。 首先是生成真实网络的能力；其次是模型的数学易处理性，这允许对网络属性进行严格的分析。

The Kronecker Graph Model is a recursive graph generation model that combines both mathematical tractability and realistic static and temporal network properties. The intuition underlying the Kronecker Graph Model is self-similarity, where the whole has the same shape as one or more of its parts.

Kronecker图模型是一个递归的图生成模型，结合了数学易处理性和实际的静态和时态网络属性。Kronecker图模型的直觉是自相似性，即整体与一个或多个部分的形状相同。

![self-similarity](../assets/img/community_growth.png?style=centerme)

The Kronecker product, a non-standard matrix operation, is a way to generate self-similar matrices.

Kronecker积是一种非标准的矩阵运算，是一种生成自相似矩阵的方法。

### The Kronecker Product Kronecker积

The Kronecker product is denoted by $$\otimes$$. For two arbitarily sized matrices $$\textbf{A} \in \mathbb{R}^{m \times n}$$ and $$\textbf{B} \in \mathbb{R}^{p \times q}$$, $$\textbf{A} \otimes \textbf{B} \in \mathbb{R}^{mp \times nq}$$ such that

$$\textbf{A} \otimes \textbf{B} = 
\begin{bmatrix} 
a_{11}\textbf{B}& \dots &a_{1n}\textbf{B}\\
\vdots &\ddots& \vdots\\
a_{m1}\textbf{B}& \dots & a_{mn}\textbf{B}
\end{bmatrix}$$

For example, we have that

$$\begin{bmatrix} 1&2\\3&4 \end{bmatrix} \otimes \begin{bmatrix} 0&5\\6&7 \end{bmatrix} 
= \begin{bmatrix} 1  \begin{bmatrix} 0&5\\6&7 \end{bmatrix}  &2  \begin{bmatrix} 0&5\\6&7 \end{bmatrix}  \\3  \begin{bmatrix} 0&5\\6&7 \end{bmatrix}  &4  \begin{bmatrix} 0&5\\6&7 \end{bmatrix}  \end{bmatrix}
= \begin{bmatrix} 
1 \times 0 & 1 \times 5 & 2 \times 0 &  2 \times 5\\
1 \times 6 & 1 \times 7 & 2 \times 6 &  2 \times 7\\
3 \times 0 & 3 \times 5 & 4 \times 0 &  4 \times 5\\
3 \times 6 & 3 \times 7 & 4 \times 6 &  4 \times 7
\end{bmatrix}
= \begin{bmatrix} 
0 & 5 & 0 &  10\\
6 & 7 & 12 &  14\\
0 & 15 & 0 &  20\\
18 & 21 & 24 &  28
\end{bmatrix}$$



To use the Kronecker product in graph generation, we define the Kronecker product of two graphs as the Kronecker product of the adjacency matrices of the two graphs. 

为了在图生成中使用Kronecker积，我们将两个图的Kronecker积定义为两个图的邻接矩阵的Kronecker积。

Beginning with the initiator matrix $$K_1$$ (an adjacency matrix of a graph), we iterate the Kronecker product to produce successively larger graphs, $$K_2 = K_1 \otimes K_1, K_3 = K_2 \otimes K_1 \dots$$, such that the Kronecker graph of order $$m$$ is defined by

从初始矩阵$$ K_1 $$（图形的邻接矩阵）开始，我们迭代Kronecker乘积以依次生成更大的图形，$$ K_2 = K_1 \otimes K_1, K_3 = K_2 \otimes K_1 \dots $$， 这样$m$阶的Kronecker图下式得到：

$$K_1^{[m]}=\dots K_m = \underbrace{K_1 \otimes K_1 \otimes \dots K_1}_{\text{m times}}=  K_{m-1} \otimes K_1$$

![Kronecker](../assets/img/small_kronecker.png?style=centerme)

Intuitively, the Kronecker power construction can be imagined as recursive growth of the communities within the graph, with nodes in the community recursively getting expanded into miniature copies of the community. 

The choice of the Kronecker initiator matrix $$K_1$$ can be varied, which iteratively affects the structure of the larger graph.

直观地，可以将Kronecker幂想象为图中的社区的递归增长，其中社区中的节点递归地从本社区的微型副本扩展。

改变Kronecker初始矩阵$$ K_1 $$的选择，递归下去当图较大时，结构会明显不同。

![Initiator](../assets/img/initiator.png?style=centerme)

### Stochastic Kronecker Graphs

### 随机Kronecker图

Up to now, we have only considered $$K_1$$ initiator matrices with binary values $$\{0, 1\}$$. However, such graphs generated from such initiator matrices have "staircase" effects in the degree distributions and other properties: individual values occur very frequently because of the discrete nature of $$K_1$$.

到目前为止，我们只考虑了二值$$ \{0，1 \} $$的$$ K_1 $$初始矩阵。但是，从此类初始矩阵生成的图在度分布和其他属性中具有“阶梯”效应：由于$$ K_1 $$的离散性质，各个值非常频繁地出现。

To negate this effect, stochasticity is introduced by relaxing the assumption that the entries in the initiator matrix can only take binary values. Instead entries in $$\Theta_1$$ can take values on the interval $$[0,1]$$, and each represents the probability of that particular edge appearing. Then the matrix (and all the generated larger matrix products) represent the probability distribution over all possible graphs from that matrix.

为了消除这种影响，可以通过放宽以下假设来引入随机性：初始矩阵中的值只能取0和1。取而代之，$$ \Theta_1 $$中的值可以取区间$$ [0,1] $$的值，并且每个值代表该特定边出现的概率。然后，矩阵（以及所有生成的矩阵乘积）表示该图所有节点对之间是否存在边的概率。

More concretely, for probaility matrix $$\Theta_1$$, we compute the $$k^{th}$$ Kronecker power $$\Theta_k$$ as the large stochastic adjacency matrix. Each entry $$p_{uv}$$ in $$\Theta_k$$ then represents the probability of edge $$(u,v)$$ appearing. 

更具体地说，对于概率矩阵$$ \Theta_1 $$，我们将$$ k ^ {th} $$ Kronecker幂$$ \Theta_k $$计算为大型随机邻接矩阵。然后，$$ \Theta_k $$中的每个条目$$ p_ {uv} $$表示出现边缘$$(u，v)$$的概率。

{% include marginnote.html id='note-bipartite-folded' note='Note that the probabilities do not have to sum up to 1 as each the probability of each edge appearing is independent from other edges.' %}

{％include marginnote.html id ='note-bipartite-folded'note ='注意，由于每个边缘出现的概率与其他边缘无关，因此概率不必总和为1。” ％}

![Stochastic](../assets/img/stochastic_graphs.png?style=centerme)

To obtain an instance of a graph, we then sample from the distribution by sampling each edge with probability given by the corresponding entry in the stochastic adjacency matrix. The sampling can be thought of as the outcomes of flipping biased coins where the bias is parameterized from each entry in the matrix. 

However, this means that the time to naively generate an instance is quadratic in the size of the graph, $$O(N^2)$$; with 1 million nodes, we perform 1 million x 1 million coin flips. 

为了获得图的一个实例，我们通过以随机邻接矩阵中相应条目给出的概率，对每个边进行采样。 采样可以被认为是抛一个不公平的硬币的结果，其中硬币正面朝上的概率是矩阵中的每个条目的值。

但是，这意味着生成实例的时间是图节点数量的两倍，即$ O(N ^ 2)$； 拥有1百万个节点时，我们需要执行1百万 x 1百万次抛硬币。

### Fast Generation of Stochastic Kronecker Graphs

A fast heuristic procedure that takes time linear in the number of edges to generate a graph exists. 

存在一种快速的启发式过程，该过程只需要和边数量相比的线性时间复杂度以生成图形。

The general idea can be described as follows: for each edge, we recurively choose sub-regions of the large stochastic matrix with probability proportional to $$p_{uv} \in \Theta_1$$ until we descend to a single cell of the large stochastic matrix. We place the edge there. For a Kronecker graph of $$k^{th}$$ power, $$\Theta_k$$, the descent will take $$k$$ steps.  

其思想可以描述如下：对于每个边，我们递归地选择大随机矩阵的子区域，其概率与 中的$$ p_ {uv} \in\Theta_1$$成正比，直到我们下降到大随机矩阵的单个单元格为止。在对应的位置生成一条边。 对于$$ k ^ {th} $$幂$$ \Theta_k $$的Kronecker图，下降将需要$$ k $$步长。

For example, we consider the case where $$\Theta_1$$ is a $$2 \times 2$$ matrix, such that

例如，我们考虑$$ \ Theta_1 $$是$$ 2 \times 2 $$矩阵的情况，

$$\Theta = \begin{bmatrix}
a & b \\
c & d
\end{bmatrix}$$

For graph $$G$$ with $$n = 2^k$$ nodes:

对于具有$$ n = 2 ^ k $$节点的图$$ G $$：

- Create normalized matrix $$L_{uv} = \frac{p_{uv}}{\sum_{u,v} p_{uv}}, p_{uv} \in \Theta_1$$
- -创建归一化矩阵$$ L_ {uv} = \frac {p_ {uv}} {\sum_{u，v} p_{uv}},p_ {uv} \in \Theta_1 $$
- For each edge: 
- 对于每个边缘：
    - For $$i = 1 \dots k$$:
	    - Start with $$x = 0, y  = 0$$
	    - Pick the row, column $$(u,v)$$ with probability $$L_{uv}$$
	    - Descend into quadrant $$(u,v)$$ based on step $$i$$ of $$G$$
	        - Set $$x = x + u \cdot 2^{k-1}$$
	        - Set $$y = y + v \cdot 2^{k-1}$$
	    - Add edge $$(x,y)$$ to $$G$$
    

If $$k=3$$, and on each step $$i$$, we pick quadrants $$b_{(0,1)}, c_{(1,0)}, d_{(1,1)}$$ respectively based on the normalized probabilities from $$L$$, then

如果$$ k = 3 $$，并且在每一步$$ i $$，我们根据L 的归一化概率选择象限$$ b _ {(0,1)}，c _ {(1,0)}，d _ {(1,1)}$$ 

$$x = 0 \cdot 2^{3-1} + 1 \cdot 2^{3-2} + 1 \cdot 2^{3-3} = 0 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 3$$
$$y = 1 \cdot 2^{3-1} + 0 \cdot 2^{3-2} + 1 \cdot 2^{3-3} = 1 \cdot 2^2 + 0 \cdot 2^1 + 1 \cdot 2^0 = 5$$

Hence, we add edge $$(3,5)$$ to the graph.

因此，我们将边$$（3,5）$$添加到图中。

In practice, the stochastic Kronecker graph model is able to generate graphs that match the properties of real world networks well. To read more about the Kronecker Graph models, refer to *J Leskovec et al., Kronecker Graphs: An Approach to Modeling Networks (2010)*.{% include sidenote.html id='note-graphnetwork' note='Estimating the initator matrice $$\Theta_1$$ and fitting Kronecker Graphs to real world networks is also discussed in this work.'%}

在实践中，随机Kronecker图模型能够生成与现实世界网络的属性完全匹配的图。要了解有关Kronecker图模型的更多信息，请参阅* J Leskovec等人，《 Kronecker图：网络建模方法》（2010）。


<br/>

|[Index](../) | [Previous](./introduction-graph-structure) | [Next](../)|
