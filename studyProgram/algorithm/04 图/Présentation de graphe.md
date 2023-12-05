# Matrice d’adjacence
邻接矩阵

![一个无向图和它的邻接矩阵](https://img-blog.csdnimg.cn/202103011006555.png)

注意：对于无向图，邻接矩阵是对角线(le diagonal)对称的

![有向图和它的邻接矩阵](https://img-blog.csdnimg.cn/20210301101417729.png)

![有向网图和它的邻接矩阵](https://img-blog.csdnimg.cn/20210301110502994.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1JlYWxfRm9vbF8=,size_16,color_FFFFFF,t_70#pic_center)

注意：0表示该状态没有指向自己的，$\infty$表示没有边从i状态指向j状态

## Codes de Matrice

通过以上对无向图、有向图和网的描述，可定义出邻接矩阵的存储结构：
```c
#define MaxVertexNum 100	//顶点数目的最大值
typedef char VertexType;	//顶点的数据类型
typedef int EdgeType;	//带权图中边上权值的数据类型
typedef struct{
	VertexType Vex[MaxVertexNum];	//顶点表
	EdgeType Edge[MaxVertexNum][MaxVertexNum];	//邻接矩阵，边表
	int vexnum, arcnum;	//图的当前顶点数和边树
}MGraph;

// from france silde
#define N_MAX
typedef struct{
int g[N_MAX][N_MAX];
int n;
}grapheM
```

## Propriété

假设R是集合S上的关系，那么我们说R是：
- 自反的，si $\forall x\in S\quad xRx$
- 对称的，si $\forall x,y\in S,\quad xRy\ yRx$
- 反对称的，si $\forall x,y\in S,\quad xRy\ et \ yRx, y=x$
- 传递的，si $\forall x,y,z\in S,\quad xRy\ et\ yRx, alors\ xRz$

假设R是集合S上的关系，其邻接矩阵为M，那么R具有以下性质：
- 自反，如果$M = M + Id_n$（其中Idn是大小为n的单位矩阵）。
- 对称，如果$M =\ ^tM$（其中$^tM$是$M$的转置）。
- 反对称，如果$M ×\ ^tM + Id_n = Id_n$（×表示矩阵的逐元素乘积）。
- 传递，如果$M^2 + M = M$（其中$M^2$是$M$的矩阵乘积）。

## Avantages et Inconvénients

### Avantages 

- 直接访问边：测试边的存在、添加或删除边都可以在常数时间内完成 $O(1)$
- 算法编写通常更简化：访问顶点的后继很容易。 
- 对矩阵上的操作进行简单的并行计算。
### Inconvénients 

- 内存占用最大：对于图中的边的数量，所需的内存空间为$O(N_MAX^2)$；
  令人困扰的案例是 $m ≤ n^2$ 
- 初始化成本：$O(n^2)$ 
- 查找顶点的后继的成本：$O(n)$，即使没有后继（访问顶点的后继很容易）
# Listes d’adjacence
邻接表

当一个图为稀疏图时（边数相对顶点较少），使用邻接矩阵法显然要浪费大量的存储空间

## Avantages et Inconvénients 

![无向图的邻接表](https://img-blog.csdnimg.cn/20210301165232511.png)

![有向图的邻接表](https://img-blog.csdnimg.cn/20210301165330914.png)

> 对于带权值的网图，可以在边表结点定义中再增加一个weight的数据域，存储权值信息即可 

## Codes de Listes

```c
#define MAXVEX 100	//图中顶点数目的最大值
type char VertexType;	//顶点类型应由用户定义
typedef int EdgeType;	//边上的权值类型应由用户定义
/*边表结点*/
typedef struct EdgeNode{
	int adjvex;	//该弧所指向的顶点的下标或者位置
	EdgeType weight;	//权值，对于非网图可以不需要
	struct EdgeNode *next;	//指向下一个邻接点
}EdgeNode;

/*顶点表结点*/
typedef struct VertexNode{
	Vertex data;	//顶点域，存储顶点信息
	EdgeNode *firstedge	//边表头指针
}VertexNode, AdjList[MAXVEX];

/*邻接表*/
typedef struct{
	AdjList adjList;
	int numVertexes, numEdges;	//图中当前顶点数和边数
}


// from france silde 
typedef struct cel{ int st;
	struct cel* suiv;
}cellule;

typedef cellule* Liste;
typedef struct{
	Liste a[N_MAX];
	int n;
}grapheL;
```

### Avantages 

- 内存占用最小：$O(N\_MAX+m)$
- 访问顶点x的后继的成本为$O(d^+(x))$（对于有向图）

### Inconvénients 

- 访问弧的起点x的成本为$O(d^+(x))$（对于有向图）
- 访问顶点x的前驱需要遍历所有邻接链表：$O(n+m)$

# Problème de coloration
着色问题

[图着色问题（超详细！！！）](https://blog.csdn.net/qq_17550379/article/details/102563756)

> 给定无向连通图G和m种不同的颜色。用这些颜色为图G的各顶点着色，每个顶点着一种颜色，是否有一种着色法使G中任意相邻的2个顶点着不同颜色?

最小颜色数(**nombre minimal**)称为 $\gamma(G)$

## Algorithme glouton
贪婪算法

1. 初始化当前颜色为1
2. 对于图中的每个顶点x： 
	1. 获取顶点x的邻居列表V 
	2. 找到邻居中尚未使用的最小颜色 
	3. 如果这个颜色小于或等于当前颜色，则将顶点x着色为该颜色 
	4. 否则，增加当前颜色并将顶点x着色。

```pseudo
function G = Coloriage(G)
	couleur courante = 1
	pour tour x sommet de G faire
		V = list des voisins de x
		couleur = plus petite couleur non encore utilisée dans V
		si couleur <= couleur courante alors colorier x avec cette couleur 
			sinon incrémenter la couleur courante et colorier x avec 
		fin
	fin faire
```

```c
#include <stdio.h>
#include <stdlib.h>

// 结构定义：图的邻接矩阵表示
typedef struct {
    int** matrix;
    int numVertices;
} Graph;

// 函数定义：图着色算法
void Coloriage(Graph* G) {
    int* colors = (int*)malloc(G->numVertices * sizeof(int));
    int currentColor = 1;

    for (int x = 0; x < G->numVertices; x++) {
        // 获取顶点x的邻居列表V
        // 省略：实现获取邻居列表的函数

        // 找到邻居中尚未使用的最小颜色
        int smallestColor = 1;
        // 省略：实现找到最小颜色的函数

        // 如果颜色小于或等于当前颜色，则将顶点x着色为该颜色
        if (smallestColor <= currentColor) {
            colors[x] = smallestColor;
        } else {
            // 否则，增加当前颜色并将顶点x着色
            currentColor++;
            colors[x] = currentColor;
        }
    }

    // 着色完成后，可以将颜色信息用于其他用途
    // 省略：输出着色结果或其他处理
}

int main() {
    // 省略：创建并初始化图的代码

    // 调用着色算法
    Coloriage(&G);

    // 省略：输出或其他处理
    return 0;
}

```

## Algorithme Welsh-Powell

> This is an iterative **_greedy_** approach.

1. 按顶点的度数降序列出顶点至集合$L$
2. 用颜色 1 为第一个顶点着色，并将相邻顶点放入集合$V_{1}$
3. 向后遍历，将不在集合$V_{1}$的顶点染上颜色 1；并将该顶点的相邻顶点放入集合$V_{1}$
4. 对$V_{1}$重复步骤 2 

```c
#include <cstdio>
#include <cstring>
#include <algorithm>
#define MAX 25
using namespace std;

bool edge[MAX][MAX];

struct Vertex {
    int color, degree;
} vertex[MAX];

int n, m; // n个点，m条边

void calc_degree() { // 计算每个点的度数
    for (int i = 1; i <= n; i++) {
        vertex[i].color = vertex[i].degree = 0;
        for (int j = 1; j <= n; j++) {
            vertex[i].degree += edge[i][j];
        }
    }
}

bool cmp(Vertex a, Vertex b) {
    return a.degree > b.degree;
}

int main() {
    while (scanf("%d%d", &n, &m) != EOF) { // 输入图的点数和边数
        memset(edge, 0, sizeof(edge)); // 初始化邻接矩阵
        for (int i = 1, u, v; i <= m; i++) {
            scanf("%d%d", &u, &v);
            edge[u][v] = edge[v][u] = 1;
        }
        calc_degree();
        sort(vertex + 1, vertex + n + 1, cmp);
        int colored_vertices_cnt = 0;
        int ncolor = 0;
        while (colored_vertices_cnt < n) {
            ncolor++;
            for (int i = 1; i <= n; i++) {
                if (vertex[i].color) continue; // 如果已经着色就跳过

                bool ok = true;
                for (int j = 1; j <= n; j++) {
                    if (!edge[i][j]) continue;
                    if (vertex[j].color == ncolor) {
                        ok = false;
                        break;
                    }
                }
                if (!ok) continue; // 如果邻接的点有相同颜色就跳过

                vertex[i].color = ncolor;
                colored_vertices_cnt++;
                printf("vertex[%d]=%d\n", i, vertex[i].color);
            }
        }
        printf("%d\n", ncolor);
    }
}
```