---
layout: post
title: javascript 继承的几种方式
date: 2019-02-14 16:45:55 +UTC800
category: [frent-end]
---

**javascript是弱类型的解释语言，本身没类继承的实现，下边是几种继承的实现方式**

{% highlight javascript linenos %}
var css = 'color:red; font-weight:bold'

function Animal(name) {
  this.name = name || 'Animal'
  // 实例方法
  this.sleep = function () {
    console.log(this.name + '正在睡觉!')
  }
}
// 原型方法 核心：将父类的实例作为子类的原型
Animal.prototype.eat = function (food) {
  console.log(this.name + '正在吃:' + food)
}
{% endhighlight %}

原型链继承方法
------
{% highlight javascript linenos %}
// 原型链继承方法
function Cat(name) {
}
Cat.prototype = new Animal()
Cat.prototype.name = 'Cat'
var cat = new Cat()
console.group('原型链继承方法')
console.log(cat.name)
cat.sleep()
cat.eat('fish')
console.log('cat instanceof Cat: %c%s', css, cat instanceof Cat)
console.log('cat instanceof Animal %c%s', css, cat instanceof Animal)
console.groupEnd()
{% endhighlight %}
**优点：**
  1. 非常纯粹的继承关系，实例是子类的实例，也是父类的实例 
  2. 父类新的原型方法，子类都能访问 
  3. 简单，易于实现
      
__缺点：__
  1. 要想为子类新增属性和方法，必须在new Animal()之后执行 
  2. 无法实现多继承 
  3. 来自原型的对象的所有属性被所有实例共享 
  4. 创建子类时无法向父类传参_

## 构造继承
`核心：使用父类的构造函数来增加子类实例，等于是复制父类的实例属性给子类（没用到原型）`
{% highlight javascript linenos %}
function Cat2(name) {
  //Animal.call(this, [arguments])
  Animal.call(this)
  this.name = name || 'Tom'
}
var cat = new Cat2()
console.group('构造继承')
console.log(cat.name)
cat.sleep()
var intStr = 'cat instanceof Animal', intStr2 = 'cat instanceof Cat2', str2 = ': %c%s'
console.log(intStr + str2, 'color:red; font-weight:bold', eval(intStr))
console.log(intStr2 + str2, 'color:red; font-weight:bold', eval(intStr2))
console.groupEnd()
{% endhighlight %}
**优点**
  1. 解决了原型链继承中子类实例共享父类引用属性的问题 
  2. 创建子类时可以向父类传参 
  3. 可以实现多继承(call多个父类对象)

__缺点：__
  1. 实例并不是父类的实例，只是子类的实例 
  2. 只有继承父类的属性和方法，不能继承原型的属性/方法 
  3. 无法实现函数的复用，每个子类都有父类实例函数的副本，影响性能

实例继承
------
`核心：为父类实例添加新特性，作为子类实例返回`
{% highlight javascript linenos %}
function Cat3(name) {
  var instance = new Animal()
  instance.name = name || 'Tom'
  return instance
}
var cat = new Cat3()
console.group('实例继承')
console.log(cat.name)
var intStr = 'cat instanceof Cat3', intStr2 = 'cat instanceof Animal', str2 = ': %c%s'
console.log(intStr + str2, css, eval(intStr))
console.log(intStr2 + str2, css, eval(intStr2))
console.groupEnd()
{% endhighlight %}
**优点：**

  1. 不限制调用方式，不管是new 子类()或子类()，返回的结果相同
__缺点：__
  1. 实例是父类的实例，不是子类的实例 
  2. 不支持多继承

拷贝继承
------
{% highlight javascript linenos %}
function Cat4(name) {
  var animal = new Animal()
  for (var p in animal) {
    Cat4.prototype[p] = animal[p]
  }
  Cat4.prototype.name = name || 'Tom'
}
var cat = new Cat4()
console.group('拷贝继承')
console.log(cat.name)
cat.sleep()
cat.eat('fish')
var intStr = 'cat instanceof Cat4', intStr2 = 'cat instanceof Animal', str2 = ': %c%s'
console.log(intStr + str2, css, eval(intStr))
console.log(intStr2 + str2, css, eval(intStr2))
console.groupEnd()
{% endhighlight %}
**优点：**
  1. 支持多继承

__缺点：__
  1. 效率低，占用内存高（因为要拷贝父类的属性） 
  2. 无法获取父类不可枚举方法（不可枚举方法，不能使用for...in访问到）

组合继承
------
`核心：通过调用父类构造，继承父类的属性并保留传参的优点，然后以挝将父类实例作为子类的原型，实现函数复用`
{% highlight javascript linenos %}
function Cat5(name) {
  Animal.call(this)
  this.name = name || 'Tom'
}
Cat5.prototype = new Animal()
Cat5.prototype.constructor = Cat5
var cat = new Cat5()
console.group('组合继承')
console.log(cat.name)
cat.sleep()
cat.eat('fish')
var intStr = 'cat instanceof Cat5', intStr2 = 'cat instanceof Animal', str2 = ': %c%s'
console.log(intStr + str2, css, eval(intStr))
console.log(intStr2 + str2, css, eval(intStr2))
console.groupEnd()
{% endhighlight %}
**优点：**
  1. 弥补了构造函数继承的缺点，可以继承实例属性/方法，也可继承原型属性/方法 
  2. 既是子类的实例，也是父类的实例 
  3. 不存在引用属性共享的问题 
  4. 可传参 
  5. 函数可复用

__缺点：__
  1. 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

寄生组合继承
------
`核心：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类构造函数的时候，就不会初始化两次实例方法/属性，避免组合继承的缺点`
{% highlight javascript linenos %}
function Cat6(name) {
  Animal.call(this)
  this.name = name || 'Tom'
}

(function(){
  // 创建一个没有实例方法的类
  var Super = function(){}
  Super.prototype = Animal.prototype
  // 将实例作为子类的原型
  Cat6.prototype = new Super()
})()
var cat = new Cat6()
console.group('寄生组合继承')
console.log(cat.name)
cat.sleep()
cat.eat('fish')
var intStr = 'cat instanceof Cat6', intStr2 = 'cat instanceof Animal', str2 = ': %c%s'
console.log(intStr + str2, css, eval(intStr))
console.log(intStr2 + str2, css, eval(intStr2))
console.groupEnd()
{% endhighlight %}
**优点：** 堪称完美

__缺点：__ 实现较为复杂

代码附录
------
{% highlight javascript linenos %}
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
  //实例引用属性
  this.features = [];
}
function Cat(name){
}
Cat.prototype = new Animal();

var tom = new Cat('Tom');
var kissy = new Cat('Kissy');

console.log(tom.name); // "Animal"
console.log(kissy.name); // "Animal"
console.log(tom.features); // []
console.log(kissy.features); // []

tom.name = 'Tom-New Name';
tom.features.push('eat');

//针对父类实例值类型成员的更改，不影响
console.log(tom.name); // "Tom-New Name"
console.log(kissy.name); // "Animal"
//针对父类实例引用类型成员的更改，会通过影响其他子类实例
console.log(tom.features); // ['eat']
console.log(kissy.features); // ['eat']
{% endhighlight %}

### 原因分析：

**关键点：**属性查找过程

执行tom.features.push，首先找tom对象的实例属性（找不到），
那么去原型对象中找，也就是Animal的实例。发现有，那么就直接在这个对象的
features属性中插入值。
在console.log(kissy.features); 的时候。同上，kissy实例上没有，那么去原型上找。
刚好原型上有，就直接返回，但是注意，这个原型对象中features属性值已经变化了。
