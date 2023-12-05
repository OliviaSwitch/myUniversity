[数据结构：图(Graph)](https://blog.csdn.net/Real_Fool_/article/details/114141377)
# Définitions

## Graphe non orienté 
(anglais: undirected graph; 无向图) 

$$
G = (V, E)
$$

- V: ensemble fini des sommets(有限顶点集)；有顶点 $v,w\in V$
- E: ensemble des arêtes (anglais: edges), relation symétrique(对称关系)；那么有顶点对 $(v,w)\in E$

## Graphe orienté 
(anglais: directed graph; 有向图) 

$$
G=(V,E)
$$

- item (同上)
- $E\subseteq V\times V$ ensemble des arcs(edges)；有向边 $<v,w>$表示从v指向w的边

## Terminologie
术语

- 完全图(**Graphe complet**)：
	- 有边的数量 $e=(n-1)n$的有向图 $G = (V, E)$ 被称为完全图。
	- 有边的数量为$e=(n-1)n/2$的无向图。
- G的逆图(**graphe réciproque**)是图G的逆向边图，表示为$G^{-1}=(V, E^{-1})$，其中$E^{-1}=\{(x, y) \in V×V | (y, x) \in E\}$（有向图）
- G相关的对称图(**graphe symétrique**)是$G_{S}=(V, E\cup E^{-1})$（有向图）
- **赋权图**：有权/加权图(**graphe valué/pondéré**)是指带有权重函数w（weight）$w：E\rightarrow R$的有向或无向图$G=(V, E)$，这样的图用$(V, E, w)$表示；也称网network
- 给定图$G = (V, E)$，图$G^{\prime} = (V, E^{\prime})$是图G的一个部分图(**graphe partiel**)，如果E′包含于E中。换句话说，通过从图G中删除一个或多个边，就可以得到部分图$G^{\prime}$。
- 子图(**sous-graphe**)对于顶点的子集合$A \subseteq V$，由A诱导的图$G = (A, E(A))$，其中边集$E(A)$由所有在$A$中的两个端点之间的$G$的边组成。换句话说，通过从$G$中删除一个或多个顶点以及所有与这些顶点相邻的边，我们得到图$G^{\prime}$。
- 一个子图的部分图是$G$的**sous-graphe partie**。
- 一个图的完全子图被称为团(**clique**)。顶点集C被称为无向图 $G=(V,E$) 的团，如果C是顶点集V的子集($C\subseteq V$)，而且任意两个C中的顶点都有边连接。另一种等价的说法是，由C诱导的子图是完全图 （有时也用“团”来指这样的子图）
- 一个没有边的子图被称为稳定(**stable**)子图。（外边？）

- 如果 $(x, y) \in E$，则y是x的一个后继(**successeur**) $x\rightarrow y$
- 如果 $(y, x) \in E$，则y是x的一个前驱(**prédécesseur**)$x\leftarrow y$
- 如果 y是x的前驱和/或后继，则x和y是相邻(**adjacents**)的。
- x的后继集合用$\Gamma^+(x)$表示，$\Gamma^+(x) = \{y | (x, y) \in E\}$。
- x的前驱集合用$\Gamma^-(x)$表示，$\Gamma^-(x) = \{y | (y, x) \in E\}$。
- x的邻居集合用$\Gamma^(x)$表示，$\Gamma^(x) = \Gamma^+(x) \cup \Gamma^-(x)$。
- x的入度(**degré intérieur**)用$d^-(x)$表示，$d^-(x) = |\Gamma^-(x)|$。
- x的出度(**degré extérieur**)用$d^+(x)$表示，$d^+(x) = |\Gamma^+(x)|$。
- x的度(**degré**)用$d(x)$表示，$d(x) = d^-(x) + d^+(x)$。

note: 
- $|V|=n,|E|=m$
- 带有环的完整图形：$m=n^{2}$
- 没有环的完整图形：$m=n(n-1)$
- 对于没有环的有向图：$\sum\limits_{x\in V}d^+(x)=\sum\limits_{x\in V}d^-(x)=\frac{1}{2}\sum\limits_{x\in V}d(x)=m$
- 对于无向图：$\sum\limits_{x\in V}d(x)=2m$

### Quelque types de graphes non-orientés
某些类型的无向图

- 平面图(**planaire**)是指可以将其节点和边画在平面上，而没有两边相交的图。
- 连通图(**connexe**)是指每对节点都连通的无向图。不连通的图形被分解为相关的组件(**composantes**)。
- **二分图**（biparti; anglais：bipartite graph）是指这样的简单图：其点集可以被划分称两个集合$Y$和$X$，其中$Y$中的任意两个节点之间没有边相连，$X$中的任意两个节点之间也没有边相连
