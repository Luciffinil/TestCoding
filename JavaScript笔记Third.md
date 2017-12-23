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

间歇调用 setInterval() 用法与超时调用一致. 一般可以使用超时调用代替.

5 系统对话框 - 同步和模态的,即显示这些对话框时代码停止执行,关掉后代码恢复执行.
alert()  警告对话框,显示一个字符串和一个 确定 按钮

confirm()  确定和取消按钮, 让用户决定是否执行给定操作
if(confirm("Are you sure?"){
    //To do
}else{
    //To do
}

prompt()  提示框,点击 确定, 返回用户输入值;否则返回 null.


6 location 对象 - 提供了与当前窗口中加载的文档有关的信息,还提供了一些导航功能
                - 既是 window 对象的属性,也是 document 对象的属性
                - 不仅保存当前文档的信息,还能将 URL 解析为独立的片段
                

  位置操作: 
  以下三种方式是相同的,立即打开新 Url,并在浏览器历史记录中生成一条记录(可使用返回键返回原本页面)
  location.assign("http://www.wrox.com");
  window.location = "http://www.wrox.com";
  location.herf = "http://www.wrox.com";          // 最常用
  
  以下方式不可使用返回键返回原来页面
  location.replace("http://www.wrox.com");

  刷新页面 - reload 后的代码可能执行也可能不执行(取决于网络延迟或系统资源),因此最好放在最后一行
  location.reload();          // 有可能从缓存中加载
  location.reload(true);      // 从服务器重新加载


7 navigator 对象 - 识别客户端浏览器的事实标准

8 history 对象 - 保存用户上网的历史记录
  开发人员无法得知用户浏览过的Url,不过可以使用 go() 方法再用户的历史记录中任意跳转.
  history.go(-1);    // 后退一页   相当于 history.back();
  history.go(1);     // 前进一页   相当于 history.forward();
  history.go(2);     // 前进二页
  history.go("wrox.com");         // 跳转到最近的包含"wrox.com"的页面
  
  history.length 表示历史记录数量
  
  Url中的hash改变时,就会生成一条历史记录.
  
  
第10章  DOM(文档对象模型)
1 DOM描绘了一个层次化的节点树,允许开发人员添加,移除和修改页面的一部分.

  Node 类型
  DOM1级定义了 Node 接口,该接口由所有节点类型实现. 每个节点都有一个 nodeType 属性, 以12个数值常量表示,用于表明节点类型.
  1 Node.ELEMENT_NODE
  2 Node.ATTRIBUTE_NODE
  3 Node.TEXT_NODE
  4 Node.CDATA_SECTION_NODE
  5 Node.ENTITY_REFERENCE_NODE
  6 Node.ENTITY_NODE;
  7 Node.PROCESSING_INSTRUCTION_NODE
  8 Node.COMMENT_NODE
  9 Node.DOCUMENT_NODE
  10 Node.DOCUMENT_TYPE_NODE
  11 Node.DOCUMENT_FRAGMENT_NODE
  12 Node.NOTATION_NODE

节点具体信息: nodeName, nodeValue
每个节点都有一个 childNodes 属性, 其中保存着一个 NodeList 对象. NodeList 是一种类数值对象(并不是 Array 实例),保存一组有序的节点. NodeList 是基于DOM结构动态执行查询的结果,是有生命,有呼吸的对象. (hasChildNodes())
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var count = someNode.childNodes.length;

将 NodeList 对象转换为数组
function convertToArrary(nodes){
    var array = null;
    try{
        array = Array.prototype.slice.call(nodes, 0);   // 不能用 Array.slice(), 因为Array不是一个实例,并没有slice方法
                                                        // 延原型链查找时,也不会查 Array.prototype
    } catch(ex) {        // 针对 IE8 以及之前版本
        array = new Array();
        for(var i=0, len=nodes.length; i<len;i++){
            array.push(nodes[i]);
        }
    }
    return array;
}    

每个节点都有一个 parentNode 属性,该属性指向文档树中的父节点. 父节点可以通过 firstChild 和 lastChild 访问第一个和最后一个孩子节点.
各节点可以通过 previousSibling 和 nextSibling 属性访问同胞节点.

每个节点都有一个 ownerDocument 属性,指向表示整个文档的文档节点.

  操作子节点
appendChild() 向 childNodes 列表末尾添加一个节点. 若该节点已存在,则移至末尾.返回新增的节点.
insertBefore() 向指定位置插入节点. 接受两个参数: 要插入的节点 作为参照的节点(该参数为null时同appendChild()).返回新增的节点.

replaceChild() 替换节点,接受两个参数
removeChild() 移除节点    这两种方法移除的节点仍属于文档所有,只不过在文档中已经没有自己的位置.

  所有节点公有方法
cloneNode()  参数为ture,深复制,复制节点及其整个子节点树. false,浅复制,只复制节点本身. - 不复制添加到dom节点中的js属性,例如事件处理程序.
normalize()  处理文本节点. 删除空文本节点,相邻文本节点合并.



2 Document 类型
注释也作为元素存在,不过html外面的注释由于各浏览器的处理不同,通常无用.

document 对象是 HTMLDocument(继承自 Document 类型)的一个实例,表示整个 HTML 页面.
Document 节点的子节点可以是 DocumentType, Element, ProcessingInstructior 或 Comment. 

var html = document.documentElement;      // 获取对 <html> 的引用
alert(html === document.childNodes[0]);   // true
var body = document.body;                 // 取得对 <body> 的引用
var title = document.title;               // 取得文档标题 <title>
var url = document.URL;                   // 取得完整的URL
var domain = document.domain;             // 取得域名
var referrer = document.referrer;         // 取得来源页面的URL

仅 domain 可以设置,但不能设置为URL中不包含的域.   p2p.wrox.com  -> wrox.com    紧绷的域名->松散的域名
可以利用这点实现不同子域的JavaScript通信.   不能从松散设置回紧绷.

  查找元素
getElementById()   严格匹配id,返回第一个匹配元素. 找不到返回 null.
getElementByTagName()  匹配元素的标签名(对html页面不区分大小写,对xml页面则区分),返回包含零或多个元素的 NodeList. 在HTML文档中, 这个方法会返回一个 HTMLCollection 对象.
HTMLCollection 对象有个方法 namedItem(),可按元素的 name 特性取得集合中的项.
var images = document.getElementByTagName("img");
var myImage = images.namedItem("myImage");
var myImage = images["myImage"];              // 效果同上

想要取得所有元素,可以向 getElementByTagName() 中传入 "*", * 在 js 和 css 中表示全部.

getElementByName() 返回HTMLCollection对象.

   文档写入 - 严格型XHTML文档不支持文档写入, 按照 application/xml+xhtml内容类型提供的页面,下面方法也无效.
document.write()
document.writeln()  自动换行

不仅可以接受字符串作为参数,直接传入 js 标签页可以.
<script type="text/javascript">
    document.write("<script type=\"text/javascript\" src=\"file.js\">" + "<\/script>");  
    // <\/script> 不加转义可能会结束上层的<script>标签
</script>

如果文档加载结束后再调用 document.write(), 那么输出的内容会重写整个页面



3 Element 类型


























