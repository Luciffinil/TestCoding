# 1 图算法

图 G = (V, E)   |V| 顶点数   |E| 边数  

1.1 图的表示  
两种标准方法(既可用于有向图,又可用于无向图): 邻接表 和 邻接矩阵  
稀疏图(E 远小于 V^2)常用 邻接表. 稠密图(E 接近 V^2) 或需求很快判断两个给定顶点是否存在连接边时, 常用 邻接矩阵  
  
邻接表: 含 |V| 个列表的数组 Adj. 每个列表包含该顶点能到达的所有顶点.(Adj[u]包含所有满足 (u,v)属于E 的顶点v)  
       对于加权图, 将权 w(u,v) 和顶点v 一起存储在 u 的邻接表中   
       占用 O(V+E) 的空间  
邻接矩阵: |V| * |V| 的矩阵, 若 (u,v)存在于 E,中 则该点值为 1
         对于加权图, 直接将点的值置为权值  
         占用 O(V^2) 的空间  
         
1.2 广度优先搜索  
首先发现和源顶点 s 距离为 k 的所有顶点, 再发现距离为 k+1 的其他顶点  
所有顶点都着色为白色,灰色,黑色. 开始时都是白色,被发现后转为灰色和黑色. 灰色和黑色是为了保证以广度优先进行搜索   
只能有一个源顶点(一般使用情况)
  
对于每个顶点 u, color[u] 存储色彩, p[u] 存储父顶点(没有父时为NIL), d[u] 表示s和u的距离. 采用FIFO队列 Q 来管理所有灰色顶点  
```
BFS(G, s)
for each vertex u∈V[G] - {s}                  // 除 s 外的所有顶点进行初始化
    color[u] <- WHITE
    d[u] <- ∞
    p[u] <- NIL
color[s] <- GRAY                              // s 初始化 
d[s] <- 0
p[s] <- NIL
Q <- ∅
ENQUEUE(Q, s)
while Q != ∅
    u <- DEQUEUE(Q)
    for eacg v∈Adj[u]
        if(color[v] = WHITE
            color[v] <- GRAY
            d[v] <- d[u] + 1
            p[v] <- u
            ENQUEUE(Q, v)
    color[u] <- BLACK            
```

以下过程将输出从 s 到 v 的最短路径上的所有顶点
```
PRINT-PATH(G,s,v)
if v=s
    print s
else
    if p[v] = NIL
        print "no path from" s "to" v "exists"
    else 
        PRINT-PATH(G,s,p[v])
        print v
```

广度优先树  
实质上是一颗连通树,可以更为形式化地定义其前趋子图.  

1.3 深度优先搜索   
开始时,所有顶点都是白色,搜索中被发现时置为灰色,结束时被置为黑色(当邻接表被完全检索之后)   
可以从多个源顶点开始搜索(一般情况下), 因此其先辈子图可以由几棵树组成.但这些树不相交,创建出一个深度优先森林.     
深度优先搜索同时为每个顶点加盖时间戳.顶点 v 第一次被发现(并置为灰色),记录下第一个时间戳 d[v], 当结束检查 v 的邻接表(并置为黑色),记录下第二个时间戳f[v].这些时间戳为 1 到 2|V| 之间的整数  
运行时间为 O(V+E)  

```
DFS(G)
for each vertex u∈V[G]
    color[u] <- WHITE
    p[u] <- NIL
time <- 0
for each vertex u∈V[G]
    if color[u] = WHITE
        DFS-VISIT(u)
        
DFS-VISIT(u)
color[u] <- GRAY
time <- time + 1
d[u] <- time
for each v∈Adj[u]
    if color[v] = WHITE
    p[v] <- u
    DFS-VISIT(v)
color[u] <- BLACK
f[u] <- time++
```

深度优先搜索性质:  
- 括号结构: 发现顶点u,用"(u"表示, 完成用"u)"表示,在各级括号正确嵌套的前提下,构成一个完善的表达式.  
  括号定理: 对任意两个顶点 u,v, 以下三个条件仅一个成立:  
    区间[d[u],f[u]] 和 [d[v],f[v]]完全不想交,则 u 和 v都不是对方后裔  
    区间[d[u],f[u]] 完全包含于 [d[v],f[v]], u 是 v 的后裔  
    区间[d[v],f[v]] 完全包含于 [d[u],f[u]], v 是 u 的后裔
- 后裔区间嵌套: v 是 u 的后裔,当且仅当 d[u] < d[v] < f[v] < f[u]
- 白色路径定理: v 是 u 的后裔,当且仅当在搜索过程中于时刻 d[u] 发现 u 时,可以从顶点 u 出发,经过一条完全由白色顶点组成的路径到达 v.  

深度优先搜索边的分类:  
- 树边: 如果 v 是在探寻边 (u,v) 时首次被发现,则(u,v)为树边(即深度优先树的边)
- 反向边: 连接顶点 u 到其某一祖先 v 的边.(有向图中的自环也是反向边 j)
- 正向边: u 到其某后裔 v 的非树边 (u,v)
- 交叉边: 存在于同一颗深度优先树的两个顶点间,且此两个顶点是兄弟关系;或不同的深度优先树顶点之间

据此可以对 DFS 做一些修改,遇到图中的边时进行分类.  
核心思想是每条边 (u,v), 第一次被探寻到时,根据 v 的颜色,对边进行分类:
- 白色WHITE 的 v 表明是树边
- 灰色GRAY 表明是反向边
- 黑色BLACK 表明是正向边或交叉边. d[u] < d[v] 为正向边, d[u] > d[v] 为交叉边  

对无向图 G 进行深度优先搜索的过程中, G 的每一条边要么是树边,要么是反向边.

1.4 拓扑排序  
对一个有向无回路图(有时称为 dag),运用深度优先搜索,进行拓扑排序,得到所有顶点的一个线性排序(即所有顶点沿水平线排列,所有有向边均从左指向右).   
```
TOPOLOGICAL-SORT(G)
call DFS(G) to compute finishing times f[v] for each vertex v
as each vertex is finished, insert it onto the front of a linked list(链表)
return the linked list of vertices
```
拓扑排序的顶点以与其完成时刻相反的顺序出现

1.5 强连通分支  
强连通分支即一个最大顶点集合 C 属于 U, 对于 C 中每一对顶点 u,v 都是互相可达的.(实质上所有顶点构成一个回路)     
G的转置为所有有向边方向进行反转. G 和 G的转置 有着完全相同的强连通分支.  
把 G 的每个强连通子图缩减为每个分支只有单个顶点可以得到一个无回路子图
```
STRONGLY-CONNECTED-COMPONENTS(G)
call DFS(G) to compute finishing times f[u] for each vertex u
compute GT(G的转置)
call DFS(GT). but in the main loop of DFS, consider the vertices in order of decreasing f[u](as computed in first line)
output the vertices of each tree in the depth-first forest formed in line 3 as a separate strongly connected component
```





