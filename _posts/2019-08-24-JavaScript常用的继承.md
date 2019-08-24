---
layout: post
title:JavaScript常用的继承
date: 2019-08-24 23:30:26
description: JavaScript常用的继承。
categories:
 - JavaScript
tags:
 - JavaScript
---


<h1>JavaScript常用的继承</h1>
<h1 id="4TK77">ES6 class <a href="https://github.com/xiaohesong/TIL/blob/master/front-end/es6/understanding-es6/class.md" target="_blank">链接</a></h1>
<h1 id="wrJz2">1.原型链继承</h1>
<p><br /></p>
<p><span style="color: #24292E;">继承的本质就是</span><strong>复制，即重写原型对象，代之以一个新类型的实例</strong><span style="color: #24292E;">。</span></p>
<p><span style="color: #24292E; background-color: #FADB14;"><br /></span></p><pre data-lang="typescript"><code>// 父类
function Person() {
  this.color = 'yellow'
}
Person.prototype.getColor = function () {
  return this.color;
}
// 子类
function Boy() {
  this.age = 18
}
//创建父类的实例，赋值给子类的prototype
Boy.prototype = new Person()

const subBoy = new Boy()
console.log(subBoy.getColor()) //yellow</code></pre>
<p><br /></p>
<p>原理是子类和父类添加了一个引用</p>
<p>缺点：<span style="color: #24292E;">多个实例对引用类型的操作会被篡改</span></p>
<p><span style="color: #24292E;"><br /></span></p>
<h1 id="uwcgg"><span style="color: #24292E;">2.</span>借用构造函数继承</h1>
<p><br /></p>
<p>核心是创建子类时调用父类的构造函数，子类的所有实例都会将父类的属性复制一份。</p>
<p><br /></p><pre data-lang="typescript"><code>    // 父类
    function Person() {
      this.color = 'yellow'
      this.kind = 'persons'
      this.getKind = function(){
        console.log('getKind...')
      }
    }
    Person.prototype.getColor = function () {
      return this.color;
    }
    // 子类
    function Boy() {
      Person.call(this)
    }
    const newBoy = new Boy()
    newBoy.kind = newBoy.kind +  'names'

    // console.log(newBoy.kind) //personsnames
    // console.log(newBoy.getColor()) // 报错
    // newBoy.getKind()//getKind...

    const newBoy2 = new Boy()
    console.log(newBoy2.kind)//persons</code></pre>
<p><br /></p>
<p>缺点： </p>
<ul>
  <li>只能继承父类的<strong>实例</strong>属性和方法，不能继承父类原型上的属性和方法</li>
  <li>无法实现复用，每个子类<span>父类</span>创建都会实例化一次，性能开销很大</li>
</ul>
<p><br /></p>
<h1 id="RNrdS">3.组合继承</h1>
<p><br /></p>
<p>综合原型继承和构造函数继承就是组合继承。</p>
<p><br /></p><pre data-lang="typescript"><code>   // 父类
    function Person() {
      this.color = 'yellow'
      this.kind = 'persons'
      this.getKind = function () {
        console.log('getKind...')
      }
    }

    Person.prototype.getColor = function () {
      return this.color;
    }

    // 子类
    function Boy() {
      Person.call(this)//调用第二次，将父类
      this.age = 18
    }
    //1 .创建父类的实例，赋值给子类的prototype
    Boy.prototype = new Person(); //调用一次
    // 2. 重写Boy.prototype的constructor属性，指向自己的构造函数SubType
    Boy.prototype.constructor = Boy;

    const boy1 = new Boy()
    console.log(boy1)</code></pre>
<p><br /></p>
<p><img alt="image.png" title="image.png" src="https://cdn.nlark.com/yuque/0/2019/png/304107/1565278163536-2cb016b0-89bc-4c73-a0b3-847e3a70a1cf.png#align=left&amp;display=inline&amp;height=230&amp;name=image.png&amp;originHeight=230&amp;originWidth=245&amp;size=15896&amp;status=done&amp;width=245" style="max-width: 600px; width: 245px;" /></p>
<p><span style="color: #24292E;">缺点：就是在使用子类创建实例对象时，其原型中会存在两份相同的属性/方法。</span></p>
<p><span style="color: #24292E;"><br /></span></p>
<h1 id="u6BH6">4.原型式继承</h1>
<p><br /></p>
<p><span style="color: #24292E;">object()对传入其中的对象执行了一次</span><code>浅复制</code><span style="color: #24292E;">，将构造函数F的原型直接指向传入的对象。</span></p>
<p><span style="color: #24292E;"><br /></span></p><pre data-lang="typescript"><code>    var person = {
      color:'yellow'
    };

    function object(obj) {
      function F() { }
      F.prototype = obj;
      return new F();
    }

    var boy = object(person);
    console.log(boy, 'boy')</code></pre>
<p><br /></p>
<p>缺点：和原型链继承原理相同，多个实例引用类型相同，<span style="color: #24292E;">存在篡改的可能。且无法传递参数。</span></p>
<blockquote>
  <p><span style="color: #24292E;"><br /></span><span style="color: #24292E;">ES5中存在</span><code>Object.create()</code><span style="color: #24292E;">的方法，能够代替上面的object方法。</span></p>
</blockquote>
<p><span style="color: #24292E;"><br /></span></p>
<h1 id="in94a"><span style="color: #24292E;">5.</span>寄生式继承</h1>
<p><span style="color: #24292E;">核心：在原型式继承的基础上，增强对象，返回构造函数... 感觉像是凑数的</span></p>
<p><br /></p><pre data-lang="typescript"><code>function createAnother(obj){
  var clone = object(obj); // 通过调用 object() 函数创建一个新对象
  clone.sayHi = function(){  // 以某种方式来增强对象
    alert(&quot;hi&quot;);
  };
  return clone; // 返回这个对象
}</code></pre>
<p><span style="color: #24292E;">缺点（同原型式继承）</span></p>
<p><br /></p>
<h1 id="SceWG">6、寄生组合式继承</h1>
<p><br /></p><pre data-lang="typescript"><code> function inherit(child, Parent) {
      var prototype = Object.create(Parent.prototype); // 创建对象，创建父类原型的一个副本
      prototype.constructor = child;                    // 增强对象，弥补因重写原型而失去的默认的constructor 属性
      child.prototype = prototype;                      // 指定对象，将新创建的对象赋值给子类的原型
    }

    // 父类
    function Person(name) {
      this.name = name;
      this.getKind = function () {
        console.log('getKind...')
      }
    }
    Person.prototype.getName = function () {
      return this.name;
    }
    // 借用构造函数传递增强子类实例属性（支持传参和避免篡改）
    function Boy(name, age) {
      Person.call(this, name);
      this.age = age;
    }

    // 将父类原型指向子类
    inherit(Boy, Person);

    //新增子类原型属性
    Boy.prototype.boyAge = function () {
      console.log(this.age)
    }

    const boy1 = new Boy(&quot;jone&quot;, 23);
    console.log(boy1,'boy1')</code></pre>
<p><img alt="image.png" title="image.png" src="https://cdn.nlark.com/yuque/0/2019/png/304107/1565279748693-bfde0679-a871-4063-b4fb-1edc41e327f8.png#align=left&amp;display=inline&amp;height=188&amp;name=image.png&amp;originHeight=188&amp;originWidth=363&amp;size=24128&amp;status=done&amp;width=363" style="max-width: 600px; width: 363px;" /></p>
<p><br /></p>
<p>优点： 只调用了一次父类的构造函数，避免了创建多余的属性，同时原型链不变，可正常使用<code>instanceof</code><span style="color: #24292E;"> 和</span><code>isPrototypeOf()</code></p>
<p><span style="color: #F5222D;">现在最成熟的方法，也是库采用的方法。</span></p>
<p><br /></p>
<h1 id="Wmf1d">7、混入方式继承多个对象</h1>
<p><br /></p><pre data-lang="typescript"><code>function MyClass() {
     SuperClass.call(this);
     OtherSuperClass.call(this);
}

// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合其它
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass;

MyClass.prototype.myMethod = function() {
     // do something
};</code></pre>
<p><br /></p>
<p><code>Object.assign</code><span style="color: #24292E;">会把 </span><code>OtherSuperClass</code><span style="color: #24292E;">原型上的函数拷贝到 </span><code>MyClass</code><span style="color: #24292E;">原型上，使 MyClass 的所有实例都可用 OtherSuperClass 的方法。</span></p>
<p><br /></p>
<h1 id="GH1UM">8、ES6类继承extends</h1>
<p><br /></p>
<p><code>extends</code><span style="color: #24292E;">关键字主要用于类声明或者类表达式中，以创建一个类，该类是另一个类的子类。</span></p>
<p><br /></p>
<p><span style="color: #24292E;">其中</span><code>constructor</code><span style="color: #24292E;">表示构造函数，一个类中只能有一个构造函数，有多个会报出</span><code>SyntaxError</code><span style="color: #24292E;">错误，如果没有显式指定构造方法，则会添加默认的 </span><code>constructor</code><span style="color: #24292E;">方法，使用例子如下。</span></p>
<p><br /></p><pre data-lang="typescript"><code>class Rectangle {
    // constructor
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
    
    // Getter
    get area() {
        return this.calcArea()
    }
    
    // Method
    calcArea() {
        return this.height * this.width;
    }
}

const rectangle = new Rectangle(10, 20);
console.log(rectangle.area);
// 输出 200

-----------------------------------------------------------------
// 继承
class Square extends Rectangle {

  constructor(length) {
    super(length, length);
    
    // 如果子类中存在构造函数，则需要在使用“this”之前首先调用 super()。
    this.name = 'Square';
  }

  get area() {
    return this.height * this.width;
  }
}

const square = new Square(10);
console.log(square.area);
// 输出 100</code></pre>
<p><br /></p><pre data-lang="typescript"><code>function _inherits(subType, superType) {
  
    // 创建对象，创建父类原型的一个副本
    // 增强对象，弥补因重写原型而失去的默认的constructor 属性
    // 指定对象，将新创建的对象赋值给子类的原型
    subType.prototype = Object.create(superType &amp;&amp; superType.prototype, {
        constructor: {
            value: subType,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    
    if (superType) {
        Object.setPrototypeOf 
            ? Object.setPrototypeOf(subType, superType) 
            : subType.__proto__ = superType;
    }
}</code></pre>
<p><br /></p>
<h1 id="k2P41">总结：</h1>
<p><br /></p>
<h2 id="2XE1H"><span style="color: #24292E;">1、函数声明和类声明的区别？</span></h2>
<p><br /></p>
<p><span style="color: #24292E;">函数声明会提升，类声明不会。首先需要声明你的类，然后访问它，否则像下面的代码会抛出一个ReferenceError。</span></p>
<p><span style="color: #24292E;"><br /></span></p><pre data-lang="typescript"><code>let p = new Rectangle(); 
// ReferenceError

class Rectangle {}</code></pre>
<h2 id="ACaS0">2. ES5继承和ES6继承的区别</h2>
<ul>
  <li>ES5的继承实质上是先创建子类的实例对象，然后再将父类的方法添加到this上（Parent.call(this)）.</li>
</ul>
<p><br /></p>
<ul>
  <li>ES6的继承有所不同，实质上是先创建父类的实例对象this，然后再用子类的构造函数修改this。因为子类没有自己的this对象，所以必须先调用父类的super()方法，否则新建实例报错。<br /></li>
</ul>
