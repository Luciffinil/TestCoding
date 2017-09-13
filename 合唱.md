>小Q和牛博士合唱一首歌曲,这首歌曲由n个音调组成,每个音调由一个正整数表示。
对于每个音调要么由小Q演唱要么由牛博士演唱,对于一系列音调演唱的难度等于所有相邻音调变化幅度之和, 例如一个音调序列是8, 8, 13, 12, 那么它的难度等于|8 - 8| + |13 - 8| + |12 - 13| = 6(其中||表示绝对值)。
现在要对把这n个音调分配给小Q或牛博士,让他们演唱的难度之和最小,请你算算最小的难度和是多少。
如样例所示: 小Q选择演唱{5, 6}难度为1, 牛博士选择演唱{1, 2, 1}难度为2,难度之和为3,这一个是最小难度和的方案了。

    输入描述:
    输入包括两行,第一行一个正整数n(1 ≤ n ≤ 2000) 第二行n个整数v[i](1 ≤ v[i] ≤ 10^6), 表示每个音调。

    输出描述:
    输出一个整数,表示小Q和牛博士演唱最小的难度和是多少。

    输入例子:
    5
    1 5 6 2 1

    输出例子1:
    3
    
```Java
import java.util.*;
  
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] v = new int[n];
        for (int i = 0; i < n; i++) {
            v[i] = sc.nextInt();
        }
        int res = 0;
        int temp = 0; // temp记录当前音调由第二个人唱，第一个人唱前面所以音调。作用是找到第二个人什么时候开腔
        int v1 = v[0]; // v1记录第一个人最后唱的音调
        int v2 = v[1]; // v2记录第二个人最后唱的音调
        for (int i = 1; i < n; i++) {
            if (Math.abs(v1 - v[i]) < Math.abs(v2 - v[i])) {
                res += Math.abs(v1 - v[i]);
                v1 = v[i];
            } else {
                res += Math.abs(v2 - v[i]);
                v2 = v[i];
            }
            if (temp < res) { // 进入此if时候，就是第二个人开始唱的时候
                res = temp;
                v1 = v[i-1];
                v2 = v[i];
            }
            temp += Math.abs(v[i] - v[i - 1]); // temp 在循环最后再改变，因为temp表示的当前音调是第二个人开始唱的，还没有形成音调差
        }
        System.out.println(res);
    }
}
```

这是最初的想法，实际上不是动态规划。这种思路的局限性有两点。
1. 没有考虑相等的情况，因为相等时就得考虑后续的音调才能确定。
2. 没有考虑后续的音调，此方法只存储一种方案，所以必须保证每一步都是最佳方案才行。但是有的时候当前最佳方案，考虑到后续音调，往往不是最佳的了。

```
例如：   
    23  
    24 13 2 4 54 23 12 53 12 23 42 13 53 12 24 12 11 24 42 52 12 32 42

按此方法得到的方案（结果为200）：           不同部分和为61
    0 1 2 3 5 6 8 9 11 13 14 15 16 17 20 21 22
                       12 24 12 11 24 12   
    4 7 10 12 18 19
           53 42
正确结果（结果为188）：                     不同部分和为49
    0 1 2 3 5 6 8 9 11 13 15 16 20 21 22
                       12 12 11 12
    4 7 10 12 14 17 18 19
           53 24 24 42
``` 

```Java
import java.util.Scanner;

 1   dp[i][j]（永远有i > j）表示某一个人最近唱的音为第i个，另一个人最近唱的是第j个时最小的难度
 2   由于只由一个人唱完肯定不是最优解。因此先在一个for循环内确定以下两种情况的初值
    dp[i][0]：第二个人唱第一个音，第一个人唱后面所有音
    dp[i][i-1]：第一个人唱最近的一个音，第二个人唱前面所有音

    dp[i][j]转移方程
    每当交换唱歌次序，两人最近一次唱的音符一定是相邻的，所以底下分相邻和不相邻讨论：
    (1). 当j == i - 1，即交换唱歌次序，dp[i][i-1]时，表明第一个人上一个音可能在第k个音（0 <= k < i-1）,由唱了最近的第i个，第二个人仍然留在第i-1个音。
    dp[i][i-1] = 对所有k求min(dp[i-1][k] + abs(arr[i] - arr[k]) ) 其中（0 <= k < i-1）
    (2). 当j < i - 1，即不交换唱歌次序时，只可能由唱到i-1音符的人续唱
    dp[i][j] = dp[i-1][j] + abs(arr[i] - arr[i-1])

    最后求出所有dp[len-1][]里的最小值即为全局最优解

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		while (sc.hasNext()) {
			int len = sc.nextInt();
			int[] v = new int[len];
			for (int i = 0; i < len; ++i) {
				v[i] = sc.nextInt();
			}
			if (len < 3) {
				System.out.println("0");
			} else {
				int[][] dp = new int[len][len];
//		  dp[0][0] = - Math.abs(v[1] - v[0]);    // 不需要，dp[i][i-1]会完成
				int acc = 0;
				for (int i = 1; i < len; ++i) {
					dp[i][i - 1] = acc;
					for (int j = 0; j < i - 1; ++j) {
						dp[i][j] = dp[i - 1][j] + Math.abs(v[i] - v[i - 1]);
						dp[i][i - 1] = Math.min(dp[i][i - 1], dp[i - 1][j] + Math.abs(v[i] - v[j]));
					}
					acc += Math.abs(v[i] - v[i - 1]);
				}
				int min = Integer.MAX_VALUE;
				for (int j = 0; j < len - 1; ++j) {
					min = Math.min(min, dp[len - 1][j]);
				}
				System.out.println(min);
			}
		}
	}
}
```
    
动态规划做法。
