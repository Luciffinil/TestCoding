第8章   BOM
1 BOM核心对象是 window,表示浏览器的一个实例.
  
  全局变量不能通过delete操作符删除,而直接在 window 对象上定义的属性可以.
  直接访问未声明的变量会抛出错误,但通过查询 window 对象, 可以知道某个可能未声明的变量是否存在.
  var newValue = oldValue;            // oldValue 未定义,抛出错误
  
  var newValue = window.oldValue;     // undefined 这是一次属性查询
  
2 窗口关系及框架 
  如果页面中包含框架,则每个框架都拥有自己的 window 对象,并且保存在 frames 集合中. 可以通过数值索引(从0开始,从上到下,从左到右)或框架名称来访问相应的window对象.
  对于在一个框架中编写的任何代码来说,其中的 window 对象指向的都是那个框架的特定实例.
  最好使用 top.frames[0] 或 top.frames["firstFrame"] 来访问. top对象始终指向最高(最外)层的框架.
  parent 对象始终指向当前框架的直接上层框架. 在没有框架的情况下,parent 一定等于 top(此时,它们都等于 window).
  self 对象始终指向 window,可以和 window 对象互换使用.
  top, parent, self 都是 window 的属性,可通过 window.top 的形式访问.
  
3 窗口位置
var leftPos = (typeof window.screenLeft == "number") ? window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ? window.screenTop : window.screenY;

对最外层 window 可使用 moveTo() 绝对位置,moveBy() 相对位置 移动窗口,不过可能被浏览器禁用.

  窗口大小
window.innerWidth
window.innerHeight


