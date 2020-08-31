
# 常用的几种继承

## 原型链继承
- ```基本思想```：利用原型让一个引用类型继承另一个引用类型的属性和方法。
- ```构造函数、原型和实例的关系```：每一个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针；
- ```原型链```：让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针，假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。这就是所谓的原型链。
``` javascript
function A (){ // 构造函数
    this.a = 123;
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
```

未完待续。。。