## 变量声明

###1. 使用`var`声明变量

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

###2. 使用`let`声明块级变量
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

	
	
###3. 使用`const`声明常量
1. 声明时必须初始化
2. 初始化后值不可更改


> 为什么需要用let

###4. 三种声明方式异同

|    关键字    |  初始化 |	声明提升	|	作用域     |      是否允许重复声明    |
| ----------  |  ----- |	------		|	--------  |      -------------     |
|    var      |   --   |	Y			|	函数级/全局 |            Y          |
|    let      |   --   |	N			|	块级       |            N          |
|    const    |    Y   |	N			|	块级       |            N          |

##ES6语法
1. ###解构赋值

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
	
4. ###模板字符串

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


##闭包(Closures)
> 能够读取其他函数内部变量的函数


```javascript
	function foo() {
		var a = 'Hello'
		
		function bar() {
			return a;
		}
		
		return bar;
	}
	
	console.log(foo()());
```

##JS集合运算

