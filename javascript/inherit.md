
# 常用的六种继承

## 1. 原型链继承
- ```基本思想```：利用原型让一个引用类型继承另一个引用类型的属性和方法。
- ```构造函数、原型和实例的关系```：每一个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针；
- ```原型链```：让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针，假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。这就是所谓的原型链。


``` javascript
function A (){ // 构造函数
    this.a = ['1','2'];
} 

A.prototype // 原型对象

A.prototype = otherInstance // A的原型对象等于其他类型的实例

A.prototype.getA = function(){
    return this.a;
}

A.prototype.constructor === A // true

var a = new A ();

a.__proto__ === A.prototype; //实例都包含一个指向原型对象的内部指针

function B (){};

B.prototype = a; 

B.prototype.__proto__ === A.prototype // B的原型对象包含一个指向A原型的指针

var b = new B(); // 此时 b可以访问b.__proto__（B.prototype和B构造函数中this指定的属性）上的属性，以及B.prototype.__proto__（A.prototype和A构造函数中this指定的属性）的属性，A.prototype又指向其他的类型的实例，如此层层递进就构成了原型链

var c = new B();
b.a.push('3');
c.a // ['1','2','3']

```

- ```原型链继承的问题```： 
    1. 包含引用类型的原型属性，会被所有实例所共享，当一个实例修改了原型属性时，所有实例的属性都发生了变化
    2. 无法通过instanceOf来判断构造函数


## 2. 借用构造函数法
- ```基本思想```：在子类型构造函数中，通过call、apply方法调用超类型构造函数


``` javascript
function SuperType () {
    this.colors = ['red','blue','green']; 
}

function SubType () {
    /**
     * 在将来SubType创建实例时执行SuperType函数并绑定this指针指向SubType的实例
     * 
     * 相当于执行了
     * (function(self){
     *      self.colors = ['red','blue','green'];
     * })(this)
     * 
    */
    SuperType.call(this); 
}

var instance1 = new SubType();
instance1.colors.push('white'); // ['red','blue','green','white'];
var instance2 = new SubType();
instance2.colors.push('black'); //['red','blue','green','black'];

```



- ```借用构造函数的问题```:
    1. 因为所有属性和方法都在构造函数中定义,函数的复用就无法实现(每创建一个对象就要重新声名函数)
    2. 在超类型中定义的方法对于子类而言是不可见的,结果所有类型都只能使用构造函数模式(```没明白```)


## 3. 组合继承(伪经典继承)

- ```基本思想```：使用原型链实现对原型属性和方法的继承，通过构造函数来实现对实例属性的继承


``` javascript
function SuperType (name) {
    this.name = name;
    this.colors = ['red','blue','green'];
}

SuperType.prototype.sayName = function(){
    return this.name;
}

function SubType (name,age){
    SuperType.call(this, name); // 调用一次
    this.age = age;
}

// 继承方法
SubType.prototype = new SuperType(); // 调用一次， 在创建子实例时会优先读取实例上的属性而后去查找原型链上的name,colors属性
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function(){
    return this.age;
}

var instance = new SubType('hello world',11); 

instance.colors.push('orange')

console.log(instance.name);
console.log(instance.__proto__.name);
console.log(instance.colors);
console.log(instance.__proto__.colors);

```


- ```存在问题```：无论在什么情况下都会调用超类型两次构造函数


## 原型式继承
- ```基本思想```：借助原型可以基于已有的对象创建新的对象，同时还不必因此而创建自定义类型。


``` javascript
function object ( o ) { // Object.create
    function F(){};
    F.prototype = o;
    return new F();
}

var SuperType = {
    name:'jack',
    friends:['a','b','c'],
}

const instance1 = object(SuperType);
const instance2 = object(SuperType);

instance1.name = 'a';
instance2.name = 'b';
instance1.friends.push('d');

console.log(SuperType.friends) // ['a','b','c','d']

```


- 原型式继承的问题
    1. 引用类型的属性在所有实例中都是共享的

## 5. 寄生式继承
- ```基本思想```：创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后在返回该对象
- ```使用场景```：在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式。


``` javascript
function object ( o ) { // Object.create
    function F(){};
    F.prototype = o;
    return new F();
}

function createAnother (original) {
    var clone = object(original); // 通过调用函数来创建一个新的对象
    clone.sayHi = function() {    // 以某种方式来增强这个对象
        console.log('Hi');
    }
    return clone;  // 返回该对象
}


```
## 6. 寄生组合式继承
- ```基本思想```：不必为了指定子类型的原型而调用超类型的构造函数，只需要将超类型原型的副本赋值给子类型的原型；

``` javascript

function inheritPrototype (subType,superType){
    var prototype = Object.create(superType); // 创建超类的副本
    prototype.constructor = subType;  // 重置构造函数
    subType.prototype = prototype;
}

function SuperType (name) {
    this.name = name;
    this.colors = ['red','blue','green'];
}

SuperType.prototype.sayName = function(){
    return this.name;
}

function SubType (name,age){
    SuperType.call(this, name); 
    this.age = age;
}

inheritPrototype(SubType,SuperType);

SubType.prototype.sayAge = function () {
    return this.age;
}


```
