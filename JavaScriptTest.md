一个判断页面是否真的关闭和刷新的好方法：

window.onbeforeunload=function (){
alert("===onbeforeunload===");
if(event.clientX>document.body.clientWidth && event.clientY < 0 || event.altKey){
     alert("你关闭了浏览器");
}else{
     alert("你正在刷新页面");
}
}
这段代码就是判断触发onbeforeunload事件时，鼠标是否点击了关闭按钮，或者按了ALT+F4来关闭网页，如果是，则认为系统是关闭网页，否则在认为系统是刷新网页。
