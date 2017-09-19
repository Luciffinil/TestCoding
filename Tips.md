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

例如：Add Two Numbers
```Java
class Solution {
	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		ListNode res = new ListNode(0);
		ListNode p = l1, q = l2, cur = res;                 // 遍历时一定要用一个新的LishNode，否则如果执行 l1 = l1.next;
		int jw = 0;                                         // 循环结束后，l1就变成了只有一个节点的链表
		while (p != null || q != null) {
			int v1 = (p != null) ? p.val : 0;
			int v2 = (q != null) ? q.val : 0;
			int v = v1 + v2 + jw;
			cur.next = new ListNode(v % 10);	   // 添加新的节点的做法
			cur = cur.next;				   // 指向新节点的对下一个节点的连接
			jw = v / 10;
			if (p != null)
				p = p.next;
			if (q != null)
				q = q.next;
		}
		if (jw > 0) { // 最后一位也有可能需要进位
			cur.next = new ListNode(jw);
		}
		return res.next;
	}
}
```
