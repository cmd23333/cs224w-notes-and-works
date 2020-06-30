---
layout: post
title: Introduction and Graph Structure
---
We begin by motivating the study of graph representations of data, or networks. Networks form a general language for desribing complex systems of interacting entities. Pictorially, rather than thinking that our dataset consists of a set of isolated data points, we consider interactions and relationships between these points.

从图形式的数据（网络）开始。 网络可以用于描述实体间互相交互的复杂系统。 进一步来说，数据集不只是由一组孤立的数据点组成的，我们还可以考虑这些点之间的相互作用和关系。

It's instructive to make a philosophical distinction between different kinds of networks. One interpretation of networks is as examples of phenomena that appear in real life; we call these networks *natural graphs*. A couple examples include

以哲学的角度区分不同种类的网络是很有必要的。对网络的一种解释是一些现实生活中出现的现象，我们称这些网络为“自然图”。 几个例子包括

* The human social network (a collection of 7+ billion individuals)
* Internet communication systems (a collection of electronic devices)
* 人类社交网络（70亿+ 个人的集合）
* 互联网通信系统（电子设备的集合）

An alternative interpretation of networks is as a data structure useful for solving a specific prediction problem. In this case, we're more interested in relationships between entities so we can efficiently perform learning tasks. We call these networks *information graphs*, and some examples include

网络的另一种解释是作为一种可用于解决特定问题数据结构。 在这种情况下，我们对实体之间的关系更感兴趣，可以有效地执行学习任务。 我们称这些网络为“信息图”，其中一些例子包括

* Scene graphs (how objects in a scene relate to one another)
* Similarity networks (in which similar points in a dataset are connected).
* 场景图（场景中的对象如何相互关联）
* 相似性网络（数据集相似的样本对互相连接）

Some of the main questions we'll be considering in this course involve how such systems are organized and what their design properties are. It's also possible to represent datasets with rich relational structure as graphs for numerous prediction tasks: in this case, we hope to explicitly model relationships for better predictive performance. Some examples of such predictive tasks include

在本课程中我们主要探讨的问题有：这类系统的组织方式、设计特性。 也可以将具有关系结构的数据集表示为用于许多预测任务的图形。在这种情况下，我们希望显式地对关系进行建模以提高预测性能。 这类预测性任务的示例包括

1. *Node classification*, where we predict the type/color of a given node
2. *Link prediction*, where we predict whether two nodes are linked
3. *Community detection*, where we identify densely linked clusters of nodes
4. *Similarity computation*, where we measure the similarity of two nodes or networks
5. *节点分类*，预测给定节点的类型/颜色
2. *链接预测*，预测两个节点是否连接
3. *社区检测*，识别密集连接的节点簇
4. *相似度计算*，计算两个节点或网络的相似度

Altogether, networks are a universal language for describing complex data, and generalize across a variety of different fields. With increased data availability and a variety of computational challenges, learning about networks leaves one poised to make a wide variety of contributions.

总而言之，网络是一种用于描述复杂数据的通用语言，并且可以在各种不同的领域应用。 随着数据的丰富度提高，伴随着各种计算挑战，学习网络可以使人们有能力做出更过的贡献。

# A Review of Graphs 一些图的知识

## Basic Concepts 基本概念

A network/graph{% include sidenote.html id='note-graphnetwork' note='Technically, a network often refers to real systems (the web, a social network, etc.) while a *graph* often refers to the mathematical representation of a network (a web graph, social graph, etc.). In these notes, we will use the terms interchangeably.' %} is defined as a collection of objects where some pairs of objects are connected by links. We define the set of objects (nodes) as $$N$$, the set of interactions (edges/links) as $$E$$, and the graph over $$N$$ and $$E$$ as $$G(N, E)$$.

网络/图（从技术上讲，网络通常是指真实的系统（网络，社交网络等），而图通常是指数学上的，网络的表示形式（网络图，社交图等）。 在这份笔记中，我们将互换使用这些术语。）定义为对象的集合，其中一些样本对互相连接。我们将对象（节点）的集合定义为$$ N $$，将关系（边缘/连接）的集合定义为$$ E $$，将$$ N $$和$$ E $$之上的图形定义为$$ G（N，E）$$。

*Undirected* graphs have symmetrical/reciprocal links (e.g. friendship on Facebook). We define the node degree $$k_i$$ of node $$i$$ in an undirected graph as the number of edges adjacent to node $$i$$. The average degree is then

*无向*图具有对称/双向链接（例如Facebook上的好友）。 我们在无向图中将节点$$ i $$的节点度数$$ k_i $$定义为与节点$$ i $$相邻的边数。那么这个图的平均度数是

$$\bar{k} = \langle k \rangle = \frac{1}{\vert N \vert} \sum_{i=1}^{\vert N \vert} k_i = \frac{2 \vert E \vert}{N}$$

*Directed* graphs have directed links (e.g. following on Twitter). We define the in-degree $$k^{in}_i$$ as the number of edges entering node $$i$$. Similarly, we define the out-degree $$k^{out}_i$$ as the number of edges leaving node $$i$$. The average degree is then

*有向*图具有定向链接（例如Twitter上关注）。 我们将入度$$ k ^ {in} _i $$定义为进入节点$$ i $$的边数，出度$$ k ^ {out} _i $$定义为离开节点$$ i $$的边数。 那么这个图的平均度数是

$$\bar{k} = \langle k \rangle = \frac{\vert E \vert}{N}$$

**Complete Graphs.** An undirected graph with the maximum number of edges (such that all pairs of nodes are connected) is called the complete graph. The complete graph has $$\vert E\vert = \binom{N}{2} = \frac{N(N-1)}{2}$$ and average degree $$\vert N\vert-1$$.

**完全图。**达到最大边数（所有节点对都互相连接）的无向图称为完全图。 完全图有$$\vert E\vert = \binom{N}{2} = \frac{N(N-1)}{2}$$个连接（任意两点相连）和平均度数$$\vert N\vert-1$$（每个点和其余N-1个点相连）。

**Bipartite Graphs.** A bipartite graph is a graph whose nodes can be divided into two disjoint sets $$U$$ and $$V$$ such that every edge connects a node in $$U$$ to a node in $$V$${% include sidenote.html id='note-graphnetwork' note='That is, there are no edges between nodes in $$U$$ and between nodes in $$V$$. We call $$U$$ and $$V$$ independent sets.'%}. We can ''fold'' bipartite graphs by creating edges within independent sets $$U$$ or $$V$$ if they share at least one common neighbor.

**二分图。**二分图的节点可以分为两个不相交的集合$$ U $$和$$ V $$，这样每个边都将$$ U $$的节点连接到$$ V $$ 的节点。 即，$ U $的节点内部和$$ V $$的节点内部没有连接。 我们称$$ U $$和$$ V $$是互相独立的点集。 我们可以通过在独立集合$$ U $$或$$ V $$中创建边（如果它们共享至少一个公共邻居）来“折叠”二分图。

{% include marginnote.html id='note-bipartite-folded' note='Here, projection $$U$$ connects nodes in $$U$$ if they share at least one neighbor in $$V$$. The same process is applied to obtain projection $$V$$.' %}

在这里，投影$$ U $$连接在$$ U $$中的节点，如果它们在$$ V $$中共享至少一个邻居。 使用相同的过程也被用在投影$$ V $$。

![bipartite-folded](../assets/img/introduction_bipartite_folded.png?style=centerme)

**Other Graph Types**. We briefly note that graphs can also include self-edges (self-loops), weights associated with edges, and multiple edges connecting nodes. These attributes can be encoded in graph representations with ease.

**其他类型的图**。 注意到，图还可以包括自我连接（环），有权重的连接以及两个节点有多个不同的连接。 这些属性可以被编码到图的表示上。

## Representing Graphs 表示图

We can represent graph $$G$$ as **adjacency matrix** $$A$$ such that $$A_{ij} = 1$$ if $$i$$ and $$j$$ are linked (and $$A_{ij} = 0$$ otherwise). Note that $$A$$ is asymmetric for directed graphs. For example, a graph with a 3-clique on nodes 1, 2, and 3 and an additional edge from node 3 to 4 can be represented as

我们可以将图$$ G $$表示为**邻接矩阵** $$ A $$，如果$$ i $$和$$ j $$被链接（则$$ A_ {ij} = 1 $$ ，否则$ A_ {ij} = 0 $）。 注意，有向图的邻接矩阵$$ A $$是不对称的。 例如，在节点1、2和3上具有3-clique，以及从节点3到4有一条连接的图可以表示为

clique团，两两之间有边的集合。
$$
A = \begin{bmatrix} 0 & 1 & 0 & 1 \\ 1 & 0 & 0 & 1 \\ 0 & 0 & 0 & 1 \\ 1 & 1 & 1 & 0 \end{bmatrix}
$$

For an undirected graph,

对于无向图来说，第i个节点的度可以对邻接矩阵的第i行或第i列求和得到

$$k_i = \sum_{j=1}^{\vert N \vert} A_{ij} \qquad \text{and} \qquad k_j = \sum_{i=1}^{\vert N \vert} A_{ij}$$

And likewise, for a directed graph,

同理，对于一个有向图，节点的出度为对邻接矩阵第i行求和，入度为对第i列求和

$$k_i^{out} = \sum_{j=1}^{\vert N \vert} A_{ij} \qquad \text{and} \qquad k_j^{in} = \sum_{i=1}^{\vert N \vert} A_{ij}$$

However, most real-world networks are sparse ($$ \vert E \vert \ll E_{max}$$, or $$\bar{k} \ll \vert N \vert -1$$). As a consequence, the adjacency matrix is filled with zeros (an undesirable property).

然而，现实生活中的图往往是稀疏的 ($$ \vert E \vert \ll E_{max}$$, or $$\bar{k} \ll \vert N \vert -1$$)。因此，邻接矩阵会有很多0存在，这不是我们喜欢的。

In order to alleviate this issue, we can represent graphs as a set of edges (an **edge list**). This makes edge lookups harder, but preserves memory.

为了解决这个问题，我们可以用边的列表来表示图。这样可以节省内存，但是不便于观察。

## Graph Connectivity 图的连通性

We call an undirected graph $$G$$ **connected** if there is a path between any pair of nodes in the graph. A **disconnected** graph is made up by two or more connected components. A **bridge edge** is an edge such that its removal disconnects $$G$$; an **articulation node** is a node such that its removal disconnects $$G$$. The adjacency matrix of networks with several components can be written in block-diagonal form (so that nonzero elements are confined to squares, and all other elements are 0). 

如果图中任意一对节点之间都有路径，我们称无向图$$ G $$ 是连通的。 “不连通”图由两个或多个连通的组件组成。把移除会使图变得不连通的边称为 **“桥边” **。把移除会使图变得不连通的节点称为 **“关节节点” **。具有多个分支的图的邻接矩阵可以写成对角块矩阵（非零元素将被限制为正方形，而所有其他元素均为0）。

We can further extend these concepts to directed graphs by definining a **strongly connected** directed graph as one which has a path from each node to every other node and vice versa (e.g. a path $$A \to B$$ and $$B \to A$$). A **weakly connected** directed graph is connected if we disregard the edge directions. We further define **strongly connected components** (SCCs) as strongly connected subgraphs of $$G$$. Nodes that can reach the SCC are part of its in-component, and nodes that can be reached from the SCC are part of its out-component.

我们可以将这些概念拓展到有向图。将“强连通”定义每个节点都具有到每个其他节点的路径，反之亦然（例如，有$ A→B $的路径 和$ B→A $的路径）。 如果忽略边的方向后是连通的，则称为弱连通。 我们进一步将“强连通组件”（SCC）定义为$$ G $$的强连通子图。 可以到达SCC的节点是其入分支的一部分，可以从SCC到达的节点是出分支的一部分。

The graph below is connected but not strongly connected. It contains one SCC (the graph $$G' = G[A, B, C]$$ induced on nodes A, B, and C).

下图是连通的，但非强连通。 它包含一个SCC（由节点A，B和C产生的诱导子图：$$ G'= G [A，B，C] $$）。

![directed-graph](../assets/img/introduction_directed_graph.png?style=centerme)

<br/>

|[Index](../) | [Previous](../) | [Next](./measuring-networks-random-graphs)|