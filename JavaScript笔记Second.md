
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
  
