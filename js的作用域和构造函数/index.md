# JS的作用域和构造函数

### 作用域
顶级作用域   > test内部执行期间作用域 > 返回的新函数对象执行期间的作用域(abcd)
```html
var abc = 1234;
			// 顶级作用域
			function test(){
				// test内部执行期间作用域
				var t1 = "11111111111111";
				
				var abcd = "hello world";
				return function (){
					// 返回的新函数对象执行期间的作用域
					console.log(abcd);
					console.log(t1);
				};
			}
```
