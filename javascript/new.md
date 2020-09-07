# 理解并实现一个new

## new做了什么

- 创建一个空的简单JavaScript对象（即{}）
- 新对象隐式原型链接到构造函数显式原型
- 将步骤1新创建的对象作为this的上下文 
- 如果函数没有返回其他对象，那么返回这个新对象  


``` javascript

function _new (_constructor,...args){
    
    var o = {}; // 1. 创建一个空的js对象
    
    o.__proto__ = Object.create(_constructor.prototype); // 2. 链接构造函数的原型（ 简单粗暴 ）
    // Object.setPrototypeOf(o, _constructor.prototype)
    // 遍历 o = Object.create(_constructor.prototype)   (在没有__proto__属性时)
    
    _constructor.apply(o,args); // 绑定this上下文

    return o;

}

```