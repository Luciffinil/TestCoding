第13章  事件
``` JavaScript
1 事件流

事件冒泡: 事件开始时由最具体的元素接受,逐级向上传播到较为不具体的节点.(常用,最大限度兼容各种浏览器)
事件捕获: 不太具体的节点更早接收到事件,而最具体的节点最后接收到事件.
DOM事件流: 事件捕获阶段, 处于目标阶段, 事件冒泡阶段

2 事件处理程序
input  onclick 事件
缺点:时差问题. 可能事件的函数没有被解析就被触发. 可以使用try-catch使错误不浮出水面. onclick="try{showMessage();}catch(ex){}"
    扩展事件处理程序的作用域链在不同浏览器中会导致不同结果.
    Html 与 Javascript 代码紧密耦合.   可以使用Javascript指定事件处理程序来代替.

3 DOM0级事件处理程序 - 事件在元素的作用域中运行
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("click");
};    

如果这些代码在页面中位于按钮后面,可能一段时间内点击无反应.

4 DOM2级事件处理程序 - 事件在元素的作用域中运行
addEventListener()          处理指定事件处理程序
removeEventListener()       删除事件处理程序
都接受三个参数: 要处理的事件名,作为事件处理程序的函数,一个布尔值(true表示捕获阶段调用事件处理程序,false,表示冒泡阶段调用事件处理)
btn.addEventListener("click", function(){
    alert(this.id);
}, false);          // 以此种方法创建, remove时无法移除内部的匿名函数,因为实际上传入remove中的匿名函数与此处不是同一个函数

可以将第二个参数函数定义出来
var handler = function(){
    alert(this.id);
};
btn.addEventListener("click", handler, false);
btn.removeEventListener("click", handler, false);

可以添加多个事件处理程序,事件以添加的顺序执行

5 ie事件处理程序 attachEvent()  detachEvent() - 事件冒泡 - 事件在全局作用域中运行  this为 window
接受两个参数: 事件处理程序名称, 函数
btn.attachEvent("onclick", function(){
    alert(this.id);
}); 

可以添加多个事件处理程序,事件以添加的顺序 逆向 执行

6 DOM事件对象
触发 dom 上的某个事件时,会产生一个事件对象event: 包含事件的元素,事件的类型,其他与特定事件相关的信息
属性和方法:
bubbles  是否冒泡
cancelable  是否可取消事件的默认行为
currentTarget   当前正在处理事件的那个元素
type    被触发事件的类型

preventDefault()    取消事件的默认行为,在cancelable为true时可以使用
stopImmediatePropagation()  取消事件进一步捕获或冒泡,同时阻止任何事件处理程序被调用
stopPropagation()   取消事件进一步捕获或冒泡,只有bubbles为true可以使用

type 可用于一个函数处理多个事件
var handler = function(event){
    switch(event.type){
        case "click"
            alert("click");
            break;
            
        case "mouseover"
            event.target.style.backgroundColor = "red";
            break;
            
        case "mouseout"
            event.target.style.backgroundColor = "";
            break;
    }
}

btn.onclick = handler;
btn.onmouseover = handler;
btn.onmouseout = handler;


7  ie中的事件对象  window.event
cancelBubble    默认false,设为true则取消事件冒泡
returnValue     默认true,设为false取消事件默认行为


8 UI事件 Uer Interface  当用户与页面上的元素交互时触发

    load事件 - 当页面完全加载后(包括所有图像,JavaScript文件,CSS文件等外部资源),就会触发window上的load事件.
可以通过Javascript来指定事件处理程序的方式
也可以为 body 元素添加一个 onload 特性
<body onload="alert('Loaded!')">

图像<img>上也可以触发load事件

ie9+,firefox,opera,chrome 支持 <script> 的load 事件

ie,opera 还支持 <link> 元素上的 load 事件.

load事件应该在 src, herf 属性赋值之前定义, 否则会直接开始下载资源.


    unload事件 - 文档被完全卸载后触发(从一个页面切换到另一个页面)
常用于清除引用,以避免内存泄漏.
在 window 上触发 unload事件 或
<body onunload="alert('Unloaded')">

生成的event只包含target属性,值为 document.
注意触发时,页面上所有对象都不存在了,调用的话会出错.

    
    
    resize事件 - 浏览器窗口被调整到一个新的高度或宽度时,会触发resize事件,在window上触发.
- 生成的event只包含target属性,值为 document.   
- firefox只会在用户停止调整窗口大小时才会触发resize事件, 其他的会在浏览器窗口变化了1像素时就触发.

    scroll事件 - 在window对象上发生, 实际表示的则是页面中相应元素的变化.
- 在文档滚动期间重复被触发,应尽量保持事件处理程序的代码简单

9 焦点事件 - 获得或失去焦点时触发
blur: 元素获得焦点时触发(该事件不会冒泡)
DOMFocusIn: 元素获得焦点时触发,仅opera支持(冒泡)
DOMFocusOut: 元素失去焦点时触发,仅opera支持(冒泡)
focus: 元素获得焦点时触发(该事件不会冒泡)
focusin: 元素获得焦点时触发(冒泡) ie5.5+,opera11.5+,chrome
focusour: 元素失去焦点时触发(冒泡) ie5.5+,opera11.5+,chrome

当焦点从一个元素移动到另一个元素,会依次触发:
focusout,focusin,blur,DOMFocusOut,focus,DOMFocusIn

10 鼠标与滚轮事件
DOM3级定义的9个鼠标事件:
click:  单击鼠标左键或按下回车键
dblclick: 双击鼠标左键
mousedown: 按下任意鼠标按钮
mouseenter: 光标从元素外部首次移动到元素范围内(不冒泡)
mouseleave:  光标移动到元素范围之外(不冒泡)
mousemove: 鼠标指针在元素内部移动时重复触发
mouseout:鼠标指针位于一个元素上方,然后用户移动至另一个元素时触发
mouseover: 鼠标指针位于一个元素外部,然后用户首次将其移入另一个元素边界之内
mouseup: 释放鼠标按钮

只有在同一个元素上相继触发 mousedown和 mouseup,才会触发 click 事件.

    鼠标位置
客户区(视口): event.clientX     event.clientY
页面坐标(浏览器):  event.pageX     event.pageY
屏幕坐标(桌面): event.screenX     event.screenY

    修改键
event.shiftKey
event.ctrlKey
event.altKey
event.metaKey   windows中为windows键, mac中为 cmd键
被按下时,返回true

    相关元素  
mouseover 事件主目标:获得光标的元素; 相关元素:失去光标的元素
mouseout 事件主目标:失去光标的元素; 相关元素:获得光标的元素

event.relatedTarget

    鼠标按钮
对于mousedown和mouseup事件来说, event.button 表示按下或释放的按钮(0表示主鼠标按钮,一般为左键,1表示滚轮,2表示次鼠标按钮,一般为右键).

    鼠标滚轮事件 mousewheel
该事件可以在任何元素上触发,最终冒泡到window对象.
event.wheelDelta 用户向前滚动鼠标滚轮时, wheelDelta是120的倍数; 向后滚动滚轮时, wheelDelta 是 -120 的倍数.

Firefox支持 DOMMouseScroll 的类似事件, 滚轮信息保存在 detail 属性中. 向前是-3倍数,向后是3的倍数


11 键盘与文本事件
keydown: 按下键盘上的任意键时触发. 按住不放,重复触发
keypress: 按下字符键或esc触发.按住不放,重复触发
keyup: 用户释放键盘上的键时触发

textInput: 文本事件,对 keypress的补充

按一下键盘上的字符键,首先触发keydown,然后是keypress,最后触发keyup. 前两个触发在文本框发生变化之前,后一个发生在文本框变化之后.

    键码
event.keyCode 与键盘上的一个特定的键对应(对数字字符字母键,keyCode值与ascii码中小写字母或数字的编码相同)  

DOM3级变化 - 新属性
key  值为相应的文本字符. 非字符键时, key的值是相应键的名(如:"Shift")
char 与key相同,不过按下非字符键时值为null

    textInput - 可编辑区域中输入实际字符时触发 
    
    
12 复合事件 - 处理IME的输入序列(Input Method Editor,输入法编辑器. 通常需按住多个键,最终只输入一个字符)


13 变动事件 - Dom中某一部分发生变化时给出提示
    删除节点
removeChild()  replaceChild()  触发  DOMNodeRomoved 事件(会冒泡)  - 触发时,节点尚未删除

event.target   被删除的节点
event.relatedNode  目标节点父节点的引用
事件发生顺序:
在目标节点上触发 DOMNodeRomoved
在目标节点上触发 DOMNodeRomovedFromDocument(不冒泡)
在目标节点的子节点上触发 DOMNodeRomovedFromDocument(不冒泡)
在目标节点的父节点上触发 DOMSubtreeModified

    插入节点
appendChild()  replaceChild() 或 insertBefore()  触发 DOMNodeInserted 事件(会冒泡) - 触发时,节点已插入

事件发生顺序:
在目标节点上触发 DOMNodeInserted
在目标节点上触发 DOMNodeInsertedIntoDocument(不冒泡)
在目标节点的父节点上触发 DOMSubtreeModified


14 HTML5事件
    contextmenu事件
    













