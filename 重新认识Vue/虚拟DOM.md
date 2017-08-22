# 虚拟DOM

例如：

	<ul>
		<li></li>
		<li></li>
		<li></li>
		<li></li>
		<li></li>
	</ul>

如果要用dom实现这个操作的话，需要：

	var eleUl = document.createElement("ul");
	for(var i = 0;i < num;i++ ){
		var eleLi = document.createElement("li");
		eleUl.appendChild(eleLi);
	}
这样做的坏处是什么呢：
每一次DOM操作都很消耗性能。这尚且不涉及重绘和回流，但是可以试一下：

	var myDiv = document.createElement("div");
	for(var item in myDiv){
		console.log(item);
	}
会发现创造一个DOM元素的消耗很大。

为了优化DOM操作，引入了虚拟DOM的概念。

在我的理解中，虚拟DOM就是创建一个模仿DOM结构的JS构造函数，通过该构造函数创建类似于DOM元素的对象。进行DOM操作时变为进行JS对象的操作，操作结束后将JS对象的值赋给DOM元素，再挂载到DOM树上。

虚拟DOM很多时候并不是最优的操作，但是对于庞大而复杂的页面结构，虚拟DOM能在效率和可维护性之间打到平衡。

另外，虚拟DOM可以提供一个中间层，让JS去写UI。