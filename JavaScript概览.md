JavaScript概览
==============

## JavaScript基础

#### 类型
  
   * 基本类型（值）包括number、boolean、string、null以及undefined。
   * 复杂类型（引用）包括array、function以及object。

#### 类型的困惑
>涉及JavaScript类型判断的一个坑，详见《JavaScript中的陷阱》。

#### 函数

#### this、call、apply
>理解this指代的上下文环境，call方法传递值，apply传递数组

#### 函数的参数数量
>函数有一个很有意思的属性——参数数量，该属性指明函数声明时可接收的数量。在JavaScript中，该属性名为length。

#### 闭包
>在JavaScript中，每次函数调用时，新的作用域就会产生。在某个作用域中定义的变量只能在该作用域或其内部作用域（该作用域中定义的作用域）中才能访问到。自执行函数是一种机制，通过这种机制声明和调用一个匿名函数，能够达到仅定义一个新作用域的作用。

#### 类
>JavaScript中没有class关键词。类只能通过函数来定义。

#### 继承
>基于原型的继承方式

#### try catch
>try/catch允许进行异常捕获

##v8中的JavaScript

#### OBJECT#KEYS
>Object.keys()

#### ARRAY#ISARRAY
>对数组使用typeof操作符会返回object，然后大部分情况下要检测数组是否真的是数组。Array.isArray()对数组返回true，对其他值则返回false。

#### 数组方法
 * 遍历数组  forEach
 * 过滤数组  filter
 * 改变数组中每个元素  map
 * ……
 
#### 字符串方法
 * 移除字符串首末的空格  trim

#### JSON
 * JSON.stringify
 * JSON.parse

#### FUNCTION#BIND
 * .bind允许改变对this的引用

#### "__PROTO__"(继承)

#### 存取器
 * __defineSetter__
 * __defineGetter__
