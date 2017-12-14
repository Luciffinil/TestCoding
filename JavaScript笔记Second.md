```JavaScript
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
  
  
  转换方法 toString()
  var colors = ["red", "green", "blue"];
  alert(colors);                 // red,green,blue  调用 toString() 方法  
  alert(colors.join("||");       // red||green||blue   不传入参数时,以 , 分隔 
  若某一项的值是 null 或 undefined, 那么该值在上面的方法中以空字符串表示.

  栈方法 push(), pop()
  var colors = new Array();
  var count = colors.push("red", "green");            // push 返回值为数组长度, 数组末尾添加两项
  alert(count);                                       // 2
  
  var item = colors.pop();                            // 取得最后一项, 并从数组中移除      
  alert(item);                                        // "green"
  alert(colors.length);                               // 1

  队列方法 shift(), unshift()
  var colors = new Array();
  var count = colors.push("red", "green");            // push 返回值为数组长度
  alert(count);                                       // 2
  
  var item = colors.shift();                            // 取得第一项, 并从数组中移除      
  alert(item);                                        // "red"
  alert(colors.length);                               // 1
  
  var count = colors.unshift("red", "blue");            // unshift 返回值为数组长度, 数组开头添加两项
  alert(count);                                         // 3

  重排序方法 reverse(), sort()
  var values = [0,1,5,10,15];
  values.reverse();                 // 反转数组项的顺序
  alert(values);                    // 15,10,5,1,0
  values.sort();                    // 按升序排列数组项, 注意sort()方法 无参数时比较的是 字符串 而不是数值
  alert(values);                    // 0,1,10,15,5  字符串 "5" > "10"
  
  function compare(v1, v2){         // 升序的 compare
      if(v1 < v2){
         return -1;
      } else if(v1 > v2){
         return 1;
      } else {
         return 0;
      }
  }
  
  values.sort(compare);
  alert(values);                    // 0,1,5,10,15
  
  对于数值类型或者其 valueOf() 方法会返回数值类型的对象类型,可以使用一个更简单的比较函数.
  function compare(v1, v2){         // 降序的 compare
         return v2 - v1;
  }
  
  操作方法 
  concat() 创建当前数组的一个副本, 并将接受到的参数添加到这个副本的末尾.
  var colors = ["red"];
  var colors2 = colors.concat("yellow", ["black", "brown"]);
  alert(colors);                    // red
  alert(colors2);                   // red,yellow,black,brown
  
  slice() 基于当前数组中的一或多个项创建一个新数组(不影响原始数组). 一个参数表示从该参数到数组末尾所有小,两个参数表示起始和结束位置间的项,不包括结束位置.
  var colors = ["red", "yellow", "black", "brown"];
  var colors2 = colors.slice(2);
  var colors3 = colors.slice(1,3);
  alert(colors2);                    // black,brown
  alert(colors3);                    // yellow,black
  
  splice() 向数组的中部插入项, 始终返回包含 从原始数组中删除的项 的数组(未删除则返回空数组)
  删除: 2个参数, 要删除的第一项位置, 要删除的项数
  插入: 3个参数, 起始位置, 0(要删除的项数), 要插入的项数
  替换: 同插入, 第二个参数不为0, 后两个参数不必相等
  var colors = ["red", "green", "blue"];     
  var removed = colors.splice(0,1);                   // 删除第一项
  alert(colors);                                      // green,blue
  alert(removed);                                     // red
  
  removed = colors.splice(1, 0, "yellow", "orange");  // 从位置 1 开始插入两项
  alert(colors);                                      // green,yellow,orange,blue
  alert(removed);                                     // 空数组[]
  
  removed = colors.splice(1, 1, "red", "black");  // 从位置 1 开始插入两项
  alert(colors);                                      // green,red,black,orange,blue
  alert(removed);                                     // yellow
  
  
  位置方法
  indexOf() 两个参数, 要查找的项, 查找起点位置的索引(可选)   从数组开头向后查找
  lastIndexOf()  同样的两个参数   从数组末尾开始向前查找
  两个方法都返回要查找的项在数组中的位置,没找到返回-1. 比较时,使用全等操作符. 注意返回的是第一个遇到的值.
  
  迭代方法
  共5个方法,每个方法都接受两个参数: 要在每一项上运行的函数, 运行该函数的作用域对象(可选) - 影响 this 的值
  每一项上运行的函数会接收三个参数: 数组项的值, 该项在数组中的位置和数组对象本身.
  every() 对数组中的每一项运行给定函数, 若该函数对每一项都返回 true, 则返回true. - 查询是否所以项都符合某个条件
  filter() 对数组中的每一项运行给定函数, 返回该函数会返回 true 的项组成的数组. - 筛选符合某条件的所有项
  forEach() 对数组中的每一项运行给定函数, 无返回值
  map() 对数组中的每一项运行给定函数, 返回每次函数调用的结果组成的数组  - 创建与传入数组一一对应的另一个数组
  some() 对数组中的每一项运行给定函数, ,如果该函数对任一项返回 true, 则返回 true  - 查询是否有某项符合条件
  var ns = [1,2,3,4,5];
  var mapR = ns.map(function(item, index, array){
      return item*2;
  });
  alert(mapR);            // 2,4,6,8,10
  
  缩小方法 reduce() reduceRight() - 累加器
  array.reduce(callbackfn[, initialVallue]);
  function callbackfn(preValue, curValue, index, array){}
  
4 Date 类型  - 从1970年1月1日零时开始经过的毫秒数来保存日期.
  Date.parse() 接受一个表示日期的字符串参数("6/12/2004" 或 "January 12,2004" ...)
  Date.UTC()  参数为 年份, 从0开始的月份(0-11), 月中的某一天(1-31), 小时数(0-23),分钟,秒以及毫秒数(仅年和月是必需的)
  var now = new Date();       // 当前日期
  var sDate = new Date(Date.parse("2004-05-25T00:00:00"));
  var aDate = new Date(Date.UTC(2005,4,5,17,55,55));
  
  toUTCString() 推荐使用方法
  valueOf()  返回自1970年1月1日的毫秒数
  getDate()  返回月份中的天数
  getDay()  返回星期几
  
5 RegExp 类型 - 正则表达式
  var experssion = / pattern / flags;                   // 字面量形式     
  var exoerssuib = bew RegExp("pattern", "flags");      // 构造函数形式
  pattern 是正则表达式, flags(一或多个标志), 用以表明正则表达式的行为.
  标志有: g 全局模式,即正则表达式应用于所有字符串,而非在发现第一个匹配项时立即停止.  - 但是也只会返回第一个匹配项
         i 不区分大小写,确定匹配项时忽略模式与字符串的大小写
         m 多行模式,即在到达一行文本末尾时还继续查找下一行中是否存在于模式匹配的项
         
  元字符必须转义  ( [ { \ ^ $ | ) ? * + . ] }       
  构造函数形式的参数都是字符串,所以要对字符进行双重转义.
  /\[bc\]at/    "\\[bc\\]at"
  /name\/age/   "name\\/age"
  /\w\\hello\\123/    "\\w\\\\hello\\\\123"
  
  实例方法
  exec() 专门为捕获组而设计的. 返回包含第一个匹配项信息的数组,没有匹配项时返回 null. 返回的数组为 Array 实例,且包含两个额外属性. index, 表示匹配项在字符串中的位置; input, 表示输入的字符串. 数组中, 第一项是与整个模式匹配的字符串, 其他项是与模式中的捕获组匹配的字符串(没有捕获组时,数组仅包含一项).
  var text = "mom and dad and baby";
  var pattern = /mom( and dad( and baby)?)?/gi;

  var matches = pattern.exec(text);
  alert(matches.index);        // 0
  alert(matches.input);        // "mom and dad and baby"
  alert(matches[0]);           // "mom and dad and baby"
  alert(matches[1]);           // " and dad and baby"
  alert(matches[2]);           // " and baby"
  
  () 捕获组
  (?:exp) 定义非捕获组,只参与匹配,不会被保存到数组中
  
  不设置 g 时, 同一字符多次调用 exec() 将始终返回第一个匹配项的信息. 
  设置为 g 时, 将会从上次匹配的位置继续向后查找新匹配项
  
  test() 匹配是返回 true,佛则返回 false
  
  构造函数属性   长属性名  短属性名   说明
  input           $_    最近一次要匹配的字符串
  lastMatch       $&    最近一次的匹配项
  lastParen       S+    最近一次匹配的捕获组
  leftContext     S`    lastMatch之前的文本
  rightContext    S'    lastMatch之后的文本
  multiline       S*    表示是否所以表达式都使用多行模式
  $1,$2...$9    存储第1,第2...第9个匹配的捕获组
  var text = "a short summer";
  var pattern = /(..)or(.)/g;
  if(pattern.test(text)){
      alert(RegExp.$1);       // sh
      alert(RegExp.$1);       // t
  }
  
6 Function  - 函数是对象,函数名是包含指针的变量. 因此没有重载.
  function sum(n1, n2){
      return n1 + n2;
  }
  alert(sum(10,10));        // 20
  var sum2 = sum;           // 不带圆括号的函数名sum表示函数指针,不是调用函数
  alert(sum2(10,10));       // 20 
  sum = null;               
  alert(sum2(10,10);        //20
  
  函数声明形式 - 解析器在执行任何代码前读取
  function sum(n1,n2){
      return n1+n2;
  }
  函数表达式形式  - 执行到时才读取
  var sum = function(n1,n2){
      return n1+n2;
  }
  
  ECMAScript 不仅可以像传递参数一样把一个函数传递给另一个函数,而且可以将一个函数作为另一个函数的结果返回.
  
  function createCompare(proName){
      return function (object1, object2) {
          var v1 = object1[proName];
          var v2 = object2[proName];
          if(v1<v2){
              return -1;
          } else if(v1>v2){
              return 1;
          } else {
              return 0;
          }
      };
  }
  var data = [{name:"z",age:10},{name:"y",age:15}];
  data.sort(createCompare("name"));
  alert(JSON.stringify(data[0]));     // {name:"y",age:15}
  data.sort(createCompare("age"));
  alert(JSON.stringify(data[0]));     // {name:"z",age:10}
  
  本来sort(compare)中的 compare无法接受函数,使用这种方式就可以了.
  
  函数内部两个特殊对象: arguments, this
  arguments 主要用途是保存函数参数, 但这个对象还有一个callee属性. 该属性是一个指针,指向拥有这个arguments对象的函数.
  
  function fac(n){
      if(n<=1){
        return1;
      } else {
        return num*fac(n-1);
      }
  }
  
  这个函数的执行与函数名fac紧紧耦合在一起,可以使用callee解耦.
  
  function fac(n){
      if(n<=1){
        return1;
      } else {
        return num*arguments.callee(n-1);
      }
  }
  
  严格模式下访问 arguments.callee, arguments.caller 都会导致错误.
  
  this 引用函数据以执行的环境对象
  
  
  函数属性
  length   函数希望接受的命名参数个数
  prototype  保存所有方法实例的存在,第六章详细介绍
  
  函数方法
  apply() call() 用于特定的作用域中调用函数 - 能够扩充函数赖以运行的作用域(好处是对象不需要于方法有任何耦合关系)
  apply() 接收两个参数: 运行函数作用域, 参数数组(可以是 Array 实例, 也可是 arguments 对象)
  call() 类似于 apply(), 不过第二个参数必须逐个列举出来.  
  
  window.color = "red";
  var o = {color: "blue"};
  
  fuction sayColor(){
      alert(this.color);      
  }
  
  sayColor();                 // red
  sayColor.call(this);        // red
  sayColor.call(window);      // red
  sayColor.call(o);           // blue

  var oSayColor = sayColor.bind(o);    // 起相同作用的bind()函数
  oSayColor();                // blue


7 基本包装类型 特殊的引用类型 Boolean, Number, String
  每当读取一个基本类型值的时候,后台就会创建一个对应的基本包装类型的对象,从而可以调用一些方法来操作. 但这个包装类型,只存在一行代码执行的瞬间,然后立即被销毁.
  对基本包装类型的实例调用 typeof 会返回 "object", 而所有基本包装类型的对象都会被转换为布尔值 true.
  var falseObject = new Boolean(false);
  var result = falseObject && true;         // falseObject 是基本包装类型,存储的是false, 但本身的值是 
  alert(result);                            // true
  
  把字符串传给 Object 构造函数,就会创建 String 实例. 同理, Number 和 Boolean.
  var obj = new Object("some text");
  alert(obj instanceof String);         // true
  
  Number 类型 toFixed() 传入一个参数, 按指定小数位返回数值的字符串表示(标准时0到20位)
              toExponential() 同样传入一个参数表示小数位数, 以指数表示法(e表示法)返回数值的字符串表示
              toPrecision() 传入一个参数表示所有数字的位数(不包括指数部分) - 1到21位小数
  var num = 99;
  alert(num.toFixed(2));      // "99.00"
  alert(num.toExponential(1));      // "9.9e+1"
  alert(num.toPrecision(1));      // "1e+2"    
  alert(num.toPrecision(2));      // "99"  
  alert(num.toPrecision(3));      // "99.0"  
  
  Stirng
  字符方法   charAt()   charCodeAt()
  字符串操作方法    concat()  slice()  substr()   substring()
  字符串位置方法    indexOf()  lastIndexOf()
  trim()方法
  大小写转换方法    toLowerCase()  toLocaleLowerCase()  toUpperCase()  toLocaleUpperCase()
  模式匹配方法      match()  search()   replace()  split()
  localeCompare()方法
  fromCharCode()方法   与charCodeAt() 执行相反的操作
  
8 单体内置对象
  Global对象: encodeURI() 转义空格等非有效字符   encodeURIComponent()  转义所有字符
              eval()   严格模式下,外部无法访问 eval() 中创建的任何变量或函数
              获取 Global 对象: 作为 window 对象的一部分 或
                               var globa = function(){
                                   return this;
                               }
  Math对象: min(), max()   
           Math.ceil() 向上舍入   Math.floor()  向下舍入   Math.round()  四舍五入
           random()方法 -  返回介于0和1之间的随机数,不包括0,1
           随机取两个数之间的随机数
           function selectFrom(lowValue, upValue){
               var choices = upValue - lowValue + 1;
               return Math.floor(Math.random * choices + lowValue);
           }
  
第六章 面向对象设计
1 数据属性: [[Configurable]] 能否删除,修改特性
           [[Enumerable]]  能否通过 for-in 循环返回属性
           [[Writable]]   能否修改属性的值
           [[Value]]
           前三个值默认为true.
           调用Object.defineProperty()修改特性时,如果不指定,前三个值都为false.
           
  访问器属性: [[Configurable]]
             [[Enumerable]]
             [[Get]]
             [[Set]]
    
  var book = {
                _year: 2004,
                edition: 1
  };
  Object.defineProperty(book, "year", {    // 不能使用 _year, 否则会陷入死循环
      get: function() {
          return this._year;
      },
      set: function(newValue) {
          if (newValue > 2004) {
              this._year = newValue;
              this.edition += newValue - 2004;
          }
      }
  });
  book.year = 2005; //执行了 setter    // 使用定义的 year 属性
  console.log(book.edition);    // 2

  _year 的下划线是常用记号,表示应该通过对象方法访问的属性. 不过直接访问也是允许的.
  不一定同时写 getter 和 setter, 利用这点可以控制属性的读取和写入
  
  定义多个属性   Object.defineProperties()
  var book = {};
  Object.defineProperties(book, {
      _year: {
          value: 2004
      },
      edition: {
          value: 1
      }
      
      year: {
          get: function() {
             return this._year;
          },
          set: function(newValue) {
              if (newValue > 2004) {
                  this._year = newValue;
                  this.edition += newValue - 2004;
              }
          }
      }
  });

  读取属性的特性  Object.getOwnPropertyDescriptor()
    
2 创建对象
  工厂模式: 解决了创建多个相似对象的问题,但没有解决对象识别的问题(即不指定一个对象的类型).
  function createPerson(name, age, job){
      var o = new Object();
      o.name = name;
      o.age = age;
      o.job = job;
      o.sayName = function(){
          alert(this.name);
      };
      return o;
  }
  var person1 = createPerson("Nicholas", 29, "Software Engineer");
  
  构造函数模式:
  function Person(name, age, job){
      this.name = name;
      this.age = age;
      this.job = job;
      this.sayName = function(){    // 相当于 this.sayName = new Function("alert(this.name)");
          alert(this.name);
      };
  }   
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  alert(Person1.constructor == Person);     // true, person1 有一个 constructor(构造函数) 属性,指向Person
  调用构造函数会经历以下四个步骤:
  (1) 创建一个对象
  (2) 将构造函数的作用域赋给新对象
  (3) 执行构造函数中代码
  (4) 返回新对象
  
  
  构造函数的问题: Person内部的sayName()方法,对于每一个Person都是不同的,即不同实例上的同名函数是不相等的. - 这对于某些情况是有用的,但是创建两个完成同样任务的Fucntion实例就没有必要了. 因此,可以把函数定义转移到构造函数外部来解决这个问题.
  function Person(name, age, job){
      this.name = name;
      this.age = age;
      this.job = job;
      this.sayName = sayName;
  }
  function sayName(){    
      alert(this.name);
  }      
  var person1 = new Person("Nicholas", 29, "Software Engineer");
  
  新的问题: 全局作用域的函数只能被某个对象调用. 而且,如果对象需要定义很多方法,那么就要定义多个全局函数,于是这个自定义的引用类型就毫无封装性可言了.所以需要通过 原型模式 来解决.
  
```  
  原型模式
  
  ![Image of Linkage](http://images0.cnblogs.com/blog2015/549190/201506/051327073016752.png)
  
  
```JavaScript



```
 













