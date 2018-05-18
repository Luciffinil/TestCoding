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

# 5 \r \n 区别与联系

\r 回车，光标回到当前行首  
\n 换行，光标移动到下一行  
Windows，每行结尾是 \r\n
Unix, 每行结尾是\n
Mac， 每行结尾是\r

# 6 接口与抽象类  
详解见 http://www.importnew.com/18780.html  
  
接口（interface）：对于行为的抽象
```java
[public] interface InterfaceName {
 
}
```
接口中的变量会被隐式地指定为public static final变量，而方法会被隐式地指定为public abstract方法且只能是public abstract方法，并且接口中所有的方法不能有具体的实现。  
  
  
抽象类（abstract class）：对一种事物的抽象
```java
[public] abstract class ClassName {
    abstract void fun();
}
```
抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。  
包含抽象方法的类称为抽象类，但并不意味着抽象类中只能有抽象方法，它和普通类一样，同样可以拥有成员变量和普通的成员方法。注意，抽象类和普通类的主要有三点区别：  
1）抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。  
2）抽象类不能用来创建对象；  
3）如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法。如果子类没有实现父类的抽象方法，则必须将子类也定义为为abstract类。  
  
  
接口与抽象类的不同：继承抽象类是一个 “是不是”的关系，而接口实现则是 “有没有”的关系。例如：门（Door）和警报（Alarm）的设计。  

门都有open( )和close( )两个动作，此时我们可以定义通过抽象类和接口来定义这个抽象概念：
```java
abstract class Door {
    public abstract void open();
    public abstract void close();
}
```
或者：  

```java
interface Door {
    public abstract void open();
    public abstract void close();
}
```
但是现在如果我们需要门具有报警alarm( )的功能，那么该如何实现？下面提供两种思路：  

1）将这三个功能都放在抽象类里面，但是这样一来所有继承于这个抽象类的子类都具备了报警功能，但是有的门并不一定具备报警功能；  

2）将这三个功能都放在接口里面，需要用到报警功能的类就需要实现这个接口中的open( )和close( )，也许这个类根本就不具备open( )和close( )这两个功能，比如火灾报警器。  

从这里可以看出，Door的open() 、close()和alarm()根本就属于两个不同范畴内的行为，open()和close()属于门本身固有的行为特性，而alarm()属于延伸的附加行为。因此最好的解决办法是单独将报警设计为一个接口，包含alarm()行为,Door设计为单独的一个抽象类，包含open和close两种行为。再设计一个报警门继承Door类和实现Alarm接口。
```java
interface Alram {
    void alarm();
}
 
abstract class Door {
    void open();
    void close();
}
 
class AlarmDoor extends Door implements Alarm {
    void oepn() {
      //....
    }
    void close() {
      //....
    }
    void alarm() {
      //....
    }
}
```


# 7 前端事件阻止 (preventDefault 和 stopPropagation)









# 8 Javascript toString()、toLocaleString()、valueOf()三个方法的区别  
https://www.cnblogs.com/niulina/p/5699031.html  
  
toLocaleString() 在处理 Date 类型的数据后, 会产生 形如 2016/7/23 下午 4:09:00 的字符串,根据地区不同产生不同字符串
toLocaleString() 在处理 Number 类型的数据后, 会产生 形如 1,234,567,890 的字符串, 自动加逗号

