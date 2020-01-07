# 函数

先来回忆下原生的 javascript 函数

```js
function sum(x, y) {
  return x + y
}
sum(1, 2);  // 输出: 1+2 = 3
```

## TypeScript 的函数声明

由于 TypeScript 是 javascript 的超集，在 TypeScript 中，函数写法是一样的，只是需要对参数加入类型声明：

```js
function sum(x: number, y: number): number{
  return x + y
}
sum(1, 2);  
// 和javascript一样，输出: 1+2 = 3
sum(1, '2');  
// 提示 'Argument of type '"2"' is not assignable to parameter of type 'number'.'
sum(1, 2, 3) // 提示错误
sum(1) // 提示错误
```
[在线演示](https://www.tslang.cn/play/index.html#src=function%20sum(x%3A%20number%2C%20y%3A%20number)%3A%20number%20%7B%0D%0A%20%20return%20x%20%2B%20y%3B%0D%0A%7D%0D%0Aconsole.log(sum(1%2C%20'2')))

## TypeScript 的函数表达式

在 TypeScript 中，同样支持函数表达式写法：

```js
let sum = function(x: number, y: number): number {
  return x + y;
}
```

TypeScript 自动推断左边变量 sum 类型。

如果我们要给 sum 也添加类型，也是可以的：

```js
let sum: (x: number, y: number) => number = function(x: number, y: number) {
  return x + y;
}
sum(1, 2);
```

注意：TypeScript 中的`=>`与 ES6 中的`=>`不相同。在 TypeScript 的类型定义中，`=>`用来表示函数的定义，左边是输入类型，右边是输出类型。在 ES6 中，`=>`表示箭头函数。

说起 ES6，就不能缺少这几个参数定义：可选参数，默认参数，剩余参数

## 可选参数

定义可选参数，既是在需要做可选参数后面加个"?"，而且可选参数必须在最后定义：
```js
function sum(x: number, y?: number): number{
  if(y){
    return x + y;
  }else{
    return x;
  }
}

let two = sum(1, 2);
let one = sum(1);
```
[在线演示](https://www.tslang.cn/play/index.html#src=function%20sum(x%3A%20number%2C%20y%3F%3A%20number)%3A%20number%7B%0D%0A%20%20if(y)%7B%0D%0A%20%20%20%20return%20x%20%2B%20y%3B%0D%0A%20%20%7Delse%7B%0D%0A%20%20%20%20return%20x%3B%0D%0A%20%20%7D%0D%0A%7D%0D%0A%0D%0Alet%20two%20%3D%20sum(1%2C%202)%3B%0D%0Alet%20one%20%3D%20sum(1)%3B)

## 默认参数

在 TypeScript 中，我们能给函数的参数添加默认值：
```js
function sum(x: number, y: number = 2): number{
  return x + y;
}

let two = sum(2, 3); // 输出 2+3 = 5
let one = sum(1); // 输出 1+2 = 3
```

## 剩余参数

在 TypeScript 中，可以使用 `...` 的方式获取函数中的剩余参数，剩余参数可以没有，也可以有任意个：

```js
// string[] 表示为数字字符串
function sum(x: string, ...y: string[]){
  return x + y.join("");
}

let a = sum('1', '2', '3', '4');  
// 输出 1234，x为'1'，y即为'2', '3', '4'
```
[在线演示](https://www.tslang.cn/play/index.html#src=function%20sum(x%3A%20string%2C%20...y%3A%20string%5B%5D)%7B%0D%0A%20%20return%20x%20%2B%20y.join(%22%22)%3B%0D%0A%7D%0D%0A%0D%0Alet%20a%20%3D%20sum('1'%2C%20'2'%2C%20'3'%2C%20'4')%3B%0D%0Aconsole.log(a)%0D%0A)


注：rest 参数应该在最后一个参数位置定义。

## 接口定义函数

还有一种是用接口的方式来定义，关键字为 "interface"：
```js
interface Sum{
  (x: number, y: number): number
}

let sum: Sum
sum = function(x: number, y: number){
  return x + y
}
let a = sum(2, 3); // 输出 2+3 = 5
```
[在线演示](https://www.tslang.cn/play/index.html#src=interface%20Sum%7B%0D%0A%20%20(x%3A%20number%2C%20y%3A%20number)%3A%20number%3B%0D%0A%7D%0D%0A%0D%0Alet%20sum%3A%20Sum%3B%0D%0Asum%20%3D%20function(x%3A%20number%2C%20y%3A%20number)%7B%0D%0A%20%20return%20x%20%2B%20y%0D%0A%7D%0D%0Alet%20a%20%3D%20sum(2%2C%203)%3B%20%2F%2F%20%E8%BE%93%E5%87%BA%202%2B3%20%3D%205%0D%0Aconsole.log(a)%0D%0A)

## 重载

TypeScript 允许你声明函数重载。JavaScript 本身是个动态语言，本身没有重载一说，但是它可以根据传入不同的 arguments 参数而返回不同操作，模拟重载。

先看看用正统类型来实现：

```js
function sum(x: number, y?: number, z?:number): number | string{
  if (y === undefined && z === undefined) {
    y = z = x;
  } else if (z === undefined) {
    z = x;
  }
  return x + y + z
}

let a = sum(2); // 输出 2+2+2 = 6
let b = sum(2, 3); // 输出 2+3+2 = 7
let c = sum(2, 3, 4); // 输出 2+3+4 = 9
```

[在线演示](https://www.tslang.cn/play/index.html#src=function%20sum(x%3A%20number%2C%20y%3F%3A%20number%2C%20z%3F%3Anumber)%3A%20number%20%7C%20string%7B%0D%0A%20%20if%20(y%20%3D%3D%3D%20undefined%20%26%26%20z%20%3D%3D%3D%20undefined)%20%7B%0D%0A%20%20%20%20y%20%3D%20z%20%3D%20x%3B%0D%0A%20%20%7D%20else%20if%20(z%20%3D%3D%3D%20undefined)%20%7B%0D%0A%20%20%20%20z%20%3D%20x%3B%0D%0A%20%20%7D%0D%0A%20%20return%20x%20%2B%20y%20%2B%20z%0D%0A%7D%0D%0Alet%20a%20%3D%20sum(2)%3B%20%2F%2F%20%E8%BE%93%E5%87%BA%202%2B2%2B2%20%3D%206%0D%0Aconsole.log(a)%0D%0A)

再看看使用 TypeScript 的重载实现：

```js
function sum(all: number): number | string;
function sum(x: number, y: number): number | string;
function sum(x: number, y: number, z: number): number | string;
function sum(x: number, y?: number, z?:number): number | string{
  if (y === undefined && z === undefined) {
    y = z = x;
  } else if (z === undefined) {
    z = x;
  }
  return x + y + z
}
let a = sum(2); // 输出 2+2+2 = 6
let b = sum(2, 3); // 输出 2+3+2 = 7
let c = sum(2, 3, 4); // 输出 2+3+4 = 9
```
[在线演示](https://www.tslang.cn/play/index.html#src=function%20sum(all%3A%20number)%3B%0D%0Afunction%20sum(x%3A%20number%2C%20y%3A%20number)%3B%0D%0Afunction%20sum(x%3A%20number%2C%20y%3A%20number%2C%20z%3A%20number)%3B%0D%0Afunction%20sum(x%3A%20number%2C%20y%3F%3A%20number%2C%20z%3F%3Anumber)%3A%20number%20%7C%20string%7B%0D%0A%20%20if%20(y%20%3D%3D%3D%20undefined%20%26%26%20z%20%3D%3D%3D%20undefined)%20%7B%0D%0A%20%20%20%20y%20%3D%20z%20%3D%20x%3B%0D%0A%20%20%7D%20else%20if%20(z%20%3D%3D%3D%20undefined)%20%7B%0D%0A%20%20%20%20z%20%3D%20x%3B%0D%0A%20%20%7D%0D%0A%20%20return%20x%20%2B%20y%20%2B%20z%0D%0A%7D%0D%0Alet%20a%20%3D%20sum(2)%3B%20%2F%2F%20%E8%BE%93%E5%87%BA%202%2B2%2B2%20%3D%206%0D%0Alet%20b%20%3D%20sum(2%2C%203)%3B%20%2F%2F%20%E8%BE%93%E5%87%BA%202%2B3%2B2%20%3D%207%0D%0Alet%20c%20%3D%20sum(2%2C%203%2C%204)%3B%20%2F%2F%20%E8%BE%93%E5%87%BA%202%2B3%2B4%20%3D%209%0D%0Aconsole.log(c)%0D%0A)

有好多人觉得 TypeScript 的重载存在的意义不大：

1. TypeScript 的重载是为了给调用者看，方便调用者知道该怎么调用（同时 tsc 也会进行静态检查以避免错误的调用）。

2. 有助于IDE的代码提示，以及返回值的类型约束。

注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。

## 柯里化

柯里化属于比较高级的函数应用，在这里顺带提一下，平时用得很多内置函数例如：`map`、`filter`、`redeuce`等就是柯里化函数，仅仅需要使用一系列箭头函数：

```js
// 一个柯里化函数
let add = (x: number) => (y: number) => x + y;

// 简单使用
add(123)(456);

// 部分应用
let add123 = add(123);

add123(456);
```
