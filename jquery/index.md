# Jquery



{{< admonition abstract >}}
jquery 是一个轻量化的JavaScript代码库，支持多种浏览器，主要特性为支持CSS3的全部选择器，方便选取各种元素，以及对于元素进行css样式定义，以及获取计算的样式
{{< /admonition >}}

###  选择和创建元素
$("</div>") $("#productprice")     
选择(tag)和创建(</>)元素，产出的是类数组的DOM集合
创建多个标签
### jquery对象和DOM元素对象
    
    jquery对象是一个类数组对象，取第一个[0]是对应的DOM元素
```html
    a=createTableNaive()
	b=$(a) 
	b[0]=a
```
	
### append方法
 a.appendChild(c) 返回是<tr>对象    
 a.append(c) 返回的是数组对象可以继续加    
 

 元素有父元素parentNode removeChild   
 
 
 父元素.append(子元素) appendTo是 子元素.appendTo(父元素) jQuery有一个方法设计规则，就是基本上所有的操作方法都是返回的是左边执行的this对象，但是find chileren方法除外，因为find children方法是在父元素向下寻找子元素集合的方法。    

### 区别于js 
js中：var item_item=$("item").value 只能选择id，而不能选择td         
通过id取到输入框input，才有value属性；td对象没有value值    


### 事件处理函数
有一个参数，参数是事件对象，这个事件对象里面有很多信息，比如那个键盘按键被按下，鼠标点击事件触发的位置，以及currentTarget就是处理当前事件的当前对象，target是说引发当前事件的根源对象
            // preventDefault会屏蔽事件的默认行为，比如a标签的点击事件，preventDefault会不让标签进行跳转
            // stopPropagation 会屏蔽事件的继续传播，不论是在捕获阶段还是在冒泡阶段，都会停止后面的事件传播。但是停止事件传播并不代表屏蔽默认行为。
            // mouseover 鼠标移上去 mousemove 鼠标在元素上移动 mouseout 鼠标移出
            // click 是单击事件 dblclick是双击事件，但实际上也会先触发两次click单击事件以后，再触发一次双击事件dblclick
            // mousedown mouseup


### getComputedStyle方法
window.getComputedStyle是获取DOM元素计算后的全部样式信息对象，也是一个类数组对象，数组的内容是所有的样式属性
    // 里面的所有的属性名称必须是animation-delay这样比如
    // a = window.getComputedStyle(document.getElementById('test1'));  console.log(a.animationDelay); console.log(a['animation-delay']); 这两个是等价的
    // 同理，jQuery 之所以能够获取当前的背景色，jQuery的写法是$("#test1").css("background-color");
    // $("tr")意思是说选定当前页面上所有的tr元素，但是
    // $("<tr>") 意思是说创建一个新的空白的tr标签    

我现在想要获取到test1元素的背景色是多少，一般来说定义它的背景色可以这么写
    //document.getElementById("test1").style.background = "yellow !important";
    //document.getElementById("test1").style.setProperty("background", "yellow", "important");
    function $A(s){
        return document.getElementById(s);
    }
    //console.log($A("test1").style.background);
    //console.log(window.getComputedStyle($A("test1"))["background-color"]);
    //console.log($("#test1").css("background-color"));
    //$("#test1").css();

### cloneNode方法    
cloneNode是克隆本节点对象为一个新对象，cloneNode有一个参数，如果不传，就只克隆本元素自己，如果传true就把自己包含所有的子元素都克隆一遍。
