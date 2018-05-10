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
  
  链接法解决碰撞
把散列到同一槽中的所有元素都放在一个链表中.槽中有一个指针,指向由所有散列到槽的元素组成的链表的头.
```
CHAINED-HASH-INSERT(T, x)
  insert x at the head of list T[h(key[x])]

CHAINED-HASH-SEARCH(T, k)
  search for an element with key k in list T[h(k)]

CHAINED-HASH-DELETE(T, x)
  delete x from list T[k(key[x])]
```




