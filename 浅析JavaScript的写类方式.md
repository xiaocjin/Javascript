浅析JavaScript的写类方式
========================

## 构造函数方式

    /**  
     * Person类：定义一个人，有个属性name，和一个getName方法  
     * @param {String} name  
     */ 
    function Person(name) {  
        this.name = name;  
        this.getName = function() {  
            return this.name;  
        }  
    } 

  这种风格让写过Java的有点亲切在于构造一个对象需要配置一些参数，参数要赋值给类里面this。但与Java的区别是JS用function来代替class，参数也无需定义类型。
  
  优点是：可以根据参数来构造不同的对象实例 ，缺点是构造时每个实例对象都会生成getName方法版本，造成了内存的浪费 。
  

## 原型方式

    /**  
     * Person类：定义一个人，有个属性name，和一个getName方法  
     */ 
    function Person(){}  
    Person.prototype.name = "jack";  
    Person.prototype.getName = function() { return this.name;} 

  原型方式的缺点就是不能通过参数来构造对象实例 (一般每个对象的属性是不相同的) ，优点是所有对象实例都共享getName方法(相对于构造函数方式)，没有造成内存浪费。
  
## 构造函数+原型

    /**  
     * Person类：定义一个人，有个属性name，和一个getName方法  
     * @param {String} name  
     */ 
    function Person(name) {  
        this.name = name;  
    }  
    Person.prototype.getName = function() {  
        return this.name;  
    } 
