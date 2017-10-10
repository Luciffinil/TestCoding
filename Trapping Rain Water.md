    Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

    For example,
    Given [0,1,0,2,1,0,1,3,2,1,2,1], return 6.

![Image of Problem](http://www.leetcode.com/static/images/problemset/rainwatertrap.png)

大部分做法是对于每一个点单独考虑，这样考虑起来比寻找接水的区域要简单的多。

### 1 暴力解法

```Java
class Solution {
	public int trap(int[] height) {
		int ans = 0;
		int len = height.length;
		for (int i = 1; i < len - 1; i++) {
			int leftMax = 0, rightMax = 0;
			for (int j = i; j >= 0; j--) {
				leftMax = Math.max(leftMax, height[j]);
			}
			for (int j = i; j < len; j++) {
				rightMax = Math.max(rightMax, height[j]);
			}
			ans += Math.min(leftMax, rightMax) - height[i];
		}
		return ans;
	}
}
```


    Time complexity: O(n2)O(n^2)O(n^2). For each element of array, we iterate the left and right parts.

    Space complexity: O(1)O(1)O(1) extra space.


### 2 动态规划

第一种解法, 不断循环求前后最高位置，实际上是一种资源浪费。

```Java
class Solution {
	public int trap(int[] height) {
		if (height == null || height.length == 0)
			return 0;
		int ans = 0;
		int len = height.length;
		int[] leftMax = new int[len];
		int[] rightMax = new int[len];
		leftMax[0] = height[0];
		rightMax[len - 1] = height[len - 1];
		for (int i = 1; i < len; i++) {
			leftMax[i] = Math.max(leftMax[i - 1], height[i]);
		}
		for (int i = len - 2; i >= 0; i--) {
			rightMax[i] = Math.max(rightMax[i + 1], height[i]);
			ans += Math.min(leftMax[i], rightMax[i]) - height[i];
		}
		return ans;
	}
}
```

    Time complexity: O(n).
    
    Space complexity: O(n)extra space.

### 3 双指针法

对方法2的改进，如何在一轮循环中得到某个点的蓄水量。因为蓄水量由 leftMax 和 rightMax 中较小的决定，由两个指针从左右向中间逼近，不断更新 leftMax 和 rightMax 的值。

```Java
class Solution {
	public int trap(int[] height) {
		if (height == null || height.length == 0)
			return 0;
		int ans = 0;
		int len = height.length;
		int left = 1, right = len - 2; // 指示计算的某一格
		int leftMax = height[0], rightMax = height[len - 1]; // 分别表示 从左起 和 从右起 最高的两堵墙
		while (left <= right) {
			if(leftMax<rightMax){
				if(height[left]>leftMax){
					leftMax = height[left];
				}else{
					ans += leftMax - height[left];
				}
				left++;
			}else{
				if(height[right]>rightMax){
					rightMax = height[right];
				}else{
					ans += rightMax - height[right];
				}
				right--;
			}
		}
		return ans;
	}
}
```

还有一种解法是用 栈 做的，而且不是独立的考虑每一个格子。主要思想是一层一层的向上抹平。

```Java
class Solution {
	public int trap(int[] height) {
		if (height == null || height.length == 0)
			return 0;
		int ans = 0;
		int cur = 0;
		int len = height.length;
		Stack<Integer> st = new Stack<>();
		while (cur < len) {
			while (!st.isEmpty() && height[cur] > height[st.peek()]) {
				int toHde = height[st.pop()];
				if (st.isEmpty())
					break;
				int l = cur - st.peek() - 1;
				int h = Math.min(height[cur], height[st.peek()]) - toHde;
				ans += l*h;
			}
			st.push(cur++);
		}
		return ans;
	}
}
```


