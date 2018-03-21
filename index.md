# JS 101

## 变量声明

### 1. 使用`var`声明变量

1. 用法
	
	```javascript
	var a,b,c=1;
	
	var a; var b; var c=1;
	```

1. 作用域
	`var`声明的变量作用域：函数级/全局
	
	```javascript
	var a=1;
	
	function foo () {
		var b = 2;
		console.log(a);
		console.log(b);
	}
	
	console.log(a);
	console.log(b); // ReferenceError
	```
	
1. 变量声明提升

	变量的声明提升至当前作用域的开始
		
	```javascript	
	function foo(){
		console.log("v:" + v);
	   	var v='hello world';
	}
	```
	```javascript
	var i = 5;  
	function bar() {  
	    console.log(i);  
	    if (true) {  
	        var i = 6;  
	    }  
	}  
	```

### 2. 使用`let`声明块级变量
1. 用法类似var
1. 作用域
 
	- `let`声明的变量作用域：块级	
	- 在JS中由一对`{}`括起来的语句集，都称作是一个块。

		```javascript
		let a = 1;
		
		function foo() {
			let b = 2;
			
			if(true){
				let c = 3
				console.log("block c:" + c);
			}
			
			console.log("function b:" + b); // ReferenceError
			console.log("function c:" + c); // ReferenceError
		}
		
		console.log("global:" + a);
		console.log("global:" + b); // ReferenceError
		console.log("global:" + c); // ReferenceError
		
		```

	
	
### 3. 使用`const`声明常量
1. 声明时必须初始化
2. 初始化后值不可更改


> 为什么需要用let

###4. 三种声明方式异同

|    关键字    |  初始化 |	声明提升	|	作用域     |      是否允许重复声明    |
| ----------  |  ----- |	------		|	--------  |      -------------     |
|    var      |   --   |	Y			|	函数级/全局 |            Y          |
|    let      |   --   |	N			|	块级       |            N          |
|    const    |    Y   |	N			|	块级       |            N          |

## ES6语法
1. ### 解构赋值

	>解构赋值语法是一个 Javascript 表达式，这使得可以将值从数组或属性从对象提取到不同的变量中。
	
	- 数组的解构赋值

		```javascript
		let a, b;
		[a, b] = [1, 2];
		console.log(a); // 1
		console.log(b); // 2
		```
		
		```javascript
		let a, b;
		[a, b, ...rest] = [1, 2, 3, 4];
		console.log(a); // 1
		console.log(b); // 2
		console.log(rest); // [3, 4]
		```
		
	- 对象的解构赋值

		```javascript
		let a, b;
		{a, b} = {b: 1, a: 2};
		console.log(a); // 2
		console.log(b); // 1
		```
		
		```javascript
		({a, b, ...rest} = {a: 1, b: 2, c: 3, d: 4});
		console.log(a); // 1
		console.log(b); // 2
		console.log(rest); //{c: 3, d: 4}
		```
2. ### 剩余参数(rest parameter)

	>用`...`表示，剩余参数语法允许我们将一个不定数量的参数表示为一个数组。
	>剩余参数也可以被解构
	
	```javascript
	function foo(a, b, ...rest) {  
   		return rest;
	}
	
	foo(1, 2, 3, 4, 5 ); // [3, 4, 5]
	```
			
	####rest parameter 和 arguments
	- rest: 只包含那些没有对应形参的实参， 数组实例。
	- arguments: 包含了传给函数的所有实参， 类数组对象。
	
3. ### 扩展运算符(spread)
	>扩展运算符也是`...`，可以理解为是rest parameter 的逆运算。即讲一个数组转为有逗号分割的参数序列。
	
	```javascript
	function foo(a, ...rest) {
	  console.log(...rest);
	}
	
	const b = [1,2,3];	
	
	foo(...b) // 2, 3
	```
	
4. ### 模板字符串

	```javascript
	//字符串拼接
	let person = {
		name: 'Neo',
		age: 17
	}
	
	console.log('Name:' + person.name + 'Age:' + person.age);
	```
	
	```javascript
	//模板字符串
	let person = {
		name: 'Neo',
		age: 17,
	}
	
	console.log(`Name: ${person.name} Age: ${person.age}`);


## 闭包(Closures)
> https://github.com/workshopper/scope-chains-closures


### 1.作用域
- 词法作用域：函数作用域在 <span style="font-size:20px" >**函数定义时**</span> 已经确定（静态作用域）
- 动态作用域：函数作用域在 <span style="font-size:20px" >**函数执行时**</span> 确定
- 块级作用域：有一对大括号 <span style="font-size:20px" >**{}**</span> 括起来的代码块，是块级作用域

	```javascript
	var value = 1;
	
	function foo() {
	    console.log(value);
	}
	
	function bar() {
	    var value = 2;
	    foo();
	}
	
	bar();
	```
	
### 2.作用域链
	- 作用域可以互相嵌套
	- 每一个嵌套的内部范围可以访问外部范围的变量
	- 由内而外，形成一个作用域链

### 3.全局作用域和变量遮蔽

#### 全局作用域

所有的JS运行时环境都必须隐式创建一个全局作用域对象（浏览器中是window，node中是global），这个对象就位于作用域链的顶端.

>JS运行环境根据以下算法来赋值一个变量：

> 1. 查找当前作用域
> 2. 如果没找到，查找直接外部作用域
> 3. 如果找到，至6
> 4. 如果没找到，重复2和3直到到达全局作用域
> 5. 如果在全局作用域没有找到，创建之（在window或global对象上）
> 6. 赋值

Note: 当不使用var或者let等定义变量的时候，这个变量就被定义为全局变量

#### 变量遮蔽

## JS集合运算

## Javascript数据类型

### 类型总览

1. 五种简单类型：Undefined, Null, Boolean, Number和String
2. 复杂数据类型：Object
3. 数据类型是动态绑定的
4. typeof 关键字

>Try it yourself
```javascript
typeof '5' = ?
typeof a=>a = ?
typeof typeof 5 = ?
typeof [1,2,3] = ?
typeof {a:5} = ?
typeof undefined = ?
```

### 简单数据类型
#### Undefined

只有一个值，即undefined
```javascript
let sss;
console.log(typeof sss); //Undefined
console.log(typeof nonExist); //Undefined
````
#### Null
只有一个值，即null，代表空对象指针。
````javascript
let aNull = null;
console.log(typeof aNull); //'Object'
````

> Null与Undefined的区别是什么？

#### Boolean

布尔值：true和false
````javascript
let jsAwesome = true;
let jsSucks = false;
````

##### *Boolean(expression)* 判断一个值是否是truthy的
````javascript
Boolean('false');
Boolean(-5);
Boolean('');
Boolean(a=>a);
````

##### 小心隐式类型转换
````javascript
const isJsAwesome = ‘sss’;
if(isJsAwesome) {
console.log('Awesome');
}
````

#### Number
代表所有数字，包括整数和浮点数
````javascript
let intNumber = 5;
let floatNumber = 5.5;
let octalNumber = 0o6;
let hexNumber = 0xA;
````

````javascript
// 小心浮点数计算
0.1+0.2===0.3 // 'false'
````

##### 数值范围常量
Number.MIN_VALUE, Number.MAX_VALUE, Infinity, -Infinity, NaN

>试着用typeof关键字查查这些常量的类型

##### 数值变换函数
Number(anything), parseInt(str, base), parseFloat(str)
````javascript
Number('5'); // 5
Number('5.5'); // 5.5
Number(true); // 1
parseInt('100', 2); // 4
````

#### String
字符串
````javascript
let aString = 'This is a string.';
````
##### 类型转换
````javascript
aNumber.toString(5); //'5'
String(1234); // '1234'
````
>String()与toString()的区别是什么?

> Try it yourself
````javascript
null.toString() = ?
String(null) = ?
````

### 复杂数据类型
#### Object
Object - Javascript的所有对象
````javascript
let cardA = new Object();
cardA.name = 'Tom';
let cardB = {name: 'Tom'};
````

Object是一种引用类型。
```javascript
let objectA = {name: 'Alice'};
let objectB = objectA;
objectB.name = 'Tom';
objectA.name = ?
```

### 常用操作符
1. 一元操作符: ++,--,+,-
```javascript
let a = 5;
a++; // a = 6
let b = '5'
b++; // b = 6+
```
2. 布尔操作符：&&,||,!
3. 加减乘除：+,-,*,/
4. 关系操作符：>，<
```javascript
23>3; // true
'23'>'3'// ?
```
5. 相等操作符: ==, ===, Object.is()

````javascript
null == undefined //true
NaN == NaN //false
false == 0 //true
true == 1 //true
true == 2 //false
"5" == 5 //true
Object.is(5, 5); //true
````

### 类型总结
1. Javascript有五种数值类型，一种引用类型。
2. typeof 关键字
3. 类型是动态绑定的，不同类型进行运算时会发生隐式类型转换。应尽可能避免隐式类型转换
4. 永远不要使用 == 

## 对象进阶

Javascript是基于**原型**的语言，而不是基于类的。**原型**是可变的。
### 对象的声明
```javascript
const person = {name: 'David'};
const name = 'David';
const personB = {name}; // Equivalent with personB = {name: name}
// 基于构造函数创建对象
const People = (name, gender)=> {this.name=name;
				 this.gender=gender; this.displayName=()=>console.log(this.name);}; // 构造函数
const aPerson = new People('David', 'male');
aPerson.name; // 'David'
```

### 对象的取值
````javascript
const person = {name: 'David', gender: 'male'};
const name = person.name;
// Alternate: name = person['name'], we always prefer the previous version
// ES6 version const {name, gender} = person;
````

> Try it yourself
````javascript
const book = {author: {name:'David', gender:'male'}};
// 请使用ES6格式在一行代码中取到name和gender的数值

````

### Array
Array(数组)是一种非常重要的对象。
````javascript
const aArray = [1,2,3,4];
aArray[1]; // 2
````
> Try it yourself
````javascript
let aArray = [1,2,3,4];
typeof aArray; // ?
````

#### Array的一些常用属性
length,
修改元素：push(), pop(),sort(),reverse(),concat()

元素迭代：every(), filter(),map(), some()

检测数组：Array.isArray()

> Try it yourself
````javascript

````

### 函数
函数是一个对象，这使他成为Javascript世界的一等公民。

````javascript
(a=>a) instanceof Object; // true
````
#### 创建函数
````javascript
function hello(name){
	console.log(`Hello, ${name}!`);
} // 函数声明

const hello2 = function(name){
	console.log(`Hello, ${name}!`);
} // 函数表达式

const hello3 = name=>console.log(`Hello, ${name}!`);

````
> 使用箭头函数创建一个函数，这个函数接受三个参数，然后打印这三个参数的平方的和: e.g., (1,2,3)=>1+4+9=14
#### this

#### 高级函数
1. 将函数作为参数的函数。
2. 将函数作为返回值的函数。
````javascript
// 这是一个高阶函数
const makeHelloPlayer = name=>{
	const text = `Hello, ${name}`;
	return ()=>console.log(text);
}
````

##### 将函数作为参数的函数
````javascript
const aArray = [1,2,3,4];
const double = aArray=>aArray.map(x=>x*2+3); // 将array中所有元素的值乘以2再加3
````

#### 将函数作为返回值的函数
````javascript
const input = [1,2,3,4];
const trible = aArray=>aArray.map(x=>x*3+1);
const multiplyBy = number=>aArray=>aArray.map(x=>x*number+1);
const multiplyBy6 = multiplyBy(6);
multiplyBy6(aArray);  // [4,7,10,13]
````

> Try it yourself
````javascript
// 定义一种符号(*): a(*)b= a+a*b
const customMultiply = (a,b)=>a+a*b;
// 定义一种符号(+): a(+)b= 2*a+b
const customAdd = (a,b)=>2*a+b;

// 声明一个高阶函数，接受(*)和(+)的计算函数，以计算a(*)b(+)b
const createMultiplyAndAdd = 'PUT YOUR CODE HERE';
const customMultiplyAndAdd = createMultiplyAndAdd(customMultiply, customAdd);
customMultiplyAndAdd(2,3); // 2(*)3(+)3 = 8(+)3 = 19 


````