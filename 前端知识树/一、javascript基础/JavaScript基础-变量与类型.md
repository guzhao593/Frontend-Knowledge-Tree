## 一、JavaScript基础

### 变量与类型

1. JavaScript规定了几种语言类型

   分为两大类， 七种语言类型：

   - 基本类型

     String

     Number

     Boolean

     Null

     Undefined

     Symbol

   - 引用类型

     Object

   基本数据类型中有两个为特殊数据类型： `null, undefined` 
   js的常见内置对象：`Date,Array,Math,Number,Boolean,String,Array,RegExp,Function...`

   基本类型的数据是存储在栈内存

   引用类型的数据是存储在堆内存，也就是，变量中保存的实际上的只是一个指针，这个指针指向内存中的另一个位置，该位置保存着对象。访问方式是按引用访问。

2. JavaScript对象的底层数据结构是什么？

   对象的底层数据结构可以通过Object.defineProperty()来设置，通过Object.getOwnPropertyDescriptor(obj，prop)来获取。

   对象的底层数据结构分为两路

   1. 数据属性
      - [[Configurable]]：表示能否通过Delete删除属性，以及除writable属性外的其他特性是否可以修改。默认值 为true
      - [[Enumerable]]：表示是否可以通过For-In循环或Object.keys()返回属性。默认值为true。
      - [[writable]]：表示能否修改属性。默认值为true
      - [[Value]]：包含这个属性的数据值。读取时，从这里取；写入时，把新值保存在这个位置。
   2. 访问器属性
      - [[Configurable]]：表示能否通过Delete删除属性，以及除writable属性外的其他特性是否可以修改。默认值 为true
      - [[Enumerable]]：表示是否可以通过For-In循环或Object.keys()返回属性。默认值为true。
      - [[Get]]：在读取属性时调用的函数。默认值为undefined。
      - [[Set]]：在写入时调用的函数。默认值为undefined。

3. Symbol类型在实际开发中的应用？手动实现一个简单的Symbol？

   Symbol是由ES6规范引入的新特性，它的功能类似于一种标识唯一性的ID。

   通常情况下，我们通过调用Symbol()函数来创建一个Symbol实例，注：不能用new操作符来创建，原因是Symbol（）不是构造函数。

   Symbol类型是基本类型。

   应用场景：

   1. 利用唯一性的特性，可以用于数据对象创建唯一性标识。注：只能用于在js中临时数据中，不要传给后端数据库。原因Symbol类型在JSON.stringify时会过滤掉。
   2. 使用Symbol定义类的私有属性/方法。

4. JavaScript中的变量在内存中的具体存储形式

   基本类型的数据是存储在栈内存

   引用类型的数据是存储在堆内存，也就是，变量中保存的实际上的只是一个指针，这个指针指向内存中的另一个位置，该位置保存着对象。访问方式是按引用访问。

   所以引用类型会存在浅复制和深复制的问题。

   浅复制只是复制了引用类型的指针，所以会出现修改同一指针的引用类型数据其中一个数据时，会同时引用改变其他的数据。

5. 基本类型对应的内置对象，以及它们之间的装箱拆箱操作

   - 装箱

     装箱，就是把基本类型转变为对应的对象。

     装箱分为隐式和显式。

     隐式装箱

     每当读取一个基本类型的值时，后台会创建一个该基本类型所对应的对象。在这个基本类型上调用方法，其实是在这个基本类型对象上调用方法。这个基本类型的对象是临时的，它只存在于方法调用那一行代码执行的瞬间，执行方法后立刻被销毁。代码示例：

     ```javascript
     var num = 123
     num.toFixed(2) // '123.00'
     // 上方代码在后台的真正步骤为
     var c = new Number(123)
     c.toFixed(2)
     c = null
     ```

     当我们访问 num 时，要从内存中读取这个数字的值，此时访问过程处于读取模式。在读取模式中，后台进行了三步处理：

     1. 创建一个 Number 类型的实例。
     2. 在实例上调用方法。
     3. 销毁实例。

     显式装箱

     通过内置对象 Boolean、Object、String 等可以对基本类型进行显示装箱。

     ```javascript
     var num = new Number(123)
     ```

     

   - 拆箱

     拆箱，就是把对象转变为基本类型的值。

     拆箱过程内部调用了抽象操作 ToPrimitive 。

     该操作接受两个参数，第一个参数是要转变的对象，第二个参数  PreferredType 是对象被期待转成的类型。第二个参数不是必须的，默认该参数为 number，即对象被期待转为数字类型。有些操作如 String(obj) 会传入 PreferredType 参数。有些操作如 obj + " " 不会传入 PreferredType。

     具体转换过程是这样的。默认情况下，ToPrimitive 先检查对象是否有 valueOf 方法，如果有则再检查 valueOf 方法是否有基本类型的返回值。如果没有 valueOf 方法或 valueOf 方法没有返回值，则调用 toString 方法。如果 toString 方法也没有返回值，产生 TypeError 错误。

     PreferredType 影响 valueOf 与 toString 的调用顺序。如果 PreferrenType 的值为 string。则先调用 toString ,再调用 valueOf。

     具体测试代码如下：

     ```javascript
     var obj = {
         valueOf : () => {console.log("valueOf"); return []},
         toString : () => {console.log("toString"); return []}
     }
     
     String(obj)
     // toString
     // valueOf
     // Uncaught TypeError: Cannot convert object to primitive value
     
     obj+' '
     //valueOf
     //toString
     // Uncaught TypeError: Cannot convert object to primitive value
     
     Number(obj)
     //valueOf
     //toString
     // Uncaught TypeError: Cannot convert object to primitive value
     ```

     

6. 理解基本类型和引用类型

   基本类型的数据是存储在栈内存

   引用类型的数据是存储在堆内存，也就是，变量中保存的实际上的只是一个指针，这个指针指向内存中的另一个位置，该位置保存着对象。访问方式是按引用访问。

   所以引用类型会存在浅复制和深复制的问题。

   浅复制只是复制了引用类型的指针，所以会出现修改同一指针的引用类型数据其中一个数据时，会同时引用改变其他的数据。

7. null与undefined的区别

   null： Null类型，代表“空值”，代表一个空对象指针，使用typeof运算得到 “object”，所以你可以认为它是一个特殊的对象值。

   undefined： Undefined类型，当一个声明了一个变量未初始化时，得到的就是undefined。

8. 至少可以说出三种判断JavaScript数据类型的方式，以及它们的优缺点，如何准确的判断数组类型

   1. typeof

      ```javascript
      值
      'undefined' - undefined类型
      'boolean' - 布尔类型
      'string' - 字符串类型
      'number' - 对象或null
      'function' - 函数
      'symbol' - symbol类型
      ```

      优点：

      - 可以检测基本类型（除了null）
      - 可以检测函数类型

      缺点：

      - 无法区别null与对象
      - 无法区别引用类型

   2. intanceof

      intanceof用于检测引用类型，可以检测到它是什么类型的实例。

      instanceof 检测一个对象A是不是另一个对象B的实例的原理是：

      查看对象B的prototype指向的对象是否在对象A的[[prototype]]链上。

      如果在，则返回true，否则返回false。特殊情况：当对象B的prototype为null则会报错。

      优点：

      - 可能用于检测引用类型
      - 可以用于检测自定义类型的判断

      缺点：

      - 不能检测基本类型
      - 原型链上可能有好几层，一个实例可能属于多个类型。

   3. constructor

      constructor的检测原理则时利用了对象的contructor属性可以返回创建此对象的构造函数。

      ```javascript
      'xz'.constructor == String // true
      (123).constructor == Number // true
      (true).constructor == Boolean // true
      [1,2].constructor == Array // true
      ({name:'xz'}).constructor == Object // true
      (function(){}).constructor == Function // true
      (new Date()).constructor == Date // true
      (Symbol()).constructor == Symbol // true
      (/xz/).constructor == RegExp // true
      ```

      优点：

      - 可以检测大部分基本类型和引用类型以及自定类型

      缺点：

      - 不能检测Null和Undefined类型

   4. Object.prototype.toString.call()

      原理：调用从Object继承来的原始的toString()方法

      ```javascrip
      Object.prototype.toString.call('xz'); //"[object String]"
      Object.prototype.toString.call(123);  //"[object Number]"
      Object.prototype.toString.call(true); //"[object Boolean]"
      Object.prototype.toString.call([1,2]); //"[object Array]"
      Object.prototype.toString.call({name:'xz'}); //"[object Object]"
      Object.prototype.toString.call(function(){}); //"[object Function]"
      Object.prototype.toString.call(null); //"[object Null]"
      Object.prototype.toString.call(undefined); //"[object Undefined]"
      Object.prototype.toString.call(); //"[object Undefined]"
      Object.prototype.toString.call(new Date()); //"[object Date]"
      Object.prototype.toString.call(/xz/);  //"[object RegExp]"
      Object.prototype.toString.call(Symbol()); //"[object Symbol]"
      
      var obj = {name:"Xzavier", age:23};
      var a = [1,2,3];
      
      function isType(obj) {
          return Object.prototype.toString.call(obj).slice(8, -1);
      }
      isType(obj);  // "Object" 
      isType(a)  // "Array"  
      ```

      优点：

      - 可以检测所有的原生类型，推荐使用

      缺点：

      - API调用太长了，返回值需要做处理。建议使用方法进行封装。
      - 不能检测自定义类型。

9. 可以发生隐式类型转换的场景以及转换原则，应如何避免或巧妙应用

10. 出现小数精度丢失的原因，JavaScript可以存储的最大数字、最大安全数字，JavaScript处理大数字的方法、避免精度丢失的方法