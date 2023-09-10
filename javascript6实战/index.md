# JavaScript6实战


- 浏览器里面的JavaScript分为主要两部分，一部分是JavaScript的核心编程，就是基础语法，函数，string boolean number 关系表达式，运算表达式等 
- 另外一部分是DOM编程，就是宿主对象上面的编程，在网页开发里面，就是对于document对象的编程 DOM document object model
- CSS里面有选择器和样式表，构成了网页的样式，结合html和css生成了网页页面，通过javascript给页面元素加上了响应逻辑，可以动态的修改页面。

### querySelectorAll方法
var collection = document.querySelectorAll("#orderinfo tbody .rowAmount");
// document下面的querySelectorAll是说按照css的选择器进行选定dom元素的一个方法

### appendChild方法
td_control.appendChild(btn);
//[].push 像后面追加 对应的还有prepend就是[].unshift向前追加
### Array slice reverse push pop shift unshift 
//数组有这些方法
//console.log(a.slice(0));
//console.log(Array.prototype.slice.call(a, 0));


### 构造函数
Object.defineProperty set get enumerable 
#### 使用prototype原型继承 
```html
var val;
			Object.defineProperty(a, "xxx", {
				set: function(s){
					val = s;
				},
				get: function () {
					return val;
				}
			});
			a.xxx = 222;
			a.xxx;
			for (var x in a) {
				
			}

```

### 函数的call apply方法 
```html
a ={
				x:1,
				y: function(){
					console.log(this.x);
				}
			}
			a.y();
			function test(a,b,c,d){
				console.log(this.x, a,b,c,d);
			}
			test.call(a,1,2,3,4);
			test.apply(a, [1,2,3,4]);
```


### javascript对象 instanceof方法 函数 bind方法实现 以及数组对象使用方法

			//test.prototype = new test2();
			//var xx = new test();
			//console.log(xx instanceof Object);
			
			function test2 (){
				this.yyy = 1234;
			}
			test.prototype = new test2();
			var b = new test();
			
			console.log(b instanceof Array);
			console.log(b.yyy);

### bind方法
	function bind(func, context) {
				return function(){
				// 在这里无论this是谁，结果都不会变
					return func.apply(context, Array.prototype.slice.call(arguments, 0));
				}
			}
			
			
			a= {
				aa: 1234,
				bb: function () {
					console.log(this.aa);
				}
			};
			a.bb();
			var f = a.bb;
			f();
			var g = bind(f, a);
			
			g();
			var b ={
				aa:2222,
				bb:g,
				cc:f
			};
			b.bb();
			b.cc();

### 事件冒泡
- dom对象的一个方法，叫做addEventListener，他有三个参数，第一个参数是事件名称，第二个参数是事件处理函数，第三个参数是是否是捕获期间的事件处理函数，如果不是默认是冒泡期间的函数
- onload这种方式是通过类似在冒泡阶段进行事件绑定的一种方式，但是如果后面继续使用addEventListener绑定冒泡阶段事件的话，后面的函数执行顺序会排在前面的.onload函数执行顺序的后面
- 如果.onload被赋值了多次，那么执行顺序还按照第一次onload和addEventListener确定的执行顺序进行执行
- onload事件和DOMContentLoaded时间的区别，是DOMContentLoaded事件是dom树完成，onload是dom树完成，并且所有的外部资源img js css都加载完成才会触发。
```html
window.onload = function(){
				console.log("onload handler 1");
			}
			// 等价于下面这个
			window.addEventListener("load", function(){
				console.log("onload handler 2");
			});
```

