---
layout: post
title: Influence Maximization
---

## Motivation 动机
Identification of influential nodes in a network has important practical uses. A good example is "viral marketing", a strategy that uses existing social networks to spread and promote a product. A well-engineered viral marking compaign will identify the most influential customers, convince them to adopt and endorse the product, and then spread the product in the social network like a virus.

识别网络中有影响力的节点是很有必要的。 例如“病毒式营销”，它是一种使用现有社交网络来传播和推广产品的策略，算法将识别出最具影响力的客户，说服他们采用并认可该产品，然后像病毒一样在社交网络中传播该产品。

The key question is how to find the most influential set of nodes? To answer this question, we will first look at two classical cascade models:

那么如何找到最具影响力的节点集？ 为了回答这个问题，我们首先来看两个经典的级联模型：

- Linear Threshold Model
- Independent Cascade Model
- 线性阈值模型
- 独立级联模型

Then, we will develop a method to find the most influential node set in the Independent Cascade Model.

然后，我们将开发一种方法来查找独立级联模型中最具影响力的节点集。

## Linear Threshold Model 线性阈值模型
In the Linear Threshold Model, we have the following setup:

在线性阈值模型中，我们具有以下设置：

- A node $$v$$ has a random threshold $$\theta_{v} \sim U[0,1]$$
- 节点$$ v $$具有随机阈值$$ \theta_ {v} \sim U [0,1] $$
- A node $$v$$ influenced by each neighbor $$w$$ according to a weight $$b_{v,w}$$, such that
- 每个邻居$$ w $$影响的节点$$ v $$的权重记为$$ b_{v,w} $$，要求

$$
\sum_{w\text{ neighbor of v }} b_{v,w}\leq 1
$$

- A node $$v$$ becomes active when at least $$\theta_{v}$$ fraction of its neighbors are active. That is
- 当节点$$ v $$的邻居中至少有$$ \theta_ {v} $$处于活动状态时，该节点被激活，即

$$
\sum_{w\text{ active neighbor of v }} b_{v,w}\geq\theta_{v}
$$

The following figure demonstrates the process:

下图演示了该过程：

![linear_threshold_model_demo](../assets/img/influence_maximization_linear_threshold_model_demo.png?style=centerme)

*(A) node V is activated and influences W and U by 0.5 and 0.2, respectively; (B) W becomes activated and influences X and U by 0.5 and 0.3, respectively; (C) U becomes activated and influences X and Y by 0.1 and 0.2, respectively; (D) X becomes activated and influences Y by 0.2; no more nodes can be activated; process stops.*

*（A）节点V被激活，对W和U的影响分别为0.5和0.2；（B）W被激活，分别以0.5和0.3影响X和U；（C）U被激活并分别以0.1和0.2影响X和Y； （D）X被激活并以0.2影响Y ，此时不能再激活任何节点，流程停止。*

## Independent Cascade Model 独立联级模型
In this model, we model the influences (activation) of nodes based on probabilities in a directed graph:

在此模型中，我们根据有向图中的概率对节点的影响（激活）进行建模：

- Given a directed finite graph $$G=(V, E)$$
- Given a node set $$S$$ starts with a new behavior (e.g. adopted new product and we say they are active)
- Each edge $$(v, w)$$ has a probability $$p_{vw}$$
- If node $$v$$ becomes active, it gets one chance to make $$w$$ active with probability $$p_{vw}$$
- Activation spread through the network
- 给定有限的有向图$$ G =(V,E)$$
- 假设节点集$$ S $$的状态均固定为激活（例如，采用的新产品）
- 每个边$$(v,w)$$都有概率$$ p_ {vw} $$
- 如果节点$$ v $$处于活动状态，则有机会以$$ p_ {vw} $$的概率使$$ w $$活动
- 激活通过网络传播

Note:

注意：

- Each edge fires only once
- If $$u$$ and $$v$$ are both active and link to $$w$$, it does not matter which tries to activate $$w$$ first
- 每个边仅有一次尝试激活的机会
- 如果$$ u $$和$$ v $$都处于活动状态并链接到$$ w $$，则哪个节点先尝试激活$$ w $$都没关系

## Influential Maximization (of the Independent Cascade Model)（独立级联模型的）影响力最大化

### Definitions
- **Most influential Set of size $$k$$** ($$k$$ is a user-defined parameter) is a set $$S$$ containing $$k$$ nodes that if activated, produces the largest expected{% include sidenote.html id='note-most-influential-set' note='Why "expected cascade size"? Due to the stochastic nature of the Independent Cascade Model, node activation is a random process, and therefore, $$f(S)$$ is a random variable. In practice, we would like to compute many random simulations and then obtain the expected value $$f(S)=\frac{1}{\mid I\mid}\sum_{i\in I}f_{i}(S)$$, where $$I$$ is a set of simulations.' %} cascade size $$f(S)$$.
- **最具影响力的大小为$$ k $$的集合 **（$$ k $$是用户定义的参数）是包含$$ k $$个节点的集合$$ S $$，如果激活这组节点，则会产生最大的期望级联激活大小$$ f(S)$$ {为什么是“期望的级联大小”？ 由于独立级联模型的随机性，节点激活是一个随机过程，因此$$ f(S)$$是一个随机变量。 实际上，我们计算许多随机模拟来获得期望值$$ f（S）= \frac {1} {\mid I \mid} \sum_ {i \in I} f_ {i}(S)$$，其中$$ I $$是一组模拟。}。
- **Influence set $$X_{u}$$ of node $$u$$** is the set of nodes that will be eventually activated by node $$u$$. An example is shown below.
- **节点$$ u $$的影响集$$ X_ {u} $$ **是节点$$ u $$最终将激活的节点集。 一个例子如下所示。

![influence_set](../assets/img/influence_maximization_influence_set.png?style=centerme)

*Red-colored nodes a and b are active. The two green areas enclose the nodes activated by a and b respectively, i.e. $$X_{a}$$ and $$X_{b}$$.*

*红色节点a和b处于活动状态。 两个绿色区域分别包围了由a和b激活的节点，即$$ X_ {a} $$和$$ X_ {b} $$。*

Note:

注意：

- It is clear that $$f(S)$$ is the size of the union of $$X_{u}$$: $$f(S)=\mid\cup_{u\in S}X_{u}\mid$$.
- Set $$S$$ is more influential, if $$f(S)$$ is larger
- 很明显，$$ f(S)$$是$$ X_ {u} $$的并集：$$ f(S)= \mid \cup_ {u\in S} X_ {u} \mid $$。
- 如果$$ f(S)$$较大，则点集$$ S $$具有更大的影响力

### Problem Setup
The influential maximization problem is then an optimization problem:

那么，影响力最大问题就是一个优化问题：
$$
\max_{S \text{ of size }k}f(S)
$$

This problem is NP-hard [[Kempe et al. 2003]](https://www.cs.cornell.edu/home/kleinber/kdd03-inf.pdf). However, there is a greedy approximation algorithm--**Hill Climbing** that gives a solution $$S$$ with the following approximation guarantee:

这个问题是NP难的。但有一个贪婪的近似算法——**Hill Climbing **，它给出具有以下近似保证的解$$ S $$：
$$
f(S)\geq(1-\frac{1}{e})f(OPT)
$$

where $$OPT$$ is the globally optimal solution.

其中$$OPT$$是全局最优解。

### Hill Climbing
**Algorithm:** at each step $$i$$, activate and pick the node $$u$$ that has the largest marginal gain $$\max_{u}f(S_{i-1}\cup\{u\})$$:

**算法：**在每一步$$ i $$，激活并选择具有最大边际增益$$\max_{u}f(S_{i-1}\cup\{u\})$$的节点$$u$$：

- Start with $$S_{0}=\{\}$$

- 以 $$S_{0}=\{\}$$开始

- For $$i=1...k$$

- 对于$$ i = 1 ... k $$

  - Activate node $$u\in V\setminus S_{i-1}$$ that $$\max_{u}f(S_{i-1}\cup{u})$$

  - Let $$S_{i}=S_{i-1}\cup{u}$$
  - 激活$$V \setminus S_ {i-1} $$中的节点$$ u $$，使$$\max_{u}f(S_{i-1}\cup{u})$$
  - 令 $$S_{i}=S_{i-1}\cup{u}$$

**Claim:** Hill Climbing produces a solution that has the approximation guarantee $$f(S)\geq(1-\frac{1}{e})f(OPT)$$.

**声明：**Hill Climbing产生的解决方案具有近似保证的解$$f(S)\geq(1-\frac{1}{e})f(OPT)$$

### Proof of the Approximation Guarantee of Hill Climbing
**Definition of Monotone:** if $$f(\emptyset)=0$$ and $$f(S)\leq f(T)$$ for all $$S\subseteq T$$, then $$f(\cdot)$$ is monotone.

**单调的定义**：如果所有$$ S \subseteq T $$的$$ f(\emptyset)= 0 $$和$$ f(S)\leq f(T)$$，则$$ f(\cdot)$$是单调的。

**Definition of Submodular:** if $$f(S\cup \{u\})-f(S)\geq f(T\cup\{u\})-f(T)$$ for any node $$u$$ and any $$S\subseteq T$$, then $$f(\cdot)$$ is submodular.

**次模函数的定义**：对于任何节点$u$和$$ S \subseteq T $$，，如果$$f(S\cup \{u\})-f(S)\geq f(T\cup\{u\})-f(T)$$则$$ f(\cdot)$$是次模的。

**Theorem [Nemhauser et al. 1978]:**{% include sidenote.html id='note-nemhauser-theorem' note='also see this [handout](http://web.stanford.edu/class/cs224w/handouts/CS224W_Influence_Maximization_Handout.pdf)' %} if $$f(\cdot)$$ is **monotone** and **submodular**, then the $$S$$ obtained by greedily adding $$k$$ elements that maximize marginal gains satisfies

**定理[Nemhauser等，1978）**：如果$$f(\cdot)$$ 是“单调”和“次模” 的，则通过贪婪地添加$$ k $$元素以使边际收益最大化来获得$$ S $$满足
$$
f(S)\geq(1-\frac{1}{e})f(OPT)
$$

Given this theorem, we need to prove that the largest expected cascade size function $$f(\cdot)$$ is monotone and submodular.

给定这个定理，我们只需要证明最大期望级联大小函数$$f(\cdot)$$是单调和亚模的即可。

**It is clear that the function $$f(\cdot)$$ is monotone based on the definition of $$f(\cdot)$${% include sidenote.html id='note-monotone' note='If no nodes are active, then the influence is 0. That is $$f(\emptyset)=0$$. Because activating more nodes will never hurt the influence, $$f(U)\leq f(V)$$ if $$U\subseteq V$$.' %}, and we only need to prove $$f(\cdot)$$ is submodular.**

**显然，根据$$f(\cdot)$$ 的定义，函数$$f(\cdot)$$是单调的。如果没有节点处于活动状态，则影响为0。即$$f(\emptyset)=0$$。因为激活更多节点永远不会有坏的影响，所以如果$$U\subseteq V$$，则$$f(U)\leq f(V)$$ 。还需要证明$$f(\cdot)$$是亚模的。**

**Fact 1 of Submodular Functions:** $$f(S)=\mid \cup_{k\in S}X_{k}\mid$$ is submodular, where $$X_{k}$$ is a set. Intuitively, the more sets you already have, the less new "area", a newly added set $$X_{k}$$ will provide.

**次模函数的事实1：** $$f(S)=\mid \cup_{k\in S}X_{k}\mid$$是次模的，其中$$ X_ {k} $$是一个集合。直观地看，您已经拥有的集合越多，新的“区域”越少，那么将新添加$$ X_ {k} $$也会更少一些。

**Fact 2 of Submodular Functions:** if $$f_{i}(\cdot)$$ are submodular and $$c_{i}\geq0$$, then $$F(\cdot)=\sum_{i}c_{i} f_{i}(\cdot)$$ is also submodular. That is a non-negative linear combination of submodular functions is a submodular function.

**次模函数的事实2：**如果$$f_{i}(\cdot)$$是次模函数且是$$c_{i}\geq0$$，则$$F(\cdot)=\sum_{i}c_{i} f_{i}(\cdot)$$也是次模函数的。即次模函数的非负线性组合也是次模函数。

**Proof that $$f(\cdot)$$ is Submodular**: we run many simulations on graph G (see sidenote 1). For the simulated world $$i$$, the node $$v$$ has an activation set $$X^{i}_{v}$$, then $$f_{i}(S)=\mid\cup_{v\in S}X^{i}_{v}\mid$$ is the size of the cascades of $$S$$ for world $$i$$. Based on Fact 1, $$f_{i}(S)$$ is submodular. The expected influence set size $$f(S)=\frac{1}{\mid I\mid}\sum_{i\in I}f_{i}(S)$$ is also submodular, due to Fact 2. QED.

**证明 $$f(\cdot)$$ 是次模的**：我们在图G上运行了许多模拟（请参见边注1）。对于平行世界$$ i $$，节点$$ v $$具有激活集$$ X ^ {i} _ {v} $$，然后$$f_{i}(S)=\mid\cup_{v\in S}X^{i}_{v}\mid$$是世界$$ i $$的$$ S $$级联的大小。根据事实1，$$f_{i}(S)$$是亚模的。由于事实2，期望影响集大小$$f(S)=\frac{1}{\mid I\mid}\sum_{i\in I}f_{i}(S)$$也是亚模的。 QED（*QED*=quod erat demonstrandum (拉丁文)，证明完毕）。

**Evaluation of $$f(S)$$ and Approximation Guarantee of Hill Climbing In Practice:** how to evaluate $$f(S)$$ is still an open question. The estimation achieved by simulating a number of possible worlds is a good enough evaluation [[Kempe et al. 2003]](https://www.cs.cornell.edu/home/kleinber/kdd03-inf.pdf):

**在实践中对$$f(S)$$的评估和Hill Climbing的近似保证：**如何评估$$f(S)$$仍然是一个悬而未决的问题。通过模拟许多可能的平行世界获得的估计值是一个足够好的评估。

- Estimate $$f(S)$$ by repeatedly simulating $$\Omega(n^{\frac{1}{\epsilon}})$$ possible worlds, where $$n$$ is the number of nodes and $$\epsilon$$ is a small positive real number
- It achieves $$(1\pm \epsilon)$$-approximation to $$f(S)$$
- Hill Climbing is now a $$(1-\frac{1}{e}-\epsilon)$$-approximation
- 通过反复模拟$$\Omega(n^{\frac{1}{\epsilon}})$$个平行世界来估算$$f(S)$$，其中$$ n $$是节点数，而$$\epsilon$$是一个小的正实数
- 它可以达到$$f(S)$$的$$(1\pm \epsilon)$$近似值
- Hill Climbing 现在是$$(1-\frac{1}{e}-\epsilon)$$的近似值

### Speed-up Hill Climbing by Sketch-Based Algorithms

### 用基于草图的算法加速Hill Climbing

**Time complexity of Hill Climbing** 时间复杂度

To find the node $$u$$ that $$\max_{u}f(S_{i-1}\cup\{u\})$$ (see the algorithm above):

要找到$$\max_{u}f(S_{i-1}\cup\{u\})$$ 的节点$$ u $$（请参见上面的算法）：

- we need to evaluate the $$X_{u}$$ (the influence set) of each of the remaining nodes which has the size of $$O(n)$$ ($$n$$ is the number of nodes in $$G$$)
- -我们需要评估其余每个节点的$$ X_ {u} $$（影响集），其大小为$$O(n)$$（其中$$ n $$是$$ G $$的总节点数）
- for each evaluation, it takes $$O(m)$$ time to flip coins for all the edges involved ($$m$$ is the number of edges in $$G$$)
- 对于每次评估，需要花费$$O(m)$$的时间来抛硬币（$$ m $$是$$ G $$的边数）
- we also need $$R$$ simulations to estimate the influence set ($$R$$ is the number of simulations/possible worlds)
- 还需要$$ R $$次模拟来估算影响集（$$ R $$是平行世界的数量）

We will do this $$k$$ (number of nodes to be selected) times. Therefore, the time complexity of Hill Climbing is $$O(k\cdot n \cdot m \cdot R)$$, which is slow. We can use **sketches** [[Cohen et al. 2014]](https://www.microsoft.com/en-us/research/wp-content/uploads/2014/08/skim_TR.pdf) to speed up the evaluation of $$X_{u}$$ by reducing the evaluation time from $$O(m)$$ to $$O(1)$${% include sidenote.html id='note-evaluate-influence' note='Besides sketches, there are other proposed approaches for efficiently evaluating the influence function: approximation by hypergraphs [[Borgs et al. 2012]](https://arxiv.org/pdf/1212.0884.pdf), approximating Riemann sum [[Lucier et al. 2015]](https://people.seas.harvard.edu/~yaron/papers/localApproxInf.pdf), sparsification of influence networks [[Mathioudakis et al. 2011]](https://chato.cl/papers/mathioudakis_bonchi_castillo_gionis_ukkonen_2011_sparsification_influence_networks.pdf), and heuristics, such as degree discount [[Chen et al. 2009]](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/weic-kdd09_influence.pdf).'%}.

我们将执行上述步骤$$ k $$（要选择的节点数）次。因此，Hill Climbing的时间复杂度为$$O(k\cdot n \cdot m \cdot R)$$，这很慢。我们可以使用**草图**，通过减少对$$ X_ {u} $$的评估来加速，从$$O(m)$$到$$O(1)$$ 。{除了草图之外，还有其他建议的方法可以有效地评估影响函数：通过超图近似等等。}

**Single Reachability Sketches**

**单一可达性草图**

- Take a possible world $$G^{i}$$ (i.e. one simulation of the graph $$G$$ using the Independent Cascade Model)
- Give each node a uniform random number $$\in [0,1]$$
- Compute the **rank** of each node $$v$$, which is the **minimum** number among the nodes that $$v$$ can reach in this world.
- 选择一个平行世界$$ G ^ {i} $$（即使用独立级联模型对图$$ G $$进行的模拟）
- 给每个节点一个均匀的随机数$$ \in [0,1] $$
- 计算每个节点$$ v $$的**rank **，代表$$ v $$在这个世界上可以到达的节点数量排名。

*Intuition: if $$v$$ can reach a large number of nodes, then its rank is likely to be small. Hence, the rank of node $$v$$ can be used to estimate the influence of node $$v$$ in $$G^{i}$$.*

*直觉：如果$$ v $$可以到达大量节点，那么其排名可能会很小。因此，节点$$ v $$的rank可用于估计节点$$ v $$在$$ G ^ {i} $$中的影响。*

However, influence estimation based on Single Reachability Sketches (i.e. single simulation of $$G$$ ) is inaccurate. To make a more accurate estimate, we need to build sketches based on many simulations{% include sidenote.html id='note-sketches' note='This is similar to take an average of $$f_{i}(S)$$ in sidenote 1, but in this case, it is achieved by using Combined Reachability Sketches.' %}, which leads to the Combined Reachability Sketches.

但是，基于单一可到达性草图（即$$ G $$的一次模拟）的影响力估算不准确。为了进行更准确的估算，我们需要基于许多模拟来构建草图{这类似于$$f_{i}(S)$$的平均花费，但可以通过使用组合可达性草图来实现。}，这会生成组合可达性草图。

**Combined Reachability Sketches**

In Combined Reachability Sketches, we simulate several possible worlds and keep the smallest $$c$$ values among the nodes that $$u$$ can reach in all the possible worlds.

在组合可达性草图中，我们模拟了多个平行世界。并且，在所有平行世界中，记录$$ u $$可到达节点的$$ c $$个最小值。

- Construct Combined Reachability Sketches:

  - Generate a number of possible worlds
  - For node $$u$$, assign uniformly distributed random numbers $$r^{i}_{v}\in[0,1]$$ to all $$(v, i)$$ pairs, where $$v$$ is the node in $$u$$'s reachable nodes set in the world $$i$$.
  - Take the $$c$$ smallest $$r^{i}_{v}$$ as the Combined Reachability Sketches
  - 产生许多平行世界
  - 对于节点$$ u $$，向所有$$(v, i)$$对分配均匀分布的随机数$$r^{i}_{v}\in[0,1]$$，其中$$ v $$是$$ u $$在世界$$ i $$中可达节点集中的节点
  - 记录$$ c $$个最小的 $$r^{i}_{v}$$ 作为组合可到达性草图

- Run Greedy for Influence Maximization:
  - Whenever the greedy algorithm asks for the node with the largest influence, pick node $$u$$ that has the smallest value in its sketch.
  - After $$u$$ is chosen, find its influence set $$X^{i}_{u}$$, mark the $$(v, i)$$ as infected and remove their $$r^{i}_{v}$$ from the sketches of other nodes.
  - 每当贪婪算法要求影响力最大的节点时，选择其草图中值最小的节点$$ u $$。
  - 选择$$ u $$后，找到其影响集$$ X ^ {i} _ {u} $$，将$$(v, i)$$ 标记为已感染，并草图中删除$$ r ^ {i } _ {v} $$

Note: using Combined Reachability Sketches does not provide an approximation guarantee on the true expected influence but an approximation guarantee with respect to the possible worlds considered.

注意：使用组合可达性草图不能为真实的期望影响提供近似保证，但可以为考察的平行世界提供近似保证。