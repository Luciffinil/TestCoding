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
### 2 可进行任意次买卖
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). 

```Java
public class Solution {
    public int maxProfit(int[] prices) {//这次的真简单，比第一道简单多了，相邻只要是上升的就要了
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

```Java
public class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length ==0 || prices == null) return 0;
        int min = prices[0];
        int res = 0;
        for (int i = 1; i < prices.length - 1; i++) {
            min = Math.min(min, prices[i]);
            if (prices[i] >= prices[i - 1] && prices[i] > prices[i + 1]) {
                res += prices[i] - min;
                min = Integer.MAX_VALUE;
            }
        }
        if(prices[prices.length -1] > min){
            res += prices[prices.length -1] - min;
        }
 
        return res;
    }
}
```

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

 
