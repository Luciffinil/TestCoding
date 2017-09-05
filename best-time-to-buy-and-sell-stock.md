Say you have an array for which the i th element is the price of a given stock on day i.
Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 1 只进行一次买卖
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit. 

```Java
public class Solution {
    public int maxProfit(int[] prices) {
        if(prices==null||prices.length==0){
            return 0;
        }
        int max=0;
        int min=prices[0];
        for(int i=0;i<prices.length;i++){
            min = Math.min(min,prices[i]);
            max=Math.max(max,prices[i]-min);
        }
        return max;
    }
}
```

只记录每个谷底的值，找到下次谷底前的最大值。

### 2 可进行任意次买卖
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). 

```Java
public class Solution {
    public int maxProfit(int[] prices) {
        int sum =0;
        for(int i=0;i<prices.length-1;i++){
            if(prices[i]<prices[i+1]){
                sum+=prices[i+1]-prices[i];
            }
        }
        return sum;
    }
}
```

对于每一个元素，只要比前一个大，则将此差值加入sum中。  原理为 e-a= e-d + d-c + c-b + b-a

```Java
public class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length ==0 || prices == null) return 0;
        int min = prices[0];
        int res = 0;
        for (int i = 1; i < prices.length - 1; i++) {
            min = Math.min(min, prices[i]);
            if (prices[i] >= prices[i - 1] && prices[i] > prices[i + 1]) { // 找到峰值所在
                res += prices[i] - min;
                min = Integer.MAX_VALUE;
            }
        }
        if(prices[prices.length -1] > min){ // 查看最后一个元素是否也为峰值
            res += prices[prices.length -1] - min;
        }
        return res;
    }
}
```

这种做法保证了最少的交易次数。

### 3 最多进行两次买卖
Design an algorithm to find the maximum profit. You may complete at most two transactions.

```Java
public class Solution {
    public int maxProfit(int[] prices) {
        int buy1 = Integer.MIN_VALUE, sell1 = 0, buy2 = Integer.MIN_VALUE, sell2 = 0;
        for(int i = 0; i < prices.length; i++) {
            buy1 = Math.max(buy1, -prices[i]);
            sell1 = Math.max(sell1, buy1 + prices[i]);
            buy2 = Math.max(buy2, sell1 - prices[i]);
            sell2 = Math.max(sell2, buy2 + prices[i]);
        }
        return sell2;
    }
}
```

buy1一直记录最小的买入价，sell1记录一次买卖的最大利润。
buy2记录一次买卖后再买一次，剩余最多的钱。sell2是两次买卖后最大利润。
for循环里的四个操作可以是任意顺序。因为只有找到谷值后才会进行实际计算，找到谷值的那一次循环只确定了buy1，下一次才计算其他三个。

### Note

2 的第一种解法 和 3 的解法，都使用了可以在同一天先卖再买的技巧，这样就可以在同一个循环的i下写公式，将连续的天数计算切割为每一天的计算，逻辑更清晰。

 
