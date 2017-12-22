```JavaScript
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

对最外层 window 可使用 resizeTo() 绝对大小,resizeBy() 相对大小 调整窗口大小,不过可能被浏览器禁用.

  导航和打开窗口
window,open()既可以导航到一个特定的URL,也可以打开一个新的浏览器窗口. 
接收4个参数: 要加载的URL(必填),窗口目标,一个特定字符串,一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值(只在不打卡新窗口情况下使用).
第二个参数, 可以是窗口或框架的名字,也可是 _self, _parent, _top 或 _blank.
var wroxWin = window.open("http://www.wrox.com/", "wroxWindow", "height=400,width=400,resizable=yes");
该方法会返回一个指向新窗口的引用. 
对于弹出的窗口,可以调用 wroxWin.close(); 方法关闭.
wroxWin.opener 指向打开该窗口的原始窗口对象.

  检测弹出窗口被屏蔽  
var blocked = false;
try{
    var wroxWin = window.open("http://www.wrox.com/", "_blank", "height=400,width=400,resizable=yes");
    if(wroxWin == null){
        blocked = true;
    }
} catch(ex){
    blocked = true;
}
if(blocked){
    alert("The Popup Was Blocked!");
}

4 间歇调用和超时调用
JavaScript 是单线程语言,但允许通过设置 超时值(指定时间后执行代码) 和 间歇时间(每隔指定时间就执行一次) 来调度代码在特定时刻执行.

超时调用 setTimeout() 接受两个参数: 要执行的代码, 以毫秒表示的时间.
超时调用会经过第二个参数的时间后,将第一个参数添加到任务队列中,而不是立即执行.(Js单线程,同时只能执行一个任务,所以可能需要等待)
该方法返回一个数值ID,表示超时调用. 可以用来在调用执行前取消该次调用.
var timeoutID = setTimeout(function(){
    alert("Hello World!");
}, 1000);
clearTimeout(timeoutID);

间歇调用 setInterval()




























































