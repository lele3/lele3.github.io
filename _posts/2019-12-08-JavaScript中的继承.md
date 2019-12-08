---
title: JavaScript中的继承
key: 20191208
tags: javascript
---

<center><h2>javascript中的继承</h2></center>

### 继承

继承是面向软件技术当中的一个概念。如果一个类别B“继承自”另一个类别A，就把这个B称为“A的子类”，而把A称为“B的父类”也可以称“A是B的超类”。继承可以使得子类具有父类别的各种属性和方法，而不需要再次编写相同的代码。在令子类别继承父类别的同时，可以重新定义某些属性，并重写某些方法，即覆盖父类别的原有属性和方法，使其获得与父类别不同的功能。另外，为子类追加新的属性和方法也是常见的做法。(引用 维基百科)
<!--more-->


通俗一点的讲： 就是我自己没有，但是别人有，那我拿过来成为自己的， 是实现复用的一种手段。

JavaScript中的继承和其他语言（例如c++、Java）的继承不太一样， JavaScript采用的是**原型**继承，那原型又是什么呢

### 原型

提到原型，脑子里就会浮现一幅这样的图。

 <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E7%9A%84%E7%BB%A7%E6%89%BF/%E5%8E%9F%E5%9E%8B.png' width = '450' height = '300' alt='原型' align='center' />
  </div>

对应代码如下

```javascript
// Person 构造函数
function Person() {}
// person 实例对象
let person = new Person()
```

我们假设原型对象为`Prototype`, 则满足

```javascript
Person.prototype === Prototype  // true
person.__proto__ === Prototype  // true
Prototype.constructor === Person // true
```

你可以把原型对象想象成一个公共区域，放在里面的任何属性和方法都是被实例所共享的

### JavaScript继承实现

基本概念介绍完了，那我们开始进入正题，来讨论讨论JavaScript中继承的实现

#### 1. 原型链继承

基本思想： 让子类的原型对象指针指向父类的实例

代码如下：

```javascript
// 父类
function Person(name) {
  this.name = name
  this.friends = []
}
Person.prototype.sayHello = function () {
  console.log(`${this.name} say hello`)
}
// 子类
function Student(studentId) {
  this.studentId = studentId
}
// Student 继承 Person
Student.prototype = new Person('simple')

let student = new Student('001')
student.sayHello() // simple say hello

let student2 = new Student('002')
student.friends.push('002')
student2.friends.push('001')
console.log('student.name', student.name) // simple
console.log('student2.name', student2.name) // simple
console.log('student.friends', student.friends) // ['001', '002']
console.log('student2.friends', student2.friends) // ['001', '002']
```

从如上代码的执行结果可以看出， `Student`继承了`Person`的`name`属性和`sayHello`方法， 实现了基本的继承，但是会发现有如下几个问题

 1. 就拿`name`属性来说，`Student`虽然继承了`name`属性，但是`name`属性是放在原型上的，也就是说所有的	`Student`实例都会共享这个属性，这显然是不合理的, 有的人可能会这么做

    ```javascript
    student1.name = 'simple_h'
    student2.name = 'student_sl'
    console.log('student.name', student.name) // simple_h
    console.log('student2.name', student2.name) // simple_sl
    ```

    我重新赋值，这不是也能合理输出嘛， 究其原理，只不过是在实例对象上新增了`name`属性，导致在获取这个`name`属性的时候不用再去原型上查找罢了，如果这么做，那原型上的`name`属性就好像是多余的了🤦‍♂️

	2. 以`friends`属性为例，因为`friends`属性是引用对象，又因为所有实例对象共享这个属性，所以当某个实例修改该实例对象时， 会影响到所有实例对象

	3. 在创建子类时，不能向父类型的构造函数中传递参数

	4. 子类原型对象的`constructor`不能正确的指向构造函数 或者说 子类原型对象没有 `constructor`

#### 2.借用构造函数

基本思想：在子类的构造函数内执行父类的构造函数

代码如下：

```javascript
// 父类
function Person(name) {
  this.name = name
  this.friends = []
}
Person.prototype.sayHello = function () {
  console.log(`${this.name} say hello`)
}
// 子类
function Student(name, studentId) {
  Person.call(this, name) // 继承父类
  this.studentId = studentId
}

let student = new Student('simple_h', '001')
student.sayHello() // Uncaught TypeError: student.sayHello is not a function

let student2 = new Student('simple_sl', '002')
student.friends.push('002')
student2.friends.push('001')
console.log('student.name', student.name) // simple_h
console.log('student2.name', student2.name) // simple_sl
console.log('student.friends', student.friends) // ['002']
console.log('student2.friends', student2.friends) // ['001']
```

从上面代码的执行结果来看，好像正好弥补的**原型链继承**的缺点，但是好像也有问题

1. 不能继承父类原型中的方法，导致子类调用`sayHello`方法报错

2. 函数都在构造函数中定义，因此复用就无从谈起了

   <div align='center'>
         <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E7%9A%84%E7%BB%A7%E6%89%BF/1.jpeg' width = '140' height = '150' alt='太难了！我就想实现一个简单的继承啊' align='center' />
    </div>

-.-

 <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E7%9A%84%E7%BB%A7%E6%89%BF/2.jpeg' width = '140' height = '150' alt='灵机一动' align='center' />
  </div>

既然借用构造函数继承可以弥补原型链继承的缺点，那我们把两者结合起来试试呀

#### 3. 组合继承

基本思想： 将原型链和借用构造函数继承的技术组合到一块，发挥二者之长。使用原型链实现对原型属性和方法的继承，通过构造函数实现对实例属性的继承

代码如下：

```javascript
// 父类
function Person(name) {
  this.name = name
  this.friends = []
}
Person.prototype.sayHello = function () {
  console.log(`${this.name} say hello`)
}
// 子类
function Student(name, studentId) {
  Person.call(this, name) // 继承父类属性
  this.studentId = studentId
}

Student.prototype = new Person()
Student.prototype.constructor = Student

// 实例化
let student = new Student('simple_h', '001')
student.sayHello() // simple_h say hello

let student2 = new Student('simple_sl', '002')
student.friends.push('002')
student2.friends.push('001')
console.log('student.name', student.name) // simple_h
console.log('student2.name', student2.name) // simple_sl
console.log('student.friends', student.friends) // ['002']
console.log('student2.friends', student2.friends) // ['001']
```

组合继承避免了原型链继承和借用构造函数的缺陷，融合了他们的优点，成为JavaScript中最常用的继承模式。但是，这么写真的没有问题吗？你难道不觉得`Student`原型对象中的`name`和`friends`属性是多余的吗？调用两次父类构造函数不也是多余的吗？

#### 4.原型式继承

基本思想：借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型

代码如下：

```javascript
function object(o) {
  function F() {}
  F.prototype = o
  return new F()
}

let person = {
  name: 'simple',
  friends: ['xiaoming', 'xiaohong']
}
let anotherPerson = object(person)
anotherPerson.name = 'zhangsan'
anotherPerson.friends.push('wangwu')

let yetAnotherPerson = object(person)
yetAnotherPerson.name = 'lisi'
yetAnotherPerson.friends.push('zhaoliu')

console.log(person.friends) // ["xiaoming", "xiaohong", "wangwu", "zhaoliu"]
```

这种原型继承要求你必须有一个对象可以作为另一个对象的基础， 如果有这么一个对象的话，可以把它传递给`object()`函数，然后再根据具体需求对得到的对象加以修饰即可。在这个例子中，`anotherPerson`和`yetAnotherPerson`的原型都是`person`, 而`person`中的`friends`属性是引用类型，这说明`friends`属性不仅仅归`person`所有，而是会被`anotherPerson`和`yetAnotherPerson`所共有，所以最后console的输出才是这样的。

ES5通过`Object.create()`方法规范了原型继承,这个方法接收两个参数： 一个用作新对象原型的对象和（可选的）一个为新对象定义额外属性的对象。

```javascript
let person = {
  name: 'simple',
  friends: ['xiaoming', 'xiaohong']
}
let anotherPerson = Object.create(person)
anotherPerson.name = 'zhangsan'
anotherPerson.friends.push('wangwu')

let yetAnotherPerson = Object.create(person)
yetAnotherPerson.name = 'lisi'
yetAnotherPerson.friends.push('zhaoliu')

console.log(person.friends) // ["xiaoming", "xiaohong", "wangwu", "zhaoliu"]
```

在没有必要兴师动众的创建构造函数，而只想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。

#### 5.寄生式继承

基本思想： 创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真的是它做了所有工作一样返回对象

代码如下：

```javascript
function createAnother(original) {
  var clone = object(original)
  clone.sayHi = function () {
    console.log('hi')
  }
  return clone
}

let person = {
  name: 'simple',
  friends: ['xiaoming', 'xiaohong']
}
let anotherPerson = createAnother(person)
anotherPerson.sayHi() // hi
```

在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。

使用寄生式继承为对象添加函数，会由于不能做到函数复用而降低效率

#### 6.寄生组合式继承

基本思想： 通过借用构造函数来继承属性，通过原型链的混成形式来继承方法， 不必为了指定子类的原型而调用父类的构造函数。

代码如下：

```javascript
function object(o) {
  function F() {}
  F.prototype = o
  return new F()
}

function inheritPrototype(subType, superType) {
  let prototype = object(superType.prototype)
  prototype.constructor = subType
  subType.prototype = prototype
}

// 父类
function Person(name) {
  this.name = name
  this.friends = []
}
Person.prototype.sayHello = function () {
  console.log(`${this.name} say hello`)
}
// 子类
function Student(name, studentId) {
  Person.call(this, name) // 继承父类属性
  this.studentId = studentId
}

inheritPrototype(Student, Person)

// 实例化
let student = new Student('simple_h', '001')
student.sayHello() // simple_h say hello

let student2 = new Student('simple_sl', '002')
student.friends.push('002')
student2.friends.push('001')
console.log('student.name', student.name) // simple_h
console.log('student2.name', student2.name) // simple_sl
console.log('student.friends', student.friends) // ['002']
console.log('student2.friends', student2.friends) // ['001']
```

寄生组合式继承正好解决了组合式继承的问题，包括只调用一次父类构造函数，避免在子类的原型上创建不必要的、多余的属性，同时，还能保持原型链不变， 因此能够正常使用`instanceof`和`isPrototypeOf()`

#### 7.ES6中的继承

代码如下：

```javascript
class Person {
  constructor(name) {
    this.name = name
    this.friends = []
  }
  sayHello () {
    console.log(`${this.name} say hello`)
  }
}

class Student extends Person {
  constructor(name, studentId) {
    super(name)
    this.studentId = studentId
  }
}

// 实例化
let student = new Student('simple_h', '001')
student.sayHello() // simple_h say hello

let student2 = new Student('simple_sl', '002')
student.friends.push('002')
student2.friends.push('001')
console.log('student.name', student.name) // simple_h
console.log('student2.name', student2.name) // simple_sl
console.log('student.friends', student.friends) // ['002']
console.log('student2.friends', student2.friends) // ['001']
```

ES6 中的继承看着是不是很熟悉，和`java, c++`等语言一样使用`class、 extends`等关键字实现继承，但`javascript`的这种实现只是一种语法糖，其内部实现仍然是通过原型继承实现的。

我们来看看babel对上述代码的转换

```javascript
"use strict";

function _typeof(obj) {
  if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") {
    _typeof = function _typeof(obj) {
      return typeof obj;
    };
  } else {
    _typeof = function _typeof(obj) {
      return obj && typeof Symbol === "function" && obj.constructor === Symbol && obj !== Symbol.prototype ? "symbol" : typeof obj;
    };
  }
  return _typeof(obj);
}

function _possibleConstructorReturn(self, call) {
  if (call && (_typeof(call) === "object" || typeof call === "function")) {
    return call;
  } 
  return _assertThisInitialized(self);
}

function _assertThisInitialized(self) {
  if (self === void 0) {
    throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
  }
  return self;
}

function _getPrototypeOf(o) {
  _getPrototypeOf = Object.setPrototypeOf
    ? Object.getPrototypeOf
    : function _getPrototypeOf(o) {
      return o.__proto__ || Object.getPrototypeOf(o);
    };
  return _getPrototypeOf(o);
}

function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: { 
      value: subClass,
      writable: true,
      configurable: true
    }
  });
  if (superClass) _setPrototypeOf(subClass, superClass);
}

function _setPrototypeOf(o, p) {
  _setPrototypeOf = Object.setPrototypeOf
    || function _setPrototypeOf(o, p) {
      o.__proto__ = p;
      return o;
    };
  return _setPrototypeOf(o, p);
}

function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i];
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if ("value" in descriptor) descriptor.writable = true;
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}

var Person =
/*#__PURE__*/
function () {
  function Person(name) {
    _classCallCheck(this, Person);

    this.name = name;
    this.friends = [];
  }

  _createClass(Person, [{
    key: "sayHello",
    value: function sayHello() {
      console.log("".concat(this.name, " say hello"));
    }
  }]);

  return Person;
}();

var Student =
/*#__PURE__*/
function (_Person) {
  _inherits(Student, _Person);

  function Student(name, studentId) {
    var _this;

    _classCallCheck(this, Student);

    _this = _possibleConstructorReturn(this, _getPrototypeOf(Student).call(this, name));
    _this.studentId = studentId;
    return _this;
  }

  return Student;
}(Person);
```

我们首先看一下父类构造函数的实现，他与我们平时写构造函数的区别就是用一个立即执行函数包裹

```javascript
var Person =
/*#__PURE__*/
function () {
  function Person(name) {
    _classCallCheck(this, Person);

    this.name = name;
    this.friends = [];
  }

  _createClass(Person, [{
    key: "sayHello",
    value: function sayHello() {
      console.log("".concat(this.name, " say hello"));
    }
  }]);

  return Person;
}();
```

这里面有两个函数，`_classCallCheck` 和`_createClass`，我们分别看看他们的作用

```javascript
function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}
```

当`instance`不是`Constructor`的实例时，抛错。换句话说，该Person函数通过`new`来调用，或者满足`instance 是 Constructor`的实例，否则报错,这涉及到`new`的执行过程

```javascript
function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i];
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if ("value" in descriptor) descriptor.writable = true;
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}
```

`_createClass`函数的作用就是通过`Object.defineProperty`来绑定属性，因为类中的方法又分为静态方法和普通方法，所以代码中对静态方法则绑定到构造函数上，普通方法则绑定到原型上。

介绍完父类构造函数，我们来看看子类的构造函数

```javascript
var Student =
/*#__PURE__*/
function (_Person) {
  _inherits(Student, _Person);

  function Student(name, studentId) {
    var _this;

    _classCallCheck(this, Student);

    _this = _possibleConstructorReturn(this, _getPrototypeOf(Student).call(this, name));
    _this.studentId = studentId;
    return _this;
  }

  return Student;
}(Person);
```

基本和父类构造函数一样，不过子类需要继承父类，而`_inherits`函数就是用来继承的

```javascript
function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: { 
      value: subClass,
      writable: true,
      configurable: true
    }
  });
  if (superClass) _setPrototypeOf(subClass, superClass);
}
```

首先是一个if判断

```javascript
if (typeof superClass !== "function" && superClass !== null) {
  throw new TypeError("Super expression must either be null or a function");
}
```

如果`superClass`不是一个`function`或者不是`null`，则报错， 也就是说父类必须是一个函数或者是`null`

在往下

```javascript
subClass.prototype = Object.create(superClass && superClass.prototype, {
  constructor: { 
    value: subClass,
    writable: true,
    configurable: true
  }
});
```

通过`Object.create`实现继承, 并重新设置了`constructor`属性

最后一句

```javascript
if (superClass) _setPrototypeOf(subClass, superClass);
```

通过`Object.setPrototypeOf`修改内部属性`[[Prototype]]`,实现函数的继承



在子类构造函数中还有一句话

```javascript
_this = _possibleConstructorReturn(this, _getPrototypeOf(Student).call(this, name));
```

这句代码的意思是通过`call`来修改父类构造函数中的`this`为子类实例，从而在子类上绑定父类的属性，实现属性的继承



以上就是`babel`对于`class`的转换

放一张js继承关系图

 <div align='center'>
       <img src='https://raw.githubusercontent.com/lele3/markDownImages/master/images/javascript/JavaScript%E4%B8%AD%E7%9A%84%E7%BB%A7%E6%89%BF/%E5%8E%9F%E5%9E%8B%E9%93%BE.png' width = '700' height = '600' alt='原型链' align='center' />
  </div>

#### 最后

这篇文章写了好久，中间写的时候感觉越写越多，遇到的知识盲区也越来越多😹，以上文章可能有很多纰漏，欢迎指正，一起进步。

#### 

