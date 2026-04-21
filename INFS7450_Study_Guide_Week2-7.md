# INFS7450 Social Media Analytics Study Guide

Version: Week 2 to Week 7, prepared on 2026-04-21

This document is designed as a living study guide. When Week 8 to Week 13 lecture slides, quizzes, tutorials, or sample questions are added later, this file should be updated rather than replaced.

中文说明：这份文档是一个持续更新版复习讲义。目前基于 Week 2 到 Week 7 的 lecture 内容，并结合历年考试题型进行定向整理。后续你补充 Week 8 到 Week 13 的 PPT、平时测试题、tutorial 题之后，可以继续在这份文件里补充新的单元和预测考点。

## How To Use This Guide

English: This course is not mainly about writing code in the final exam. The final exam is more likely to test whether you can understand graph concepts, apply formulas to small graphs by hand, explain algorithms clearly, and compare models or methods.

中文：这门课期末考试的重点不是现场写代码，而是考你是否能理解图相关概念、对小图手算公式、清楚解释算法流程，并比较不同模型或方法。

English: For high marks, do not only read the slides. You need to convert each lecture topic into three things: vocabulary, calculation method, and exam-style explanation.

中文：想拿高分，不能只是看懂 slides。你需要把每个 lecture topic 转换成三类东西：重点词汇、计算方法、考试型解释。

Recommended study order:

1. Learn the vocabulary and concept meaning first.
2. Memorise the formula and understand every symbol.
3. Practise with a small example.
4. Redo past exam questions of the same type.
5. Write a short English answer template for conceptual questions.

中文学习顺序：

1. 先掌握重点词汇和概念含义。
2. 再记公式，并理解每个符号是什么意思。
3. 用一个小例子演示公式。
4. 重做同类型历年考试题。
5. 为概念题准备英文短答模板。

## Unit Map Based On Current Lectures

| Unit | Lecture | Main Topic | Exam Direction |
|---|---|---|---|
| Unit 1 | Week 2 | Graph Essentials | Graph representation, degree, paths, components, diameter, BFS/DFS |
| Unit 2 | Week 3 | Node Measures I | Degree, closeness, harmonic, eigenvector, Katz centrality |
| Unit 3 | Week 4 | Node Measures II | PageRank, HITS, betweenness centrality |
| Unit 4 | Week 5 | Network Measures and Models | Degree distribution, clustering coefficient, average path length, random graph |
| Unit 5 | Week 6 | Small-World and Assortativity | Small-world model, configuration model, modularity, Pearson correlation |
| Unit 6 | Week 7 | Network Effects and Information Diffusion | Influence, homophily, LTM, ICM, herd behavior, information cascade |
| Unit 7 | Week 8 to Week 13 | To be updated | Community detection, link prediction, embedding, GNN, epidemics, and other later topics |

中文单元划分：目前老师讲到 Week 7，所以这份讲义先分成 6 个已学单元，并预留第 7 个后续更新单元。每个单元都按照考试用途整理，而不是只按照 PPT 页码整理。

---

# Unit 1: Graph Essentials

Related lecture: Week 2, Graph Essentials

考试定位：这是整门课的底层语言。后面所有 centrality、clustering、diffusion、community detection、link prediction 都默认你能读懂 graph。

## 1.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| Graph | 图 | 用节点和边表示对象之间的关系 |
| Node / Vertex | 节点 / 顶点 | 图中的对象，例如用户、网页、帖子 |
| Edge / Link | 边 / 连接 | 节点之间的关系，例如朋友关系、关注关系 |
| Directed Graph | 有向图 | 边有方向，例如 A follows B |
| Undirected Graph | 无向图 | 边没有方向，例如朋友关系 |
| Degree | 度 | 一个节点连接了多少条边 |
| In-degree | 入度 | 有多少边指向该节点 |
| Out-degree | 出度 | 该节点指出去多少条边 |
| Degree Distribution | 度分布 | 每种度数的节点比例 |
| Adjacency Matrix | 邻接矩阵 | 用矩阵表示节点之间是否相连 |
| Edge List | 边列表 | 直接列出所有边 |
| Adjacency List | 邻接列表 | 对每个节点列出它的邻居 |
| Walk | 游走 | 可以重复节点和边的连续路径 |
| Trail | 迹 | 不重复边的 walk |
| Path | 路径 | 不重复节点的 walk |
| Cycle | 环 | 起点和终点相同的 path |
| Component | 连通分量 | 内部互相可达、外部不相连的一组节点 |
| Shortest Path | 最短路径 | 两节点之间边数最少的路径 |
| Diameter | 直径 | 所有最短路径中最长的距离 |

## 1.2 Core Concept Explanations

### Graph

English: A graph is a structure made of nodes and edges. Nodes represent objects, and edges represent relationships or interactions between objects. In social media analytics, users, posts, hashtags, web pages, or products can all be modeled as nodes.

中文：图是由节点和边构成的结构。节点表示对象，边表示对象之间的关系或互动。在社交媒体分析中，用户、帖子、话题标签、网页、商品都可以被建模为节点。

Exam use: If an exam question gives a social scenario, first identify what the nodes are and what the edges represent.

中文考试用法：如果考试题给你一个社交场景，第一步要判断什么是节点，什么是边。

### Directed Graph vs Undirected Graph

English: In a directed graph, an edge has direction. For example, if user A follows user B, the edge goes from A to B. In an undirected graph, the edge has no direction. Friendship is often modeled as undirected because the relationship is mutual.

中文：有向图里的边有方向。例如 A 关注 B，边从 A 指向 B。无向图里的边没有方向，例如朋友关系通常被建模为无向，因为这种关系是相互的。

Exam use: In a directed graph, always separate in-degree and out-degree. In an undirected graph, the degree is just the number of incident edges.

中文考试用法：有向图中必须区分入度和出度。无向图中 degree 就是该节点连接的边数。

### Degree Distribution

English: Degree distribution tells us the fraction of nodes with each degree value. It is a network-level summary because it describes the whole graph, not just one node.

中文：度分布描述每种度数的节点在整个图中所占比例。它是网络层面的统计量，因为它描述的是整个图，而不是单个节点。

Exam use: This is a very common multiple-choice calculation. Do not choose from the options directly. First write down the degree of every node, then count how many nodes have degree 1, degree 2, degree 3, and so on.

中文考试用法：这是非常常见的选择题计算。不要直接看选项猜。先写出每个节点的 degree，再统计 degree 为 1、2、3 等的节点数量。

### Graph Representation

English: A graph can be represented by an adjacency matrix, edge list, or adjacency list. The adjacency matrix is convenient for mathematical operations, but it wastes space when the graph is large and sparse. Edge lists are compact and often used in datasets. Adjacency lists are efficient for finding neighbours.

中文：图可以用邻接矩阵、边列表或邻接列表表示。邻接矩阵适合数学计算，但当图很大且稀疏时会浪费空间。边列表简洁，常见于数据集。邻接列表适合快速查找邻居。

Exam use: Past exams ask students to generate adjacency matrix, edge list, and adjacency list from a given graph. You must be able to convert between these formats.

中文考试用法：历年考试会让你根据图写出 adjacency matrix、edge list 和 adjacency list。你必须能在这三种表示之间转换。

### Shortest Path and Diameter

English: The shortest path between two nodes is the path with the smallest number of edges. The diameter of a graph is the maximum shortest-path distance among all node pairs.

中文：两个节点之间的最短路径是边数最少的路径。图的直径是所有节点对之间最短路径距离的最大值。

Exam use: Diameter questions are usually easy marks if you systematically list all shortest distances. The risk is missing one long pair.

中文考试用法：diameter 题如果你系统列出所有节点对距离，一般是送分题。最大风险是漏掉某一对距离最长的节点。

## 1.3 Key Formulas

### Degree Distribution

Formula:

```text
P(k) = N_k / N
```

Meaning:

```text
P(k) = proportion of nodes with degree k
N_k = number of nodes with degree k
N = total number of nodes
```

中文含义：

```text
P(k) = 度为 k 的节点比例
N_k = 度为 k 的节点数量
N = 总节点数量
```

### Number of Edges in a Complete Undirected Graph

Formula:

```text
E_max = N(N - 1) / 2
```

中文含义：如果无向图中每两个节点之间都有一条边，那么最多有 N(N - 1) / 2 条边。

### Sum of Degrees in an Undirected Graph

Formula:

```text
sum of degrees = 2m
```

Meaning:

```text
m = number of edges
```

中文含义：无向图中所有节点度数之和等于边数的两倍，因为每条边会贡献两个端点的 degree。

## 1.4 Formula Example

Example graph:

```text
Edges: (1,2), (1,3), (2,3), (3,4), (4,5)
Nodes: 1, 2, 3, 4, 5
```

Step 1: Find the degree of each node.

```text
degree(1) = 2, connected to 2 and 3
degree(2) = 2, connected to 1 and 3
degree(3) = 3, connected to 1, 2, and 4
degree(4) = 2, connected to 3 and 5
degree(5) = 1, connected to 4
```

中文：先列每个节点的 degree，不要跳过这一步。

Step 2: Count how many nodes have each degree.

```text
Degree 1: node 5, so N_1 = 1
Degree 2: nodes 1, 2, 4, so N_2 = 3
Degree 3: node 3, so N_3 = 1
Total nodes N = 5
```

Step 3: Compute degree distribution.

```text
P(1) = 1 / 5
P(2) = 3 / 5
P(3) = 1 / 5
```

中文结论：这个图的 degree distribution 是 P(1)=1/5, P(2)=3/5, P(3)=1/5。

## 1.5 Exam Targets

Must know:

- Compute degree distribution from a graph.
- Convert a graph into adjacency matrix, edge list, and adjacency list.
- Distinguish directed and undirected edges.
- Compute shortest path and diameter.
- Understand BFS and DFS traversal order when a tie-breaking rule is given.

中文重点：

- 会算 degree distribution。
- 会写 adjacency matrix、edge list、adjacency list。
- 会区分 directed graph 和 undirected graph。
- 会算 shortest path 和 diameter。
- 会根据 leftmost 或 rightmost tie-breaking rule 推 BFS 和 DFS。

Prediction for this year:

English: Since Week 2 is the foundation and past exams repeatedly test graph representation and degree distribution, this year is very likely to include at least one small graph calculation from this unit. A BFS/DFS traversal question is also possible if tutorials or quizzes reinforce traversal algorithms.

中文预测：由于 Week 2 是基础，而且历年考试反复考 graph representation 和 degree distribution，今年很可能至少出现一道小图计算题。如果后续 tutorial 或 quiz 强调 BFS/DFS，也可能考遍历顺序。

---

# Unit 2: Node Measures I

Related lecture: Week 3, Node Measures I

考试定位：这是前半卷的高频计算区。你需要能根据图手算 centrality，不只是会背定义。

## 2.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| Centrality | 中心性 | 衡量节点重要性 |
| Degree Centrality | 度中心性 | 看节点直接连接数 |
| Closeness Centrality | 接近中心性 | 看节点到其他节点是否整体更近 |
| Harmonic Centrality | 调和中心性 | 适合处理不连通图 |
| Distance | 距离 | 两节点之间最短路径长度 |
| Reachable | 可达 | 一个节点可以通过路径到达另一个节点 |
| Disconnected Graph | 不连通图 | 并非所有节点之间都有路径 |
| Eigenvector Centrality | 特征向量中心性 | 考虑邻居的重要性 |
| Katz Centrality | Katz 中心性 | 在 eigenvector centrality 上加入基础分数 |
| Alpha | 衰减参数 | 控制远距离连接影响 |
| Beta | 基础分数 | 给每个节点的初始重要性 |

## 2.2 Core Concept Explanations

### Centrality

English: Centrality measures how important or influential a node is in a network. Different centrality measures define importance differently. Some focus on direct connections, some focus on distance, and some focus on being connected to other important nodes.

中文：中心性用来衡量一个节点在网络中有多重要或多有影响力。不同中心性指标对“重要”的定义不同。有的看直接连接数量，有的看距离，有的看是否连接到其他重要节点。

Exam use: If a question asks which node is most important, first identify which centrality measure is being used.

中文考试用法：如果题目问哪个节点最重要，先看题目指定的是哪一种 centrality。

### Degree Centrality

English: Degree centrality measures the number of direct connections a node has. In social media, a user with high degree may have many friends, followers, or interactions.

中文：度中心性衡量一个节点有多少直接连接。在社交媒体中，degree 高的用户可能有很多朋友、粉丝或互动。

Limitation:

English: Degree centrality only considers the quantity of neighbours, not whether those neighbours are important.

中文：degree centrality 只考虑邻居数量，不考虑邻居本身是否重要。

### Closeness Centrality

English: Closeness centrality measures how close a node is to all other nodes. A node with high closeness can reach the rest of the network through short paths.

中文：接近中心性衡量一个节点到其他所有节点是否距离较短。closeness 高的节点可以通过较短路径到达网络其他部分。

Limitation:

English: Closeness centrality has problems in disconnected graphs because unreachable nodes have infinite distance.

中文：closeness centrality 在不连通图中有问题，因为不可达节点的距离是无穷大。

### Harmonic Centrality

English: Harmonic centrality sums the inverse of distances from other nodes. If a node is unreachable, its contribution becomes zero because 1 divided by infinity is zero.

中文：调和中心性把到其他节点的距离取倒数后相加。如果某个节点不可达，它的贡献就是 0，因为 1 除以无穷大等于 0。

Exam use: Harmonic centrality is frequently tested because it is simple to calculate once you have the distance table.

中文考试用法：harmonic centrality 是高频考点，因为只要你列出距离表，就能直接计算。

### Eigenvector Centrality

English: Eigenvector centrality says that a node is important if it is connected to other important nodes. This is different from degree centrality because not all neighbours contribute equally.

中文：特征向量中心性认为，如果一个节点连接到了其他重要节点，那么它本身也更重要。这和 degree centrality 不同，因为不是所有邻居的贡献都相同。

Exam use: The exam often asks for limitations of eigenvector centrality in directed graphs.

中文考试用法：考试经常问 eigenvector centrality 在 directed graph 中的局限。

### Katz Centrality

English: Katz centrality improves eigenvector centrality by adding a constant baseline score to every node. This prevents some nodes from receiving zero centrality simply because of the directed graph structure.

中文：Katz centrality 在 eigenvector centrality 的基础上给每个节点加入一个基础分数。这样可以避免某些节点因为有向图结构而中心性为 0。

## 2.3 Key Formulas

### Degree Centrality

Formula:

```text
C_D(v) = degree(v)
```

中文含义：节点 v 的 degree centrality 就是它的 degree。

### Closeness Centrality

Common formula:

```text
C_C(v) = (N - 1) / sum_{u != v} d(v, u)
```

Meaning:

```text
d(v, u) = shortest path distance between v and u
N = total number of nodes
```

中文含义：节点 v 到其他节点的距离总和越小，closeness centrality 越大。

### Harmonic Centrality

Formula:

```text
C_H(v) = sum_{u != v} 1 / d(v, u)
```

Normalized version:

```text
C_H_norm(v) = C_H(v) / (N - 1)
```

中文含义：把 v 到其他每个节点的距离取倒数，再求和。如果不可达，则该项为 0。

### Katz Centrality

Vector formula:

```text
c = alpha A^T c + beta 1
```

Equivalent form:

```text
c = (I - alpha A^T)^(-1) beta 1
```

中文含义：

```text
A = adjacency matrix
alpha = 衰减参数
beta = 基础分数
1 = 全 1 向量
```

## 2.4 Formula Example: Harmonic Centrality

Example graph:

```text
Edges: A-B, B-C, C-D, B-E
```

We calculate harmonic centrality of node B.

Step 1: Find distances from B.

```text
d(B,A) = 1
d(B,C) = 1
d(B,D) = 2
d(B,E) = 1
```

中文：先写出 B 到其他所有节点的最短路径距离。

Step 2: Take inverse distances.

```text
1/d(B,A) = 1/1 = 1
1/d(B,C) = 1/1 = 1
1/d(B,D) = 1/2 = 0.5
1/d(B,E) = 1/1 = 1
```

Step 3: Sum them.

```text
C_H(B) = 1 + 1 + 0.5 + 1 = 3.5
```

Step 4: If normalized harmonic centrality is required:

```text
C_H_norm(B) = 3.5 / 4 = 0.875
```

中文结论：B 的 harmonic centrality 是 3.5。如果题目要求 normalized harmonic centrality，则结果是 0.875。

## 2.5 Exam Targets

Must know:

- Degree centrality is direct-neighbour count.
- Closeness centrality depends on total shortest-path distance.
- Harmonic centrality uses inverse distance and works better for disconnected graphs.
- Eigenvector centrality values are influenced by important neighbours.
- Katz centrality adds a baseline score.
- Eigenvector centrality has limitations in directed graphs.

中文重点：

- degree centrality 看直接连接数。
- closeness centrality 看距离总和。
- harmonic centrality 看距离倒数和，适合不连通图。
- eigenvector centrality 看是否连接到重要节点。
- Katz centrality 加入基础分数。
- eigenvector centrality 在有向图上有局限。

Prediction for this year:

English: Harmonic centrality and HITS-style iterative scores have appeared repeatedly. Since Week 3 focuses on centrality definitions and calculations, this year may ask students to compute one centrality value from a graph and explain why another measure is more appropriate for disconnected or directed graphs.

中文预测：harmonic centrality 和迭代型 centrality 近年反复出现。由于 Week 3 重点讲中心性定义和计算，今年可能会让你从图中计算某个中心性值，并解释为什么某个指标更适合不连通图或有向图。

---

# Unit 3: Node Measures II

Related lecture: Week 4, Node Measures II

考试定位：这是算法流程题和短答题的核心单元。HITS、PageRank、betweenness 都是高频考点。

## 3.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| PageRank | 网页排名 | 衡量节点或网页的重要性 |
| Random Surfer | 随机浏览者 | PageRank 的直觉模型 |
| Dead End | 死胡同 / 终止节点 | 没有 outgoing edge 的节点 |
| Spider Trap | 蜘蛛陷阱 | rank value 被困住的一组节点 |
| Teleportation | 随机跳转 | 解决 spider trap 的方法 |
| Damping Factor | 阻尼因子 | 控制继续沿边走或随机跳转的概率 |
| HITS | 超链接诱导主题搜索 | 同时计算 hub 和 authority |
| Hub | 枢纽 | 指向很多好 authority 的节点 |
| Authority | 权威 | 被很多好 hub 指向的节点 |
| Normalization | 归一化 | 把向量缩放到统一尺度 |
| Betweenness Centrality | 介数中心性 | 衡量节点是否位于许多最短路径上 |
| Shortest Path Count | 最短路径数量 | betweenness 计算需要 |

## 3.2 Core Concept Explanations

### PageRank

English: PageRank measures node importance by considering both incoming links and the importance of the nodes that provide those links. A page linked by many important pages receives a high PageRank score.

中文：PageRank 通过 incoming links 和这些来源节点本身的重要性来衡量节点重要性。如果一个网页被很多重要网页指向，它会获得较高的 PageRank 分数。

Exam use: You should be able to explain PageRank intuitively and describe the dead-end and spider-trap problems.

中文考试用法：你要能解释 PageRank 的直觉，并说明 dead-end 和 spider-trap 问题。

### Dead End

English: A dead end is a node with no outgoing edges. In PageRank, rank value can flow into this node but cannot flow out, which breaks the random-surfer process.

中文：dead end 是没有 outgoing edge 的节点。在 PageRank 中，rank value 可以流入这个节点，但无法流出，会破坏随机浏览过程。

### Spider Trap

English: A spider trap is a group of nodes that link to each other but do not link outside the group. Rank value can become trapped inside the group.

中文：spider trap 是一组内部互相链接但几乎不链接到外部的节点。rank value 会被困在这组节点里面。

### HITS

English: HITS assigns two scores to each node: an authority score and a hub score. A good authority is pointed to by many good hubs. A good hub points to many good authorities.

中文：HITS 给每个节点两个分数：authority score 和 hub score。好的 authority 被很多好的 hub 指向。好的 hub 指向很多好的 authority。

Exam use: HITS is often tested as an iteration calculation. The most common mistake is reversing the direction of authority and hub updates.

中文考试用法：HITS 经常以迭代计算题出现。最常见错误是把 authority 和 hub 的方向搞反。

### Betweenness Centrality

English: Betweenness centrality measures how often a node lies on shortest paths between other node pairs. A node with high betweenness can act as a bridge or broker in the network.

中文：介数中心性衡量一个节点出现在其他节点对最短路径上的频率。betweenness 高的节点通常是网络中的桥梁或中介。

Exam use: If the graph is small, compute shortest paths pair by pair. If the question shows a forward step from Brandes algorithm, use path counts and dependencies carefully.

中文考试用法：如果图很小，就逐个节点对算最短路径。如果题目给出 Brandes algorithm 的 forward step，则要仔细使用 shortest path counts 和 dependency。

## 3.3 Key Formulas

### PageRank

Common formula:

```text
PR(i) = beta + alpha * sum_{j -> i} PR(j) / outdegree(j)
```

Meaning:

```text
j -> i = node j has an edge to node i
outdegree(j) = number of outgoing links from j
alpha = damping factor
beta = base score or teleportation term
```

中文含义：节点 i 的 PageRank 来自所有指向 i 的节点 j。每个 j 会把自己的 PageRank 按 outdegree 平均分给它指向的节点。

### HITS

Authority update:

```text
a_i = sum_{j -> i} h_j
```

Hub update:

```text
h_i = sum_{i -> j} a_j
```

Normalization:

```text
c_normalized = c / ||c||_2
```

中文含义：

```text
authority 分数来自指向自己的 hub 分数
hub 分数来自自己指向的 authority 分数
归一化通常使用 L2 norm
```

### Betweenness Centrality

Formula:

```text
C_B(v) = sum_{s != v != t} sigma_st(v) / sigma_st
```

Meaning:

```text
sigma_st = number of shortest paths from s to t
sigma_st(v) = number of those shortest paths that pass through v
```

中文含义：看所有节点对 s 和 t 的最短路径中，有多少比例经过节点 v。

## 3.4 Formula Example: HITS

Example directed graph:

```text
1 -> 2
1 -> 3
2 -> 3
```

Initial scores:

```text
a_1 = a_2 = a_3 = 1
h_1 = h_2 = h_3 = 1
```

Step 1: Update authority scores from incoming hub scores.

```text
a_1 = 0, because no node points to 1
a_2 = h_1 = 1
a_3 = h_1 + h_2 = 1 + 1 = 2
```

Step 2: Normalize authority vector.

```text
||a||_2 = sqrt(0^2 + 1^2 + 2^2) = sqrt(5)
a = [0, 1/sqrt(5), 2/sqrt(5)] = [0, 0.45, 0.89]
```

Step 3: Update hub scores from outgoing authority scores.

```text
h_1 = a_2 + a_3 = 1 + 2 = 3
h_2 = a_3 = 2
h_3 = 0
```

Step 4: Normalize hub vector.

```text
||h||_2 = sqrt(3^2 + 2^2 + 0^2) = sqrt(13)
h = [3/sqrt(13), 2/sqrt(13), 0] = [0.83, 0.55, 0]
```

中文提醒：考试中如果题目已经给了前两次 iteration，你必须接着题目给定的数值继续算，不要重新初始化。还要看清楚题目问的是 hub score 还是 authority score。

## 3.5 Formula Example: Betweenness

Example graph:

```text
A - B - C - D
```

Calculate betweenness of B.

Node pairs excluding B:

```text
(A, C): shortest path A-B-C passes through B, contribution = 1
(A, D): shortest path A-B-C-D passes through B, contribution = 1
(C, D): shortest path C-D does not pass through B, contribution = 0
```

Therefore:

```text
C_B(B) = 1 + 1 + 0 = 2
```

中文结论：B 的 betweenness centrality 是 2，因为它位于 A-C 和 A-D 的最短路径上。

## 3.6 Exam Targets

Must know:

- Explain PageRank.
- Explain dead end and spider trap.
- Explain teleportation or damping factor.
- Compute one or two iterations of HITS.
- Distinguish hub and authority.
- Compute simple betweenness centrality.

中文重点：

- 会解释 PageRank。
- 会解释 dead end 和 spider trap。
- 会解释 teleportation 或 damping factor。
- 会手推 HITS 一到两轮。
- 会区分 hub 和 authority。
- 会算简单 betweenness centrality。

Prediction for this year:

English: HITS is one of the safest predicted calculation topics because it appears frequently and matches the lecture emphasis. PageRank conceptual questions are also likely, especially dead end, spider trap, and why teleportation is needed.

中文预测：HITS 是最稳的预测计算题之一，因为它高频出现，而且和 lecture 重点高度一致。PageRank 的概念题也很可能出现，尤其是 dead end、spider trap 以及为什么需要 teleportation。

---

# Unit 4: Network Measures and Models

Related lecture: Week 5, Network Measures and Models

考试定位：这一单元既考计算题，也考短答题。clustering coefficient、average shortest path、power-law、random graph、small-world 都很重要。

## 4.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| Network Property | 网络性质 | 描述整个网络结构 |
| Average Shortest Path Length | 平均最短路径长度 | 衡量网络整体距离 |
| Clustering Coefficient | 聚类系数 | 衡量邻居之间互相连接程度 |
| Average Clustering Coefficient | 平均聚类系数 | 所有节点聚类系数的平均 |
| Giant Component | 巨型连通分量 | 最大的连通部分 |
| Power-law Distribution | 幂律分布 | 真实网络常见的度分布 |
| Scale-free Network | 无标度网络 | 少数节点度很高，多数节点度很低 |
| Random Graph | 随机图 | 用随机过程生成的图 |
| Erdős-Rényi Model | ER 随机图模型 | G(n,p) 模型 |
| Phase Transition | 相变 | 网络从不连通到出现巨型分量的变化 |

## 4.2 Core Concept Explanations

### Network Properties

English: Network properties describe the overall structure of a graph. Instead of asking whether one node is important, they ask what the whole network looks like.

中文：网络性质描述整个图的结构。它不问某一个节点是否重要，而是问整个网络长什么样。

Important properties:

- Degree distribution
- Average shortest path length
- Average clustering coefficient
- Size of giant component

中文重点性质：

- 度分布
- 平均最短路径长度
- 平均聚类系数
- 巨型连通分量大小

### Clustering Coefficient

English: The clustering coefficient of a node measures how connected its neighbours are to each other. If all of a node's neighbours are connected to one another, the clustering coefficient is high.

中文：某个节点的聚类系数衡量它的邻居之间互相连接的程度。如果一个节点的所有邻居之间都互相连接，那么它的聚类系数很高。

Exam use: This is one of the most common calculation questions. The key is to count edges among neighbours, not edges from the node itself.

中文考试用法：这是最常见计算题之一。关键是数邻居之间的边，而不是数该节点自己连出去的边。

### Average Shortest Path Length

English: Average shortest path length is the average distance over all node pairs. It tells us how many steps are needed on average to travel between nodes in the network.

中文：平均最短路径长度是所有节点对最短距离的平均值。它表示在网络中从一个节点到另一个节点平均需要多少步。

### Power-law Distribution

English: A power-law degree distribution means that most nodes have low degree, while a small number of nodes have extremely high degree. This pattern appears in many real-world networks.

中文：幂律度分布表示大多数节点度很低，但少数节点度极高。许多真实世界网络都有这种模式。

Examples:

- Social networks
- Web graphs
- Citation networks
- Product co-purchasing networks

中文例子：

- 社交网络
- 网页图
- 引用网络
- 商品共同购买网络

### Random Graph Model

English: In the Erdős-Rényi random graph model G(n,p), there are n nodes, and each possible edge exists independently with probability p.

中文：在 Erdős-Rényi 随机图模型 G(n,p) 中，有 n 个节点，每一对节点之间是否存在边由概率 p 独立决定。

Limitation:

English: Random graphs usually do not match real social networks well because they tend to have low clustering and do not naturally produce strong power-law degree distributions.

中文：随机图通常不能很好模拟真实社交网络，因为它们的聚类系数较低，也不会自然产生明显的幂律度分布。

## 4.3 Key Formulas

### Clustering Coefficient

Formula:

```text
C_i = 2e_i / (k_i (k_i - 1))
```

Meaning:

```text
k_i = degree of node i
e_i = number of edges among neighbours of node i
```

中文含义：

```text
k_i = 节点 i 的邻居数量
e_i = 节点 i 的邻居之间实际存在的边数
```

### Average Clustering Coefficient

Formula:

```text
C = (1/N) * sum_i C_i
```

中文含义：把所有节点的 clustering coefficient 相加，再除以节点总数。

### Average Shortest Path Length

Formula for undirected connected graph:

```text
L = 2 / (N(N - 1)) * sum_{i < j} d(i, j)
```

中文含义：把所有不同节点对的最短路径距离相加，再除以节点对数量。

### ER Random Graph Expected Degree

Formula:

```text
E[degree] = p(N - 1)
```

中文含义：在 G(n,p) 随机图中，一个节点的期望 degree 是 p(N - 1)。

## 4.4 Formula Example: Clustering Coefficient

Example graph:

```text
Edges: (1,2), (1,3), (2,3), (3,4)
Nodes: 1, 2, 3, 4
```

Calculate clustering coefficient for each node.

Node 1:

```text
Neighbours of 1: 2, 3
k_1 = 2
Among neighbours 2 and 3, edge (2,3) exists
e_1 = 1
C_1 = 2 * 1 / (2 * 1) = 1
```

Node 2:

```text
Neighbours of 2: 1, 3
Edge (1,3) exists
C_2 = 1
```

Node 3:

```text
Neighbours of 3: 1, 2, 4
k_3 = 3
Possible neighbour-neighbour edges: (1,2), (1,4), (2,4)
Actual neighbour-neighbour edge: (1,2)
e_3 = 1
C_3 = 2 * 1 / (3 * 2) = 1/3
```

Node 4:

```text
Neighbours of 4: 3
k_4 = 1
C_4 = 0 by convention
```

Average clustering coefficient:

```text
C = (1 + 1 + 1/3 + 0) / 4 = 7/12 = 0.583
```

中文结论：平均聚类系数是 7/12，约等于 0.583。

## 4.5 Formula Example: Average Shortest Path Length

Use the same graph:

```text
Edges: (1,2), (1,3), (2,3), (3,4)
```

List all node-pair distances:

```text
d(1,2) = 1
d(1,3) = 1
d(1,4) = 2
d(2,3) = 1
d(2,4) = 2
d(3,4) = 1
```

Sum:

```text
1 + 1 + 2 + 1 + 2 + 1 = 8
```

Number of node pairs:

```text
4 * 3 / 2 = 6
```

Average shortest path length:

```text
L = 8 / 6 = 4/3 = 1.333
```

中文结论：这个图的 average shortest path length 是 4/3。

## 4.6 Exam Targets

Must know:

- Compute clustering coefficient for one node.
- Compute average clustering coefficient.
- Compute average shortest path length.
- Explain power-law distribution.
- Explain why real networks are not exactly like random graphs.
- Understand giant component and phase transition at a conceptual level.

中文重点：

- 会算单个节点的 clustering coefficient。
- 会算 average clustering coefficient。
- 会算 average shortest path length。
- 会解释 power-law distribution。
- 会解释为什么真实网络和随机图不同。
- 理解 giant component 和 phase transition 的基本含义。

Prediction for this year:

English: A clustering coefficient calculation is very likely. A short-answer question may ask for power-law distribution or why real social networks differ from ER random graphs. If the lecturer emphasizes MSN or real-world network examples, expect a comparison question between real networks and random graphs.

中文预测：clustering coefficient 计算题非常可能出现。短答题可能问 power-law distribution，或者问为什么真实社交网络不同于 ER random graph。如果老师强调 MSN 或真实网络案例，今年可能考 real network 与 random graph 的比较。

---

# Unit 5: Small-World and Assortativity

Related lecture: Week 6, Assortativity in Social Networks

考试定位：这一单元是短答和计算混合区。small-world、configuration model、Pearson correlation、influence/homophily/confounding 都很容易考。

## 5.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| Small-world Model | 小世界模型 | 解释高聚类和短路径如何共存 |
| Regular Lattice | 规则格子 | small-world model 的初始结构 |
| Rewiring | 重连 | 生成 small-world graph 的关键操作 |
| Rewiring Probability | 重连概率 | 控制随机边数量 |
| Assortativity | 同配性 | 衡量相似节点是否倾向连接 |
| Influence | 影响 | 朋友影响导致相似 |
| Homophily | 同质性 | 相似的人更容易成为朋友 |
| Confounding Factor | 混杂因素 | 外部共同因素导致相似 |
| Categorical Attribute | 类别属性 | 性别、颜色、职业等类别变量 |
| Numerical Attribute | 数值属性 | 年龄、收入、评分等数值变量 |
| Modularity | 模块度 | 衡量同类节点连接是否超过随机期望 |
| Configuration Model | 配置模型 | 根据给定 degree sequence 生成随机图 |
| Pearson Correlation | 皮尔逊相关 | 衡量边两端数值属性相关性 |

## 5.2 Core Concept Explanations

### Small-world Model

English: A small-world graph has high clustering coefficient and short average shortest path length. This means local groups are tightly connected, but the whole network is still easy to navigate through a few long-range shortcuts.

中文：小世界图同时具有较高的聚类系数和较短的平均最短路径长度。这意味着局部小圈子内部连接紧密，但整个网络仍然可以通过少数长距离捷径快速到达。

Generation process:

English: Start from a regular lattice, then randomly rewire some edges with probability beta. A small number of random shortcuts can greatly reduce average path length while preserving clustering.

中文：生成过程是先从 regular lattice 开始，然后以 beta 概率随机重连部分边。少数随机捷径就能显著降低平均路径长度，同时保留较高聚类。

### Assortativity

English: Assortativity describes the tendency of nodes with similar attributes to connect with each other. For example, people with similar age, location, interests, or habits may be more likely to form social ties.

中文：同配性描述具有相似属性的节点更倾向于互相连接。例如，年龄、地点、兴趣、习惯相似的人更容易建立社交关系。

Exam use: When explaining assortativity, always consider influence, homophily, and confounding factors.

中文考试用法：解释 assortativity 时，通常要从 influence、homophily、confounding factor 三个角度回答。

### Influence

English: Influence means that one person's behavior changes because of their friends or neighbours. The similarity is caused after the social connection exists.

中文：influence 指一个人的行为因为朋友或邻居而改变。相似性是在社交连接已经存在之后产生的。

Example:

English: A student starts smoking because many of their friends smoke.

中文：一个学生因为身边很多朋友吸烟，所以自己也开始吸烟。

### Homophily

English: Homophily means that similar people are more likely to become connected in the first place. The similarity exists before the connection is formed.

中文：homophily 指本来相似的人更容易建立连接。相似性在关系形成之前就已经存在。

Example:

English: Two students who already smoke become friends because they share the same habit.

中文：两个本来就吸烟的学生因为习惯相似而成为朋友。

### Confounding Factor

English: A confounding factor is an external factor that affects both people and makes them similar, even if the friendship itself is not the cause.

中文：confounding factor 是影响两个人的外部因素，使他们看起来相似，但这种相似不一定是朋友关系导致的。

Example:

English: Two students smoke because they live in the same environment where smoking is common.

中文：两个学生吸烟是因为他们处在同一个吸烟常见的环境中，而不一定是彼此影响。

### Configuration Model

English: The configuration model generates a random graph from a predetermined degree sequence. It is useful because it keeps node degrees fixed while randomizing the connections.

中文：configuration model 根据预先给定的 degree sequence 生成随机图。它的作用是保持每个节点的 degree 不变，同时随机化连接方式。

Exam use: This is a likely short-answer question. You should be able to describe the algorithm step by step and state the approximate probability that two nodes are connected.

中文考试用法：这是很可能出现的短答题。你要能逐步描述算法，并写出两个节点之间存在边的近似概率。

## 5.3 Key Formulas

### Configuration Model Edge Probability

Formula:

```text
P(edge between v_i and v_j) approximately = d_i d_j / (2m)
```

Meaning:

```text
d_i = degree of node v_i
d_j = degree of node v_j
m = number of edges in the graph
```

中文含义：在 configuration model 中，degree 越高的两个节点越可能被随机连接。

### Pearson Correlation Coefficient

Formula:

```text
rho(X_L, X_R) = covariance(X_L, X_R) / (std(X_L) * std(X_R))
```

Equivalent computational form:

```text
rho = (E[X_L X_R] - E[X_L]E[X_R]) / sqrt((E[X_L^2] - E[X_L]^2)(E[X_R^2] - E[X_R]^2))
```

中文含义：

```text
X_L = 每条边左端点的属性值
X_R = 每条边右端点的属性值
rho > 0 表示正相关
rho < 0 表示负相关
rho 接近 0 表示线性相关弱
```

### Modularity

Common idea:

```text
Q = actual same-type connections - expected same-type connections under a random model
```

中文含义：modularity 衡量同类节点之间实际连接数量是否超过随机模型下的期望连接数量。

## 5.4 Formula Example: Configuration Model

Suppose:

```text
d_i = 3
d_j = 2
m = 8
```

Use the approximate formula:

```text
P(edge between v_i and v_j) = d_i d_j / (2m)
```

Substitute values:

```text
P = 3 * 2 / (2 * 8) = 6 / 16 = 0.375
```

中文结论：在这个 configuration model 中，节点 vi 和 vj 之间出现边的近似概率是 0.375。

## 5.5 Formula Example: Pearson Correlation

Example:

```text
Edges: (1,2), (2,3), (3,4)
Node attributes:
x_1 = 1
x_2 = 2
x_3 = 4
x_4 = 5
```

Step 1: Build X_L and X_R from the edge list.

```text
For edge (1,2): X_L = 1, X_R = 2
For edge (2,3): X_L = 2, X_R = 4
For edge (3,4): X_L = 4, X_R = 5

X_L = [1, 2, 4]
X_R = [2, 4, 5]
```

中文：先根据 edge list 构造左端点变量 X_L 和右端点变量 X_R。这一步最容易错。

Step 2: Compute means.

```text
E[X_L] = (1 + 2 + 4) / 3 = 7/3
E[X_R] = (2 + 4 + 5) / 3 = 11/3
```

Step 3: Compute E[X_L X_R].

```text
X_L X_R = [1*2, 2*4, 4*5] = [2, 8, 20]
E[X_L X_R] = (2 + 8 + 20) / 3 = 10
```

Step 4: Compute covariance.

```text
covariance = E[X_L X_R] - E[X_L]E[X_R]
covariance = 10 - (7/3)(11/3)
covariance = 10 - 77/9 = 13/9
```

Step 5: Compute variances.

```text
E[X_L^2] = (1^2 + 2^2 + 4^2) / 3 = 7
var(X_L) = 7 - (7/3)^2 = 7 - 49/9 = 14/9

E[X_R^2] = (2^2 + 4^2 + 5^2) / 3 = 15
var(X_R) = 15 - (11/3)^2 = 15 - 121/9 = 14/9
```

Step 6: Compute Pearson correlation.

```text
rho = (13/9) / sqrt((14/9)(14/9))
rho = (13/9) / (14/9)
rho = 13/14 = 0.929
```

中文结论：Pearson correlation 约为 0.929，表示边两端属性呈强正相关。

## 5.6 Exam Targets

Must know:

- Explain small-world graph properties.
- Describe small-world graph generation.
- Distinguish influence, homophily, and confounding factor.
- Describe configuration model step by step.
- Use configuration model probability formula.
- Compute Pearson correlation from an edge list.
- Understand modularity conceptually.

中文重点：

- 会解释 small-world graph 的性质。
- 会描述 small-world graph 的生成过程。
- 会区分 influence、homophily、confounding factor。
- 会逐步描述 configuration model。
- 会用 configuration model 的概率公式。
- 会根据 edge list 算 Pearson correlation。
- 理解 modularity 的基本含义。

Prediction for this year:

English: Pearson correlation has appeared repeatedly and is very likely to remain testable. Configuration model is also likely because it connects directly to random graph baselines and assortativity significance. A conceptual question about smoking, friendship, or shared behavior may ask students to explain influence, homophily, and confounding factors.

中文预测：Pearson correlation 连续出现，仍然非常可能考。configuration model 也很可能考，因为它和随机图基准、同配性显著性直接相关。关于吸烟、朋友关系或共同行为的情景题，可能要求从 influence、homophily、confounding factor 三个角度解释。

---

# Unit 6: Network Effects and Information Diffusion

Related lecture: Week 7, Network Effects and Information Diffusion

考试定位：这一单元非常容易出过程题和比较题。LTM、ICM、influence、homophily、herd behavior、information cascade 都要会解释。

## 6.1 Key Vocabulary

| English Term | 中文对应 | Why It Matters |
|---|---|---|
| Influence | 影响 | 一个用户改变另一个用户行为 |
| Influence Measure | 影响力指标 | 衡量谁更有影响力 |
| Spearman Rank Correlation | Spearman 等级相关 | 比较两个排名是否相似 |
| Linear Threshold Model | 线性阈值模型 | 基于累计影响和阈值的扩散模型 |
| Independent Cascade Model | 独立级联模型 | 基于概率尝试激活邻居的扩散模型 |
| Activation | 激活 | 节点采纳某行为或接收信息 |
| Active Node | 激活节点 | 已采纳行为或信息的节点 |
| Inactive Node | 未激活节点 | 尚未采纳行为或信息的节点 |
| Threshold | 阈值 | 节点激活所需的最小影响量 |
| Activation Probability | 激活概率 | ICM 中边传播成功的概率 |
| Herd Behavior | 羊群行为 | 人们根据公开信息跟随群体 |
| Information Cascade | 信息级联 | 信息通过网络邻居逐层传播 |
| Local Information | 局部信息 | 来自邻居的信息 |
| Global Information | 全局信息 | 所有人可观察的信息 |

## 6.2 Core Concept Explanations

### Influence

English: Influence is the process by which one user's behavior, opinion, or state affects another user. In a network, influence usually flows along edges.

中文：influence 指一个用户的行为、观点或状态影响另一个用户的过程。在网络中，影响通常沿着边传播。

Exam use: Influence is often compared with homophily and confounding factors. The key difference is causality after connection.

中文考试用法：influence 经常和 homophily、confounding factor 对比。关键区别是：连接存在之后产生因果影响。

### Linear Threshold Model

English: In the Linear Threshold Model, an inactive node becomes active when the total influence from its active neighbours reaches or exceeds its threshold.

中文：在线性阈值模型中，一个未激活节点会在来自已激活邻居的总影响达到或超过阈值时被激活。

Exam use: LTM questions usually ask which nodes are activated at step 2, step 3, or step 4.

中文考试用法：LTM 题通常问 step 2、step 3 或 step 4 哪些节点会被激活。

### Independent Cascade Model

English: In the Independent Cascade Model, a newly active node gets one chance to activate each inactive neighbour with a given probability. If it fails, it usually cannot try again later.

中文：在独立级联模型中，一个新激活节点通常只有一次机会，以给定概率激活每个未激活邻居。如果失败，之后通常不能再次尝试。

Exam use: ICM questions require careful step-by-step tracing. You must only allow newly activated nodes to attempt activation in the next step.

中文考试用法：ICM 题需要逐步跟踪。必须注意通常只有新激活的节点在下一步尝试激活邻居。

### LTM vs ICM

English: LTM is based on accumulated influence and threshold. ICM is based on independent probabilistic attempts along edges. LTM is like pressure accumulating over time, while ICM is like one-time contagion attempts.

中文：LTM 基于累计影响和阈值。ICM 基于沿边进行的独立概率尝试。LTM 像压力逐渐累积，ICM 像一次性的传染尝试。

### Herd Behavior

English: Herd behavior occurs when individuals follow the actions of others based on public or global information, even if their private information suggests a different choice.

中文：herd behavior 指个体根据公开或全局信息跟随他人行为，即使自己的私人信息可能暗示不同选择。

### Information Cascade

English: Information cascade occurs when information spreads through a network from node to node. It depends on local network structure and neighbour interactions.

中文：information cascade 指信息在网络中从一个节点传播到另一个节点。它依赖局部网络结构和邻居互动。

Difference:

English: Herd behavior is usually based on public/global observation. Information cascade is usually based on local network propagation.

中文：herd behavior 通常基于公开或全局观察。information cascade 通常基于局部网络传播。

## 6.3 Key Formulas and Rules

### Linear Threshold Model Activation Rule

Formula:

```text
Activate v if sum_{u active} w_{u,v} >= theta_v
```

Meaning:

```text
w_{u,v} = influence weight from u to v
theta_v = threshold of node v
```

中文含义：如果来自已激活邻居的影响权重总和大于或等于节点 v 的阈值，节点 v 被激活。

### Independent Cascade Model Rule

Rule:

```text
When node u becomes active at time t, it has one chance at time t+1 to activate each inactive neighbour v with probability p_{u,v}.
```

中文含义：当节点 u 在 t 时刻激活，它在 t+1 时刻有一次机会以概率 p 激活每个未激活邻居。

### Spearman Rank Correlation

Formula:

```text
rho = 1 - (6 * sum d_i^2) / (n(n^2 - 1))
```

Meaning:

```text
d_i = difference between two ranks for item i
n = number of ranked items
```

中文含义：Spearman rank correlation 用来比较两个排名是否一致。越接近 1，排名越相似。

## 6.4 Formula Example: LTM

Example:

```text
Initial active node: A

Thresholds:
theta_B = 0.5
theta_C = 0.4
theta_D = 0.6

Influence weights:
A -> B = 0.5
A -> C = 0.5
B -> D = 0.4
C -> D = 0.3
```

Step 1:

```text
Active: A
Inactive: B, C, D
```

Step 2: Check B and C.

```text
B receives influence from A: 0.5
0.5 >= theta_B = 0.5, so B becomes active

C receives influence from A: 0.5
0.5 >= theta_C = 0.4, so C becomes active
```

After step 2:

```text
Active: A, B, C
Inactive: D
```

Step 3: Check D.

```text
D receives influence from B and C:
0.4 + 0.3 = 0.7

0.7 >= theta_D = 0.6, so D becomes active
```

Final result:

```text
Step 2 activated nodes: B, C
Step 3 activated node: D
```

中文结论：LTM 题要每一步重新检查 inactive nodes，并把所有 active neighbours 的影响加起来。

## 6.5 Formula Example: ICM

Example:

```text
Initial active node: A

Activation probabilities:
A -> B = 0.6
A -> C = 0.3
B -> D = 0.5

Given random values:
A tries B: random value = 0.4
A tries C: random value = 0.7
B tries D: random value = 0.2
```

Rule:

```text
If random value <= activation probability, activation succeeds.
```

Step 1:

```text
Active: A
```

Step 2: A tries to activate B and C.

```text
A -> B: 0.4 <= 0.6, success, B becomes active
A -> C: 0.7 > 0.3, fail, C remains inactive
```

Step 3: Newly active B tries to activate D.

```text
B -> D: 0.2 <= 0.5, success, D becomes active
```

Final result:

```text
Step 2 activated node: B
Step 3 activated node: D
C remains inactive
```

中文结论：ICM 题要注意“新激活节点”才会在下一步尝试激活邻居，并且每条边通常只有一次机会。

## 6.6 Exam Targets

Must know:

- Explain influence.
- Compare influence, homophily, and confounding factor.
- Trace LTM activation step by step.
- Trace ICM activation step by step.
- Compare LTM and ICM.
- Explain herd behavior.
- Explain information cascade.
- Distinguish global public information from local network information.

中文重点：

- 会解释 influence。
- 会比较 influence、homophily、confounding factor。
- 会逐步推 LTM。
- 会逐步推 ICM。
- 会比较 LTM 和 ICM。
- 会解释 herd behavior。
- 会解释 information cascade。
- 会区分全局公开信息和局部网络信息。

Prediction for this year:

English: A diffusion process question is likely because Week 7 gives both LTM and ICM algorithms. A conceptual comparison between LTM and ICM is also highly likely. Since the lecture includes herd behavior and information cascade, this year may include a short-answer question asking students to distinguish them.

中文预测：diffusion 过程题很可能出现，因为 Week 7 同时讲了 LTM 和 ICM。LTM 与 ICM 的比较题也很可能出现。由于 lecture 包含 herd behavior 和 information cascade，今年可能考一道短答题让你区分两者。

---

# Unit 7: Later Topics To Be Updated

Related lectures: Week 8 to Week 13, not fully available yet

中文说明：这一单元目前是预测区。它不是只根据历年考试死记硬背，而是结合 Week 2 到 Week 7 的知识走向、历年后半卷常见主题、以及 social media analytics 课程的典型结构进行发散预测。等你补充后续 lecture 和 quiz 后，这部分需要精确更新。

## 7.1 Predicted Topic: Community Detection

Likely vocabulary:

| English Term | 中文对应 |
|---|---|
| Community Detection | 社区检测 |
| Ratio Cut | 比例切割 |
| Normalized Cut | 归一化切割 |
| Modularity | 模块度 |
| Spectral Clustering | 谱聚类 |
| Laplacian Matrix | 拉普拉斯矩阵 |
| K-means | K 均值聚类 |

English: Community detection aims to divide a graph into groups where nodes inside the same group are densely connected and nodes across different groups are sparsely connected.

中文：社区检测的目标是把图划分成若干组，使组内节点连接密集，组间连接稀疏。

Expected exam direction:

- Calculate ratio cut or normalized cut for a given partition.
- Explain spectral clustering steps.
- Explain why graph partitioning should avoid very unbalanced cuts.

中文预测考法：

- 给定划分，计算 ratio cut 或 normalized cut。
- 解释 spectral clustering 的步骤。
- 解释为什么图划分不能只追求 cut 最小，还要避免极端不平衡划分。

## 7.2 Predicted Topic: Link Prediction

Likely vocabulary:

| English Term | 中文对应 |
|---|---|
| Link Prediction | 链接预测 |
| Common Neighbours | 共同邻居 |
| Jaccard Similarity | Jaccard 相似度 |
| Preferential Attachment | 优先连接 |
| Cosine Similarity | 余弦相似度 |
| Adamic-Adar | Adamic-Adar 指标 |

English: Link prediction estimates whether two nodes are likely to form a link in the future or whether a missing edge should exist.

中文：链接预测用于估计两个节点未来是否可能形成连接，或判断一个缺失边是否应该存在。

Key formulas:

```text
Common Neighbours = |N(u) intersection N(v)|
Jaccard = |N(u) intersection N(v)| / |N(u) union N(v)|
Preferential Attachment = |N(u)| * |N(v)|
Cosine Similarity = |N(u) intersection N(v)| / sqrt(|N(u)| * |N(v)|)
```

Expected exam direction:

- Calculate several link prediction scores for one candidate edge.
- Pay attention to whether the node itself is included in its neighbourhood.
- Compare what each metric prefers.

中文预测考法：

- 给一个候选边，计算多个 link prediction 分数。
- 注意题目是否说明 node 是否 included in neighbourhood。
- 比较不同指标偏好的连接类型。

## 7.3 Predicted Topic: Node Embedding

Likely vocabulary:

| English Term | 中文对应 |
|---|---|
| Node Embedding | 节点嵌入 |
| Graph Representation Learning | 图表示学习 |
| Encoder | 编码器 |
| Decoder | 解码器 |
| Shallow Embedding | 浅层嵌入 |
| Low-dimensional Vector | 低维向量 |
| Similarity Function | 相似度函数 |

English: Node embedding maps each node into a low-dimensional vector while preserving graph structure. The encoder produces node vectors, and the decoder uses those vectors to reconstruct relationships such as similarity or edges.

中文：节点嵌入把每个节点映射成低维向量，同时尽量保留图结构。encoder 生成节点向量，decoder 使用这些向量重建节点关系，例如相似性或边。

Expected exam direction:

- Explain encoder and decoder in graph representation learning.
- Give an example of encoder and decoder.
- Explain limitations of shallow embedding methods.
- Compare graph representation learning with traditional machine learning on graphs.

中文预测考法：

- 解释 graph representation learning 中 encoder 和 decoder 的作用。
- 举 encoder 和 decoder 的例子。
- 解释 shallow embedding 的局限。
- 比较 graph representation learning 与传统图机器学习。

## 7.4 Predicted Topic: Graph Neural Networks

Likely vocabulary:

| English Term | 中文对应 |
|---|---|
| Graph Neural Network | 图神经网络 |
| Message Passing | 消息传递 |
| Aggregation | 聚合 |
| Node Feature | 节点特征 |
| Node Label | 节点标签 |
| Computational Graph | 计算图 |
| Supervised Learning | 监督学习 |
| Unsupervised Learning | 无监督学习 |
| Self-supervised Learning | 自监督学习 |

English: A Graph Neural Network updates a node representation by aggregating information from its neighbours. After multiple layers, a node can capture information from multi-hop neighbourhoods.

中文：图神经网络通过聚合邻居信息来更新节点表示。经过多层后，一个节点可以捕捉多跳邻居的信息。

Expected exam direction:

- Explain message passing.
- Explain how to handle absence of node features.
- Explain how to handle absence of node labels.
- Compare supervised and unsupervised training for GNNs.
- Describe how to construct a computational graph for a node.

中文预测考法：

- 解释 message passing。
- 解释没有 node features 时怎么办。
- 解释没有 node labels 时怎么办。
- 比较 GNN 的 supervised 和 unsupervised training。
- 描述如何为一个节点构造 computational graph。

## 7.5 Predicted Topic: Epidemic Models

Likely vocabulary:

| English Term | 中文对应 |
|---|---|
| Epidemic Model | 流行病模型 |
| SI Model | 易感-感染模型 |
| SIS Model | 易感-感染-易感模型 |
| SIR Model | 易感-感染-康复模型 |
| Infection Rate | 感染率 |
| Recovery Rate | 康复率 |

English: Epidemic models describe how a disease, behavior, or information spreads through a population or network.

中文：流行病模型描述疾病、行为或信息如何在人群或网络中传播。

Expected exam direction:

- List basic epidemic models.
- Explain differences among SI, SIS, and SIR.
- Connect epidemic spread with information diffusion.

中文预测考法：

- 列出基本 epidemic models。
- 解释 SI、SIS、SIR 的区别。
- 把 epidemic spread 和 information diffusion 联系起来。

---

# Cross-Unit Formula Sheet

## Graph Basics

```text
P(k) = N_k / N
```

Use for degree distribution.

中文：用于计算 degree distribution。

```text
sum of degrees = 2m
```

Use for undirected graphs.

中文：用于无向图，所有节点 degree 之和等于 2 倍边数。

## Centrality

```text
C_D(v) = degree(v)
```

Use for degree centrality.

中文：用于 degree centrality。

```text
C_C(v) = (N - 1) / sum_{u != v} d(v,u)
```

Use for closeness centrality.

中文：用于 closeness centrality。

```text
C_H(v) = sum_{u != v} 1 / d(v,u)
```

Use for harmonic centrality.

中文：用于 harmonic centrality。

```text
a_i = sum_{j -> i} h_j
h_i = sum_{i -> j} a_j
```

Use for HITS.

中文：用于 HITS，authority 从 incoming hub 来，hub 从 outgoing authority 来。

```text
C_B(v) = sum sigma_st(v) / sigma_st
```

Use for betweenness centrality.

中文：用于 betweenness centrality。

## Network Properties

```text
C_i = 2e_i / (k_i(k_i - 1))
```

Use for clustering coefficient.

中文：用于 clustering coefficient。

```text
L = 2 / (N(N - 1)) * sum_{i < j} d(i,j)
```

Use for average shortest path length.

中文：用于 average shortest path length。

## Assortativity and Diffusion

```text
P(edge between v_i and v_j) approximately = d_i d_j / (2m)
```

Use for configuration model.

中文：用于 configuration model 中两个节点连接概率的近似。

```text
rho = covariance(X_L, X_R) / (std(X_L) * std(X_R))
```

Use for Pearson correlation.

中文：用于 Pearson correlation。

```text
Activate v if sum active influence >= theta_v
```

Use for Linear Threshold Model.

中文：用于 LTM。

```text
New active node gets one chance to activate each inactive neighbour with probability p
```

Use for Independent Cascade Model.

中文：用于 ICM。

## Link Prediction

```text
Common Neighbours = |N(u) intersection N(v)|
```

```text
Jaccard = |N(u) intersection N(v)| / |N(u) union N(v)|
```

```text
Preferential Attachment = |N(u)| * |N(v)|
```

```text
Cosine Similarity = |N(u) intersection N(v)| / sqrt(|N(u)| * |N(v)|)
```

中文：这些公式用于 link prediction。考试时一定看清楚 node 是否 included in neighbourhood。

---

# High-Frequency Exam Checklist

## Calculation Questions

Very likely:

- Degree distribution
- Harmonic centrality
- HITS iteration
- Clustering coefficient
- Diameter or average shortest path length
- Pearson correlation from edge list
- LTM activation process
- ICM activation process
- Jaccard similarity
- Common neighbours, preferential attachment, cosine similarity

中文：这些是最应该优先练熟的计算题。目标是看到图后能稳定手算，而不是只会背公式。

## Short-Answer Questions

Very likely:

- Limitations of eigenvector centrality in directed graphs
- Dead end and spider trap in PageRank
- Small-world graph properties and generation
- Configuration model steps
- Influence vs homophily vs confounding factor
- LTM vs ICM
- Spectral clustering steps
- Encoder and decoder in node embedding
- GNN message passing
- What to do when node features or node labels are absent

中文：这些是后半卷常见短答题。建议每个主题准备 4 到 6 句英文模板。

## This Year's Broader Prediction

English: Based on the current lecture progression, this year's exam is likely to keep the traditional balance: early questions will test graph calculations, while later questions will test model explanation and method comparison. Because the course is still only at Week 7, later topics may shift the final emphasis. However, it is very likely that the exam will combine old stable topics with newer lecture-specific topics.

中文：根据目前 lecture 进度，今年考试很可能仍然保持传统结构：前半部分考图计算，后半部分考模型解释和方法比较。因为目前只讲到 Week 7，后续主题仍可能改变最终重点。不过很可能会把历年稳定高频考点和今年 lecture 特别强调的新内容结合起来考。

Predicted stable topics:

- Graph basics and representation
- Centrality calculations
- HITS and PageRank
- Clustering coefficient
- Small-world model
- Assortativity and Pearson correlation
- LTM and ICM

Predicted later-topic topics:

- Community detection and spectral clustering
- Link prediction metrics
- Node embedding encoder-decoder framework
- GNN message passing and training settings
- Epidemic models if covered in later lectures

中文总结：目前最稳的方向是 graph basics、centrality、HITS/PageRank、clustering、small-world、assortativity、diffusion。后续如果老师讲 community detection、link prediction、embedding、GNN、epidemics，这些很可能进入后半卷短答题。

---

# Update Plan For Future Materials

When new lecture slides are added:

1. Add a new unit or update Unit 7.
2. Extract key vocabulary.
3. Add bilingual concept explanations.
4. Add formulas and worked examples.
5. Map the topic to past exam questions.
6. Add prediction for this year's exam.

中文后续更新方式：

1. 新增 lecture 后，先补一个新的单元或更新 Unit 7。
2. 提取重点词汇。
3. 加入中英文概念解释。
4. 加入公式和例题演示。
5. 映射到历年考试题。
6. 更新今年考试预测。

## Version Notes

Current version:

- Covers Week 2 to Week 7 lecture content.
- Uses past exams from 2021 to 2025 as reference for exam style.
- Includes broader prediction beyond past exams.
- Leaves Week 8 to Week 13 open for future updates.

中文版本说明：

- 当前版本覆盖 Week 2 到 Week 7。
- 参考了 2021 到 2025 历年考试的题型。
- 不只根据历年题，还加入了基于 lecture 走向的发散预测。
- Week 8 到 Week 13 会在你提供新材料后继续补充。
