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
元素标签名 nodeName, tagName. HTML中实际输出的全部是大写表示, XML中会与源代码中保持一致.

  HTML元素 - 由HTMLElement类型表示,该类型继承自Element,并添加了一些属性.
id: 元素在文档中的唯一标识符
title: 附加说明信息,鼠标移动到元素所在位置时显示
className: class的特性,即为元素指定的CSS类.(class是ECMAScript的保留字,不能使用)

  取得特性
getAttribute(), setAttribute(), removeAttribute()  
alert(div.getAttribute("class"));

特殊的特性
div.getAttribute("style")  返回style特性值中包含的Css文本
div.style                  返回一个对象

div.getAttribute("onclick")  返回代码的字符串
div.onclick                  返回一个JavaScript函数

因此,常用属性方式取得特性,只有在取得自定义特性值时,使用 getAttribute() 方法.

  设置特性 
setAttribute() 可用来操作自定义特性, 使用属性设置自定义特性, 该属性不会自动成为元素的特性.


  attributes 属性 - 包含一个 NamedNodeMap,与NodeList类似.
getNamedItem(name): 返回nodeName 等于 name 的节点,也可使用方括号语法
removeNamedItem(name) 移除,返回被移除的Attr节点
setNamedItem(name) 添加
item(pos) 返回位于数字 pos 位置处的节点

var id = element.attributes.getNamedItem("id").nodeValue;
var id = element.attributes["id"].nodeValue;

遍历元素特性时,能用的上 attributes 属性.
function outputAttributes(element){
    var pairs = new Array(),
        attrName,
        attrValue,
        i,
        len;
    for(i=0, len=e.attributes.length;i<len;i++){
        attrName = e.attributes[i].nodeName;
        attrValue = e.attributes[i].nodeValue;
        pairs.push(attrName + "=\"" + attrValue + "\"");    
    }
    return pairs.join(" ");         // 返回一个以空格隔开的字符串,序列化长字符串的一种常用技巧
}



  创建元素
document.createElement() 只接受一个参数,即标签名
之后添加元素的特性
再添加到文档树(appendChild,insertBefore...)

ie种可以使用createElement,直接传入完整的元素标签.


  元素的子节点
<ul id="myList">
    <li>item1</li>
    <li>item2</li>
    <li>item3</li>
</ul>    
除了ie, 其他浏览器通过childNodes属性遍历子节点时,会包含7个元素,3个<li>和4个文本节点(<li>之间的空白符).
因此,操作前应检查 nodeType属性. 只有 nodeType 为1,表示元素节点才进行操作.


4 Text 类型 - 没有子节点
通过 nodeValue 或 data 属性访问文本
  
  创建文本节点
document.createTextNode()
  合并文本节点
element.normalize();
  分割文本节点
element.splitText(5);


5 Comment 类型 - 注释,不支持子节点,继承自Text类型
通过 nodeValue 或 data 属性访问文本
浏览器不会识别</html>标签后面的注释


6 CDATASection类型 - 只针对基于XML的文档,表示 CDATA 区域. 继承自Text类型

7 DocumentFragment 类型 - 不能直接添加到文档中,但可以作为一个 仓库 来使用,保存可能会添加到文档中的节点.



  DOM 操作技术
动态脚本
function loadScript(url){
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = url;
    document.body.appendChild(script);
}

loadScript("client.js");

动态样式
function loadStyles(url){
    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.herf = url;
    var head = document.getElementByTagName("head")[0];
    head.appendChild(link);           // 放在head才能保证在所有浏览器中的行为一致
}

loadStyles("styles.css");

还有一种操作方式,是直接传入script和css语句,作为文本方式传入.
style.appendChild(document.createTextNode(script/css));
对ie:  script.text = code;
       style.styleSheet.cssText = css;


操作表格
insertRow()
insertCell()

使用NodeList
"动态的"集合: NodeList, NamedNodeMap, HTMLCollection
因为这些集合是动态变化的,迭代时应使用如下方式:
for(i=0,len=divs.length;i<len;i++){}



第11章  DOM扩展

1 Selection API - 让浏览器原生支持CSS查询
  
由 Documen, ELement, DocumentFragment 调用下面两个方法
  querySelector()方法 - 接受一个CSS选择符
var body = document.querySelection("body");               // 取得标签名为 body 的元素 
var myDiv = document.querySelection("#myDiv");            // 取得 id 为 myDiv 的元素
var selected = document.querySelection(".selected");      // 取得类为 selected 的第一个
var img = document.querySelection("img.button");          // 取得类为button 的第一个 img 元素


  querySelectionAll()方法 - 返回所以匹配的元素,一个 NodeList 的实例(该NodeList不会动态查询,只是一组元素的快照)
var strongs = document.querySelectionAll("p strong");     // 取得所以 <p> 元素中的所有 <strong> 元素

  
  matchesSelector() 方法 - 接受一个CSS选择符 
若果调用元素与该选择符匹配,返回 true; 否则返回 false.  

2 元素遍历
childNodes 会返回元素间的空格作为文本节点. 未解决这个问题,定义了一组属性:
childElementCount: 返回子元素个数(不包括文本节点和注释)
firstElementChild: 
lastElementChild:
previousElementSibling:
nextElementSibling:


3 HTML5
getElementByClassName() - 传入一个包含一个或多个类名的字符串,返回 NodeList(动态).

  操作类名的新方式 - classList属性,极大降低修改多个class的复杂度
classList 是DOMTokenList 的实例,可以使用item()或方括号语法获取每个元素. classList定义了如下方法:
add(): 将给定字符串添加到列表中.若值存在,就不添加了.
contains(): 是否存在
remove(): 删除
toggle(): 若存在,删除;若没有,则添加

<div class="bd user disabled">...</div>

div.classList.remove("user");


4 焦点管理
document.activeElement 属性 - 始终会引用 DOM 中当前获得了焦点的元素
元素获得焦点的方式: 页面加载(加载完时,保存document.body),用户输入(通常是按Tab键),代码中调用focus()方法

document.hasFocus() 通过检测文档是否获得了焦点,可以知道用户是不是正在与页面交互

无障碍web应用的一个主要标志就是恰当的焦点管理.


5 HTMLDocument
  document.readyState 属性, 通过它来实现一个指示文档已经加载完成的指示器. 该属性有两个值:
loading: 正在加载文档
complete: 已加载完文档

  兼容模式
  document.compatMode
CSS1Compat  标准模式
BackCompat  混杂模式

  document.head 来引用 <head> 元素
  

6 字符集属性
document.charset  文档实际使用的字符集
document.defaultCharset     默认浏览器及操作系统的设置字符集


7 自定义数据属性 - 前缀需加 data-
添加了自定义属性后,可以通过元素的 dataset 属性来访问自定义属性的值. dataset 属性的值是 DOMStringMap 的一个实例,即名值对的映射.
<div id="myDiv" data-appId="123"></div>

var div = document.getElementById("myDiv");
var appId = div.dataset.appId;

可用来给元素添加一些不可见的数据以便进行其他处理.


7 插入标记 - 直接插入 HTML 字符串
  innerHTML - 大部分插入的<script>元素不会执行其中的脚本
读模式下, innerHTML 属性返回与调用元素的所有子节点(包括元素,注释和文本节点)对应的HTML标记. - 不同浏览器会返回不完全相同的值
写模式下, innerHTML 会根据指定的值创建新的 DOM 树,然后用此DOM 树完全替换调用元素原先的所有子节点.

<col>,<colgroup>,<frameset>,<head>,<html>,<style>,<table>,<tbody>,<thead>,<tfoot>,<tr> 不支持 innerHTML.

  outerHTML - 其他的与 innerHTML 相同. 不过读取和写入时,会同时读取和替换调用元素本身

<div id="myDiv">abc</div>

var div = document.getElementById("myDiv");
alert(div.innerHTML);                               // abc
alert(div.outerHTML);                               // <div id="myDiv">abc</div>
div.innerHTML = "<p>Hello</p>";                     // div 变为 <div id="myDiv"><p>Hello</p></div>
div.outerHTML = "<p>Hello</p>";                     // div 变为 <p>Hello</p>

  
  insertAdjacentHTML() 方法 - 接受两个参数: 插入位置, 要插入的HTML文本
第一个参数有以下几个值:
beforebegin: 当前元素之前插入一个紧邻的同辈元素
afterbegin: 当前元素之下插入一个新的子元素,或在第一个子元素之前插入 
beforeend: 当前元素之下 或最后一个子元素之后
afterend: 当前元素之后

  内存
上述三种方法,可能导致浏览器内存占用问题. (与元素的事件处理管理的关系没有被删除)因此,最好手工删除要被替换的元素的所有事件处理程序和JS对象属性.

  性能
设置 innerHTML 和 outerHTML 时会创建 HTML 解析器.因此不宜频繁调用,最好准备好html语句,一次操作.  
  

8 以下两个方法作用对象是元素的容器. 
  scrollIntoView()方法 - 参数文件true或不填,会让调用元素顶部与视口顶部尽可能平齐. false,底部与视口顶部平齐.
  可在所有HTML元素上调用

  scrollIntoViewIfNeeded() 只有当前元素在视口中不可见的情况下,才滚动浏览器窗口或容器元素,最终让它可见
  参数为 true 时,表示尽量将元素显示在视口中部(垂直方向).
  
  以下两个方法影响的则是元素本身
  scrollByLines()   将元素内容滚动指定的行高
  scrollByPages()   将元素的内容滚动指定的页面高度
  
  
9 children属性 - 只包含元素中同样还是元素的子节点

10 contain() 获取节点是否为元素的后代信息
   compareDocumentPosition() 也能确定节点间的关系. 返回一个表示节点关系的掩码.
1   无关
2   给定节点在DOM树种位于参考节点之前
4   给定节点在DOM树种位于参考节点之后
8   包含
16  被包含

var result = document.documentElement.compareDocumentPosition(document.body);     
// result 为 20. body被html包含(16),位于其之后(4). 共20
alert(!!(result & 16)); 
// !! 会将数值转换为 布尔值, 这句话用来确定是否包含16,若包含,则是子节点,返回true





第 12 章 DOM2 DOM3
1 检查是否支持DOM扩展模块
var supportsDOM2Core = document.implementation.hasFeature("Core","2.0");

2 针对 XML  命名空间 - 不同XML文档的元素可以混合在一起
HTML不支持XML命名空间, 但 XHTML 支持.
对于混合使用两种语言的情况下,命名空间用处就很大.
<html xmlns="http://www.w3.org/1999/xhtml">                                     // xhtml 命名空间
    <head>
        <title>Example</title>
    </head>
    <body>
        <svg xmlns="http://www.w3.org/2000/svg" version="1.1"                  // svg 命名空间
            viewBox="0 0 100 100" style="width:100%; height:100%">
            <rect x="0" y="0" width="100" height="100" style="fill:red"/>
        </svg>
    </body>
</html>    

3 document.defaultView 指向拥有给定文档的窗口(或框架)的指针

4 DOM3 针对为DOM节点添加额外数据引入了方法: setUserData()
var div = document.createElement("div");
div.setUserData("name", "Nicholas", function(operation,key,value,src,dest){
// function 的5个参数: 操作类型(1表示复制,2表示导入,3表示删除.4表示重命名), 数据键, 数据值,源节点, 目标节点
    if(operation == 1){
        dest.setUserData(key,value,function(){};
    }
});

5 框架用 HTMLFrameElement 表示, 内嵌框架用 HTMLIFrameElement. 它们的属性 contentDocument 指向框架内容的文档对象.
访问框架或内嵌框架的文档对象要受到跨域安全策略的限制,若框架中页面来自其他域或不同子域,或使用了不同协议,那访问该框架文档对象就会出错.


6 style特性
对于使用短划线的CSS属性名,必须将其转换成驼峰大小写形式,才能通过Javascript访问. (如 background-image    style.backgroundImage)
myDiv.style.backgroundColor = "red";

style 的属性和方法:
cssText: 访问和修改style特性中的整体CSS代码
length: CSS属性数量, 遍历时使用,使用item(i)或[i]返回 css元素的属性名
getPropertyValue(): 取得特定Css属性的值
getPropertyCSSValue(): 含两个属性, cssText为属性值, cssValueType 表示值的类型(0表示继承的值,1表示基本的值,2值列表,3自定义的值)
removeProperty()
setProperty(propertyName, value, priorty): priorty可为 important 或一个空字符串

计算的样式: document.defaultView.getComputedStyle() 
接受两个参数: 要取得计算样式的元素, 伪元素字符串(可以为null)
返回一个CSSStyleDeclaration 对象,这个方法返回的对象,是包含其他样式表层叠而来并影响当前元素样式信息的

ie不支持该方法,不过有一个 currentStyle属性,包含当前元素全部计算后的样式.
style 中, border 是个综合属性(border-left-width, border-top-color等), 不一定能正确读取.


7 操作样式表 - CSSStyleSheet 类型(包括<link> 和 <style>)
通过 document.styleSheets[i] 来访问
或直接用元素的属性  element.sheet(对ie则是  element.styleSheet)

  css规则
CSSRule 对象表示样式表中的每一条规则,是一个供其他多种类型继承的基类型,其中最常见的就是 CSSStyleRule 类型.
CSSStyleRule 对象包含下列属性:
cssText: 返回整条规则对应的文本
selecotrText: 返回当前规则的选择符文本.
style: 一个CSSStyleDeclaration 对象,可通过它设置和取得规则中特定的样式值.

div.box{
    background-color: blue;
    width: 100px;
    height: 20px;
}

var sheet = document.styleSheets[0];
var rules = sheets.cssRules || sheet.rules;
var rule = rules[0];                  // 取得第一条规则,即div.box的规则
alert(rule.selectorText);             // div.box
alert(rule.style.backgroundColor);    // blue

  创建规则  insertRule()
sheet.insertRule("body{background-color:silver}",0);      // 第二个参数是插入规则的位置



8  元素大小
  偏移量
  














