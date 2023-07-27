# 前端学习3

### 1、流式布局
- 标准文档流，从上到下，从左到右
- 内联元素
    块级元素的内联元素可定位
        display：inline-block
- 行高
### 2、浮动布局
Relative和absolute两种定位方式
- 1、 fixed 浮动固定
- 2、 absolute 绝对于父元素的定位（body）
        margin：auto
        margin-left：10px
        margin-right：10px
- 3、 relative 相对于自身定位 
        transform (-50%,-50%)
- 4、 清除浮动
        float: left
        overflow：hidden 父元素的高度随着子元素的高度变化
### 3、定位布局
- top
- left
- bottom
```html
.mainContent {
				width: 800px;
				margin: 0 auto;
			} 
.popModal {
				
				position: absolute;
				width: 800px;
				height: 400px;
				top: 50%;
				left: 50%;
				/*百分比是相对于父级定位元素相对的宽度进行计算*/
				/*margin-left: -10%; 
				margin-top: -10%;*/
				transform: translate(-50%, -50%); /*百分比是相对于元素自身宽度进行计算*/
				background: green;
				border-radius: 20px;
			}   
<body>
		<div class="mainContent">
		<!--
		position定位
		relative 相对自身的原始位置进行定位
		absolute 相对于最近的带有定位元素样式的父元素进行定位
		-->
			<img src="airplane1.jpg" />
			<div class="popModal">我是一个弹窗</div>
		</div>
</body> 
```

