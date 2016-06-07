---
layout:     post
title:      "套路题のprototype"
subtitle:   "prototype"
date:       2016-06-06 03:09:30
author:     "iMaple"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - prototype
    - 套路题
---

>首先prototype作为最常见的面（tao）试（lu）题经常会出现在我们前端面试或者笔试题中，如果掌握不扎实（像我），往往会吃不少亏，因此这里要把跳过的坑还原并记录一下，好让下次不再跳进去。

准备好了么？？？好的，请听题：

##### 套路1：

```
function foo(){
	this.a = 0;
	this.b = function(){
		alert(this.a); // 弹窗1
	}
}
foo.prototype={
	b : function(){
		this.a = 10;
		alert(this.a);// 弹窗2
	}
}
var f = new foo();
alert(f.a); // 弹窗3
f.b();// 弹窗4
// 请输出弹窗的内容
```

经过一段思考过后，此时你内心肯定会有这样的疑问：为什么你prototype后面的=号前后不空空格？？？（气死你们这些强迫症，2333333。）
好了言归正传，答案是： 0 跟 0。

为什么？？？为什么你不问为什么？？？（啪啪，好吧，我自己打脸）

或者换成如下套路可能你就明白了

```
function foo(){
	this.a = 0;
    // 构造方法
	this.b = function(){
		alert(this.a); // 弹窗1
	}
}
// 类方法
foo.c = function(){ //注意，这里是跟上面的代码的主要区别
        this.a = 5;
        alert(this.a); // 弹窗2
}
// 原型方法
foo.prototype={
	d : function(){ //注意，这里是跟上面的代码的主要区别
		this.a = 10;
		alert(this.a);// 弹窗3
	}
}
var f = new foo();
alert(f.a); // 弹窗4
f.b();
foo.c(); // f.c();会报错，因为方法c是属于foo类的方法，与实例无关
f.d();
// 请输出弹窗的内容
```

是这样的，方法b属于foo类的构造方法，而方法c则是属于foo类的类方法，d才是foo对象的原型方法，这里考察的其实是同名的原型方法与构造函数之间的关系，当实例对象调用某个方法时，对象本身会在构造函数中查找该方法，如果找得到则直接调用该方法，如果找不到再去原型方法中查找该方法，而类方法与实例对象之间无直接关系，因此无需考虑。

##### 套路2：

好的，如果套路1你看得一头雾水，那么没关系，请接着看完套路2，可能会更容易理解一些。

```
function Circle( radius ){
  this.r = radius;
  this.des = "圆形";   
  this.showInfo = function(){
  alert("这是一个"+this.des);  
  }
}  
function Circle_area(r){ return Circle.PI*this.r*this.r; }
function Circle_perimeter(r){ return 2*Circle.PI*r;}
Circle.PI = 3.14;
Circle.perimeter = Circle_perimeter;
Circle.prototype.area = Circle_area;

var c = new Circle(3);

//测试类属性
//alert(Circle.PI )
//alert(c.PI)
//alert(c.constructor.PI)
//alert(Circle.des)
//alert(c.des)
//请依次输出结果
```

好的，这里直接给出答案及解释

```
//测试类属性
//alert(Circle.PI )//3.14
//alert(c.PI)//undefined 因为类属性是和类本身，也就是函数本身相关联的，和类实例没有直接关系。
//alert(c.constructor.PI)//3.14 如果想通过类实例访问类属性，那么就需要先访问该实例的构造函数，进而访问该类属性
//alert(Circle.des)//undefined 因为函数Circle函数中的this.des中的this指代的不是函数本身，而是调用r的对象，而且只能是对象。
//alert(c.des)//圆形 this此时为实例化的 对象c。
```

结论：
面向对象的角度：类属性是类对象的直接属性，且该属性与基于该类对象生成的实例对象没有直接关系，无法直接调用。可以直接通过 类名.属性名 调用该类属性。如果想通过该类对象的实例对象调用类属性，那么可以使用 对象实例.constructor属性调用该对象的类对象，然后通过类对象调用其类属性
javascript函数角度：类属性是javascript函数对象的直接属性变量（这里之所以称之为属性变量是由于javascript变量和属性的同一性），且该属性变量与基于该函数对象构造出来的对象引用（生成了一个对象，这个对象实际上是一个空对象，并且保存了对构造函数以及构造函数初始化时函数内部this关键字下的相关属性和函数的引用[c.prototype和构造函数中this.下面的相关属性、函数]：）没有直接关系，如果想通过基于构造函数生成的对象c调用构造函数对象的属性变量PI，那么需要通过c.constructor属性找到该构造函数对象，并通过该对象获取其属性变量。

还没完呢！

```
//测试类方法
//alert(Circle.perimeter(3));
//alert( c.perimeter(3) );
//alert(c.constructor.perimeter(3));
//alert(c.area(3))//
//alert(Circle.area(3));
//请依次输出结果
```

同样先给出答案：

```
//测试类方法
//alert(Circle.perimeter(3)); //18.4 直接调用函数的类方法。
//alert( c.perimeter(3) ); //FF:c.perimeter is not a function IE:对象或属性不支持此方法。因为perimeter函数是Circle类的类方法，和实例对象没有直接关系
//alert(c.constructor.perimeter(3));//18.84 调用该对象构造函数（类函数）的方法（函数）。
//alert(c.area(3))//28.25.... Circle类的prototype原型属性下的area方法将会被Circle类的实例对象继承。
//alert(Circle.area(3));//FF: 错误： Circle.area is not a function 因为area方法是Circle类的原型属性的方法，并不是Circle类的直接方法。
```

结论：同上，把属性换成了方法，把属性变量换成了函数。

```
//测试prototype对象属性
//alert(c.prototype);
//alert(Circle.prototype); 
//alert(Circle.prototype.constructor)
//alert(Circle.prototype.area(3));
//alert(Circle.prototype.PI) 
//alert(Circle.prototype.constructor.PI)
//请依次输入结果
```

答案：

```
//测试prototype对象属性
//alert(c.prototype); //undefined 实例对象没有ptototype属性
//alert(Circle.prototype); //object Object 
//alert(Circle.prototype.constructor)//返回Circle的函数体（函数代码体），相当于alert(Circle)
//alert(Circle.prototype.area(3));//NaN 方法调用成功，但是返回结果却是NaN，原因是area函数内部的this.r是undefined。
//alert(Circle.prototype.PI) //undefined因为PI属性是Circle类函数的直接属性，并不会在prototype属性下存在
//alert(Circle.prototype.constructor.PI)//3.14 通过Circle类的原型对象调用该原型对象的构造函数（类函数），再通过类函数调用PI属性。
```

结论：
prototype原型对象是javascript基于原型链表实现的一个重要属性。
Javascript角度：
1. 实例对象没有prototype属性，只有构造函数才有prototype属性，也就是说构造函数本身保存了对prototype属性的引用。。
2. prototype属性对象有一个constructor属性，保存了引用他自己的构造函数的引用（看起来像是个循环：A指向B，B指向A...）
3. prototype对象（不要被我这里的属性对象，对象，对象属性搞晕乎了，说是属性对象，就是说当前这个东西他首先是某个对象的属性， 同时自己也是个对象。对象属性就是说它是某个对象的属性。）的属性变量和属性对象将会被该prototype对象引用的构造函数所创建的对象继承(function A(){} A.prototype.pa = function(){} var oA = new A(); 那么oA将会继承属性函数pa)。

这里对 对象属性，对象方法不再做详细测试。
1. javascript对象实例的在通过其构造函数进行实例化的过程中，保存了对构造函数中所有this关键字引用的属性和方法的引用（这里不讨论对象直接量语法的情况）。但如果构造函数中没有通过this指定，对象实例将无法调用该方法。
2. javascript可以通过构造函数创建多个实例，实例会通过__proto__属性继承原型对象的属性和方法。如果对实例对象的属性和方法进行读写操作，不会影响其原型对象的属性和方法，也就是说，对于原型对象，javascript实例对象只能读，不能写。那当我们对实例对象的属性和方法进行修改的时候也可以 改变其值这是为什么呢？其实当我们试图在实例对象中使用继承自原型对象的属性或方法的时候，javascript会在我们的实例对象中复 制一个属性或方法的副本，这样，我们操作的时候，其实操作的就是实例对象自己的属性或方法了。

```
//测试__proto__属性
//alert(c.__proto__)
//alert(c.__proto__.PI)
//alert(c.__proto__.area(3))
//请依次输出结果
```

答案：

```
//测试__proto__属性
//alert(c.__proto__)//FF:object IE8:undefined 该属性指向Circle.prototype，也就是说调用该对象将会返回Circle的prototype属性。
//由于IE8及以下版本不支持__proto__属性，所以以下结果都在FF下得出。
//alert(c.__proto__.PI)//undefined 因为函数原型下面没有PI属性，PI是类函数Circle的直接属性
//alert(c.__proto__.area(3))//NaN 该函数执行成功，返回值为NaN是由于函数体中的this.r为undefined。
```

结论：__proto__属性保存了对创建该对象的构造函数引用prototype属性的引用，也就是说构造函数可以引用prototype，基于该构造函数生成的实例也可以引用，只不过引用的方式不一样。

##### 文章引用

- [javascript 类属性、类方法、类实例、实例属性、实例方法、prototype、__proto__ 测试与小结](http://www.cnblogs.com/mrsunny/archive/2011/05/09/2041185.html)