
标签（空格分隔）： javascript
javascript this 的理解
---
在javascript 中this 的操作场景很丰富，它可以是全局对象，当前对象或者任意对象，这完全取决于函数的调用方式
javascript 中函数的调用有以下几种方式：
 1. 作为对象方法的调用
 2. 作为函数的调用
 3. 作为构造函数的调用
 4. apply 和 call 的使用
----------
例子：
----------
**作为对象方法的调用**
在javascript 中，函数也是对象，因此函数可以作为一个对象的属性，此时该函数被称为对象的方法。在使用这种调用方式时，this 被自然绑定到该对象中
/**
* nth element in the fibonacci series.
* @param n >= 0
* @return the nth element, >= 0.
*/
var perSon={
  name:'LiLei',
  age:'22',
  sayName:function(name){
   alert(this.name);//这里的this 指向的是 perSon
   //换一种形式书写 
    this.name=name;//这里的this指向的应是sayName 这个函数 
     如果在调用的时候传参，这时候的name 就是传递的参数值；
 }
};
perSon.sayName();


----------


**作为函数的调用**
函数也可以直接被调用，此时 this 绑定到全局对象。在浏览器中，window 就是该全局对象。比如下面的例子：函数被调用时，this 被绑定到全局对象，接下来执行赋值语句，相当于隐式的声明了一个全局变量，这显然不是调用者希望的。
function makeNoSense(x) { 
this.x = x; 
} 
makeNoSense(5); 
x;// x 已经成为一个值为 5 的全局变量
对于内部函数，即声明在另外一个函数体内的函数，这种绑定到全局对象的方式会产生另外一个问题。我们仍然以前面提到的 point 对象为例，这次我们希望在 moveTo 方法内定义两个函数，分别将 x，y 坐标进行平移。结果可能出乎大家意料，不仅 point 对象没有移动，反而多出两个全局变量 x，y。
清单 4. point.js
var point = { 
    x : 0, 
    y : 0, 
moveTo : function(x, y) { 
    // 内部函数
    var moveX = function(x) { 
    this.x = x;//this 绑定到了哪里？ moveT为当前对象
   }; 
   // 内部函数
   var moveY = function(y) { 
   this.y = y;//this 绑定到了哪里？moveT为当前对象
   }; 
   moveX(x); 
   moveY(y); 
   } 
}; 
point.moveTo(1, 1); 
point.x; //==>0 
point.y; //==>0 
x; //==>1 
y; //==>1


----------


**作为构造函数的调用**


----------


JavaScript 支持面向对象式编程，与主流的面向对象式编程语言不同，JavaScript 并没有类（class）的概念，而是使用基于原型（prototype）的继承方式。相应的，JavaScript 中的构造函数也很特殊，如果不使用 new 调用，则和普通函数一样。作为又一项约定俗成的准则，**构造函数以大写字母开头**，提醒调用者使用正确的方式调用。如果调用正确，this 绑定到新创建的对象上。


----------


function Person(name,age){
 this.name=name;
 this.age=age;
}


----------


----------
**使用 apply 或 call 调用**
让我们再一次重申，在 JavaScript 中函数也是对象，对象则有方法，apply 和 call 就是函数对象的方法。这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即 this 绑定的对象。很多 JavaScript 中的技巧以及类库都用到了该方法。让我们看一个具体的例子：
function Point(x, y){ 
   this.x = x; 
   this.y = y; 
   this.moveTo = function(x, y){ 
       this.x = x; 
       this.y = y; 
   } 
} 
 
var p1 = new Point(0, 0); 
var p2 = {x: 0, y: 0}; 
p1.moveTo(1, 1); 
p1.moveTo.apply(p2, [10, 10]);//执行这个操作的时候，此时的p2 就拥有了p1的moveTo()方法
这就是apply的用处。

 

