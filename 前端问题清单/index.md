# 前端问题清单

#### 布局相关
1. margin-top:5px 上边距
2. a标签的文字居中？     
a标签如果是内联元素，内联元素在一个块级元素内部怎么垂直居中：改变父元素行高line-height写成和父元素的高度一样，它内部的内联元素垂直方向上就剧中了。    
3. z-index        
在z轴上的层级，数大的排在数小的上面,页面上有很多绝对定位的窗口，都可能互相覆盖，谁在上面，谁在下面，就靠z-index排序.   
z-index的排序是根据父级走。relative absolute fixed都可以用z-index，默认的static静态定位不能用z-index，static不涉及元素重叠。
4. `<ul>`和`<li>`    
可以用来写目录`<ul>`相当于一个列表,`<li>`指定样式。
```html
.submenu_container, .menu_wrapper {
            padding: 0px;
            margin: 0px;
            cursor: pointer;
        }
.menu {
            float: left;
            position: relative;
            width: 20%;
            height: 64px;
            line-height: 64px;
            margin: 0px;
            padding: 0px;
            list-style: none;
            color: #eee;
            text-align: center;
        }
```
#### 样式相关
1. 	https://icons.mydrivers.com/2020/www/icon_2020.png 这种图标怎么找到定位？
2. 怎么实现下拉菜单？       
{{< admonition abstract >}}
1. js  
js实现，可以实现那种鼠标一放上去不立即显示子菜单，而是延迟0.2 0.3秒显示，因为可能用户是不小心鼠标移上去的
2. hover伪类实现   
下拉菜单是菜单的一个子元素，默认是隐藏的;在鼠标放在菜单上就是hover状态时候才显示出来
{{< /admonition >}}
3. 注意float和display inline-block不要同时写，inline-block不起作用
#### 代码相关
1. 输入框元件一定要指定name吗，有什么作用？

