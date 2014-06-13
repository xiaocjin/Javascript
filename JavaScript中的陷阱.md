JavaScript中的陷阱
==================

## 函数和操作符

#### 双等号
  
  ==操作符比较时会进行类型的强制转换，这意味着它可以比较两个不同类型的对象，在执行比较之前它将会尝试把这两个对象转换成同一个类型，由于双等号具有强制类型转换的行为，所以它会打破一般的传递性规则。
  
   * "" == 0 //true - 空字符串会被强制转换为数字0.
   * 0 == "0" //true - 数字0会被强制转换成字符串"0"
   * "" == "0" //false - 两操作数都是字符串所以不执行强制转换

#### parseInt不把10作为数字基数

如果你忽略parseInt的第二个参数，那么数字的基数将由下面的规则所决定：
* 默认基数为10，即按10进制解析
* 如果数字以0x开头，那么基数为16，即按16进制解析
* 如果数字以0开头，那么基数为8，即按8进制解析

>ECMAScript5方面的说明：ECMAScript已不再支持8进制的解析假设，另外，如果忽略parseInt的第二个参数将会引起JSLint的警告。

#### 字符串替换

  字符串替换函数仅仅会替换第一个匹配项，并不能替换你所期望的全部匹配项。
  
  * "bob".replace("b", "x"); // "xob"
  * "bob".replace(/b/, "x"); // "xob" (使用了正则表达式)
  

  如果要替换所有的匹配项，我们可以使用正则表达式，并为他它添加全局修饰符

  * "bob".replace(/b/g, "x"); // "xox"
  * "bob".replace(new RegExp("b", "g"), "x"); // "xox" (alternate explicit RegExp)
  
>全局修饰符确保了替换函数找到第一个匹配项后不会停止对下一个匹配项的替换。

#### “+"操作符会执行相加操作和字符串连接操作

  在执行相 加操作前需要先将其转换成数字类型
  
  * 1 + document.getElementById("inputElem").value; // 连接操作 
  * 1 + Number(document.getElementById("inputElem").value); // 相加操作 
  
  相减操作会尝试将操作数转换成数字类型

  * "3" - "1"; // 2 
  
  很多时候我们用数字和空串相加来实现数字转换成字符串的操作，代码如下：

  * 3 + ""; // "3" 
  
  但是这样做并不好，所以我们可以用String(3)来取代上面的方法。

#### typeof

  typeof这会返回一个javascript基本类型的实例的类型。Array实际上不是基本类型，所以typeof Array对象将返回Object
  
  * typeof {} === "object" //true
  * typeof "" === "string" //true
  * typeof [] === "array"; //false

>另外说明一点，”typeof null“将返回”object“，这个有点诡异。

#### instanceof
  
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

## 作用域

#### 没有块作用域

  因为你可能已经注意到上一个观点，javascript中没有块作用域的概念，只有函数作用域。可以试试下面的代码：

    for(var i=0; i<10; i++) {  
        console.log(i);  
    }  
    var i;  
    console.log(i); // 10 
当i被定义在for循环中，退出循环后它人被保留在这个作用域内，所以最后调用console.log输出了10。这里有一个JSLint警告来让你避免这个问题：强制将所有的变量定义在函数的开头。 我们有可能通过写一个立即执行的function来创建一个作用域：

    (function (){  
        for(var i=0; i<10; i++) {  
            console.log(i);  
        }  
    }());  
    var i;  
    console.log(i); // undefined 
当你在内部函数之前声明一个变量，然后在函数里重声明这个变量，那将会出现一个奇怪的问题，示例代码如下：

    var x = 3;  
    (function (){  
        console.log(x + 2); // 5  
        x = 0; //No var declaration  
    }()); 
但是，如果你在内部函数中重新声明x变量，会出现一个奇怪的问题：

    var x = 3;  
    (function (){  
        console.log(x + 2); //NaN - x is not defined  
        var x = 0; //var declaration  
    }()); 
这是因为在函数中x变量被重新定义了，这说明了翻译程序将var表达式移动到了函数顶部了，最终就变成这样执行了：

    var x = 3;  
    (function (){  
        var x;  
        console.log(x + 2); //NaN - x is not defined  
        x = 0;  
    }()); 

#### 全局变量
  
  下面的代码，它将尝试声明两个值相等的局部变量：
  
  >var a = b = 3;
  
  这样非常正确的得到了a=3和b=3，但是a在局部作用域中而b在全局作用域中，”b=3“将会被先执行，全局操作的结果，3，再被分配给局部变量a。
  
  下面的代码声明了两个值为3的变量，这样能达到预期的效果：
  
  >var a = 3,  
  b = a; 
  
#### “this”和内部函数

  “this“关键字通常指当前正在执行的函数所在的对象，然而，如果函数并没有在对象上被调用，比如在内部函数中，”this“就被设置为全局对象(window).

    var obj = {  
        doSomething: function () {  
            var a = "bob";  
            console.log(this); // 当前执行的对象  
            (function () {  
                console.log(this); // window - "this" is reset  
                console.log(a); // "bob" - still in scope  
            }());  
        }  
    };  
    obj.doSomething(); 
