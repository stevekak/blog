# 作用域与闭包

## 作用域

> 如下代码 function foo就形成了一个作用域，foo内部可以访问外部的变量，而外部不能访问foo内部的变量

``` javascript

var a = 'outer'

function foo () {
    var b = 'inner'
    console.log(a);
}

```

## 闭包

> 根据作用域可以访问外部变量的特点，即函数可以访问他被创建时所处的上下文环境，

``` javascript

function foo () {
    var a = 'in foo';
    function bar () {
        console.log(a);
    }
    return bar;
}

bar();

```
> 在foo中声明的bar访问了foo函数中的a，并将bar作为返回值返回出来，导致在foo外部的变量可以访问foo作用域内部的变量a，就形成了闭包，