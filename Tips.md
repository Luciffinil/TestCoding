# 1 减少时间复杂度

可以用 HashMap 代替 List 或者 数组，因为 Map 的查询时间复杂度为O（1）。

例如：2Sum
```Java
public int[] twoSum(int[] numbers, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i = 0; i < numbers.length; i++) {
            if(map.containsKey(target - numbers[i]))
                return new int[]{map.get(target - numbers[i]), i+1};
            map.put(numbers[i], i+1);
        }
        return null;
}
```

# 2 自定义ListNode 的用法
