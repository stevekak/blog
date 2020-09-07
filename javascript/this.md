# this 与 执行上下文

# function中的this：每个函数的this都是在调用时绑定的

## 1. 默认绑定
 
``` javascript

function foo () {
    console.log(this.a);
}

var a = 1;  // 相当于 this.a = 1 ；在非严格模式下， 在浏览器中全局this === window ， node 中 this=== global

foo(); // 1 ， 由于foo运行在全局作用域当中，相当于this.foo()

```

## 2. 隐式绑定

``` javascript
function foo () {
    console.log(this.a);
}

var a = 123;

var o = {
    a:222,
    foo:foo,
}

o.foo(); // 222

var bar = o.foo();

bar(); // 123; 

```

## 3. 显式绑定

> 通过call、apply、bind对this进行绑定

``` javascript

function foo () {
    console.log(this.a)
}

var o = {
    a:'this is o'
}

function bar () {
    foo.call(o)
}


var obj = {
    a:'this is obj',
    foo:bar,
}

obj.foo(); // this is o

```

## 4. new 绑定

- 创建一个空的简单JavaScript对象（即{}）
- 新对象隐式原型链接到构造函数显式原型
- 将步骤1新创建的对象作为this的上下文 
- 如果函数没有返回其他对象，那么返回这个新对象  

``` javascript

function Foo(){
    console.log(this)
}

Foo.prototype.func = function(){console.log(this)}

var foo = new Foo();

foo.func() // Foo {}
 
var bar = foo.func();

bar() // this , global , window

```


## 5. 总结

> 无论函数是怎样被创建的，都可以通过找调用它的对象，来找到this的绑定位置