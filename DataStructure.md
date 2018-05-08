# 1 栈
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

# 2 队列  
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

# 3 链表



