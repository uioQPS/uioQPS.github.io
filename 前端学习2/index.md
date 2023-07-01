# 前端学习2


单选框
```html
<input id="t1" type="radio" name="gender" value="1" />1
<input id="t2" type="radio" name="gender" value="2" />2
<input id="t3" type="radio" name="gender" value="3" checked />3
<input id="t4" type="radio" name="gender" value="4" />4
<input id="t5" type="radio" name="gender" value="5" />5
<input id="t6" type="radio" name="gender" value="6" />6
```
输入框
```html
<input name="user_id" value="005" type="text"  />
<textarea name="content"  maxlength="10" placeholder="please write content..." required>
</textarea>
```
下拉框
```html
<select name="country" >
	<option value="China">China</option>
	<option value="UK">UK</option>
	<option value="Japan">Japan</option>
	<option value="USA">USA</option>
</select>
```
submit按钮
```html
<input type="submit" value="Submit" />
```
有序列表
```html
<ol>
	<li>vgfb</li>
	<li>derrf</li>
	<li>yus</li>
</ol>
```
无序列表
```html
<style>
		ul{
			list-style: none;
		}
		ul li{
			padding: 20px 10px; //列表项行距
		}
		ul li:hover{
			background: aqua;
			cursor: cell;
			border-style: solid; //鼠标选中效果
		}
</style>
<ul>
	<li>vgfb</li>
	<li>derrf</li>
	<li>yus</li>
</ul>
```
