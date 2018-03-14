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

# 3 WEB中URL的相对路径

```JavaScript
相对路径有两种格式

第一种：不以“/"开头，基于当前网页所在目录和相对路径构建URL

<a href="b.html">link</a>

URL = "http://www.ipo.com/w" + "b.html"

第二种：以“/”开头，基于当前域和相对路径构建URL

<a href="/w/b.html">link</a>

URL = "http://www.ipo.com" + "/w/b.html"
```

# 4 反射机制获取并修改私有属性方法
```java
public class PrivateClass2
{
    private String name = "zhangsan";
    
    private String sayHello(String name)
    {
        return "Hello: " + name;
    }
    
    public String getName()
    {
        return name;
    }
}
```

```java
import java.lang.reflect.Field;

public class TestPrivate2
{
    public static void main(String[] args) throws Exception
    {
        PrivateClass2 p = new PrivateClass2();
        Class<?> classType = p.getClass();

        Field field = classType.getDeclaredField("name");

        field.setAccessible(true); // 抑制Java对修饰符的检查，如果不加 set 的时候会报错
        field.set(p, "lisi");

        System.out.println(p.getName());
	
	// 获取Method对象
        Method method = classType.getDeclaredMethod("sayHello",
                new Class[] { String.class });

        method.setAccessible(true); // 抑制Java的访问控制检查
        // 如果不加上上面这句，将会Error: TestPrivate can not access a member of class PrivateClass with modifiers "private"
	
        String str = (String) method.invoke(p, new Object[] { "zhangsan" });

        System.out.println(str);
    }
}
```
