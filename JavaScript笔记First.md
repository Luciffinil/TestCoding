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
   alert(null == undefined);        // true
   alert(null === undefined);       // false
   
13 ECMAScript 中不存在块级作用域,循环内部定义的变量也可以在外部访问到.   

14 label 语句
   例:   
   var num = 0;
   
   outermost:                                // label 标记
   for(var i=0;i<10;i++){
         for(var j=0;j<10;j++){
             if(i == 5 && j == 5){
                  continue outermost;
             }
             num++;
         }
   }
   
   alert(num);                               // 95
   
15 switch 语句中可以使用任何数据类型. case的值不一定是常量, 可以使变量, 甚至是表达式.

16 函数定义时不必指定是否返回值, return 之后的语句不会被执行.
   ECMAScript 不介意传递进来多少参数,也不在乎参数是什么数据类型. - 实际上,函数体内通过 arguments 对象来访问这个参数数组,从而获取每一个传递过来的参数. 但是 arguments 对象只是与数组类似,并不是 Array 的实例. 可以通过 arguments[0] 访问第一个参数, arguments[1] 访问第二个参数,可以通过length 来确定传递进来的参数个数. 因此,参数的命名非必需.
   严格模式下, 无法对 arguments 对象赋值.
   函数没有重载, 同名函数取最后定义的版本. 但可以利用传入不同参数,实现不同功能. 通过 arguments.length 来区别.
   
第四章
1 JavaScript 不允许之间访问内存中的位置, 即不能直接操作对象的内存空间. 在操作对象时, 实际上是在操作对象的引用而不是实际的对象.
  参数传递  基本类型按值传递,引用类型按共享传递.
  在局部作用域中修改对象会在全局作用域中反映出来,可能是按引用传递,也可能是按共享传递.
  
2 instanceof 根据原型链来识别

3 web浏览器中,全局执行环境被认为是 window 对象.

4 执行环境(execution context), 定义了变量或函数有权访问的其他数据, 决定了它们各自的行为. 
  每个执行环境都有一个与之关联的变量对象(variable object),环境中定义的所有变量和函数都保存在这个对象中.
  当执行流进入一个函数时, 函数的环境就会被推入一个环境栈中. 而在函数执行之后,栈将其环境弹出,把控制权返回给之前的执行环境.
  当代码在一个环境中执行时, 会创建变量对象的一个作用域链(scope chain), 用来保证对执行环境有权访问的所有变量和函数的有效访问. 作用域前端始终是当前执行的代码所在环境的变量对象. 下一个变量对象来自包含(外部)环境, 一直延续到全局执行环境.
  标识符解析是沿着作用域链一级一级地搜素标识符的过程, 从当前环境向上直到全局环境.
  
5 作用域链的延长:
         try-catch 语句的 catch 块
         with 语句

6 JavaScript  没有块级作用域, 会将块级作用域中的 var 声明的变量添加到最接近的执行环境中. 
  初始化变量时没有使用 var 声明, 该变量会自动被添加到全局环境. - 严格模式下导致错误
  
7 垃圾收集:   标记清楚(mark-and-sweep)     - 常用方法
             引用计数(reference counting) - 循环引用 会导致引用计数无法回收垃圾
  循环引用:   function problem(){
                  var objectA = new Object();
                  var objectB = new Object();
                  
                  objectA.one = object B;
                  objectB.two = object A;
             }

8 BOM, DOM 中的对象是使用 C++ 以 COM(Component Object Model, 组件对象模型) 对象的形式实现的, 而 COM 对象的垃圾收集机制就是引用计数策略.因此,JavaScript 访问的 COM 对象依然是基于引用计数策略. IE9 把 BOM 和 Dom 对象转换成真正的 JavaScript 对象.

9  解除引用(dereferencing)  将其值设为 null   -  不仅有助于消除循环引用,也对垃圾收集有好处

第五章
1 创建 Object 实例方式:
  第一种 使用 new 操作符, 后跟 Object 构造函数.
  var person = new Object();    // 与 var person = {}; 相同
  person.name = "Nicholas";
  person.age = 29;
  
  第二种 使用 对象字面量 表示法.
  var person = {         // 这是一个表达式上下文
      name : "Nicholas";
      age : 29
  };
  
  传入参数时,最好的做法是对必需值使用命名参数,对多个可选参数使用字面量来封装.
  
2 访问对象属性, 可以使用电表示法, JavaScript 也可以使用方括号表示法来访问.
  alert(person.name);                // alert(person["name"];
  方括号法主要优点是可以通过变量来访问属性:
  var proName = "name";
  alert(person[proName]);
  若属性名中包含会导致语法错误的字符,或者属性名使用的是关键字或保留字,也可以使用方括号表示法.
  person["first name"] = "Nicholas";
  
3 Array 数组的每一项可以保存任何类型的数据, 即每一个位置可保存不同类型数据.
  创建数组基本方式:
  第一种 Array 构造函数.
  var colors = new Array();
  var colors = new Array("red", "blue", "green");
  第二种 数组字面量
  var colors = [];
  var colors = ["red", "blue", "green"];
  
  length 属性的设置,可以从数组末尾移除项或向数组中添加新项.
  var colors = ["red", "blue", "green"];
  colors.length = 2;
  alert(colors[2]);                          // undefined
  
  var colors = ["red", "blue", "green"];
  colors[colors.length] = "black";           // 在第四个位置添加 black
  
  var colors = ["red", "blue", "green"];
  colors[99] = "black" ;
  alert(colors.length);                      // 100
  
  Array.isArray(value) 检测 value 是否是数组
  
4 转换方法
  var colors = ["red", "green", "blue"];
  alert(colors);                 // red,green,blue  调用 toString() 方法  
  alert(colors.join("||");       // red||green||blue   不传入参数时,以 , 分隔 
  若某一项的值是 null 或 undefined, 那么该值在上面的方法中以空字符串表示.




   
