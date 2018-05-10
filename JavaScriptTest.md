一个判断页面是否真的关闭和刷新的好方法：  

```JavaScript
window.onbeforeunload=function (){
alert("===onbeforeunload===");
if(event.clientX>document.body.clientWidth && event.clientY < 0 || event.altKey){
     alert("你关闭了浏览器");
}else{
     alert("你正在刷新页面");
}
}
```
这段代码就是判断触发onbeforeunload事件时，鼠标是否点击了关闭按钮，或者按了ALT+F4来关闭网页，如果是，则认为系统是关闭网页，否则在认为系统是刷新网页。


# 1 Ext-js  isContained属性
当一个组件被 added 时, 使用 this.up() 或 this.down() 都不能取得元素,此时可以使用 isContained 属性,获取上一层的元素.
当事件 beforerender 触发后(或更早?), isContained 属性消失,此时可以正常使用 this.up() 或 this.down().
