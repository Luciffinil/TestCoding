# 1-1 栈
后进先出（LIFO） last-in, first-out  
以数组 S[1..n] 来实现一个至多 n 个元素的栈。 top[S] 指向最近插入的元素。 S[1]表示栈底元素，S[top[S]] 是栈顶元素。  
栈下溢：空栈弹出       栈上溢：top[S] 超过 n  
```
PUSH（S， x）
  if top[S]=n                     // 栈满
    then error "overflow"
  else top[S] <- top[S]+1
       S[top[S]] <- x
  
POP（S）
  if top[S]=0                     // 栈空
    then error "underflow"
  else top[S] <- top[S]-1
       return S[top[S]+1]
```

# 1-2 队列  
先进先出（FIFO）  
以数组 Q[1..n] 来实现一个至多 n 个元素的队列。 队列的头 head[Q]，新元素插入的地方为 tail[Q]。  
队列中各元素位置为 head[Q], head[Q]+1, ... , tail[Q]-1, 最后一个位置进行 “卷绕”，即队列中 n 个元素排成环形，位置1接在位置n之后。  
head[Q]=tail[Q]，队列为空，此时删除一个元素，会导致队列下溢。  
head[Q]=tail[Q]+1，队列满，此时插入一个元素，会导致队列上溢。  
  
PS: 卷绕成环，作用是有效利用空间，不需要删除元素后移动整个队列，以环来进行插入操作。  
``` 未检查溢出
ENQUEUE(Q, x)
  Q[tail[Q]] <- x
  if tail[Q] = length[Q]
    then tail[Q] <- 1
    else tail[Q] <- tail[Q]+1
    
DEQUEUE(Q)
  x <- Q[head[Q]]
  if head[Q]=length[Q]
    then head[Q] <- 1
    else head[Q] <- head[Q]+1
  return x    
```

# 1-3 链表
双链表L，每个元素都是一个对象，每个对象包含一个关键字域(key)和两个指针域：next 和 prev。  
对某个元素x, 若prev[x]=NIL,表示x没有前驱结点,为链表第一个元素,即头(head). 若next[x]=NIL,表示x没有后继结点,为链表最后一个元素,即尾(tail).  
head[L]指向表的第一个元素. 若head[L]=NIL,则链表为空.  
```
LIST-SEARCH(L, k)
x <- head[L]
while x != NIL  and key[x] != k
  do x <- next[x]
return x

LIST-INSERT(L, x)
next[x] <- head[L]
if head[L] != NIL
  then prev[head[L]] <- x
head[L] <- x
prev[x] <- NIL

LIST-DELETE(L, x)
if prev[x] != NIL
  then next[prev[x]] <- next[x]
else head[L] <- next[x]
if next[x] != NIL
  then prev[next[x]] <- prev[x]
```

增加哨兵元素 nil[L], 使一个一般的双向链表变成一个带哨兵的环形双向链表, nil[L] 介于头尾之间.   
哨兵(sentinel)是个哑(dummy)对象,可以简化边界条件. 对于 NIL 的引用,可以引用 nil[L] 来代替.   
next[nil[L]]指向链表的头, prev[nil[L]]指向表尾.同时,表尾的 next 和表头的 prev 指向 nil[L].  
空链表仅包含哨兵元素, next[nil[L]], prev[nil[L]]都可被置为 nil[L].  
```
LIST-SEARCH(L, k)
x <- next[nil[L]]
while x != NIL  and key[x] != k
  do x <- next[x]
return x

LIST-INSERT(L, x)
next[x] <- next[nil[L]]
prev[next[nil[L]]] <- x
next[nil[L]] <- x
prev[x] <- nil[L]

LIST-DELETE(L, x)
next[prev[x]] <- next[x]
prev[next[x]] <- prev[x]
```

# 1-4 指针和对象的实现
使用数组和数组下标来构造对象及指针,用于在没有显式指针数据类型时实现链表数据结构.   

  对象的多重数组表示  
三个数组next, key, prev 表示一个链表. 变量 L 中存放表头元素的下标. NIL 用不指向数组中任意位置的整数表示(如: 0 或 -1).   
  
  对象的单数组表示  
单个数组 A 存放链表. 一个对象占用一个连续的子数组 A[j..k]. 对象的每一个域对应着 0 到 k-j 之间的一个偏移量,而对象的指针是下标j. key,next,prev对应的偏移量为0,1,2.(例:对于给定的指针i,为了读 prev[i], 读 A[i+2] 即可).  

  分配和释放对象  
假设多重数组长度为 m,某一时刻,动态数组包含n<=m个元素,还有m-n个元素是自由的.  
把自由对象安排成一个单链表,称为 自由表. 自由表仅用到 next 数组,存放表中的next指针. 自由表表头被保存在全局变量 free 中. 当链表 L 表示的动态集合非空时,自由表将与 表 L 交错在一起, 每个对象或在表L中,或在自由表中,但不能同时存在于两个表中.  
自由表是一个栈,下一个分配的对象是最近被释放的那个.   
一般来说,一个自由表可以被几个链表公用.(这几个链表存储在相同的空间)  
```
ALLOCATE-OBJECT()
if free = NIL
  then error "out of space"
else x <- free
     free <- next[x]
     return x     


FREE-OBJECT(x)
next[x] <- free
free <- x
```

# 1-5 有根树的表示

  二叉树  
用域 p, left, right 来存放指向父亲,左儿子,右儿子的指针.  

  分支数无限制的有根树  
用 p[x] 指向父亲, left-child[x] 指向结点 x 的最左孩子, right-sibling[x] 指向结点 x 紧右边的兄弟.  


# 2-1 直接寻址表
实际存储的关键字数比可能的关键字总数较小时,这时采用散列表就会较直接数组寻址更为有效.  
某一个动态集合,每个元素都有一个取自全域 U={0,1,..,m-1} 的关键字.(假设没有两个元素具有相同的关键字)  
用一个数组(直接寻址表)T[0..m-1],每个位置对应全域U的一个关键字.  
```
DIRECT-ADDRESS-SEARCH(T,k)
  return T[k]
  
DIRECT-ADDRESS-INSERT(T,k)
  T[key[x]] <- x
  
DIRECT-ADDRESS-DELETE(T,k)  
  T[key[x]] <- NIL
```

# 2-2 散列表
利用散列函数h,根据关键字k计算出槽的位置.函数h将关键字域U映射到散列表T[0..m-1]的槽位上:   
  h : U -> {0,1,..,m-1}  
采用散列函数的目的就在于缩小需求处理的下标范围.  
两个关键字可能映射到同一个槽上,造成碰撞(collision). 因此选择h时主导思想为使h尽可能"随机",使碰撞最小化.   
  
  2-2-1 散列函数  
- 简单一致散列(simple uniform hashing): 任何元素散列到m个槽中的每一个的可能性是相同的,且与其他元素已被散列到什么位置上是独立无关的.   
- 对于关键字不是自然数的情况,应该使用某种方法将它们解释为自然数. 例如:字符串pt, ASCII中 p=112, t=116, 如果按 128 作为基数的话, pt就是 (112*128+116=14452).   
  
  除法散列法  h(k) = k mod m   
m 为散列表 T 中槽的总数. 需要注意m的选择.   
首先,m 不应该是2的幂. 因为若 m=2^p, 则 h(k)就是 k 的 p 个最低位数字.    
```
x % (2^p) = x & (2^p - 1)
```
然后, m 应选与2的整数幂不太接近的质数.   

  乘法散列法 h(k) = (m(kA mod 1))向下取整   
第一步, 以关键字 k 乘一个常数 A(0<A<1),并抽出 kA 的小数部分(kA mod 1). 然后,以 m 乘以这个值,再取结果的底(floor).   
m 没有什么特别要求,一般取2的某个幂次.   
  
  全域散列   
执行开始时,从一簇仔细设计的函数中,随机选择一个作为散列函数.   
首先,选择一个足够大的质数 p,使得每一个可能的关键字 k 都落在 0 到 p-1 的范围内(包括0,p-1). 设 Z 表示集合{0,1,..,p-1}, Z* 表示集合{1,2..p-1}.假定关键字域的大小大于散列表中的槽位数, 即 p>m.   
对于任何 a 属于 Z*, b 属于 Z,定义   
   h(k) = ((ak + b) mod p) mod m   
取得所有的 a ,b, 得到p(p-1)个函数的函数簇. 


  2-2-2 链接法解决碰撞  
把散列到同一槽中的所有元素都放在一个链表中.槽中有一个指针,指向由所有散列到槽的元素组成的链表的头.  
```
CHAINED-HASH-INSERT(T, x)
  insert x at the head of list T[h(key[x])]

CHAINED-HASH-SEARCH(T, k)
  search for an element with key k in list T[h(k)]

CHAINED-HASH-DELETE(T, x)
  delete x from list T[k(key[x])]
```
给定一个能存放n个元素,具有m个槽位的散列表T,T的装载因子(load factor)为 n/m,表示一个链中平均存储的元素数.  
证明全部字典操作平均情况下可以在 O(1) 时间内完成.(详见 算法导论 P136)  

  2-2-3 开放寻址法解决碰撞  
计算出要存取的各个槽. 为了确定要探查哪些槽,需要将 探查号(从 0 开始)以作为其第二个输入参数.  
```
HASH-INSERT(T,k)
i <- 0
repeat j <- h(k,i)
       if T[j]=NIL
        then T[j] <- k
             return j
       else i <- i+1
until i=m
error "overflow"

HASH-SEARCH(T,k)
i <- 0
repeat j <- h(k,i)
       if T[j]=k
        then return j
       i <- i+1
until  T[j]=NIL or i=m 
return NIL
```
删除操作比较困难,若仅将槽 i 置为 NIL 来标识的话, 则无法检测关键字为 h(k, i+1) 以及以后的关键字了.(因为 T[h(k,i)]=NIL 时,循环结束).   
解决方案是在槽 i 中置特殊值 DELETED. 但这样的话, 查找时间就不再依赖于 装载因子 了.    
因此,一般涉及到删除关键字的应用,往往采用链接法来解决碰撞.   
  
探查序列: 保证对每个关键字 k, <h(k,0),h(k,1),...,h(k,m-1)> 都是<0,1,...,m-1>的一个排列. 但下列这些探查序列都达不到 一致散列 要求的 m! 个探查序列.  
 
- 线性探查. h(k,i) = (h'(k) + i) mod m, i=0,1,...,m-1   h'(k) 为普通散列函数.   
  只有 m 种不同的探查序列. 连续占用槽越来越长,平均查找时间也会随之增加.  
  
- 二次探查. h(k,i)=(h'(k) + ai + bi^2) mod m  
  为了能够充分利用散列表,a,b,m 的值都要受到限制.(思考题 11-3)  
  初始探查决定了整个序列,因此也只有 m 个不同的探查序列.  
  
- 双重散列 h(k,i) = (h1(k) + ih2(k)) mod m   
  为了能查找整个散列表,值 h2(k)要与表的大小 m 互质. (可以取 m 为2的幂,并设计一个总是产生奇数的h2. 也可以取 m 为质数, h2总产生较m小的正整数.)   
  例如: h1(k)= k mod m, h2(k) = 1+(k mod m'). 其中 m' 略小于 m (如 m-1).  
  用了 O(m^2) 种探查序列.  
  
  2-2-4 完全散列
利用一种两级的散列方案,每一级上都采用全域散列.第一级利用某一散列函数 h,将 n 个关键字散列到 m 个槽中. 然后在散列到槽 j 中的关键字, 建立一个较小的二次散列表 Sj, 与其相关的散列函数为 hj. 为了确保第二级上不出现碰撞,需要让散列表 Sj 的大小 mj 为散列到槽 j 中的关键字数 nj 的平方.  

# 3-1 二叉查找树
对一颗 n 个结点的二叉查找树, 随机构造的树期望高度为 O(lg n),从而此树上基本动态集合操作的平均时间为 O(lg n).  
每个结点都是一个对象,除了 key 域和卫星数据外, left,right,p 分别指向左儿子,右儿子,父结点.  
二叉查找树, 对于结点x, 左子树的关键字最大不超过 key[x], 右子树关键字最小不小于 key[x].  
前序遍历: 中左右   中序遍历: 左中右    后序遍历: 左右中   
```
INORDER-TREE-WALK(x)
if x != NIL
  then INORDER-TREE-WALK(left[x])
       print key[x]
       INORDER-TREE-WALK(right[x])
```
二叉查找树的基础操作时间复杂度为 O(h).

  3-1-1  查询二叉查找树
```
TREE-SEARCH(x,k)
if x = NIL or k = key[x]
  then return x
if k < key[x]
  then return TREE-SEARCH(left[x],k)
else
  return TREE-SEARCH(right[x],k)
  
ITERATIVE-TREE-SEARCH(x,k)
while x != NIL and k != key[x]
  do if k < key[x]
    then x <- left[x]
  else
    x <- right[x]
return x    
```

 3-1-2 最大关键字元素和最小关键字元素
```
TREE-MINIMUM(x)
while left[x] != NIL
  do x <- left[x]
return x  

TREE-MAXIMUM(x)
while right[x] != NIL
  do x <- right[x]
return x  
```

  3-1-3 前趋和后继  
某一结点x的后继: 大于 key[x] 的最小者所在的结点  
```
TREE-SUCCESSOR(x)
if right[x] != NIL                              // 第一种情况, x 有右子树
  then return TREE-MINIMUM(right[x])
y <- p[x]                                       // 第二种情况, x 无右子树,且 x 是父结点的左孩子,或父结点为NIL
while y != NIL and x = right[y]                 // 第二种情况, x 无右子树,且 x 是父结点的右孩子
do x <- y
     y <- p[y]
return y
  
```

某一结点x的前趋: 小于 key[x] 的最大者所在的结点  
```
TREE-PREDECESSOR(x)
if left[x] != NIL                              // 第一种情况, x 有左子树
  then return TREE-MAXIMUM(left[x])
y <- p[x]                                      // 第二种情况, x 无右子树,且 x 是父结点的右孩子,或父结点为NIL
while y != NIL and x = left[y]                 // 第二种情况, x 无右子树,且 x 是父结点的左孩子
do x <- y
   y <- p[y]
return y
```

  3-1-4 插入和删除  
```
TREE-INSERT(T,z)
y <- NIL
x <- root[T]
while x != NIL                  // 找到适合的结点放置z, 以 y 记录该位置
  do y <- x
     if key[z] < key[x]
      x <- left[x]
     else
      x <- right[x]
p[z] <- y
if y = NIL
  root[T] <- z
else
  if key[z] < key[y]
    left[y] <- z
  else 
    right[y] <- z
```
```
TREE-DELETE(T,z)
if left[z] = NIL or right[z] = NIL        // 选择需要更改的结点 y,当z没有孩子或只有一个孩子时,操作z本身.当z有两个孩子时,需要操作z的后继
  y <- z
else 
  y <- TREE-SUCCESSOR(z)
  
if left[y] != NIL                         // x 最多只有一个孩子.若z只有一个孩子或没有,则y即z. 若z有两个孩子,其后继不可能有左孩子.
  x <- left[y]
else
  x <- right[y]
  
if x != NIL                               // 将 x 连上 y 的父结点
  p[x] <- p[y]

if p[y] = NIL
  root[T] <- x                            
else if y = left[p[y]]                    // 将 y 父结点指向 x
       left[p[y]] <- x
     else
       right[p[y]] <- x

if y != x                                 // 只有 z 有两个孩子时实际起作用.将 z 替换为 y, y从原位置删除,来到 z 的位置
  key[z] <- key[y]
  copy y's satellite data into z
return y
```

# 4 红黑树的性质
红黑树是平衡查找树的一种, 在最坏情况下, 基本的动态集合操作时间为 O(lg n).  
每个结点增加一个存储位表示结点颜色(RED/BLACK),任一条从根到叶子的路径不会比任何其他路径长出两倍,接近平衡.  
每个结点五个域: color, key, left, right, p.  
内结点: 除叶子结点之外所有结点  
使用哨兵 nil[T] 来取代所有 key 为 NIL 的空结点,这样使得 NIL 看起来像一个普通结点  
从某结点出发(不包括该结点)到达一个叶结点的任意一条路径,黑色结点的个数称为该结点 x 的黑高度, bh(x).  
  
红黑性质:   
- 每个结点或红或黑
- 根节点是黑
- 每个叶结点(NIL)是黑
- 若一个结点为红,则它两个儿子都是黑
- 对每个结点,从该结点到其子孙结点的所有路径上都包含相同数目的黑色结点  
  
一棵有 n 个内结点的红黑树的高度至多为 2lg(n+1).  

  4-1 旋转
```
LEFT-ROTATE(T,x)          // 左旋右孩子不为 nil[T]
y <- right[x]
right[x] <- left[y]       // y 左孩子给 x
p[left[y]] <- x

p[y] <- p[x]              // 确定 y 的位置
if p[x] = nil[T]
  root[T] <- y
else if x = left[p[x]]
       left[p[x]] <- y
     else 
       right[p[x]] <- y

left[y] <- x              // 确定 x 的位置
p[x] <- y

```
```
RIGHT-ROTATE(T,x)          // 右旋左孩子不为 nil[T]
y <- left[x]
left[x] <- right[y]       // y 右孩子给 x
p[right[y]] <- x

p[y] <- p[x]              // 确定 y 的位置
if p[x] = nil[T]
  root[T] <- y
else if x = left[p[x]]
       left[p[x]] <- y
     else 
       right[p[x]] <- y

right[y] <- x              // 确定 x 的位置
p[x] <- y
```

  4-2 插入  
```
RB-INSERT(T,z)
y <- nil[T]
x <- root[T]
while x != nil[T]
  do y <- x
  if key[z] < key[x]
    x <- left[x]
  else
    x <- right[x]
p[z] <- y
if y = nil[T]
  root[T] <- z
else if key[z] < key[y]
       left[y] <- z 
     else
       right[y] <- z
left[z] <- nil[T]
right[z] <- nil[T]
color[z] <- RED
RB-INSERT-FIXUP(T,z)      // 由于新加入的结点,需要进行红黑树的重排
```

对于插入的重排,性质2可能被破坏(若插入的z为根结点),性质4可能被破坏(若z的父结点为红)
```
RB-INSERT-FIXUP(T,z)
while color[p[z]] = RED              // 若 z 父结点为黑,不需重排. 因此首先 z 的父结点一定是 RED 才进行处理
  if p[z] = left[p[p[z]]]            // 这种 if 的两种情况是对称的
    y <- right[p[p[z]]]              // 取 y 为 z 的叔结点
    if color[y] = RED                // CASE1: 父结点 RED,叔结点 RED. 将父与叔结点置为 Black, 祖父结点置为 Red
      color[p[z]] <- BLACK
      color[y] <- BLACk
      color[p[p[x]]] <- RED
      z <- p[p[z]]                    // 指针 z 上移两层
    else if z = right[p[z]]           // CASE2: 父结点 RED,叔结点 BLACK,z 是右孩子. 将 z 转变为 左孩子
           z <- p[z]
           LEFT-ROTATE(T,z)
         color[p[z]] <- BLACK         // CASE3: 父结点 RED,叔结点 BLACK,z 是左孩子.
         color[p[p[z]]] <- RED
         RIGHT- ROTATE(T, p[p[z]])    // 此时由于 z 的父结点已为 BLACK, 因此无需再进入 while,重排完成
  else                                // 以下为对称的三种情况
    y <- left[p[p[z]]]                // 取 y 为 z 的叔结点
    if color[y] = RED                 // CASE4: 父结点 RED,叔结点 RED. 将父与叔结点置为 Black, 祖父结点置为 Red
      color[p[z]] <- BLACK 
      color[y] <- BLACk
      color[p[p[x]]] <- RED
      z <- p[p[z]]                    // 指针 z 上移两层, 因为祖父结点变红,还需继续进入 while 检查
    else if z = left[p[z]]           // CASE5: 父结点 RED,叔结点 BLACK,z 是左孩子. 将 z 转变为 右孩子
           z <- p[z]
           RIGHT-ROTATE(T,z)
         color[p[z]] <- BLACK         // CASE6: 父结点 RED,叔结点 BLACK,z 是右孩子.
         color[p[p[z]]] <- RED
         LEFT- ROTATE(T, p[p[z]])    // 此时由于 z 的父结点已为 BLACK, 因此无需再进入 while,重排完成
color[root[T]] <- BLACK               // z 为根结点的情况

```

  4-3 删除  
```
RB-DELETE(T,z)
if left[z] = nil[T] or right[z] = nil[T]
  y <- z
else
  y <- TREE-SUCCESSOR(z)
if left[y] != nil[T]
  `
```






 
