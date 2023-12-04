# Vue

## Vue
### 数据的双向绑定
- 数据层（Model）：应用的数据及业务逻辑      
- 视图层（View）：应用的展示效果，各类UI组件     
- 业务逻辑层（ViewModel）：框架封装的核心，它负责将数据与视图关联起来  
        数据变化后更新视图    
        视图变化后更新数据    
`MVVM`这里的控制层的核心功能便是 “数据双向绑定”  

### Vue指令
#### v-if
- 跟在某一个标签后
- 后面的变量为true，这个元素就存在，否则消失
```html
<span v-if="seen">Now you see me</span>
<button @click="toggle">切换显示与隐藏</button>
```
toggle是需要在methods里指定的方法名称   
toggle:function(){this.seen=!this.seen}   
改变变量的方法

#### v-bind
v-bind把某个数据属性的值绑定到相应的DOM属性上去，非事件属性   
```html
<span v-bind:title="message">鼠标放上去有message变量显示</span>
```
v-on绑定的是事件属性

#### v-on
```html
<button v-on:click="reverse">reverse</button>
```
reverse是method中指定的方法

#### v-for
数组循环，可以进行数组对应的输出
```html
<li v-for="todo in todos">
{{ todo.text }}}
</li>
```
data需要指定todo的字典数组      
todos:[{text: 'learn Vue'},{text:'hello Vue'},{}]    

#### Vue语法
div的id指定的是`<script>`中`new`的Vue      
三要素：
- el：指定DOM节点的定位符 
- data: {},
- methods: {}


#### 资源推荐
1. 面试官推荐 https://vue3js.cn/interview/vue/structure.html
2. 实现数据绑定的做法：发布者-订阅者模式（backbone.js）
![](Vue/1.jpg)

   

