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

使用List（或HashTable）: 主要使用 contains 检查是否已存在，存在则删除。对于只出现两次的可以如此设计。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		List<Integer> ls = new ArrayList<>();
		for (int num:nums) {
			if(ls.contains(num)){
				ls.remove((Object)num);
			}else{
				ls.add(num);
			}
		}
		return ls.get(0);
	}
}
```

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

数学做法： 2*（a+b+c）-(2a+2b+c) = c 利用 hashSet 不存储重复数据的特性。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		Set<Integer> st = new HashSet<>();
		int sum1 = 0, sum2 = 0;
		for (int num : nums) {
			st.add(num);
			sum1 += num;
		}
		Iterator<Integer> it = st.iterator();
		while (it.hasNext()) {
			sum2 += it.next();
		}
		return 2 * sum2 - sum1;
	}
}
```

### Single Number II 

> Given an array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

> Note:
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory? 

其他做法类似，只讨论 位运算做法。

1 按每一位，将所有数的那一位相加，按 3 取模。这样就能得到结果所有位的值，再加起来就行了。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		int ans = 0;
		for (int i = 0; i < 32; i++) {  // 假设所有数字都是32位以内的
			int bitSum = 0;
			for (int j = 0; j < nums.length; j++) {
				bitSum += (nums[j] >> i) & 1;  // 按每一位，将所有数的那一位相加，按 3 取模。
			}
			ans |= (bitSum % 3) << i; // 所有位的值加起来
		}
		return ans;
	}
}
```

2 用ones来记录只出现过一次的bits，用twos来记录只出现过两次的bits，ones&twos实际上就记录了出现过三次的bits，这时候我们来模拟进行出现3次就抵消为0的操作，抹去ones和twos中都为1的bits。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		int ones = 0, twos=0,threes =0;
		for (int num:nums) {
			twos |= ones & num;
			ones ^= num;
			threes = ones & twos;
			ones &= ~threes;
			twos &= ~threes;
		}
		return ones;
	}
}
```

3 对于方法2，还有一种更简化的版本。

```Java
class Solution {
	public int singleNumber(int[] nums) {
		int ones = 0, twos = 0;
		for (int num:nums) {
			ones = (ones ^ num) & ~twos;
			twos = (twos ^ num) & ~ones;
		}
		return ones;
	}
}
```












