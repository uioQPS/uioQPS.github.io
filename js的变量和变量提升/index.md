# JS的变量和变量提升

### JavaScript 简介
1. JavaScript是一种运行在浏览器中的解释型的编程语言。在Web世界里，只有JavaScript能跨平台、跨浏览器驱动网页，与用户交互。

2. JavaScript代码放到`<head>`中
```html
<head>
  <script>
    alert('Hello, world');
  </script>
</head>
```
3. JavaScript可以对DOM树进行增查删改的操作，对doument对象，方法进行操作。
```html
.getElementbyId()
```
4. 嵌入式JS
```html
<button onclick=" JS code"></button>
```
完成事件绑定

#### 变量
1. js里面变量声明规则，变量只能以_或者英文字母开头，后面可以包括数字英文下划线
2. javascript 里面对象描述是{开头}结尾的，然后是一堆一堆的key value值 key值如果不带引号就按变量声明规则规则处理，如果带引号，只要是合法字符串就可以
```html
var a = {
				aaa: 1234,
				12_34_555_1: "qqqq",
				"aaa-+bbb": "qqqqq",
				bbb: "wwwww",
				eee: function() {
					console.log(this);
					console.log(this.bbb);
					console.log("---------------------------");
					//console.log("12345");
				}
a.eee();
```
3. 对象的使用一种是.aaa 这种方法只能后面跟合法的变量名 如果是其他的数字类型或者非法变量名但是合法字符串只能["xxxx"]
4. 函数调用的时候，函数执行上下文对象this看函数.前面的对象就是this的值

{{< admonition abstract >}}
**变量涵盖范围**    
函数    
对象    
布尔值 true false    
字符串    
数字 isNaN（）不是数字就是true    
{{< /admonition >}}   




