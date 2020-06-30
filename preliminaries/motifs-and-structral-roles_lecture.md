---
layout: post
title: Motifs and Structral Rules in Network

---

## Subgraphs 子图

In this section, we will begin by introducing the defination of subgraphs. Subnetworks, or subgraphs, are the building blocks of networks which enable us to characterize and discriminate networks.

在本节中，我们将介绍子图的定义。 子网络或子图是网络的构建块，使我们能够表征和区分网络。

For example, in Figure 1 we show all the non-isomorphic directed subgraphs of size 3. These subgraphs differ from each other in the number of edges or direction of edges. 

例如，在图1中，我们显示了所有大小为3的非同构有向子图。这些子图在边数或边方向上互不相同。

![Figure 1](../assets/img/Subgraphs_example.png?style=centerme)

### Motifs 注：这个词是真的不知道怎么翻译。

Network motifs are recurring, significant patterns of interconnections in the network. Here, pattern means it is small induced subgraph. Note induced subgraph of graph G is a graph formed from a subset X of the vertices of graph G and all of the edges connecting pairs of vertices in subset X. 

网络的motifs是网络中出现频率高的、重要的连接模式， 模式意味着motifs是诱导子图。 图G的诱导子图是由图G节点的子集X和所有X的节点对存在边组成的图。

Recurrence of motif represents it occurs with high frequency. We allow overlapping of motifs.

motifs需要满足在图中出现的频率很高， 我们允许motifs重叠。

Significance of a motif means it is more frequent than expected. The key idea here is we say subgraphs that occur in a real network much more often than in a random network have functional significance. Significance can be measured using Z-score which is defined as: 

motifs的重要性意味着它的出现次数比预期的要更频繁。 这里的关键思想是，在实际网络中的子图比在随机网络中出现的频率要高得多。 可以使用Z分数来衡量，定义为：
$$
Z_{i} = \frac{N_{i}^{real} - \overline N_{i}^{rand}}{std(N_{i}^{rand})} 
$$

where $$N_{i}^{real}$$ is #(subgraphs of type i) in network $$G^{real}$$ and $$N_{i}^{rand}$$ is #(subgraphs of type i) in randomized network $$G^{rand}$$.

其中$$ N_ {i} ^ {real} $$是真实网络$$ G ^ {real} $$中类型为i的子图数量，而$$ N_ {i} ^ {rand} $$是随机网络中类型为i的子图数量。

Network significance profile (SP) is defined as: 

网络的重要性特征（SP）定义为：
$$
SP_{i} = \frac{Z_{i}}{\sqrt{\sum_{j} {Z_j^{2}}}} 
$$

where SP is a vector of normalized Z-scores.

即SP是归一化Z分数的向量。

### Configuration Model 配置模型

Configuration model is a random graph with a given degree sequence $$k_1$$, $$k_2$$, ..., $$k_N$$ which can be used as a "null" model and then compared with real network. Configuration model can be generated in an easy way as shown in Figure 2. 

配置模型是具有给定度序列$$k_1$$, $$k_2$$, ..., $$k_N$$ 的随机图，可以当作“null”模型，然后与真实网络进行比较。 配置模型可以很容易地生成，如图2所示。

![Figure 2](../assets/img/Configuration_Model.png?style=centerme)

Another way for generation is as following:
1) start from a given graph G;
2) select a pair of edges A->B, C->D at random, exchange the endpoints to give A->D, C->B, repeat the switching step Q* $$\vert E\vert$$ times.

另一种生成方式如下：
1）从给定的图G开始；
2）随机选择一对边A-> B，C-> D，交换端点，使A-> D，C-> B，重复交换步骤Q * $$ \vert E \vert $$次 。

By this way, we will get a randomly rewired graph with same node degrees and randomly rewired edges.

通过这种方式，我们将获得一个具有相同节点度和随机边的随机图。

### Graphlets 

Graphlets are connected non-isomorphic subgraphs. It allows us to obtain a node-level subgraph metric. Graphlet Degree Vector (GDV) is a vector with the frequency of the node in each orbit position, it counts the number of graphlets that a node touches. GDV provides a measure of a node's local network topology. 

Graphlets是连通的非同构子图，是一种节点级的子图度量。Graphlet度向量（Graphlet Degree Vector，GDV）的每个位置上计算节点所触摸的graphlet的数量。 GDV提供了节点的本地拓扑网络的度量。

### Finding Motifs and Graphlets

Finding size-k motifs/graphlets requires us: 
1) enumerate all size-k connected subgraphs; 
2) count the number of occurrences of each subgraph type.

寻找大小为k的motifs/graphlets需要我们：
1）列举所有大小为k的连通子图；
2）计算每种子图类型的出现次数。

Just knowing if a certain subgraph exists in a graph is a hard computational problem. Also, computation time grows exponentially as the size of the motif/graphlet increases.

仅仅是知道图中是否存在某个子图就是一个艰巨的计算问题。 而且，计算消耗的时间随着motifs/graphlets的大小的增加而呈指数增长。

### ESU Algorithm
Exact Subgraph Enumeration (ESU) Algorithm involves two sets,  while $$V_{subgraph}$$ contains nodes in currently constructed subgraph, and $$V_{extension}$$ is a set of candidate nodes to extend the motif.  The basic idea of ESU is firstly starting with a node v, then adding nodes u to $$V_{extension}$$ set when u's node id is larger than that of v, and u may only be neighbored to some newly added node w but not of any node already in $$V_{subgraph}$$. 

精确子图枚举（ESU）算法涉及两集合：$$ V_ {subgraph} $$包含当前已构造的子图中的节点，而$$ V_ {extension} $$是一组用于扩展motif的候选节点。 ESU的基本思想是首先从节点v开始，然后添加id大于v的节点u到$$ V_ {extension} $$集，并且u可能与某个新添加的节点w相邻，但不包含$$ V_ {subgraph} $$中已经存在的任何节点。

ESU is implemented as a recursive function, Figure 3 shows the pseudocode of this algorithm:

ESU被实现为递归函数，图3显示了该算法的伪代码：

![Figure 3](../assets/img/Exact_Subgraph_Enumeration.png?style=centerme)

## Structural Roles in Networks 网络中的结构性角色

### Roles 

We can consider roles as functions of nodes in a network which can be measured by structural behaviors. Note roles are different from groups/communities. Roles are based on the similarity of ties between subsets of nodes. Nodes with the same role have similar structural properties, but they don't need to be in direct or even indirect interaction with each other. Group/community is formed based on adjacency, proximity or reachability, nodes in the same community are well-connected to each other.

我们可以将role视为网络中节点的函数，可以通过结构行为对其进行度量。角色不同于组/社区。 role基于节点子集之间关系的相似性。 具有相同角色的节点具有相似的结构属性，但是它们不必彼此直接或间接连接。 组/社区是基于直接或间接连接而形成的，同一社区中的节点之间相互连通。

### Structural equivalence 结构等价
We say nodes u and v are structurally equivalent if they have the same relationships to each other. Structurally equivalent nodes are likely to be similar in many different ways. For example, node u and v are structurally equivalent in Figure 4 since they connect other nodes in the same way.

我们说节点u和v在结构上是等价的，如果它们之间具有相同的关系。 结构上等价的节点可能以许多不同方式相似。 例如，图4中的节点u和v在结构上等价，因为它们以相同的方式连接其他节点。

![Figure 4](../assets/img/structurally_equivalent.png?style=centerme)

### RoIX
Roles allow us to identify different properties of nodes in network. Here we will introduce an automatic structural roles discovery method called RolX. It's an unsupervised learning approach without prior knowledge. Figure 5 is the RoIX approach overview.

角色使我们能够识别网络中节点的不同属性。 在这里，我们将介绍一种称为RolX的自动结构性角色发现方法。 这是一种没有先验知识的无监督学习方法。 图5是RoIX方法概述。

![Figure 5](../assets/img/RoIX.png?style=centerme)

### Recursive Feature Extraction 递归特征提取
The basic idea of recursive feature extraction is to aggregate features of a node and use them to generate new recursive features. By this way we can turn network connectivity into structural features. 

递归特征提取的基本思想是聚合节点的特征，并使用它们生成新的递归特征。 通过这种方式，我们可以将网络中节点的连接提取成结构化的特征。

The base set of a node's neighborhood features include:

基本的节点邻接特征包括：

1. Local features, which are all measures of the node degree. 
2. EgoNet features, which are computed on the node's egonet and may include the number of within-egonet edges, and the number of edges entering/leaving egonet. Here egonet of a node  includes the node itself, its neighbors and any edges in the induced subgraph on these nodes.
3. 局部特征，即和节点的度有关的属性。
4. EgoNet特征，在节点的ego net上计算，可能包括ego net内部的边的数量以及进入/离开egonet的边的数量。 这里的节点的Ego Net包括节点本身、他的邻居，和这些节点的诱导子图中的任何边。

To generate recursive features, firstly we start with the base set of node features, then use the set of current node features to generate additional features and repeat. The number of possible recursive features grows exponentially with each recursive iteration. 

为了生成递归特征，首先我们从基本的节点特征集开始，然后使用当前节点特征集生成新特征并重复这些操作。 每次递归迭代时，可能的递归特征的数量呈指数增长。

