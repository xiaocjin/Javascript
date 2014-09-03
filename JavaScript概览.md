JavaScript概览
==============

## JavaScript基础

#### 类型
  
  ==操作符比较时会进行类型的强制转换，这意味着它可以比较两个不同类型的对象，在执行比较之前它将会尝试把这两个对象转换成同一个类型，由于双等号具有强制类型转换的行为，所以它会打破一般的传递性规则。
  
   * "" == 0 //true - 空字符串会被强制转换为数字0.
   * 0 == "0" //true - 数字0会被强制转换成字符串"0"
   * "" == "0" //false - 两操作数都是字符串所以不执行强制转换

#### 类型的困惑

如果你忽略parseInt的第二个参数，那么数字的基数将由下面的规则所决定：
* 默认基数为10，即按10进制解析
* 如果数字以0x开头，那么基数为16，即按16进制解析
* 如果数字以0开头，那么基数为8，即按8进制解析

>ECMAScript5方面的说明：ECMAScript已不再支持8进制的解析假设，另外，如果忽略parseInt的第二个参数将会引起JSLint的警告。

#### 函数

  字符串替换函数仅仅会替换第一个匹配项，并不能替换你所期望的全部匹配项。
  
  * "bob".replace("b", "x"); // "xob"
  * "bob".replace(/b/, "x"); // "xob" (使用了正则表达式)
  

  如果要替换所有的匹配项，我们可以使用正则表达式，并为他它添加全局修饰符

  * "bob".replace(/b/g, "x"); // "xox"
  * "bob".replace(new RegExp("b", "g"), "x"); // "xox" (alternate explicit RegExp)
  
>全局修饰符确保了替换函数找到第一个匹配项后不会停止对下一个匹配项的替换。

#### this、call、apply

  在执行相 加操作前需要先将其转换成数字类型
  
  * 1 + document.getElementById("inputElem").value; // 连接操作 
  * 1 + Number(document.getElementById("inputElem").value); // 相加操作 
  
  相减操作会尝试将操作数转换成数字类型

  * "3" - "1"; // 2 
  
  很多时候我们用数字和空串相加来实现数字转换成字符串的操作，代码如下：

  * 3 + ""; // "3" 
  
  但是这样做并不好，所以我们可以用String(3)来取代上面的方法。

#### 函数的参数数量

  typeof这会返回一个javascript基本类型的实例的类型。Array实际上不是基本类型，所以typeof Array对象将返回Object
  
  * typeof {} === "object" //true
  * typeof "" === "string" //true
  * typeof [] === "array"; //false

>另外说明一点，”typeof null“将返回”object“，这个有点诡异。

#### 闭包
  
  instanceof返回指定对象是否是由某个类构造的实例，这个对我们检查指定对象是否是自定义类型之一很有帮助，但是，如果你是用文本语法创建的内置类型那可能会得出错误的结果.
  
  * "hello" instanceof String; //false
  * new String("hello") instanceof String; //true 
  
  由于Array实际上不是内置类型(只是伪装成内置类型 - 因此对它使用typeof不能得到预期的结果)，但是使用instanceof就能得到预期效果了

  * ["item1", "item2"] instanceof Array;  //true  
  * new Array("item1", "item2") instanceof Array;  //true 
  
>总的来说，如果你想测试Boolean, String, Number, 或者Function的类型，你可以使用typeof，对于其他的任何类型，你可以使用instanceof测试。

  在一个function中，有一个预定义变量叫“arguments”，它以一个array的形式传递给function。然而，它并不是真正的array，它只是一个类似array的对象，带有长度属性并且属性值从0-length。非常奇怪...你可以用下面的小伎俩将它转换成真正的数组：
  
  * var args = Array.prototype.slice.call(arguments, 0); 

>这个对由getElementsByTagName返回的NodeList对象也是一样的 - 它们都可以用以上的代码转换成合适的数组。

## 类型和构造函数

#### 使用“new”关键字构造内置类型

  Javascript中有Object, Array, Boolean, Number, String, 和Function这些类型，他们各自都有各自的文字语法，所以就不需要显式构造函数了。
  然而，如果你使用new关键字来构造上面其中的一种类型，你实际上将会得到一个类型为Object并且继承自你要构造的类型的原型的对象(Function类型除外)。所以尽管你用new关键字构造了一个Number类型，它也将是一个Object类型.
  
  * typeof new Number(123); // "object" 
  * typeof Number(123); // "number"
  * typeof 123; // "number"

#### 使用“new”关键字来构造任何东西

  使用new关键字调用函数会创建一个新的对象，然后调用新对象上下文中的函数，最后再返回该对象。相反的，如果不使用new关键在调用函数，那它将会变成一个全局对象。

#### 没有Integer类型

  数值计算是相对缓慢的，因为没有Integer类型。只有Number类型 - Number是IEEE标准中双精度浮点运算(64位)类型。这就意味着Number会引起下面的精度舍入错误：
  
  * 0.1 + 0.2 === 0.3 //false 

#### 类

#### 继承

#### try catch
>try/catch允许进行异常捕获

