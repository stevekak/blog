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
    
    _this.f = this;

    _this.f(...args);

}
```