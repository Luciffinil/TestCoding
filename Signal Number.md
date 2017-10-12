### Single Number 

> Given an array of integers, every element appears twice except for one. Find that single one.

> Note:
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory? 

普通做法：排序后，找出不同于其他的数。不过由于使用了排序，时间复杂度不是线性。
```Java
class Solution {
	public int singleNumber(int[] nums) {
		int len = nums.length;
		if (len == 1)
			return nums[0];
		Arrays.sort(nums);
		for (int i = 0; i < len - 1; i+=2) { // 步进值为 2，因为其他数都只出现两次
			if (nums[i] != nums[i + 1])
				return nums[i];
		}
		return nums[len-1];
	}
}
```

使用HashTable:

位运算做法： A XOR A = 0, B XOR 0 = B 且 异或操作是可交换的。因此，依次把所有数进行异或操作，就可消除所有出现两次的数。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		int ans = 0;
		for (int num:nums) {
			ans ^= num;
		}
		return ans;
	}
}
```

