>魔法王国一共有n个城市,编号为0~n-1号,n个城市之间的道路连接起来恰好构成一棵树。
小易现在在0号城市,每次行动小易会从当前所在的城市走到与其相邻的一个城市,小易最多能行动L次。
如果小易到达过某个城市就视为小易游历过这个城市了,小易现在要制定好的旅游计划使他能游历最多的城市,请你帮他计算一下他最多能游历过多少个城市(注意0号城市已经游历了,游历过的城市不重复计算)。 

    输入描述:
    输入包括两行,第一行包括两个正整数n(2 ≤ n ≤ 50)和L(1 ≤ L ≤ 100),表示城市个数和小易能行动的次数。
    第二行包括n-1个整数parent[i](0 ≤ parent[i] ≤ i), 对于每个合法的i(0 ≤ i ≤ n - 2),在(i+1)号城市和parent[i]间有一条道路连接。

    输出描述:
    输出一个整数,表示小易最多能游历的城市数量。

    输入例子1:
    5 2
    0 1 2 3

    输出例子1:
    3
    
先理解一下输入描述。 parent[i] 存储的是城市的编号，i 代表的就是partent[]数组的序号，而每个 编号为 parent[i] 的城市 和编号为（i+1） 的城市有连接。 
0 ≤ parent[i] ≤ i 这个限制条件的意思实际上是：parent[i] 作为 i+1 的父节点，其编号一定小于 i+1 。 这就使得编号构成的树具有一个属性，即位于上层的编号一定小于下层。
上面的属性使得，可以使用一个 for 循环得到树的高度成为可能。
```Java
for(int i = 1; i < n; i++){
  tr[i] = tr[parent[i-1]] + 1;
  h = Math.max(h, tr[i]);
}
```

本题最佳做法，就是不建树，直接找到树的高度。具体分析：
- 若 L ≤ maxLen ，显而易见得结果；
- 若 L > maxLen ，意味着可以往回走，要知道越短的树链往回走的代价越低。如果从末端往回走，消耗的代价非常高，最坏情况是较短的树链都连接在最远的树根上，整条最长链都要回走；**如果已经知道最终步数会有剩余，则可以先消耗富余的步数走短链，最后才走最长链**；
- 继续对 rest = L - maxLen 进行讨论：
  - 若树链上存在某个节点拥有另一条子链，其长度 x 必定小于或等于该祖先到原链末端的长度，考察树链上每个节点到叶子的一条最短子链：
  - 当 x > rest/2 可以在中途预先用掉 rest 步而不影响要走的 maxLen 最长链，可达城市增加 rest/2 个；
  - 当 x ≤ rest/2 可以在中途预先用掉 2x 步而不影响要走的 maxLen 最长链，可达城市增加 x 个；
  - 当所有的 x 总和 sum(x) ≤ rest/2 说明富余的步数足够把最短链到次最长链都走一遍，可达城市为全部 n 个。 
        本小节讨论可知 rest/2 决定了能多走的城市数量，总共能走 min(n, 1 + rest/2 + maxLen) 个城市。 
        
```Java
import java.util.*;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int n = sc.nextInt();
		int L = sc.nextInt();
		int[] tr = new int[n];
		int[] parent = new int[n];
		for (int i = 0; i < n - 1; i++) {
			parent[i] = sc.nextInt();
		}
		int h = 0;
		for (int i = 1; i < n; i++) {
			tr[i] = tr[parent[i - 1]] + 1;
			h = Math.max(h, tr[i]);
		}
		int d = Math.min(L, h);
		System.out.println(Math.min((n), 1 + d + (L - d) / 2));
	}
}
```
