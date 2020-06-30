---
layout: post
title: Outbreak Detection in Networks
---

## Introduction
The general goal of outbreak detection in networks is that given a dynamic process spreading over a network, we want to select a set of nodes to detect the process efficiently. Outbreak detection in networks has many applications in real life. For example, where should we place sensors to quickly detect contaminations in a water distribution network? Which person should we follow on Twitter to avoid missing important stories?

网络中爆发检测的目标是，在动态过程遍布网络的情况下，我们希望选择一组节点来有效地检测该过程。 网络中的爆发检测在现实生活中有许多应用。 例如，我们应该在哪里放置传感器以快速检测配水网中的污染物？ 我们应该在Twitter上关注哪个人，以避免错过重要故事？

The following figure shows the different effects of placing sensors at two different locations in a network:

下图显示了将传感器放置在网络中两个不同位置的不同效果：

![sensor_placement](../assets/img/outbreak_detection_sensor_placement.png?style=centerme)
*(A) The given network. (B) An outbreak $$i$$ starts and spreads as shown. (C) Placing a sensor at the blue position saves more people and also detects earlier than placing a sensor at the green position, though costs more.*

*A）给定的网络。 B）爆发$$ i $$开始传播。 C）将传感器放置在蓝色位置可以救更多人，并且比将传感器放置在绿色位置更早地进行检测，尽管费用更高。*

## Problem Setup
The outbreak detection problem is defined as below:

爆发检测问题定义如下：

- Given: a graph $$G(V,E)$$ and data on how outbreaks spread over this $$G$$ (for each outbreak $$i$$, we knew the time $$T(u,i)$$ when the outbreak $$i$$ contaminates node $$u$$).
- Goal: select a subset of nodes $$S$$ that maximize the expected reward:
- 已知：图$$G(V,E)$$和爆发如何在$$ G $$中扩散的数据（对于每次爆发$$ i $$，我们知道时间 $$T(u,i)$$ 代表爆发$$ i $$感染节点$$ u $$的时间
- 目标：选择子节点集$ S $，以使期望奖励最大化：

$$
\max_{S\subseteq U}f(S)=\sum_{i}p(i)\cdot f_{i}(S)
$$

$$
\text{subject to cost }c(S)\leq B
$$

where

其中

- $$p(i)$$: probability of outbreak $$i$$ occurring
- $$f_{i}(S)$$: rewarding for detecting outbreak $$i$$ using "sensors" $$S$${% include sidenote.html id='note-outbreak-detection-problem-setup' note='It is obvious that $$p(i)\cdot f_{i}(S)$$ is the expected reward for detecting the outbreak $$i$$' %}
- $$B$$: total budget of placing "sensors"
- $$p(i)$$:爆发$$ i $$的发生的可能性
- $$f_{i}(S)$$:奖励使用“传感器”S检测爆发的奖励。$$p(i)\cdot f_{i}(S)$$是检测爆发$ i $的预期奖励。$$ 
- B:放置“传感器”的总费用

**The Reward** can be one of the following three:

**奖励**可以是以下三种之一：

- Minimize the time to detection
- Maximize the number of detected propagations
- Minimize the number of infected people
- 减少检测时间
- 最大化检测到的传播数量
- 减少感染人数

**The Cost** is context-dependent. Examples are:

**费用**与具体环境有关。 例如：

- Reading big blogs is more time consuming
- Placing a sensor in a remote location is more expensive
- 阅读大型博客比较耗时
- 将传感器放置在偏远位置比较昂贵

## Outbreak Detection Formalization 形式化爆发检测问题

### Objective Function for Sensor Placements传感器放置的目标函数

Define the **penalty $$\pi_{i}(t)$$** for detecting outbreak $$i$$ at time $$t$$, which can be one of the following:{% include sidenote.html id='note-outbreak-detection-penalty-note' note='Notice: in all the three cases detecting sooner does not hurt! Formally, this means, for all three cases, $$\pi_{i}(t)$$ is monotonically nondecreasing in $$t$$.'%}

定义用于在时间$ t $时检测到爆发$ i $的惩罚$$\pi_{i}(t)$$，可以是以下之一：{注意：在所有这三种情况下，尽早检测不会造成伤害！这意味着$$\pi_{i}(t)$$是单调不减函数。}

- **Time to Detection (DT)**
  
- **检测时间（DT）**
  
  - How long does it take to detect an outbreak?

  - Penalty for detecting at time $$t$$: $$\pi_{i}(t)=t$$
  - 要用多长时间才能检测到一次爆发？
  - 在t时刻检测到爆发的惩罚 $$\pi_{i}(t)=t$$
  
- **Detection Likelihood (DL)**
  
- **检测概率（DL）**
  
  - How many outbreaks do we detect?
- Penalty for detecting at time $$t$$: $$\pi_{i}(0)=0$$, $$\pi_{i}(\infty)=1$${% include sidenote.html id='note-penalty-dl' note='this is a binary outcome:  $$\pi_{i}(0)=0$$ means we detect the outbreak and we pay 0 penalty, while $$\pi_{i}(\infty)=1$$ means we fail to detect the outbreak and we pay 1 penalty. That is we do not incur any penalty if we detect the outbreak in finite time, otherwise we incur penalty 1.'%}
  - 我们发现了几起疫情？
  - 在$$ t $$时刻检测到的惩罚：$$\pi_{i}(0)=0$$，$$\pi_{i}(\infty)=1$${这是一个二值化的结果$$\pi_{i}(0)=0$$表示我们检测到了爆发，所以惩罚为0，而$$\pi_{i}(\infty)=1$$ 表示爆发了100000000000年还未能检测到疫情，惩罚为1。}
  
- **Population Affected (PA)**
  
- **被感染的人数（PA）**
  
  - How many people/nodes get infected during an outbreak?
  - Penalty for detecting at time $$t$$: $$\pi_{i}(t)=$$ number of infected nodes in the outbreak $$i$$ by time $$t$$
  - 爆发期间有多少人/节点被感染？
  - 在$$ t $$时刻检测到的惩罚：$$\pi_{i}(t)=$$到$$ t $$时刻爆发$$ i $$感染的总节点数

The objective **reward function $$f_{i}(S)$$ of a sensor placement $$S$$** is defined as penalty reduction:

放置传感器$$ S $$ 的目标函数$$f_{i}(S)$$ 被定义为使惩罚降低的量：
$$
f_{i}(S)=\pi_{i}(\infty)-\pi_{i}(T(S,i))
$$

where $$T(S,i)$$ is the time when the set of "sensors" $$S$$ detects the outbreak $$i$$.

其中， $$T(S,i)$$ 是传感器集S检测到爆发i的时间。

### Claim 1: $$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$ is monotone{% include sidenote.html id='note-monotone' note='For the definition of monotone, see [Influence Maximization](influence-maximization)' %}

### 声明1：$$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$是单调的

Firstly, we do not reduce the penalty, if we do not place any sensors. Therefore, $$f_{i}(\emptyset)=0$$ and $$f(\emptyset)=\sum_{i}p(i)\cdot f_{i}(\emptyset)=0$$.

首先，如果我们不放置任何传感器，我们不会减少惩罚。 因此，$$f_{i}(\emptyset)=0$$ 并且$$f(\emptyset)=\sum_{i}p(i)\cdot f_{i}(\emptyset)=0$$。

Secondly, for all $$A\subseteq B\subseteq V$$ ($$V$$ is all the nodes in $$G$$), $$T(A,i)\geq T(B,i)$$, and

其次，对于所有$$A\subseteq B\subseteq V$$ （$$ V $$是$$ G $$中的所有节点），$$T(A,i)\geq T(B,i)$$，并且：
$$
\begin{align*}
f_{i}(A)-f_{i}(B)&=\pi_{i}(\infty)-\pi_{i}(T(A,i))-[\pi_{i}(\infty)-\pi_{i}(T(B,i))]\\
&=\pi_{i}(T(B,i))-\pi_{i}(T(A,i))
\end{align*}
$$

Because $$\pi_{i}(t)$$ is monotonically nondecreasing in $$t$$ (see sidenote 2), $$f_{i}(A)-f_{i}(B)<0$$. Therefore, $$f_{i}(S)$$ is nondecreasing. It is obvious that $$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$ is also nondecreasing, since $$p(i)\geq 0$$.

因为$$\pi_{i}(t)$$在$$ t $$中单调不减，所以$$f_{i}(A)-f_{i}(B)<0$$。 因此，$$f_{i}(S)$$单调不减。 显然，$$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$ 也是不减的，因为$$p(i)\geq 0$$。

### Claim 2: $$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$ is submodular{% include sidenote.html id='note-submodular' note='For the definition of submodular, see [Influence Maximization](influence-maximization)' %}

### 声明2：$$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$是次模函数

This is to proof for all $$A\subseteq B\subseteq V$$ $$x\in V \setminus B$$:

这是为了证明对所有 $$A\subseteq B\subseteq V$$，$$x\in V \setminus B$$：
$$
f(A\cup \{x\})-f(A)\geq f(B\cup\{x\})-f(B)
$$

There are three cases when sensor $$x$$ detects the outbreak $$i$$:

在三种情况下，传感器$$ x $$检测到爆发$$ i $$：

1. $$T(B,i)\leq T(A, i)<T(x,i)$$ ($$x$$ detects late): nobody benefits. That is $$f_{i}(A\cup\{x\})=f_{i}(A)$$ and $$f_{i}(B\cup\{x\})=f_{i}(B)$$. Therefore, $$f(A\cup \{x\})-f(A)=0= f(B\cup\{x\})-f(B)$$
2. $$T(B, i)\leq T(x, i)<T(A,i)$$ ($$x$$ detects after $$B$$ but before $$A$$): $$x$$ only helps to improve the solution of $$A$$ but not $$B$$. Therefore, $$f(A\cup \{x\})-f(A)\geq 0 = f(B\cup\{x\})-f(B)$$
3. $$T(x, i)<T(B,i)\leq T(A,i)$$ ($$x$$ detects early): $$f(A\cup \{x\})-f(A)=[\pi_{i}(\infty)-\pi_{i}(T(x,t))]-f_{i}(A)$$$$ \geq [\pi_{i}(\infty)-\pi_{i}(T(x,t))]-f_{i}(B) = f(B\cup\{x\})-f(B)$${% include sidenote.html id='note-submodularity-proof1' note='Inequality is due to the nondecreasingness of $$f_{i}(\cdot)$$, i.e. $$f_{i}(A)\leq f_{i}(B)$$ (see Claim 1).'%}
4. $$T(B,i)\leq T(A, i)<T(x,i)$$，$$ x $$较晚检测到：无人受益。即$$f_{i}(A\cup\{x\})=f_{i}(A)$$ ，$$f_{i}(B\cup\{x\})=f_{i}(B)$$。因此，$$f(A\cup \{x\})-f(A) = 0 = f(B\cup\{x\})-f(B)$$
2. $$T(B, i)\leq T(x, i)<T(A,i)$$ （$$ x $$在$$ B $$之后但在$$ A $$之前检测到）：$$x$$仅有助于改善$$ A $$的解决方案，而无助于$$ B $$。因此，$$f(A\cup \{x\})-f(A)\geq 0 = f(B\cup\{x\})-f(B)$$
3. $$T(x, i)<T(B,i)\leq T(A,i)$$ （$$ x $$最早检测到）：$$f(A\cup \{x\})-f(A)=[\pi_{i}(\infty)-\pi_{i}(T(x,t))]-f_{i}(A) \geq [\pi_{i}(\infty)-\pi_{i}(T(x,t))]-f_{i}(B) = f(B\cup\{x\})-f(B)$${由于$$f_{i}(\cdot)$$单调不减，即$$f_{i}(A)\leq f_{i}(B)$$}

Therefore, $$f_{i}(S)$$ is submodular. Because $$p(i)\geq 0$$, $$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$ is also submodular.{% include sidenote.html id='note-submodularity-proof1' note='Fact: a non-negative linear combination of submodular functions is a submodular function.'%}

因此，$$f_{i}(S)$$是次模的。因为$$p(i)\geq 0$$，所以$$f(S)=\sum_{i}p(i)\cdot f_{i}(S)$$也是次模的。

We know that the Hill Climbing algorithm works for optimizing problems with nondecreasing submodular objectives. However, it does not work well in this problem:

爬山算法可用于优化次模目标函数的问题。但它不能很好地解决此问题：

- Hill Climbing only works for the cases that each sensor costs the same. For this problem, each sensor has cost $$c(s)$$.
- 爬山仅适用于每个传感器成本相同的情况。对于此问题，每个传感器的成本为$$c(s)$$。
- Hill Climbing is also slow: at each iteration, we need to re-evaluate marginal gains of all nodes. The run time is $$O(\mid V\mid\cdot k)$$ for placing $$k$$ sensors.
- 爬山也很慢：在每次迭代中，我们都需要重新评估所有节点的边际收益。放置$$ k $$个传感器的运行时间为$$O(\mid V\mid\cdot k)$$。

Hence, we need a new fast algorithm that can handle cost constraints.

因此，我们需要一种可以处理限制成本的新型快速算法。

## CELF: Algorithm for Optimziating Submodular Functions Under Cost Constraints

## CELF：成本约束下的优化次模函数的算法

### Bad Algorithm 1: Hill Climbing that ignores the cost

### 错误的算法1：忽略费用的爬山

**Algorithm**

- Ignore sensor cost $$c(s)$$
- Repeatedly select sensor with highest marginal gain
- Do this until the budget is exhausted
- 忽略传感器成本$$c(s)$$
- 重复选择具有最高边际增益的传感器
- 这样做直到预算用尽

**This can fail arbitrarily bad!** Example:

**这可能会导致很离谱的结果**，比如

- Given $$n$$ sensors and a budget $$B$$
- $$s_{1}$$: reward $$r$$, cost $$B$$
- $$s_{2}$$,..., $$s_{n}$$: reward $$r-\epsilon$$, cost $$\epsilon$$ ($$\epsilon$$ is an arbitrary positive small number)
- Hill Climbing always prefers $$s_{1}$$ to other cheaper sensors, resulting in an arbitrarily bad solution with reward $$r$$ instead of the optimal solution with reward $$\frac{B(r-\epsilon)}{\epsilon}$$, when $$\epsilon \rightarrow 0$$.
- 假设有$ n $个传感器和预算$ B $
- $$ s_ {1} $$：奖励$$ r $$，花费$$ B $$
- $$ s_ {2} $$，...，$$ s_ {n} $$：奖励$$ r- \epsilon $$，花费$$ \epsilon $$（$$ \epsilon $$是任意的正数）
- 爬山总是比其他便宜的传感器更喜欢$$ s_ {1} $$，从而导致奖励为$ r $，这是一个很糟糕的解决方案，而不是奖励为 $$\frac{B(r-\epsilon)}{\epsilon}$$的最优解决方案。

### Bad Algorithm 2: optimization using benefit-cost ratio

### 错误算法2：使用收益成本比进行优化

**Algorithm**

- Greedily pick the sensor $$s_{i}$$ that maximizes the benefit to cost ratio until the budget runs out, i.e. always pick
- 贪婪地选择传感器$$ s_ {i} $$，以在预算用完之前最大化收益/成本比，即始终选择

$$
s_{i}=\arg\max_{s\in(V\setminus A_{i-1})}\frac{f(A_{i-1}\cup\{s\})-f(A_{i-1})}{c(s)}
$$

**This can fail arbitrarily bad!** Example:

**这也会导致很离谱的结果**

- Given 2 sensors and a budget $$B$$
- $$s_{1}$$: reward $$2\epsilon$$, cost $$\epsilon$$
- $$s_{2}$$: reward $$B$$, cost $$B$$
- Then the benefit ratios for the first selection are: 2 and 1, respectively
- This algorithm will pick $$s_{1}$$ and then cannot afford $$s_{2}$$, resulting in an arbitrarily bad solution with reward $$2\epsilon$$ instead of the optimal solution $$B$$, when $$\epsilon \rightarrow 0$$.
- 假设有2个传感器，预算为$ B $
- $$ s_ {1} $$：奖励$$ 2 \epsilon $$，花费$$ \epsilon $$
- $$ s_ {2} $$：奖励$$ B $$，花费$$ B $$
- 那么选择二者的收益率分别是：2和1
- 此算法将选择$$ s_ {1} $$，然后负担不起$$ s_ {2} $$，从而导致奖励为$$ 2 \epsilon $$的垃圾解决方案，而不是最优解决方案$$ B $$。

### Solution: CELF (Cost-Effective Lazy Forward-selection)

**CELF** is a two-pass greedy algorithm [[Leskovec et al. 2007]](https://www.cs.cmu.edu/~jure/pubs/detect-kdd07.pdf):

CELF是两个步骤的贪婪算法：

- Get solution $$S'$$ using unit-cost greedy (Bad Algorithm 1)
- Get solution $$S''$$ using benefit-cost greedy (Bad Algorithm 2)
- Final solution $$S=\arg\max[f(S'), f(S'')]$$
- 忽略成本，获取解决方案$$ S'$$（错误算法1）
- 使用收益成本比，获取解决方案$$ S''$$（错误算法2）
- 最终解决方案 $$S=\arg\max[f(S'), f(S'')]$$

**Approximation Guarantee**
- CELF achieves $$\frac{1}{2}(1-\frac{1}{e})$$ factor approximation.
- CELF获得$$\frac{1}{2}(1-\frac{1}{e})$$近似值。


CELF also uses a lazy evaluation of $$f(S)$$ (see below) to speedup Hill Climbing.

CELF还使用$$f(S)$$的惰性评估（请参见下文）来加快爬坡速度。

## Lazy Hill Climbing: Speedup Hill Climbing

### Intuition

- In Hill Climbing, in round $$i+1$$, we have picked $$S_{i}=\{S_{1},...,S_{i}\}$$ sensors. Now, pick $$s_{i+1}=\arg\max_{u}f(S_{i}\cup \{u\})-f(S_{i})$$
- By submodularity $$f(S_{i}\cup\{u\})-f(S_{i})\geq f(S_{j}\cup\{u\})-f(S_{j})$$ for $$i<j$$.
- Let $$\delta_{i}(u)=f(S_{i}\cup\{u\})-f(S_{i})$$ and $$\delta_{j}(u)=f(S_{j}\cup\{u\})-f(S_{j})$$ be the marginal gains. Then, we can use $$\delta_{i}$$ as upper bound on $$\delta_{j}$$ for ($$j>i$$)
- 在“爬山算法”中，第$$ i + 1 $$轮，我们已经选择了 $$S_{i}=\{S_{1},...,S_{i}\}$$。现在，选择$$s_{i+1}=\arg\max_{u}f(S_{i}\cup \{u\})-f(S_{i})$$
- 通过次模函数$$f(S_{i}\cup\{u\})-f(S_{i})\geq f(S_{j}\cup\{u\})-f(S_{j})$$，对$$ i <j $$。
- 让$$\delta_{i}(u)=f(S_{i}\cup\{u\})-f(S_{i})$$和 $$\delta_{j}(u)=f(S_{j}\cup\{u\})-f(S_{j})$$表示边际收益。然后，我们可以将$$\delta_{i}$$当作的$$\delta_{j}$$的上限（$$ j> i $$）。

### Lazy Hill Climbing Algorithm:
- Keep an ordered list of marginal benefits $$\delta_{i-1}$$ from previous iteration
- Re-evaluate $$\delta_{i}$$ only for the top nodes
- Reorder and prune from the top nodes
- 保留前一次迭代边际收益 $$\delta_{i-1}$$ 的有序列表
- 仅对顶部节点重新评估 $$\delta_{i}$$
- 从顶部节点重新排序和修剪

The following figure show the process.

![lazy_evaluation](../assets/img/outbreak_detection_lazy_evaluation.png?style=centerme)

*(A) Evaluate and pick the node with the largest marginal gain $$\delta$$. (B) reorder the marginal gain for each sensor in decreasing order. (C) Re-evaluate the $$\delta$$s in order and pick the possible best one by using previous $$\delta$$s as upper bounds. (D) Reorder and repeat.*

*A）评估并选择具有最大边际收益$$ \delta $$的节点。 B）以降序重新排列每个传感器的边际增益。 C）重新评估$$\delta$$ s的顺序，并使用以前的$$\delta$$ s作为上限，选择可能的最佳选择。 D）重新排序并重复。*

Note: the worst case of Lazy Hill Climbing has the same time complexity as normal Hill Climbing. However, it is on average much faster in practice.

注意：最差的惰性爬山情况与普通爬山具有相同的时间复杂度。但实际上平均起来要快得多。

## Data-Dependent Bound on the Solution Quality

## 取决于数据的解决方案质量

### Introduction
- Value of the bound depends on the input data
- On "easy data", Hill Climbing may do better than the $$(1-\frac{1}{e})$$ bound for submodular functions
- 上下限取决于输入数据
- 在“简单数据”上，爬坡的效果可能会好于次模函数的$$(1-\frac{1}{e})$$

### Data-Dependent Bound
Suppose $$S$$ is some solution to $$f(S)$$ subjected to $$\mid S \mid\leq k$$, and $$f(S)$$ is monotone and submodular.

假设$$ S $$是对$$f(S)$$，受限于$$\mid S \mid\leq k$$的某种解决方案，而 $$f(S)$$是单调和亚模的。

- Let $$OPT={t_{i},...,t_{k}}$$ be the optimal solution
- For each $$u$$ let $$\delta(u)=f(S\cup\{u\})-f(S)$$
- Order $$\delta(u)$$ so that $$\delta(1)\geq \delta(2)\geq...$$
- Then, the **data-dependent bound** is $$f(OPT)\leq f(S)+\sum^{k}_{i=1}\delta(i)$$
- 记$$ OPT = {t_ {i},...,t_ {k}} $$是最佳解决方案
- 对于每个$$ u $$，让$$\delta(u)=f(S\cup\{u\})-f(S)$$
- 排序$$\delta(u)$$，以使$$\delta(1)\geq \delta(2)\geq...$$
- 然后，“与数据相关的界限”为$$f(OPT)\leq f(S)+\sum^{k}_{i=1}\delta(i)$$

Proof:{% include sidenote.html id='note-data-dependent-bound-proof' note='For the first inequality, see [the lemma in 3.4.1 of this handout](http://web.stanford.edu/class/cs224w/handouts/CS224W_Influence_Maximization_Handout.pdf). For the last inequality in the proof: instead of taking $$t_{i}\in OPT$$ of benefit $$\delta(t_{i})$$, we take the best possible element $$\delta(i)$$, because we do not know $$t_{i}$$.'%}

证明：{对于证明中的最后一个不等式：我们取最大可能的元素$$\delta(i)$$，而不是从收益 $$\delta(t_{i})$$中提取$$t_{i}\in OPT$$，因为我们不知道$$ t_ {i} $$。}
$$
\begin{align*}
  f(OPT)&\leq f(OPT\cup S)\\
  &=f(S)+f(OPT\cup S)-f(S)\\
  &\leq f(S)+\sum^{k}_{i=1}[f(S\cup\{t_{i}\})-f(S)]\\
  &=f(S)+\sum^{k}_{i=1}\delta(t_{i})\\
  &\leq f(S)+\sum^{k}_{i=1}\delta(i)
\end{align*}
$$

Note:

- This bound hold for the solution $$S$$ (subjected to $$\mid S \mid\leq k$$) of any algorithm having the objective function $$f(S)$$ monotone and submodular.
- The bound is data-dependent, and for some inputs it can be very "loose" (worse than $$(1-\frac{1}{e})$$)
- 具有目标函数 $$f(S)$$ 单调和次模函数的任何算法的解$$ S $$（受$$\mid S \mid\leq k$$约束）有下限保证。
- 界限取决于数据，对于某些输入，界限可能非常“松散”（比$$(1-\frac{1}{e})$$差）

