```JavaScript
每创建一个函数,就会同时创建它的prototype对象,这个对象也会自动获得constructor属性.

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
```
```Javascript
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
  function Person(){
  }

  person.prototype.name = "Nicholas";
  person.prototype.age = 29;
  person.prototype.job = "Software Engineer";
  person.prototype.sayName = function(){
      alert(this.name);
  };
  var person1 = new Person;
  person1.sayName();             // "Nicholas"

  var person2 = new Person;
  person2.sayName();             // "Nicholas"

  alert(person1.sayName == person2.sayName);        //true

  上图展示了 Person构造函数, Person.prototype 原型模型 以及 person 实例之间的关系
  Person.prototype指向原型对象, 而 Person.prototype.constructor 又指回 构造函数. 实例与构造函数没有直接关系.
  使用 Object.getPrototypOf()可以获得实例中[[Prototype]]的值.
  
  每当代码读取某个对象的某个属性时, 都会执行一次搜索. 首先从对象实例本身开始搜索, 如果没找到,再去搜索指针指向的原型对象.
  给实例中添加属性,若该属性与实例原型中的一个属性同名,则该属性会屏蔽原型中的属性.不过不会对原型的值进行更改.
  person1.hasOwnProperty("name") : 只有在实例中存在 name 属性时,返回 true
  "name" in person1   只要能访问到 name 属性,无论在实例中还是原型中, 返回 true
  
  function hasPrototypeProperty(object, name){    // 返回true说明属性为原型属性
      return !object.hasOwnProperty(name) && (name in object);
  }
  
  for-in 循环,返回的 是所以能通过对象访问的,可枚举的(enumerated)属性,既包括实例中的属性,也包括原型中的属性 (所有开发人员定义的属性都是可枚举的)
  for(var prop in object){
      do something;
  }
  不可枚举的方法和属性包括: hasOwnProperty(), toString(), constructor, prototype等.
  取得所有可枚举的实例属性,可以使用 Object.keys(Person.prototype)方法.
  取得所有实例属性,不论是否可枚举,可以使用 Object.getOwnPropertyNames(Person.prototype)方法
  
  更简单的原型语法
  function Person(){
  }

  Person.prototype = {
      name: "Nicholas",
      age: 29,
      job: "Software Engineer",
      sayName: function(){
          alert(this.name);
      }
  };
  不过这样的话,constructor属性不再指向Person,而是指向Object.
  因此,可以设置constructor的值.
  Person.prototype = {
      constructor: Person
      name: "Nicholas",
      age: 29,
      job: "Software Engineer",
      sayName: function(){
          alert(this.name);
      }
  };
  这会导致 constructor属性的[[Enumerable]]属性被设置为true,因此可以使用Object.defineProperty().
  function Person(){
  }

  Person.prototype = {
      name: "Nicholas",
      age: 29,
      job: "Software Engineer",
      sayName: function(){
          alert(this.name);
      }
  };
  
  Object.defineProperty(Person.prototype, "constructor",{
      enumerable: false,
      value: Person
  });
  
  
  原型的动态性
  实例化后,再向原型中添加属性方法,依然立即可以在实例中反映出来. 但是如果是重写整个原型对象, 该原型对象实际上就不再是实例对象所指向的原型了.
  
  原型对象的问题
  function Person(){
  }
  
  Person.prototype = {
      constructor: Person,
      friends: ["a","b"],
      age: 28,
      name: "Nicholas"
  };
  
  var person1 = new Person();
  var person2 = new Person();
  person1.friends.push("c");      // 修改的 friends 属性存在于原型中,而不是存在于 person1 实例中
  alert(person1.friends);         // "a,b,c"
  alert(person2.friends);         // "a,b,c"
  
  组合使用构造函数模式和原型模式: 构造函数用于定义实例属性, 原型模式用于定义方法和共享的属性.
  
  动态原型模式
  function Person(name, age, job){
      this.name = name;
      this.age = age;
      this.job = job;
      
      if(typeof this.sayName != "function"){
          Person.prototype.sayName = function(){
              alert(this.name);
          };
      }
  }
  
  寄生构造函数模式(不推荐) - 可以在特殊情况下用来为对象创建构造函数 - 工厂模式套用构造函数方法
  function SpecialArray(){
      var values = new Array();
      values.push.apply(values, arguments);
      values.toPipedString = function(){
          return this.join("|");
      }
      return values;
  }
  var colors = new SpecialArray("red","blue","greed");
  alert(colors.toPipedString());                // "red|blue|green"
  
  稳妥构造函数模式 - 不把属性挂在返回的对象属性下,只通过方法显示
  稳妥对象: 没有公共属性,而且其方法也不引用this的对象.
  function Person(name, age, job){
      var o = new Object();
      
      o.sayName = function(){
          alert(name);
      }
      return o;
  }
  var friend = Person("Nic", 29, "se");
  friend.sayName();           // "Nic"
  friend.name;                // undefined
 
  
3 继承  - 原型对象等于另一个类型的实例
  接口继承: 继承方法签名(ECMAScript的函数没有签名,无法实现接口继承)
  实现继承: 继承实现方法(ECMAScript的实现继承主要依靠原型链实现的)
  
  function SuperType(){
      this.property = true;
  }
  
  SuperType.prototype.getSuperValue = function(){
      return this.property;
  }
  
  function SubType(){
      this.subproperty = false;
  }
  
  SubType.prototype = new SuperType();
  
  SubType.prototype.getSubValue = function(){
      return this.subproperty;
  }
  
  var instance = new SubType();
  alert(instance.getSuperValue());                    // true
  
```
![继承关系](http://images.cnitblog.com/blog/362290/201305/21215810-0c113df43d1948a28f2a0535fd974e57.png)
  
```JavaScript
  isPrototypeOf() 是否为某个特定原型
 
  重写超类中方法或添加某个方法,一定要放在替换原型的语句之后(即继承关系声明之后).
 
 
  原型链的问题 - (1)引用类型值的原型, 原来的实例属性会变成现在的原型属性
               (2)无法再不影响所有对象实例的情况下,给超类的构造函数传递参数
  
  借用构造函数(伪造对象,经典继承) - 在子类型构造函数的内部调用超类型构造函数
  function SuperType(){
      this.color = {"red"};
  }
  
  function SubType(){
      // 继承SuperType
      SuperType.call(this);
  }
  
  var instance1 = new SubType();
  instance1.colors.push("black");
  alert(instance1.colors);          // "red,black"
  
  var instance2 = new SubType();
  alert(instance2.colors);          // "red"
  
  同时,call()还可以直接传递参数给超类. 
  
  
  组合继承(伪经典继承) - 原型链实现对原型属性和方法的继承,借用构造函数来实现对实例属性的继承.
  function SuperType(name){
      this.name = name;
      this.color = {"red"};
  }
  
  SuperType.prototype.sayName = function(){
      alert(this.name);
  }
  
  function SubType(name, age){
      // 继承属性
      SuperType.call(this, name);          //第二次调用 SuperType
      this.age = age; 
  }
  
  // 继承方法
  SubType.prototype = new SuperType();     //第一次调用 SuperType
  
  SubType.prototype.sayAge = function(){
      alert(this.age);
  }
  
  var instance1 = new SubType("Nicholas", 29);
  instance1.colors.push("black");
  alert(instance1.colors);          // "red,black"
  instance1.sayName();              // "Nicholas"
  instance1.sayAge();               // 29
  
  组合继承最大的问题: 无论什么情况下, 都会调用两次超类型构造函数. 一次是在创建子类型原型的时候,一次是在子类型构造函数内部.
  
  寄生式继承 - object() 函数内部,先创建一个临时性构造函数,然后将传入的对象作为这个构造函数的原型,最后返回这个临时类型的一个新实例.
  function createAnother(original){
      var clone = object(original);             // 返回 original 函数的一个副本  
      clone.sayHi = function(){                 // 增强
          alert("hi");  
      };
      return clone;                             // 返回这个对象
  }
  
  寄生式组合继承 - 使用寄生式继承来继承超类型的原型.然后将结果指定给子类型的原型
  function inheritPrototype(subType, superType){
      var prototype = object(superType.prototype);      // 创建对象
      prototype.constructor = subType;                  // 增强对象
      subType.prototype = prototype;                    // 指向对象
  }
  
  function SuperType(name){
      this.name = name;
      this.color = {"red"};
  }
  
  SuperType.prototype.sayName = function(){
      alert(this.name);
  }
  
  function SubType(name, age){
      // 继承属性
      SuperType.call(this, name);         
      this.age = age; 
  }
  
 inheritPrototype(SubType,SuperType);
  
  SubType.prototype.sayAge = function(){
      alert(this.age);
  }
  
  只调用一次 SuperType, 避免了在SubType.prototype上创建多余的属性.
  
  
  
第7章  函数表达式
1 递归
前面 function 部分提到了递归可以使用  arguments.cellee 来解耦合,不过严格模式下无法使用.因此可以通过函数表达式达成效果.
var factorial = (function f(n){
    if (n <= 1){
        return 1;
    } else {
        return n * f(n-1);
    }
});

console.log(factorial(4)); //24 4*3*2*1

这样,即使把函数赋值给另一个变量,函数名字 f 依然有效.

2 闭包 - 有权访问另一个函数作用域中的变量的函数,一般就是在一个函数内部创建另一个函数  
  闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。
  闭包的副作用是闭包只能取得包含函数中任何变量的最后一个值。
  function creFunc(){
      var result = new Array();
      
      for(var i=0;i<10;i++){
          result[i] = function(){
              return i;
          };
      }
      return result;        // 实际返回的数组中,所有的值都是 10. 因为每个函数的作用域链中都保存着creFunc的活动回血,引用同一个变量i
  }
  alert(creFunc()[0]());    // 10     creFunc() 返回一个函数数组,[0]取第一个函数,()表示执行该函数
  
  改造如下可实现预期效果
  function creFunc(){
      var result = new Array();
      
      for(var i=0;i<10;i++){
          result[i] = (function(num){
              return function(){
                  return num;
              })(i);                   //  result[i] = (function()...)(i) 将i作为参数传入
          };
      }
      return result;        
  }
  alert(creFunc()[0]());              // 0    i作为参数按值传入,不再指向i变量
  
  
  
  this 对象 - this 对象指向的是调用该方法的环境
  匿名函数执行环境具有全局性,所以其 this 对象通常指向 window.
  var name = "window";
  var object = {
      name = "object";
      getNameFunc :function(){
          return function(){
              return this.name;
          }
      };
  };
  alert(object.getNameFunc()());           // "window"   getNameFun函数的返回值是匿名函数
  
  
  var name = "window";
  var object = {
      name = "object";
      getNameFunc :function(){
          var that = this;
          return function(){
              return that.name;
          }
      };
  };
  alert(object.getNameFunc()());           // "object"   用 that 记录下 object的环境
  
  
  var name = "window";
  var object = {
      name = "object";
      getNameFunc :function(){
          return this.name;
      };
  };
  alert(object.getNameFunc());                       // "object"
  alert((object.getNameFunc)());                     // "object"    方法加了括号,相当于引用这个函数
  alert((object.getNameFunc = object.getNameFunc)());             // "window"   前面相等操作,返回函数本身,但不包含object环境
  
  
  内存泄漏 - 闭包引用的元素不会被回收
  
  
  
  模仿块级作用域 - 匿名函数
  Js中,块语句中定义的变量,实际上是在包含函数中而非语句中创建的.
  
  用作块级作用域(私有作用域)的匿名函数语法:
  (function(){                // 函数声明包含在一对圆括号中,表示它实际上是一个函数表达式(函数声明转化为函数表达式的方法)
      // 块级作用域
  })();                       // 紧随其后的括号会立即调用这个函数

  这种技术常在全局作用域中被用在函数外部,从而限制向全局作用域中添加过多变量和函数.
  
  
  
  私有变量 - 函数的参数,局部变量,函数内部定义的其他函数
  
  访问私有变量和私有函数的公有方法被为  特权方法. 两种在对象上创建特权方法的方式.
  一是在构造函数中定义. - 每个实例都会创建一组同样的新方法
  function MyObject(){
      var privateVariable = 10;                  // 局部变量,无法直接访问
      function privateFunction(){
          return false;
      }
      
      // 特权方法
      this.publicMethod = function(){
          privateVariable++;
          return privateFunction();
      }
  }
  
  二是静态私有变量 - 通过在私有作用域中定义私有变量或函数.所以私有变量和函数都是由实例共享的.
  (function(){
      var privateVariable = 10;                  
      function privateFunction(){
          return false;
      }
      
      // 构造函数, 不使用var,所以Myobject是一个全局变量
      MyObject = function(){
      };
      
      // 特权方法
      MyObject.prototype.publicMethod = function(){
          privateVariable++;
          return privateFunction();
      };
  })();
  
  
  
  
  模块模式 -  为单例创建私有变量和特权方法
  var singleton = (function(){
  
      var privateVariable = 10;                  
      function privateFunction(){
          return false;
      }
      
      return {
          publicProperty: true,
          publicMethod : function(){
              privateVariable++;
              return privateFunction();
          }    
      };
  })());
  
  模块模式可以创建一个对象并以某些数据对其进行初始化,同时还要公开一些能够访问这些私有数据的方法.
  
  
  增强模块模式  - 返回对象之前加入对其增强的代码,适用于单例必须是某种类型的实例,且必须添加某些属性或方法的情况
  var singleton = (function(){
  
      var privateVariable = 10;                  
      function privateFunction(){
          return false;
      }
      
      var object = new CustomType();
      
      object.publicProperty = true;
      
      object.publicMethod = function(){
              privateVariable++;
              return privateFunction();
      };
      
      return object;
  })());
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
```
 













