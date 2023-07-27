# 父元素塌陷问题


### float导致的父元素高度塌陷
在文档流中，当父元素的高度设置为自适应的时候（height：auto），此时父元素的高度默认会被子元素撑开。但是当为我们子元素设置浮动（float）以后，子元素就会完全脱离文档流，导致子元素无法撑开父元素高度的情况出现。
#### 流式布局
一种等比例缩放布局方式，在CSS代码中使用百分比来设置宽度，也称百分比自适应的布局。
可以保证当前屏幕分辨率发生改变的时候，页面中的元素大小也可以跟着改变，所以流式布局是移动端开发常用的一种布局。
#### 浮动布局
元素的水平方向浮动，意味着元素只能左右移动而不能上下移动。

一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止。

浮动元素之后的元素将围绕它。

浮动元素之前的元素将不会受到影响。

如果图像是右浮动，下面的文本流将环绕在它左边：

<div align=center>
<img src="/images/0727.png" width=800px height=600px />
</div>

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8"/>
<title>浮动布局</title>
<style type="text/css">
.container{
	width: 970px;
	margin: 0 auto;
}
.head, .foot{
	clear: both;
	margin: 10px auto;
}
.middle{
	clear: both;
	margin: 10px auto;
}
.item-1{
	background-color: red;
	width: 277px;
	height: 103px;
	float: left;
}
.item-2{
	background-color:chartreuse;
	width: 137px;
	height: 49px;
	float: right;
}
.item-3{
	background-color:chartreuse;
	width: 679px;
	height: 46px;
	float: right;
	margin-top: 8px;
}
 
.item-4{
	background-color:gold;
	width: 310px;
	height: 435px;
	float: left;
	margin-right: 10px;
}
.item-5{
	background-color:lightblue;
	width: 450px;
	height: 240px;
	float: right;
	margin-right: 10px;
}
.item-6{
	background-color: lightblue;
	width: 450px;
	height: 110px;
	float: right;
	margin-top: 10px;
	margin-right: 10px;
}
.item-7{
	background-color: lightblue;
	width: 450px;
	height: 30px;
	float: right;
	margin-top: 10px;
	margin-right: 10px;
}
.item-8{
	background-color: purple;
	width: 190px;
	height: 400px;
	float: right;
}
.item-9{
	background-color: green;
	width: 650px;
	height: 25px;
	float: right;
	margin-top: 10px;
}
.item-10{
	background-color: blue;
	width: 970px;
	height: 35px;
}
/* 清除浮动 */
.clearfix:after{
  content:""; 
  display:block; 
  visibility:hidden; 
  height:0;
  clear:both;
}
.clearfix{zoom:1;}
</style>
</head>
<body>
<div class="container">
	<div class="head clearfix">
		<div class="item-1"></div>
		<div class="item-2"></div>
		<div class="item-3"></div>
	</div>
	<div class="middle clearfix">
		<div class="item-4"></div>
		<div class="item-8"></div>
		<div class="item-5"></div>
		<div class="item-6"></div>
		<div class="item-7"></div>
		<div class="item-9"></div>
	</div>
	<div class="foot">
		<div class="item-10"></div>
	</div>	
</div>
</body>
</html>
```
#### 如何解决父元素塌陷？
1. 给父元素添加固定高度。
（缺点：添加了固定高度的父元素高度不再自适应）
2. 给父元素添加属性overflow: hidden;
这时父元素就变成bfc block formatting context
（缺点：当子元素有定位属性时,容器以外的部分会被裁剪掉)
3. 在子元素的末尾添加一个高度为0的空白 div来清除浮动属性。
（缺点：在页面中添加无意义的div会造成代码冗余）
4. 给父元素中添加一个伪元素
```html
 .父元素:after{
                 content: "";
                 height: 0;
                 clear: both;
                 overflow: hidden;
                 display: block;
                 visibility: hidden;
            } 
```
当高度塌陷是因为position设置为absolute所导致的时候，上面的四种方法除了第一种把高度写死之外，其它三种方法都是无效的。
解决父元素高度塌陷的通常解决办法是在父元素中开启BFC。当子元素脱离文档流的原因是float，则可以通过开启BFC解决。但是如果子元素脱离文档流是因为absolute或者fixed，则开启BFC同样不管用。
{{< admonition abstract >}}
内联元素又名行内元素(inline element)，和其对应的是块元素(block element)，都是html规范中的概念。
{{< /admonition >}}
