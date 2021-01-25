# -## 事件触发的三个阶段

捕获阶段：window向事件触发处进行传播，寻找监听函数的过程；</br>
冒泡阶段：事件触发处向window传播，寻找监听函数的过程。</br>

```
<div id="outer">
	<p id="inner">Click!</p>
</div>
```

当点击Click的时候，是先触发div元素还是先触发p元素？

* 事件冒泡：

浏览器检查实际点击的元素是否在冒泡阶段中注册了一个事件处理程序，如果是则运行该事件处理程序，然后移动到下一个元素最直接的祖先元素，完成秀昂痛的事情，直至最外层的祖先元素。

因此在事件冒泡的概念下在P元素上发生click事件的顺序应该是:</br>
` p ->  div -> body -> html -> document `

* 事件捕获

浏览器检查最外层的元素，是否在捕获阶段中注册了一个事件处理程序，如果是，则运行它。然后，移动到最外层祖先中的下一个元素，并执行相同操作，直至到达实际点击的元素。因此在事件捕获的过程中，在P元素上发生Click时间的顺序应该是：</br>
` document -> html -> body -> div -> p`

* 事件捕获

事件监听函数:`element.addEventListener(<event-name>,<callback>,<use-capture>)`.在element对象上添加一个事件监听器，当监听到有事件发生的时候，调用这个回调函数。函数中的参数，表示该事件监听是在“捕获”阶段监听还是在“冒泡”阶段监听。如果没有指定，useCapture默认为false.

 * 其他事件监听方式
 
 1. HTML内联属性
 
 类似`<button onclick="alert('点击该按钮');">点击这个按钮</button>`的方式，这种方式会使得JS与HTML高度耦合，不利于开发和维护，不推荐使用。
 
 2.DOM属性绑定
 
 使用DON元素的onXXX属性设置，简单易懂，兼容性好。缺点是只能绑定一个处理函数。
 ```
 var btn = documetn.querySelector('button');
 btn.onclick = function(event){alert('Hello world');};
 ```
 
 * 事件捕获和事件冒泡同时存在的情况
 
 事件按照什么样的顺序出发的，按照事件触发的三个阶段依次分析就行，但是也有特例是如果给一个目标节点注册冒泡和捕获事件，事件触发会按照注册的顺序执行。
 
 ```
 //以下会先打印冒泡然后是捕获
 node.addEventListener('click',(event) => {trueconsole.log('冒泡')},false);
 node.addEventListener('click',(event) => {trueconsole.log('捕获')},true
 ```
 
 * 事件对象
 
 事件处理函数可以附加在各种对象上，包括DON元素，window对象等。当时间发生时，event对象就会被创建并以此传递给事件监听器。
 
 在处理函数中，将event对象作为第一个参数，可以访问DOM Event接口。
 ```
 function foo(evt) {
 
  alert(evt);
  }
  table_el.onclick = foo;
 ```
 
 event常用的属性和方法：
 
 **属性**
 
 * event.bubbles(只读):
 
 返回一个布尔值，表明当前事件是否会向DOM树上层元素冒泡
 * event.cancelable(只读):
 * event.currentTarget
 * event.target
 * event.defaultPrevented(只读)：
 * event.type(只读)：
 
 
