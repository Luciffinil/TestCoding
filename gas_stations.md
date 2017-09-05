There are N gas stations along a circular route, where the amount of gas at station i isgas[i].

You have a car with an unlimited gas tank and it costscost[i]of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

Note:
The solution is guaranteed to be unique. 

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {
        //total记录全局是否有解，sum记录i到i+1是否可行
        int total = 0, sum = 0;
        int index = 0;
        for (int i = 0; i < gas.size(); i++) {
            sum += gas[i] - cost[i];        //本次消耗
            total += gas[i] - cost[i];  //总消耗
            if (sum < 0) {
                sum = 0;
                index = i + 1;    //记录解的位置
            }
        }
        return total >= 0 ? index : -1; //只要total>=0，肯定有解
    }
};
```

sum 的作用非常重要，当 sum<0 时，表示起点不可能在当前循环的i之前。所以假定 i+1 是起点，将 sum 置为零进行计算。

Note：
不必考虑i=gas.size()-1。假如position = len -1，那么最后一段的sum值必然为负，且sum大于total，那么此时total应该是负数。

```C++
// 从start出发， 如果油量足够， 可以一直向后走 end++；  油量不够的时候，
// start向后退  最终 start == end的时候，如果有解一定是当前 start所在位置
class Solution {
public:  int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {      
        int start = gas.size() - 1;
        int end = 0;
        int sum = gas[start] - cost[start];
        while(start > end){
            if(sum >= 0){
                sum += gas[end] - cost[end];
                ++end;
            }else{
                --start;
                sum += gas[start] - cost[start];
            }
        }
        return sum >=0 ? start : -1;
         
         
    }
};
```

这种方法从两个方向逼近，起始点先设置为 gas.size() - 1，如果剩余油量足够的话，就继续前行。否则起始点向前推一站。






