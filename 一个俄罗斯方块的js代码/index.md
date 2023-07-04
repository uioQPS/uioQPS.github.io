# 一个俄罗斯方块的js代码


## 定义initGame函数
初始化stack的二维数组
根据键盘判断左右移动方向
设置pause变量
调用游戏开始函数
## 定义endGame函数
timer
## 定义runGame函数
```html
if (!currentBlock) {
		if (stack[3][9]) {
			endGame();
			return;
		}
		currentBlock = [4, 10];
		redraw();
		return;
	}
	if (canMove(currentBlock)) {
		moveDown(currentBlock);
	} else {
		stack[currentBlock[0] - 1][currentBlock[1] - 1] = true;
		currentBlock = null;
	}
	checkClear();
	redraw();
```
## 定义canMove函数
```html
var can;
	var max;
	for (var i = 0; i < stack[currentBlock[0] - 1].length; i++) {
		if (stack[currentBlock[0] - 1][i]) {
			max = i + 1;
		}
	}
	if (block[1] - 1 > 0 && (block[1] - max > 1 || !max)) {
		return true;
	} else {
		return false;
	}
```
## 定义reDraw函数
## 定义makeElement函数
## 定义moveDown函数
## 定义pressLeft函数
## 定义checkClear函数
## 定义togglepause函数
