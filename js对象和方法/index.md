# JS对象和方法

对象   
document   
方法   
getElementById()   
变量   
var userInfo = document.getElementById("userInfo")   
函数   
function addrow(){}   
事件绑定   
td4.innerHTML = "<a onclick='delete(this)'>删除</a>"   
a标签绑定了按钮事件   

#### A example
var row = document.createElement("tr");   
var td1 = document.createElement("td");   
row.appendChild(td1);   
tr类型能增加td类型的子元素   

1. 申明一个变量；通过一个找/新建id的方法   
var userInfo = document.getElementById("userInfo");   
var row = document.createElement("tr");   
2. 给变量赋值；通过获取id/变量名的值   
td_unitprice.innerHTML= $("productunitprice").value;   
3. 把变量加入父级   
row.appendChild(td_id);   

#### 表的结构   
table有不同的tag：tbody；   
tbody属于数组，类似于tr；   
tr标签的对象有appendchild的方法增加td元素。   
$("orderinfo").getElementsByTagName("tbody")[0].appendChild(row[a]);   

#### 绑定事件   
```html
myButton.onclick = function(){
			$("orderinfo").getElementsByTagName("tbody")[0].removeChild(row);
		};
```





