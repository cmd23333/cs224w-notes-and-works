---
layout: post
title: PageRank
---

In this section, we study PageRank which is a method for ranking webpages by importance using link structure in the web graph. This is a commonly used [algorithm](http://ilpubs.stanford.edu:8090/422/) for web search popularized by Google. Before discussing PageRank, let us first conceptualize the web as a graph and attempt to study its structure using the language of graph theory.

PageRank是一种使用Web图中的链接结构，按重要性对网页进行排名的方法。由Google发扬光大的算法，被广泛用于网页搜索常用。 在讨论PageRank之前，让我们先将web概念化为图，然后尝试使用图论语言研究其结构。

## The web as a graph

We can represent the entire web as a graph by letting the web pages be nodes and the hyperlinks connecting them be directed edges. We will also make a set of simplifying assumptions:

可以通过将网页作为节点并将连接它们的超链接作为有向边来将整个网络表示为图。 一组简化的假设：

- Consider only static webpages
- Ignore dark matter on the web (i.e. inaccessible material, pages behind firewalls)
- All links are navigable. Transactional links (eg: like, buy, follow etc.) are not considered
- 仅考虑静态网页
- 忽略网络上的暗物质（即无法访问的材料，被防火墙拦截的页面）
- 只考虑可浏览的网页连接。 不考虑transactional的链接（例如：喜欢，购买，关注等，即一种动作。）

Conceptualizing the web this way is similar to how some popular search engines view it. For instance, Google indexes web pages using crawlers that explore the web by following links in a breadth-first order. Some other examples of graphs that can be viewed this way are: the graph of citations between scientific research papers, references in an encyclopedia.

以这种方式概念化网络，类似于某些流行的搜索引擎。例如，Google使用爬虫为网页编制索引，这些爬虫通过广度优先顺序访问链接来浏览网络。可以通过这种方式查看的其他例子包括：科研论文之间的引文图，百科全书中的参考文献等。

### What does the graph of the web look like

### 网络图是什么样的

In the year 2000, the founders of AltaVista ran [an experiment](https://www.sciencedirect.com/science/article/abs/pii/S1389128600000839) to explore what the Web looked like. They asked the question: given a node $$v$$: what are the nodes $$v$$ can reach? What other nodes can reach $$v$$?

2000年，AltaVista的创始人进行了一项实验以探索Web长什么样。 他们提出了一个问题：给定一个节点$$ v $$：$$ v $$可以到达哪些节点？ 还有哪些其他节点可以达到$$ v $$？

This produced two sets of nodes:

这产生了两个节点集：
$$
In(v) = \{ w| w \textrm{ can reach } v\}\\
Out(v) =\{ w| v \textrm{ can reach } w\}
$$

These sets can be ascertained by running a simple BFS. For example, in the following graph:

这些集合可以通过运行一个简单的BFS来计算得到。 例如，在下图中：



![BFS](../assets/img/BFS.png?style=centerme)

$$
In(A) = {A,B,C,E,G}\\
Out(A) = {A,B,C,D,F}
$$

### More on directed graphs

There are two types of directed graphs:
- <b>Strongly connected graphs</b>: A graph where any node can reach any other node. 
- <b>强连通的图</b>：任何节点都可以到达任何其他节点的图。
- <b>Directed Acyclic Graph (DAG)</b>: A graph where there are no cycles and if $$u$$ can reach $$v$$, then $$v$$ cannot reach $$u$$
- <b>有向无环图（DAG）</b>：没有环的图，如果$$ u $$可以达到$$ v $$，则$$ v $$不能达到$$ u $$

Any directed graph can be represented as a combination of these two types. To do so: 

任何有向图都可以表示为这两种类型的组合。为此：

1. <b>Identify the strongly connected components (SCCs) within a directed graph</b>: An SCC is a set of nodes $$\textbf{S}$$ in a graph $$\textbf{G}$$ that is strongly connected and that there is no larger set in $$\textbf{G}$$ containing $$\textbf{S}$$ which is also strongly connected. SCCs can be identified by running a BFS on all in-links into a given node and another BFS on all the out-links from the same node and then calculating the intersection over these two sets.
2. <b>识别有向图内的强连接组件（SCC）</b>：SCC是图$$ \textbf {G} $$中的一组强连通节点$$ \textbf {S} $$，并且$$ \textbf {G} $$中没有更大的包含$$ \textbf {S} $$的强连通集合。可以通过以下方式来识别SCC：在给定节点的所有入边运行BFS，并在同一节点的所有出边中运行另一个BFS，然后计算这两个集合的交集。
3. <b>Merge the SCCs into supernodes, creating a new graph G'</b>: Create an edge between the nodes of G' if there is an edge between corresponding SCCs in G.
4. <b>将SCC合并到超节点中，创建新图G'</b>：如果G中相应SCC之间存在边，则在G'的超节点之间创建边。
5. G' is now a DAG
6. 直到G'是DAG

![SCC_DAG](../assets/img/SCC_DAG.png?style=centerme)

### Bowtie structure of the web graph

网络图的Bowtie结构

Broder et al. (1999) took a large snapshot of the web and tried to understand how the SCCs in the web graph fit together as a DAG

Broder等（1999）对网络进行了大快照，并试图了解网络图中的SCC如何作为DAG组合在一起

-	Their findings are presented in the figure below. Here the starting nodes are sorted by the number of nodes that BFS visits when starting from that node
-	他们的发现如下图中所示。 在这里，节点按BFS以该节点为起点能访问到的节点数进行排序

![BowTie_1](../assets/img/BowTie_1.png?style=centerme)

-	If you look at the nodes on the left colored blue, they are able to reach a very small number of nodes before stopping
-	观察左边蓝色的节点，它们在BFS停止之前只能到达很少数量的节点
-	If you look at the nodes on the right colored magenta, they are able to reach a very large number of nodes before stopping
-	观察右侧洋红色的节点，它们可以在停止之前到达大量节点
-	Using this, we can determine the number of nodes in the IN and OUT component of the bowtie shaped web graph shown below. 
-	计算得到如下所示的蝴蝶结形网络图的IN和OUT组件中的节点数。

![BowTie_2](../assets/img/BowTie_2.png?style=centerme)

- SCC corresponds to the largest strongly connected component in the graph.
- SCC对应图中最大的强连通组件。
- The IN component corresponds to nodes which have out-links to SCC but no in-links from SCC. The OUT component corresponds to nodes which have in-links from SCC but no out-links to SCC. 
- IN组件对应于具有到SCC的出链接但没有来自SCC的入链接的节点。 OUT组件对应于具有来自SCC的入链接但没有到SCC的出链接的节点。
- Nodes with both in and out links to the SCC fall in SCC.
- 同时具有到SCC入和出链接的节点属于SCC。
- Tendrils correspond to edges going out from IN, or to edges going into OUT. TUBES are connections from IN to OUT that bypass SCC.
- 卷须对应于从IN伸出的边，或对应于进入OUT的边。TUBES是从IN到OUT的绕过SCC的连接。
-	Disconnected components are not connected to SCC at all
-	孤立组件和上面的几类节点都没有连接

## PageRank - Ranking nodes on the graph

Not all pages on the web are equal. When we run a query, we don’t want to find all the pages that contain query words. Instead, we want to learn how we rank the pages. How can we decide on the importance of pages based on the link structure of the web graph?

并非网络上的所有页面都同样重要。当运行查询时，我们不想找到所有包含查询词的页面。相反，我们想学习如何对页面进行排名。那么如何根据网络图的链接结构确定某个页面的重要性？

<b>The main idea here is that links are votes</b>

<b>这里的主要思想是链接也是投票</b>

In-links into a page are counted as votes for that page which helps determine the importance of the page. In-links from important pages count more. This turns into a recursive relationship: page A's importance depends on page B, whose importance depends on page C and so on.

进入页面的链接被视为给该页面的投票，这有助于确定该页面的重要性。更重要的页面的in-links权值更高。这变成了一种递归关系：页面A的重要性取决于页面B，而页面B的重要性取决于页面C，依此类推。

Each link's vote is proportional to the importance of its source page. In the figure below, node $$j$$ has links from $$i$$ and $$k$$ which each contribute importance values equal to $$\frac{r_i}{3}$$ and $$\frac{r_k}{4}$$. This is because there are three out-links from node $$i$$ and four out-links from node $$k$$. Similarly, node $$j$$'s importance is equally distributed between the three out-links that it shares with other nodes.

每个链接的投票权重与其源网页的重要性成正比。在下图中，节点$$ j $$具有$$ i $$和$$ k $$的链接，每个链接的重要性值等于$$ \frac {r_i} {3} $$和$$ \frac { r_k} {4} $$。这是因为节点$$ i $$有三个出链接，节点$$ k $$有四个出链接。同样，节点$$ j $$的重要性平均分配给了它的三个出链接节点。

![LinkWeights](../assets/img/LinkWeights.png?style=centerme)

To summarize, a page is important if it is pointed to by other important pages. Given a set of pages, we can define the page rank $$r_j$$ for node $$j$$ as:

总而言之，如果其他重要的网页指向该网页，则该网页很重要。 给定一组网页，我们可以将节点$$ j $$的网页排名得分$$ r_j $$定义为：
$$
\textbf{r}_j = \sum_{i\rightarrow j}\frac{\textbf{r}_i}{d_i} 
$$

where $$i$$ refers to every node that has an outgoing edge into $$j$$ and $$d_i$$ is the out degree of node i.

其中$$ i $$指的是有出连接到$$ j $$的节点，而$$ d_i $$是节点i的出度。

### Matrix formulation矩阵形式
We can formulate these relationships as $$N$$ PageRank equations using $$N$$ variables. We could use Gaussian elimination to solve but this would take a very long time for large graphs such as the web graph. 

我们可以用$$ N $$个变量将这些关系公式化为$$ N $$ 个PageRank方程。使用高斯消去法求解，但是对于大型图（例如网络图），这将花费很长时间。

Instead, we can represent the graph as an adjacency matrix $$\textbf{M}$$ that is column stochastic. This means that all the columns must add up to 1. So for node $$j$$ all the entries, in column $$j$$ will sum to 1. 

相反，我们可以将图表示为列随机的邻接矩阵$$ \textbf {M} $$。 这意味着所有列的总和必须为1。因此，对于节点$$ j $$，所有条目在第$ j $列中的总和为1。
$$
\textrm{If } j \rightarrow i \textrm{ , then } \textbf{M}_{ij} = \frac{1}{d_j}\\
$$

$$r_i$$ is the importance score of page $$i$$

$$r_i$$ 是页面$$i$$的重要性分数，一开始
$$
\sum_i \textbf{r}_i = 1
$$

This will fulfill the requirement that the importance of every node must sum to 1. We now rewrite the PageRank vector as the following matrix equation:

这将满足每个节点初始时的重要性总和必须为1的要求。我们现在将PageRank向量重写为以下矩阵方程式：
$$
\textbf{r} = \textbf{M}.\textbf{r}
$$

In this case, the PageRank vector will be the eigenvector of the stochastic web matrix $$\textbf{M}$$ that corresponds to the eigenvalue of 1. 

在这种情况下，PageRank向量将是对应特征值为1的随机网络矩阵$$ \textbf {M} $$的特征向量。

### Random walk formulation

随着$$ t $$接近无穷大，随机游走将达到稳定状态：

$$
p（t + 1）= \ textbf {M} \ cdot p（t）= p（t）
$$

当我们求解方程式$$ \ textbf {r} = \ textbf {M} \ cdot \ textbf {r} $$; $$ \ textbf {r} $$实际上只是当$$ t $$接近无穷大时，该冲浪者在时间$$ \ textbf {t} $$处的概率分布。 它在图形上模拟了该随机沃克过程的平稳分布。

PageRank relations are very related to random walks. Imagine a web graph and a random surfer that surfs the graph. At any time $$t$$, the surfer is at any page $$i$$. The surfer will then select any outgoing link uniformly at random and make a new step at time $$t+1$$, and this process continues infinitely. Let $$p(t)$$ be the vector whose $$i$$th component is the probability that a surfer is at page $$i$$ at time $$t$$. $$p(t)$$ is then a probability distribution over pages at a given time $$t$$.

PageRank与随机游走非常相关。想象一个网络图和浏览该图的随机网上冲浪者。在任意时间$$ t $$，冲浪者都在任意网页$$ i $$。 然后，冲浪者将随机地选择一个出链接，并在$ t + 1 $时刻浏览下一个网页，并且此过程将无限期地继续。记向量$$p(t)$$，其第$$ i $$个分量是冲浪者在时间$ t $处在页面$ i $的概率。 那么，$$p(t)$$是在给定时间$ t $时冲浪者所在页面的概率分布。
$$
p(t+1) = \textbf{M}\cdot p(t) 
$$

As $$t$$ approaches infinity, the random walk will reach a steady state:

随着$$ t $$趋于无穷大，随机游走将达到稳定状态：
$$
p(t+1) = \textbf{M}\cdot p(t) = p(t)
$$

When we solve the equation $$\textbf{r}=\textbf{M}\cdot \textbf{r}$$; $$\textbf{r}$$ is really just the probability distribution of where this surfer will be at time $$\textbf{t}$$, when $$t$$ approaches infinity. It’s modeling the stationary distribution of this random walker process on the graph.

当我们求解方程 $$\textbf{r}=\textbf{M}\cdot \textbf{r}$$时; $$ \textbf {r} $$实际上只是当$$ t $$接近无穷大时，该冲浪者在时间$$ \textbf {t} $$时处在的页面的概率分布。 它在图上模拟了该随机游走过程的平稳分布。

### Power law iteration幂律迭代

Starting from any vector $$u$$, the limit $$\textbf{M}(\textbf{M}(… \textbf{M}(\textbf{M} \textbf{u})))$$ is the long-term distribution of the surfers. In other words, r is the limit when we take a vector $$u$$ and multiply with $$\textbf{M}$$ long enough. This means that we can efficiently solve for $$\textbf{r}$$ using power iterations. 

从任意向量$$ u $$开始，极限 $$\textbf{M}(\textbf{M}(… \textbf{M}(\textbf{M} \textbf{u})))$$是冲浪者浏览的网页达到稳定时的分布。换句话说，r是我们取一个向量$$ u $$并乘以$$ \textbf {M} $$足够多次时的极限。 这意味着我们可以使用幂律迭代有效地求解$$ \textbf {r} $$。

<b>Limiting distribution = principal eigenvector of $\textbf{M}$ = PageRank</b>

<b>极限分布= $ \textbf {M} $的主要特征向量= PageRank </b>

Initialize: $$ \textbf{r}^{(0)} = [\frac{1}{N},...,\frac{1}{N}]^T$$\\
Iterate: $$\textbf{r}^{(t+1)} = \textbf{M}.\textbf{r}^{(t)}$$\\
Stop when: $$|\textbf{r}^{(t+1)} - \textbf{r}^{(t)} | < \epsilon $$

We keep iterating until we converge based on epsilon. In practice, this tends to converge within 50-100 iterations. 

我们不断迭代，直到基于epsilon收敛为止。 实际操作中，通常在50-100次迭代中收敛。

### Example

![PRExample1](../assets/img/PRExample1.png?style=centerme)
![PRExample2](../assets/img/PRExample2.png?style=centerme)

The $$\textbf{r}$$ vector you are left with is the page rank or page importances. So page $$y$$ has importance $$\frac{6}{15}$$, page $$a$$ has importance $$\frac{6}{15}$$ and page $$m$$ has importance $$\frac{3}{15}$$

最后的$$ \textbf {r} $$向量是页面排名或页面重要性。 因此，页面$$ y $$具有重要性$$ \frac {6} {15} $$，页面$$ a $$具有重要性$$ \frac {6} {15} $$，页面$$ m $$具有重要性$$\frac{3}{15}$$


### PageRank: Problems

1. <b>Dead ends</b>: These are pages that have in-links but no out-links. As a random surfer, it’s like coming to a cliff and having nowhere else to go. This would imply that the adjacency matrix is no longer column stochastic and will leak out importance.
2. <b>死路</b>：这些页面具有入链接，但没有出链接。 作为一个随机的冲浪者，这就像来到悬崖上，无处可去。 这将意味着邻接矩阵不再是列随机的（b这一列的和为0，没有出边），并且会泄漏重要性。
  ![DeadEnd](../assets/img/DeadEnd.png?style=centerme)
3. <b>Spider traps</b>:: These are pages with only self-edges as outgoing edges causing the surfer to get trapped. Eventually the spider trap will absorb all importance. Given a graph with a self-loop in $$b$$, the random surfer will eventually navigate to $$b$$ and get stuck in $$b$$ for the rest of the iterations. Power iteration will converge with $$b$$ having all the importance and leave $$a$$ with no importance.
4. <b>蜘蛛陷阱</b>：这些页面仅具有连向自己的出连接，从而导致冲浪者被困住。最终，蜘蛛陷阱将吸收所有的重要性。给定一个带有自环$ b $的图，随机冲浪者最终将导航到$$ b $$，并在其余的迭代中陷入$ b $。幂次迭代将收敛到$$ b $$具有了所有的重要性，而使$$ a $$不重要。
  ![SpiderTrap](../assets/img/SpiderTrap.png?style=centerme)

<b>How do we solve this? Using random teleportation or random jumps!</b>

<b>我们该如何解决？使用随机传送或随机跳跃！</b>

Whenver a random walker makes a step, the surfer has two options. It can flip a coin and with probability 𝜷 continue to follow the links, or with probability $$(1- 𝜷)$$ it will teleport to a different webpage. Where do you jump? to any of the nodes with equal probability. Usually 𝜷 is set around 0.8 to 0.9. 

每当随机游走者迈出一步时，有两个选择。它可以掷硬币并以概率𝜷继续跟随链接走，或者以 $$(1- 𝜷)$$的概率随机传送到另一个网页。通常𝜷设置在0.8到0.9左右。

In case of a spider trap: Teleport out in a finite number of steps.\\
In case of a dead end: Teleport out with a total probability of 1. This will make the matrix column stochastic.

如果是蜘蛛陷阱：会在有限的步数内传送出去。
如果出现死角：以总概率1传送出去。这将使矩阵是列随机的。

Putting this together, the PageRank equation (as proposed by [Brin-Page, 98](http://snap.stanford.edu/class/cs224w-readings/Brin98Anatomy.pdf)) can be written as:

综上所述，PageRank方程可以写为：
$$
r_j = \sum_{i \rightarrow j} \frac{r_i}{d_i} + (1 - 𝜷) \frac{1}{N} 
$$

We can now define the Google Matrix A and apply power iteration to solve for $$\textbf{r}$$ as before

现在，我们可以定义Google Matrix A并应用幂迭代来像以前一样解决
$$
A = 𝜷\textbf{M} + (1 - 𝜷)[\frac{1}{N}]_{NXN}
$$

$$
\textbf{r} = \textbf{A} \cdot \textbf{r}
$$

Note: This formulation assumes that $$\textbf{M}$$ has no dead ends. We can either preprocess matrix $$\textbf{M}$$ to remove all dead ends or explicitly follow random teleport links with probability 1.0 from dead-ends.

注意：此公式假定$$ \textbf {M} $$没有死角。我们可以对矩阵$$ \textbf {M} $$进行预处理以删除所有死角，也可以在死角中以概率1.0显式地随机传送。

### Computing PageRank: Sparse matrix formulation

### 计算PageRank：稀疏矩阵公式

The key step in computing page rank is the matrix-vector multiplication

计算page rank的关键步骤是矩阵向量乘法
$$
\textbf{r}_{new} = \textbf{A} \cdot \textbf{r}_{old}
$$

We want to be able to iterate this as many times as possible. If $$\textbf{A}$$, $$\textbf{r}_{old}$$, $$\textbf{r}_{new}$$ are small and can fit in memory then there is no problem. But if $$N$$ = 1 billion pages and each entry is 4 bytes, then just for storing $$\textbf{r}_{old}$$ and $$\textbf{r}_{new}$$, we would need 8GB of memory. Matrix A would have $$N^2 = 10^{18}$$ entries, which would require close to 10 million GB of memory! 

我们希望能够对此进行尽可能多的迭代。如果$$ \textbf {A} $$，$$ \textbf{r}_ {old} $$，$$\textbf {r} _ {new} $$很小并且可以容纳在内存中，那么就没有问题。但是，如果$$ N $$ = 10亿页并且每个条目都是4个字节，那么仅用于存储$$ \textbf {r} _ {old} $$和$$ \textbf {r} _ {new} $$，我们将需要8GB的内存。矩阵A将有$$ N ^ 2 = 10 ^ {18} $$个条目，这将需要近1000万GB的内存！

We can rearrange the computation to look like this:

我们可以重新组织一下计算，如下所示：
$$
\textbf{r} = 𝜷 \textbf{M} \cdot \textbf{r} + \frac{[𝟏 − 𝜷]}{𝑵}
$$

This is easier to compute because $$\textbf{M}$$ is a sparse matrix, multiplying it with a scalar is still sparse, and then multiplying it with a vector is not as computationally intensive. After this, we simply add a constant which is the probability of the random walker directly jumping to $$\textbf{r}_{new}$$. The amount of memory that we now need goes down from $$O(N^2)$$ to $$O(N)$$. At every iteration, some of the pagerank can leak out and by renormalizing M we can re-insert the leaked page rank.

这很容易计算，因为$$ \textbf {M} $$是一个稀疏矩阵，将它与标量相乘仍然是稀疏的，然后再将其与向量相乘就不那么计算密集。此后，我们只需添加一个常数，该常数就是随机游走者直接跳到$$ \textbf {r} _ {new} $$的概率。我们现在需要的内存量从$$O(N^2)$$降至$$O(N)$$。在每次迭代中，某些页面排名可能会泄漏出去，这时通过对M重新进行规范化，就可以重新插入泄漏的page rank。

Here is an example of how PageRank would work if applied it to a graph:
![PRGraphExample](../assets/img/PRGraphExample.png?style=centerme)

In the figure above, within each node is its pagerank score or importance score. Scores sum to 100, size of the node is proportional to its score.  Node B has very high importance because a lot of nodes point to it. These nodes still have importance without in-links because random walker jump can jump to them. Node C has only one-link but since its from B it also becomes very important. However, C's importance is still less than B because B has a lot of other in-links going into it.

在上图中，每个节点内是其page rank得分或重要性得分。分数总和为100，节点的大小与其分数成正比。节点B具有非常高的重要性，因为很多节点都指向它。这些节点在没有入链接的情况下仍然有重要性，因为随机步行者可以通过随机TP跳跃到它们。节点C只有一个入链接，但是由于它来自B，因此它也变得非常重要。 但是，C的重要性仍然不如B，因为B还有很多其他入链接。

### PageRank Algorithm

Input: 
- Directed graph G (can have spider traps and dead ends)
- Parameter 𝜷

Output: 
- PageRank vector $$r^{new}$$

Set: 
$$r_j^{old} = \frac{1}{N}$$:

Repeat until convergence: 

$$
\sum_j|r_j^{new} - r_j^{old}| < \epsilon
$$

$$
\forall j: r_j^{'new} = \sum_{i \rightarrow j}𝜷 \frac{r_i^{old}}{d_i}\\
	   r_j^{'new} = 0 \textrm{ if in-degree of j is } 0
$$

Now re-insert the leaked PageRank:

$$
\forall j: r_j^{new} = r_j^{'new} + \frac{1-s}{N} \textrm{ where } S = \sum_jr_j^{'new}\\
r^{old} = r^{new}
$$


## Personalized PageRank and random walk with restarts

Imagine we have a bipartite graph consisting of users on one side (circles in the figure below) and items on the other (squares). We would like to ask how related two items are or how related two users are. Given that the users have already purchased in the past - what can we recommend to them, based on what they have in common with other users.  

想象一下，我们有一个二部图，它由用户（下图中的圆圈）和项目（正方形）组成。 我们想问两个项目之间的相关性或两个用户之间的相关性。 鉴于用户过去已经购买过或根据他们与其他用户的共同点，我们可以向他们推荐什么？

![ItemProduct](../assets/img/ItemProduct.png?style=centerme)

We can use different metrics to quantify this such as shortest path or number of common neighbors, however these are not very versatile. Instead, we can use a modified version of PageRank that doesn't rank all pages by importance rather it ranks them by proximity to a given set. This set is called the teleport set $$\textbf{S}$$ and this method is called Personalized PageRank. 

我们可以使用不同的指标来量化，例如最短路径或共同邻居的数量，但是这些功能不是很通用。 相反，我们可以使用PageRank的修改版本，该版本不会按重要性对所有页面进行排名，而是根据与给定集合的接近程度对它们进行排名。 此集称为传送集$$ \textbf {S} $$，此方法称为“个性化的PageRank”。

One way to implement this is to take the teleport set and compute the pagerank vector using power iteration. However, in this case, since we only have a single node in S, its quicker to just do a simple random walk. So, the random walker starts at node $$\textbf{Q}$$ and then whenever it teleports it goes back to $$\textbf{Q}$$. This will give us all the nodes that are most similar to $$\textbf{Q}$$ by identifying those with the highest visit counts. We thus achieve a very simple recommender system that works very well in practice, and we can call it random walk with restarts.

一种实现方法是对传送集使用幂律迭代来计算pagerank向量。 但在这种情况下，由于S中只有一个节点，因此执行简单的随机游走会更快。 因此，随机游走者从节点$$ \textbf {Q} $$开始，然后每当它TP时，它就会回到$$ \textbf {Q} $$。访问量最高的节点即为与$$ \textbf {Q} $$最相似的节点。 因此，我们获得了一个非常简单的推荐系统，该系统在实践中效果很好，并且我们可以将其称为带重启的随机游走。

![QPPR](../assets/img/QPPR.png?style=centerme)

Random walk with restarts is able to account for

重新启动的随机游走能够包含

- Multiple connections
- Multiple paths
- Direct and indirect connections
- Degree of the node
- 多连接
- 多路径
- 直接和间接连接
- 节点的度


## PageRank Summary:

- <b>Normal pagerank</b>:
Teleportation vector is uniform

- <b>常规pagerank</b>：

  随机跳跃向量是均匀的

- <b>Personalized PageRank</b>: 
Teleport to a topic specific set of pages. 
Nodes can have different probabilities of surfer landing there

- <b>个性化PageRank </b>：

  传送到特定主题的页面集。

  随机传送到的节点可能有不同的概率

- <b>Random walk with restarts</b>:

- 带有重新启动的随机游走
Topic specific pagerank where teleport is always to the same node. In this case, we don't need power iteration we can just use random walk and its very fast and easy
  
  特定于主题的pagerank，其中传送始终是到同一节点的。在这种情况下，我们不需要幂次迭代，使用快速简单的随机游走即可。
