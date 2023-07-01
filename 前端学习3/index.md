# 前端学习3

Relative和absolute两种定位方式
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

