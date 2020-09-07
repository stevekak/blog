# 8种数据类型

## Boolean
## Null
## Undefined
## Number
## String
## BigInt
## Symbol
## Object


## 类型转换

### 1. - * / 类型转换
- 对各种非Number类型运用数学运算符（- * /）时，会将非Number类型转换为Number类型

``` javascript

- true // -1
- '12' // -12

```

### 2. 加法的特殊性

- 当一侧为String类型，被识别为字符串拼接，并会优先将另一侧转换为字符串类型。
- 当一侧为Number类型，另一侧为原始类型，则将原始类型转换为Number类型。
- 当一侧为Number类型，另一侧为引用类型，将引用类型和Number类型转换成字符串后拼接。

### 3. 逻辑语句类型转换 ==

- NaN和其他任何类型比较永远返回false
- Boolean和其他任何类型的比较，Boolean类型首先转换成Number类型
- String和Number比较时，先把String变成Number
- null == undefined 为 true，num、undefined和其他任何类型的比较都为false
- 原始类型和引用类型比较时，引用类型先调用valueOf再调用toString方法，直到得到原始类型，否则比较结果为false

``` javascript

'[object Object]' == {} 

```