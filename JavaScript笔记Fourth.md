第13章  事件

1 事件流

事件冒泡: 事件开始时由最具体的元素接受,逐级向上传播到较为不具体的节点.(常用)
事件捕获: 不太具体的节点更早接收到事件,而最具体的节点最后接收到事件.
DOM事件流: 事件捕获阶段, 处于目标阶段, 事件冒泡阶段

2 事件处理程序
input  onclick 事件
缺点:时差问题. 可能事件的函数没有被解析就被触发. 可以使用try-catch使错误不浮出水面. onclick="try{showMessage();}catch(ex){}"
    扩展事件处理程序的作用域链在不同浏览器中会导致不同结果.
    Html 与 Javascript 代码紧密耦合.   可以使用Javascript指定事件处理程序来代替.

3 DOM0级事件处理程序
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("click");
};    

如果这些代码在页面中位于按钮后面,可能一段时间内点击无反应.

4 DOM2级事件处理程序
