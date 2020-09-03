# call的简单实现

> ```call``` 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

### 简单示例

``` javascript

function test (a) {
    this.a = a;
}

var o = {};

test.call(o,'A');

console.log(o); // { a : 'A' }

```

### 简单实现

``` javascript
Function.prototype._call = function(_this,...args) {
    
    _this.f = this;  // 将当前的实例func赋值到，参数this的一个属性上，

    _this.f(...args); // 调用参数_this上的func，由于此时func中的this指向了_this,实现了this的绑定

}
```

# apply的简单实现
> ```apply```与```call```的区别只在于参数的传递形式师数组

``` javascript
Function.prototype._apply = function(_this,args) {

    _this.f = this;

    _this.f(...args);
}

```

# bind的简单实现

> ```bind```与```call```和```apply```的区别是，只是绑定了this和参数，并没有执行

### 简单实现
``` javascript

Function.prototype._bind = function (_this,...args){


    _this.f = this;

    return function f (...others) { // 返回一个执行 _this.f的函数，并拼接参数
        const realArgs = [...args,...others];
        _this.f(...realArgs);
    }
}

```
