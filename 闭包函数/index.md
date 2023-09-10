# 闭包函数

### 闭包函数怎么写（函数内部的函数）
```html
document.addEventListener("mouseup", mouseUpHandler);
				return {
					cancel: function(){
						elem.removeEventListener("mousedown", mouseDownHandler);
						document.removeEventListener("mousemove", mouseMoveHandler);
						document.removeEventListener("mouseup", mouseUpHandler);
					}
				};
btn.onclick = function(){
					$("orderinfo").getElementsByTagName("tbody")[0].removeChild(row);
				};
return{
	getsum:function(){
	   return amount;
	}
}

```

### 闭包函数怎么调用
```html
var draggables = {};
			function enableDrag(id){
				if (!draggables[id]) {
					draggables[id] = dragInit($(id));
				}
			}
			function disableDrag(id) {
				if (!draggables[id]) {
					return;
				}
				draggables[id].cancel();
				delete draggables[id];
			}
```
			

### 添加总金额的功能
```html
td_amount.className="rowAmount";//把要加的项加上类名，便于选择
<tfoot>
			<td colspan="3">总金额</td>
			<td colspan="2" id="totalAmount">0</td>
</tfoot>
//表格加入一行固定的总金额在最后 id是totalAmount
function calcDraw(){
			var cellection=document.querySelectorAll("#item_table tbody .rowAmount")
			var amount=0;
			for(var i= 0;i<collection.length;i++){
				amount+=Number(collection[i].innerHTML);
			}
			$("totalAmount").innerHTML=amount;
		}
//修改totalAmount 的值，通过选择器指定类名来累加得到值
```


