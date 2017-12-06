第一章  
```
1 文档对象模型(DOM, Document Object Model), 是针对XML但经过扩展用于HTML的应用程序编程接口 - 提供访问和操作网页内容的方法和接口。  
DOM 把整个页面映射为一个多层节点结构。  
DOM1级： 映射文档结构  
DOM2级： CSS支持，DOM核心支持XML命名空间  
         DOM视图，DOM事件，DOM样式，DOM遍历和范围  
DOM3级： DOM加载和保存，DOM验证，扩展DOM核心——支持XML1.0规范  

2 浏览器对象模型（BOM，Browser Object Model）： 处理浏览器窗口和框架，以及针对浏览器的JavaScript扩展 - 提供与浏览器交互的方法和接口。  
  扩展包括： - 弹出新浏览器窗口的功能  
            - 移动,缩放和关闭浏览器窗口的功能  
            - 提供浏览器详细信息的navigator对象  
            - 提供浏览器所加载页面的详细信息的location对象  
            - 提供用户显示器分辨率详细信息的screen对象  
            - 对cookie的支持  
            - 向 XMLHTTPRequest 和 IE 的 ActiveXObject 这样的自定义对象  
            

第二章  
1 JavaScript 引用放在 <body> 元素中内容的后面,避免先加载 Js 时, 页面完全空白, 加载出现明显延迟.  

2 XHTML 代码规则比 HTML 严格得多,因此可以使用 CDate 来包含 JavaScript 代码.  
```
```js
<script>
//<![CDATA[
    function compare(){
    }
//]]>
</script>
```
  使用注释,是因为有些浏览器不兼容 XHTML, 因此不支持 CData 片段.
  
3 标准模式
```html
<!-- HTML 5-->
<!DOCUTYPE html>
```
