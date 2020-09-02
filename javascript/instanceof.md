# instanceof 的理解和实现

### 先通过一段mdn上的代码了解 instanceof 的特性

``` javascript
var simpleStr = "This is a simple string"; 
var myString  = new String();
var newStr    = new String("String created with constructor");
var myDate    = new Date();
var myObj     = {};
var myNonObj  = Object.create(null);

simpleStr instanceof String; // 返回 false, 检查原型链会找到 undefined
myString  instanceof String; // 返回 true
newStr    instanceof String; // 返回 true
myString  instanceof Object; // 返回 true

myObj instanceof Object;    // 返回 true, 尽管原型没有定义
({})  instanceof Object;    // 返回 true, 同上
myNonObj instanceof Object; // 返回 false, 一种创建非 Object 实例的对象的方法

myString instanceof Date; //返回 false

myDate instanceof Date;     // 返回 true
myDate instanceof Object;   // 返回 true
myDate instanceof String;   // 返回 false

function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
var mycar = new Car("Honda", "Accord", 1998);
mycar instanceof Car;    // 返回 true
mycar instanceof Object; // 返回 true

```

```instanceof``` 的主要原理就是判断右边的值在不在左边的值的原型（如果有原型的话）链上

``` javascript

function _instanceof(obj , constructor){
    let prototype = constructor.prototype;
    let __proto__ = obj.__proto__;
    while ( 1 ) {
        if (prototype === null) {
            return false;
        }

        if ( __proto__ === prototype ) {
            return true;
        }

        __proto__ = __proto__.__proto__;
    }
}

```