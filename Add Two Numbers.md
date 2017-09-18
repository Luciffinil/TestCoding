> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list. (You may assume the two numbers do not contain any leading zero, except the number 0 itself.)

    Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
    Output: 7 -> 0 -> 8
   
```Java
class Solution {
	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		ListNode res = new ListNode(0);
		ListNode p = l1, q = l2, cur = res;
		int jw = 0;
		while (p != null || q != null) {
			int v1 = (p != null) ? p.val : 0;
			int v2 = (q != null) ? q.val : 0;
			int v = v1 + v2 + jw;
			cur.next = new ListNode(v % 10);
			cur = cur.next;
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

题目要求每个 Node 存储都是个位数，所以一定有进位的问题。
