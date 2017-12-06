第一章  
```JavaScript
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

         <script>
         //<![CDATA[
             function compare(){
             }
         //]]>
         </script>

  使用注释,是因为有些浏览器不兼容 XHTML, 因此不支持 CData 片段.
  
3 标准模式
         <!-- HTML 5-->
         <!DOCUTYPE html>
         
第三章
1 标识符第一个字符必须是字母,下划线 _ 或美元符号 $.

2 严格模式

function doSomething(){
    "use strict";
}

3 基本数据类型: Undefined, Null, Boolean, Number, String
  复杂数据类型: Object - 本质上由一组无序的名值对组成
  
4 alert(typeof Null);      // "Object"
         
  var message;
  alert(typeof message);   // "undefined"  未初始化
  alert(typeof age);       // "undefined"  未声明

4 若定义的变量准备在将来用于保存对象,最好将该变量初始化为 null, 而不是其他值.
         
  undefined 值派生自 null 值,因此
  alert(null == undefined);         // true
  
5 Boolean() 转型函数
  Number 转型为 Boolean, 0 和 NaN(not a number) 转化为 false
  Undefined 转型 为 Boolean, n/a 和 N/A (not appplicable) 转为 true, undefined 转化为 false
  
6 Number 支持8进制(前导0)以及16进制(前导0x), 但严格模式下无效.
  
  永远不要测试某个特定的浮点数值 - 基于 IEEE754 数值的浮点计算的通病.
  
  isFinite() 确定一个数值是否有穷
  
  NaN 与任何值都不相等,包括 NaN本身   isNaN()
  
  转型函数 Number() 
  null 转为 0; undefiend 转为 NaN; 字符串中可识别十六进制, 无法识别八进制, 空字符串转为 0; Boolean true 转 1, false 转0.
  
  转型函数 parseInt()
  第一个字符不为数字或负号,返回NaN; 
  parseInt("1234blue");  // 1234   
  parseInt("12.5");      // 12
  parseInt("10", 8);     //  8    第二个参数,表示解析的进制
  
  转型函数 parseFloat()
  仅第一个小数点有效, 只识别十进制.
  parseFloat("12.5.6.7");      // 12.5
  parseFloat("0xAF");          // 0  
  
7 String null 和 undefined 没有 toString() 方法.
  转型函数 String(), null 返回 "null", undefined 返回 "undefined"
  
8 对于位运算，JavaScript仅支持32位整型数.
  左移 <<: 以 0 填充空位,不影响符号位.
  有符号右移 >>: 以符号位值来填充空位.
  无符号右移 >>>: 以 0 填充空位,会使负数变大正数
  
9 对于下面两个布尔操作符, 有一个操作数不是布尔值的情况下,不一定返回布尔值
  逻辑与 && : (短路操作) 第二个操作数为主
  若第一个操作数是对象,或两个操作数都是对象, 返回第二个操作数;
  若第二个操作数是对象,只有在第一个操作数求值结果为 true 情况下才返回该对象
  若有一个操作数为 null, NaN, undefined, 返回 null, NaN, undefined

  逻辑或 || : (短路操作) 第一个操作数为主  - 可用来避免为变量赋值 null 或 undefined
  var myObject = preferredObject || backupObject;
  
  若第一个操作数是对象,或两个操作数都是对象, 返回第一个操作数;
  若第一个操作数求值结果为 false, 则返回第二个操作数
  若两个操作数都为 null, NaN, undefined, 返回 null, NaN, undefined

10 非零的有限数除以 0 ,结果是 Infinity.   ??? ECMAScript 中,任何数值除以0会返 NaN ??? 
   var c = a % b;   // a 无穷大, b 有限大 或 a 有限大, b 为 0 或 a,b 都无限大, 得到 c 为 NaN
   
11 任何操作数与 NaN 比较, 都返回 false.
   字符串之间比较时, 会比较第一个字符的Ascll码值.
   
12 全等(===), 只有两个操作数未经转换就相等的情况下返回 true.
   
