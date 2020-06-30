---
layout: post
title: Spectral Clustering
---

Here we study the important class of spectral methods for understanding networks on a global level. By "spectral" we mean the spectrum, or eigenvalues, of matrices derived from graphs, which will give us insight into the structure of the graphs themselves. In particular, we will explore spectral clustering algorithms, which take advantage of these tools for clustering nodes in graphs.

这一章，我们将学习一种从全局范围内理解网络的方法——谱方法。 “谱”是指从图得出的矩阵的频谱或特征值，这将使我们深入了解图本身的结构。 特别的，我们将探索谱聚类算法，该算法利用这些工具对图中的节点进行聚类。

The spectral clustering algorithms we will explore generally consist of three basic stages.

我们将探索的频谱聚类算法包括三个阶段。

1. Preprocessing: construct a matrix representation of a graph, such as the adjacency matrix (but we will explore other options)
2. Decomposition: compute the eigenvectors and eigenvalues of the matrix, and use these to create a low-dimensional representation space
3. Grouping: assign points to clusters based on their representation in this space
4. 预处理：构造图的矩阵表示形式，例如邻接矩阵（还有其他选择）
5. 分解：计算矩阵的特征向量和特征值，并使用它们创建低维表示空间
6. 分组：根据节点在低维空间中的表示，对节点分组聚类

# Graph Partitioning 图分区
Let's formalize the task we would like to solve. We start out with an undirected graph $$G(V, E)$$. Our goal is to partition $$V$$ into two disjoint groups $$A, B$$ (so $$A \cap B = \emptyset$$ and $$A \cup B = V$$) in a way that maximizes the number of connections internal to the groups and minimizes the number of connections between the two groups.

让我们形式化要解决的任务。从无向图$$ G(V,E)$$开始，我们的目标是将$$ V $$分成两个不相交的组$$ A、B $$（因此$$ A \cap B = \emptyset $$并且$$ A \cup B = V $$），使得组内部的连接数最大，组之间的连接数最小。

To further formalize the objective, let's introduce some terminology:

为了进一步确定目标，让我们介绍一些术语：

- Cut: how much connection there is between two disjoint sets of nodes. $$cut(A, B) = \sum_{i \in A, j \in B} w_{ij}$$ where $$w_{ij}$$ is the weight of the edge between nodes $$i$$ and $$j$$.
- Minimum cut: $$\arg \min_{A, B} cut(A, B)$$
- 割：两个不相交的节点集之间有多少连接。 $$ cut(A,B)= \sum_ {i\in A,j\in B} w_ {ij} $$，其中$$ w_ {ij} $$是节点$$ i $$和$$ j $$之间边的权重。
- 最小割：$$ \arg \min_ {A,B}cut(A,B)$$

Since we want to minimize the number of connections between $$A$$ and $$B$$, we might decide to make the minimum cut our objective. However, we find that we end up with very unintuitive clusters this way -- we can often simply set $$A$$ to be a single node with very few outgoing connections, and $$B$$ to be the rest of the network, to get a very small cut. What we need is a measure that also considers internal cluster connectivity.

由于我们想最大程度地减少$$ A $$和$$ B $$之间的连接数，因此可以将最小割作为目标。但是，我们发现以这种方式导致的簇非常不直观——可以简单地将$$ A $$设置为具有很少外连的单个节点、将$$ B $$设置为网络的其余部分，以获得非常小的割。所以，我们还需要考虑簇的内部连接性。

Enter the **conductance**, which balances between-group and within-group connectivity concerns. We define $$\phi(A, B) = \frac{cut(A, B)}{min(vol(A), vol(B))}$$ where $$vol(A) = \sum_{i \in A} k_i$$, the total (weighted) degree of the nodes in $$A$$. We can roughly think of conductance as analogous to a surface area to volume ratio: the numerator is the area of the shared surface between $$A$$ and $$B$$, and the denominator measures volume while trying to ensure $$A$$ and $$B$$ have similar volumes. Because of this nuanced measure, picking $$A$$ and $$B$$ to minimize the conductance results in more balanced partitions than minimizing the cut. The challenge then becomes to efficiently find a good partition, since minimizing conductance is NP-hard.

加入“**conductance(电导？)**” ，它将平衡簇间和簇内连接问题。我们定义$$ \phi(A,B)= \frac {cut(A,B)} {min(vol(A),vol(B))} $$其中$$ vol(A)= \sum_ {i\in A} k_i $$，表示$$ A $$中节点的总（加权）度。可以粗略地认为conductance类似于表面积与体积之比：分子是在$$ A $$和$$ B $$之间的共享表面的面积，分母度量体积，同时试图确保$$ A$$和$$ B $$的体积相似。由于采取了这种细微的措施，因此选择$ A $和 $B $来最小化电导会使得划分更加均衡。由于要最大程度地减小电导是NP难题，因此面临的挑战是如何有效地找到一个良好的划分。

# Spectral Graph Partitioning 谱方法划分图
Enter spectral graph partitioning, a method that will allow us to pin down the conductance using eigenvectors. We'll start by introducing some basic techniques in spectral graph theory.

谱图划分是一种使用特征向量确定conductance的方法。从谱图理论的一些基本技术开始。

The goal of spectral graph theory is to analyze the "spectrum" of matrices representing graphs. By spectrum we mean the set $$\Lambda = \{\lambda_1, \ldots, \lambda_n\}$$ of eigenvalues $$\lambda_i$$ of a matrix representing a graph, in order of their magnitudes, along with their corresponding eigenvalues. For example, the largest eigenvector/eigenvalue pair for the adjacency matrix of a d-regular graph is the all-ones vector $$x = (1, 1, \ldots, 1)$$, with eigenvalue $$\lambda = d$$. Exercise: what are some eigenvectors for a disconnected graph with two components, each component d-regular? Note that by the spectral theorem, the adjacency matrix (which is real and symmetric) has a complete spectrum of orthogonal eigenvectors.

谱图理论的目的是分析代表图的矩阵的“频谱”。所谓频谱，是指代表图形的矩阵的特征值$$ \lambda_i $$的集合 $$\Lambda = \{\lambda_1, \ldots, \lambda_n\}$$，按照特征值的大小排序。例如，d-正则图的邻接矩阵的最大特征向量是全一向量$$ x =(1,1,\ldots,1)$$，特征值$$ \lambda = d $$。提问：具有两个分量（每个分量为d-regular）的不连通图的特征向量是什么？注意，根据谱理论，邻接矩阵（是实数和对称的）具有完整的谱，包含正交的特征向量。

What kinds of matrices can we analyze using spectral graph theory?

我们可以使用谱图理论分析哪些矩阵？

1. The adjacency matrix: this matrix is a good starting point due to its direct relation to graph structure. It also has the important property of being symmetric, which means that it has a complete spectrum of real-valued, orthogonal eigenvectors.
2. 邻接矩阵：由于该矩阵与图结构直接相关，因此它是一个很好的起点。它还具有对称性，这意味着它具有完整的实值正交特征向量谱。
3. Laplacian matrix $$L$$: it's defined by $$L = D - A$$ where $$D$$ is a diagonal matrix such that $$D_{ii}$$ equals the degree of node $$i$$ and $$A$$ is the adjacency matrix of the graph. The Laplacian takes us farther from the direct structure of a graph, but has some interesting properties which will take us towards deeper structural aspects of our graph. We note that the all-ones vector is an eigenvector of the Laplacian with eigenvalue 0. In addition, since $$L$$ is symmetric, it has a complete spectrum of real-valued, orthogonal eigenvectors. Finally, $$L$$ is positive-semidefinite, which means it has three equivalent properties: its eigenvalues are all non-negative, $$L = N^T N$$ for some matrix $$N$$ and $$x^T Lx \geq 0$$ for every vector $$x$$. This property allows us to use new linear algebra tools to understand $$L$$ and thus our original graph.
4. 拉普拉斯矩阵$$ L $$：$$ L = D-A $$，其中$$ D $$是对角矩阵，等于节点$$i$$的度，$$ A $$是图的邻接矩阵。拉普拉斯矩阵使我们从图的直接结构更进一步，它具有一些图更深层次的结构信息。我们注意到全一向量是特征值为0的拉普拉斯矩阵的特征向量。此外，由于$$ L $$是对称的，因此它具有完整的实值、正交特征向量谱。最后，$$ L $$是正半定的，这意味着它具有下面三个等价属性：其特征值全部为非负值，对于某些矩阵$$ N $$使得$$L = N^T N$$，对于任何向量$$ x $$，$$x^T Lx \geq 0$$。此属性使我们可以使用新的线性代数工具来理解$$ L $$，从而了解我们的原始图形。

In particular, $$\lambda_2$$, the second smallest eigenvalue of $$L$$, is already fascinating and studying it will let us make big strides in understanding graph clustering. By the theory of Rayleigh quotients, we have that $$\lambda_2 = \min_{x: x^T w_1 = 0} \frac{x^T L x}{x^T x}$$ where $$w_1$$ is the eigenvector corresponding to eigenvalue $$\lambda_1$$; in other words, we minimize the objective in the subspace of vectors orthogonal to the first eigenvector in order to find the second eigenvector (remember that $$L$$ is symmetric and thus has an orthogonal basis of eigenvalues). On a high level, Rayleigh quotients frame the eigenvector search as an optimization problem, letting us bring optimization techniques to bear. Note that the objective value does not depend on the magnitude of $$x$$, so we can constrain its magnitude to be 1. Note additionally that we know that the first eigenvector of $$L$$ is the all-ones vector with eigenvalue 0, so saying that $$x$$ is orthogonal to this vector is equivalent to saying that $$\sum_i x_i = 0$$.

特别是$$ \lambda_2 $$，即$$ L $$的第二小的特征值，对其的研究使我们在理解图聚类方面取得了长足的进步。根据瑞利熵理论，我们有$$ \lambda_2 = \min_ {x：x ^ T w_1 = 0} \frac {x ^ TL x} {x ^ T x} $$，其中$$ w_1 $$是特征值$$ \lambda_1 $$对应的特征向量。换句话说，我们在子空间中，寻找与第一个特征向量正交的向量，使得目标最小化以找到第二个特征向量（$$L$$ 是对称的，因此具有正交基）。在较高的层次上，瑞利熵构架将特征向量搜索视为一个优化问题，这使我们可以使用优化算法来解决。注意，目标值和$$ x $$的大小无关，因此可以将其大小限制为1（单位向量）。另外，我们知道$$ L $$的第一个特征向量是全一向量，特征值为0，因此$$ x $$正交于此向量等价于$$ \sum_i x_i = 0 $$。

Using these properties and the definition of $$L$$, we can write out a more concrete formula for $$\lambda_2$$: $$\lambda_2 = \min_x \frac{\sum_{(i, j) \in E} (x_i - x_j)^2}{\sum_i x_i^2}$$, subject to the constraint $$\sum_i x_i = 0$$. If we additionally constrain $$x$$ to have unit length, the objective turns into simply $$\min_x \sum_{(i, j) \in E} (x_i - x_j)^2$$.

使用这些属性和$$ L $$的定义，我们可以为$$ \lambda_2 $$写出更具体的公式：$$ \lambda_2 = \min_x \frac {\sum _ {(i，j)\in E }(x_i-x_j)^ 2} {\sum_i x_i ^ 2} $$，遵从$$ \sum_i x_i = 0 $$。如果另外限制$$ x $$具有单位长度，则目标变为$$ \min_x \sum _ {(i,j)\in E}(x_i-x_j)^ 2 $$。

How does $$\lambda_2$$ relate to our original objective of finding a best partition of our graph? Let's express our partition $$(A, B)$$ as a vector $$y$$ defined by $$y_i = 1$$ if $$i \in A$$ and $$y_i = -1$$ if $$i \in B$$. Instead of using the conductance here, let's first try to minimize the cut while taking care of the problem of balancing partition sizes by enforcing that $$\vert A\vert = \vert B\vert$$ (balance size of partitions), which amounts to constraining $$\sum_i y_i = 0$$. Given this size constraint, let's minimize the cut of the partition, i.e. find $$y$$ that minimizes $$\sum_{(i, j) \in E} (y_i - y_j)^2$$. Note that the entries of $$y$$ must be $$+1$$ or $$-1$$, which has the consequence that the length of $$y$$ is fixed. *This optimization problem looks a lot like the definition of $$\lambda_2$$!* Indeed, by our findings above we have that this objective is minimized by $$\lambda_2$$ of our Laplacian, and the optimal clustering $$y$$ is given by its corresponding eigenvector, known as the **Fiedler vector**.

所以，$$ \lambda_2 $$与我们最初的目标——找到图的最佳划分有何关系？将划分$$(A,B)$$表示为一个矢量$ y $，如果$$ i \in A $$则$$ y_i = -1 $$，同时$ y_i = 1 $当$ i \in B $。先不使用电导，通过限制$$ \vert A \vert = \vert B \vert $$来尝试在平衡两个簇大小的同时尽量减少切割。等价于限制$$ \sum_i y_i = 0 $$。给定此限制来最小化划分的割，即找到最小化$$ \ sum _ {(i,j)\in E}(y_i-y_j)^ 2 $$的$$ y $$。$$ y $$的取值只能是或$$-1 $$，这使$y$的长度是固定的。 *这个优化问题看起来很像$$ \lambda_2 $$的定义。*实际上，拉普拉斯矩阵的$$ \lambda_2 $$ 可以使这个目标最小化。而且最优的划分$y$由其对应的特征向量给定，称为“**Fiedler向量**”。

Now that we have a link between an eigenvalue of $$L$$ and graph partitioning, let's push the connection further and see if we can get rid of the hard $$\vert A\vert = \vert B\vert$$ constraint -- maybe there is a link between the more flexible conductance measure and $$\lambda_2$$. Let's rephrase conductance here in the following way: if a graph $$G$$ is partitioned into $$A$$ and $$B$$ where $$\vert A\vert \leq \vert B\vert$$, then the conductance of the cut is defined as $$\beta = cut(A, B)/\vert A\vert$$. A result called the Cheeger inequality links $$\beta$$ to $$\lambda_2$$: in particular, $$\frac{\beta^2}{2k_{max}} \leq \lambda_2 \leq 2\beta$$ where $$k_{max}$$ is the maximum node degree in the graph. The upper bound on $$\lambda_2$$ is most useful to us for graph partitioning, since we are trying to minimize the conductance; it says that $$\lambda_2$$ gives us a good estimate of the conductance -- we never overestimate it more than by a factor of 2! The corresponding eigenvector $$x$$ is defined by $$x_i = -1/a$$ if $$i \in A$$ and $$x_j = 1/b$$ if $$i \in B$$; the signs of the entries of $$x$$ give us the partition assignments of each node.

现在，我们已经在$$ L $$的特征值和图分区之间建立了联系，更进一步，看看能否摆脱$$ \vert A \vert = \vert B \vert $$的约束——也许更灵活的电导度量与$$ \lambda_2 $$之间存在联系。用以下方式重新定义电导：如果将图$$ G $$划分为$$ A $$和$$ B $$，$$ \vert A \vert \leq \vert B\vert $$，则割的电导率定义为$$ \beta = cut(A,B)/ \vert A\vert $$。根据Cheeger不等式的结果，将$$ \beta $$链接到$$ \lambda_2 $$：特别的，$$ \frac {\beta ^ 2} {2k_ {max}} \leq \lambda_2 \leq 2 \beta $$，其中$$ k_ {max} $$是图中的最大节点度。 $$ \lambda_2 $$的上限对于我们进行图分区最有用，因为我们正在尝试最小化电导。它说明$$ \lambda_2 $$给我们一个很好的电导估计值——不会比$$ \lambda_2 $$的一半小。对应的特征向量$$ x $$由$$ x_i = -1 / a $$定义，如果 $$i \in A$$ ，反之如果$$ i \in B $$，则$$ x_j = 1 / b $$。 $$ x $$条目的符号就是每个节点的划分。

# Spectral Partitioning Algorithm 谱划分算法
Let's put all our findings together to state the spectral partitioning algorithm.

让我们把上面的东西汇总一下，阐述一下谱划分算法。

1. Preprocessing: build the Laplacian matrix $$L$$ of the graph
2. Decomposition: map vertices to their corresponding entries in the second eigenvector
3. Grouping: sort these entries and split the list in two to arrive at a graph partition
4. 预处理：构建图的拉普拉斯矩阵$$ L $$
5. 分解：将节点映射到第二个特征向量中的相应条目
6. 分组：对这些条目进行排序，然后将列表分成两部分，获得图划分

Some practical considerations emerge.

一些实际的考虑出现了。

- How do we choose a splitting point in step 3? There's flexibility here -- we can use simple approaches like splitting at zero or the median value, or more expensive approaches like minimizing the normalized cut in one dimension.
- 如何在步骤3中选择分割点？这很灵活——我们可以使用简单的方法（例如零分割或中值分割），也可以使用更高级的方法（例如最小化标准化割）。
- How do we partition a graph into more than two clusters? We could divide the graph into two clusters, then further subdivide those clusters, etc (Hagen et al '92)...but that can be inefficient and unstable. Instead, we can cluster using multiple eigenvectors, letting each node be represented by its component in these eigenvectors, then cluster these representations, e.g. through k-means (Shi-Malik '00), which is commonly used in recent papers. This method is also more principled in the sense that it approximates the optimal k-way normalized cut, emphasizes cohesive clusters and maps points to a well-separated embedded space. Furthermore, using an eigenvector basis ensures that less information is lost, since we can choose to keep the (more informative) components corresponding to bigger eigenvalues.
- 如何将图划分为两个以上的簇？我们可以将图分为两个簇，然后再进一步细分这些簇，依此类推。但这可能效率低下且不稳定。相反，我们可以使用多个特征向量进行聚类，让每个节点由这些特征向量表示，然后对这些表示进行聚类，例如通过k-means算法。从某种意义上说，该方法也更为原则，因为它近似于最佳的K类归一化切割，强调了聚类算法本身的要求，并将节点映射到一个充分可分的嵌入。此外，使用特征向量可确保较少的信息丢失，因为我们可以选择保持信息量较大的分量（对应于较大的特征值）。
- How do we select the number of clusters? We can try to pick the number of clusters $$k$$ to maximize the **eigengap**, the absolute difference between two consecutive eigenvalues (ordered by descending magnitude).
- 我们如何选择簇数？我们可以尝试选择簇数$ k $以最大化**eigengap**，即两个连续特征值之间的绝对差（按降序排列）。

# Motif-Based Spectral Clustering 基于Motif的谱聚类
What if we want to cluster by higher-level patterns than raw edges? We can instead cluster graph motifs into "modules". We can do everything in an analogous way. Let's start by proposing analogous definitions for cut, volume and conductance:

如果我们想使用比原始边更高级别的模式进行聚类怎么办？可以将图的motifs聚类为“模块”。用和上一小节类似的方式，从割、体积和电导的类似定义开始：

- $$cut_M(S)$$ is the number of motifs for which some nodes in the motif are in one side of the cut and the rest of the nodes are in the other cut
- $$vol_M(S)$$ is the number of motif endpoints in $$S$$ for the motif $$M$$
- We define $$\phi(S) = cut_M(S) / vol_M(S)$$
- $$ cut_M(S)$$是motifs的数量，motif中的某些节点位于割的一侧，而其余节点位于另一侧。
- $$ vol_M(S)$$是motif$$ M $$中的端点数量
- 定义$$ \phi(S)= cut_M(S)/ vol_M(S)$$

How do we find clusters of motifs? Given a motif $$M$$ and graph $$G$$, we'd like to find a set of nodes $$S$$ that minimizes $$\phi_M(S)$$. This problem is NP-hard, so we will again make use of spectral methods, namely **motif spectral clustering**:

我们如何找到motif簇？给定一个motif$$ M $$和图$$ G $$，我们想找到一组将$$ \phi_M(S)$$最小化的节点集$$ S $$。这个问题是NP难的，因此我们将再次使用谱方法，即**motif谱聚类**：

1. Preprocessing: create a matrix $$W^{(M)}$$ defined by $$W_{ij}^{(M)}$$ equals the number of times edge $$(i, j)$$ participates in $$M$$.
2. Decomposition: use standard spectral clustering on $$W^{(M)}$$.
3. Grouping: same as standard spectral clustering
4. 预处理：创建由$$ W_ {ij} ^ {(M)} $$定义的矩阵$$ W ^ {(M)} $$，$$ W_ {ij} ^ {(M)} $$为边$$(i,j)$$参与$$ M $$的次数。
5. 分解：在$$ W ^ {(M)} $$上使用标准谱聚类。
6. 分组：与标准谱聚类相同

Again, we can prove a motif version of the Cheeger inequality to show that the motif conductance found by our algorithm is bounded above by $$4\sqrt{\phi_M^*}$$, where $$\phi_M^*$$ is the optimal conductance.

再次，可以证明Cheeger不等式的motif形式，我们的算法找到的motif电导在小于$$ 4 \sqrt {\phi_M ^ *} $$，其中$$ \ phi_M ^ * $$是最佳电导。

We can apply this method to cluster the food web (which has motifs dictated by biology) and gene regulatory networks (in which directed, signed triads play an important role).

我们可以将这种方法应用于食物网（由生物学决定的图案）和基因调控网络（有方向、符号的triads会扮演重要角色）等等。