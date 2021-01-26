## 事件触发的三个阶段

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
   
   返回一个布尔值，表明当前事件是否会向DOM树上层元素冒泡</br>
 
 * event.cancelable(只读):
  
   事件的cancelable属性表明该事件是否可以被取消默认行为，如果该事件可以用preventDefault()可以阻止与事件关联的默认行为，则返回true,否则为false。如果该事件的cancelable属性为false,则该事件的监听器无法阻止默认行为，调用prenentDefault()将产生错误。
 * event.currentTarget：

   当事件遍历DOM时，标识事件的当前目标。它总是引用事件处理程序附加到的元素，而不是event.target,event.target标识事件发生的元素
 
 当事件遍历DOM时，表示事件的当前目标。
 
 ```
 function hide(e){
 e.currentTarget.style.visibility = "hidden";
 console.log(e.currentTarget);
 //该函数用于事件处理器时：this === e.currentTarget
 }
 
 var ps = document.getElementsByTagName('p');
 
 for(var i = 0;i < ps.length;i++){
 //console:打印被点击的p元素
 ps[i].addEventListener('click',hide,false);
 }
 //console:打印body元素
 document.body,addEventListener('click',hide,false);
 
 ```
 上述代码运行后，点击网页中的p节点，由于注册的事件监听器都是冒泡属性，所以会依次打击点击的p节点和body节点。
 
 * event.target：
  
    一个触发事件的对象的引用。
 * event.defaultPrevented(只读)：
   
   返回一个布尔值，表明当前事件是否调用的event.preventDefault()方法
 * event.type(只读)：
 
   只读属性Event.type会返回一个字符串，表示该事件对象的事件类型。
   
 **方法**
 
 * preventDefault():
 
   该方法可以禁止一切默认的行为，例如点击a标签时，会打开一个新页面，如果为a标签监听事件click同时调用该方法，则不会打开新页面。
 
 * event.stopPropagation() 
 
   阻止捕获和冒泡阶段中当前时间的进一步传播。
   
## 事件委托

事件监听后，检测顺序就会从被绑定的DOM下滑到触发的元素，再冒泡会绑定的DOM上。如果你监听了一个DOM节点，那就等于你监听了其所有的后代节点。

使用事件委托能够避免对特定的每个节点添加事件监听器；事件监听器是被添加到它们的父元素上。事件监听器会分析从子元素冒泡上来的事件，找到是哪个子元素的事件，利用事件冒泡的特性，将里层的事件委托给外层时间。根据event对象的属性进行事件委托，改善性能。
```
var ul  = document.creatElement('ul');
document.body.appendChild(ul);

var  li1 = document.createElement('li');
var li2 = document.createElement('li');
ul.appendChild(li1);
ul.appendChild(li2);

function hide(e){
e.target.style.visibility = 'hidden';
}

ul.addEventListener('click',hide,false);
```
