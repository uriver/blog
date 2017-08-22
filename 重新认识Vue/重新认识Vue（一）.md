# 重新认识Vue：构造函数
> 在使用Vue的时候跳过了前面的步骤，而直接用的Vue-cli，模块化的开发导致对Vue本身的使用原理不熟悉，所以现在想由浅入深慢慢探索Vue的实现。  
> 源码的解析主要都是参考网上的文章。

## 创建一个Vue应用
在index.html中引入Vue.js，然后创建一个Vue应用：

	//HTML
	<div id="app">
	  {{ message }}
	</div>

	//JS
	var app = new Vue({
	  el: '#app',
	  data: {
	    message: 'Hello Vue!'
	  }
	})
这是官网上给出的第一个例子。刚刚接触Vue的时候没有探究过，而现在看来确是研究Vue的入口。

首先，**用new操作符声明了一个APP对象**，也就是说，这里出现的Vue其实是一个构造函数。

### 构造函数源码学习

在github上clone下Vue的源码，Vue源码放在项目的src文件夹。而构造函数的最底层源码放在`src/core/instance/index.js`目录下。  
 
构造函数源码路径：  
**web-runtime-with-compiler.js -> Web-runtime.js ->src/core/index.js ->src/core/instance/index.js**  
（怎么找到的？当然是看别人的源码分析教程了！）

从最底层**src/core/instance/index.js**看起 ：

	import { initMixin } from './init'
	import { stateMixin } from './state'
	import { renderMixin } from './render'
	import { eventsMixin } from './events'
	import { lifecycleMixin } from './lifecycle'
	import { warn } from '../util/index'
	
	function Vue (options) {
	  if (process.env.NODE_ENV !== 'production' &&
	    !(this instanceof Vue)
	  ) {
	    warn('Vue is a constructor and should be called with the `new` keyword')
	  }
	  this._init(options)
	}
	
	initMixin(Vue)
	stateMixin(Vue)
	eventsMixin(Vue)
	lifecycleMixin(Vue)
	renderMixin(Vue)
	
	export default Vue
由源码可见，定义了Vue构造函数后，以该构造函数为参数，调用了五个方法，最后导出Vue。

这五个方法的作用是给vue的原型绑定各种属性或方法，怎么实现的就不管了，这不是重点！（重点是看不懂……）

调用完这五个方法后，Vue就变成这个样子了：

	// initMixin(Vue)    src/core/instance/init.js **************************************************
	Vue.prototype._init = function (options?: Object) {}
	
	// stateMixin(Vue)    src/core/instance/state.js **************************************************
	Vue.prototype.$data
	Vue.prototype.$set = set
	Vue.prototype.$delete = del
	Vue.prototype.$watch = function(){}
	
	// renderMixin(Vue)    src/core/instance/render.js **************************************************
	Vue.prototype.$nextTick = function (fn: Function) {}
	Vue.prototype._render = function (): VNode {}
	Vue.prototype._s = _toString
	Vue.prototype._v = createTextVNode
	Vue.prototype._n = toNumber
	Vue.prototype._e = createEmptyVNode
	Vue.prototype._q = looseEqual
	Vue.prototype._i = looseIndexOf
	Vue.prototype._m = function(){}
	Vue.prototype._o = function(){}
	Vue.prototype._f = function resolveFilter (id) {}
	Vue.prototype._l = function(){}
	Vue.prototype._t = function(){}
	Vue.prototype._b = function(){}
	Vue.prototype._k = function(){}
	
	// eventsMixin(Vue)    src/core/instance/events.js **************************************************
	Vue.prototype.$on = function (event: string, fn: Function): Component {}
	Vue.prototype.$once = function (event: string, fn: Function): Component {}
	Vue.prototype.$off = function (event?: string, fn?: Function): Component {}
	Vue.prototype.$emit = function (event: string): Component {}
	
	// lifecycleMixin(Vue)    src/core/instance/lifecycle.js **************************************************
	Vue.prototype._mount = function(){}
	Vue.prototype._update = function (vnode: VNode, hydrating?: boolean) {}
	Vue.prototype._updateFromParent = function(){}
	Vue.prototype.$forceUpdate = function () {}
	Vue.prototype.$destroy = function () {}

由此可见，Vue在挂载了一堆属性和方法后，文章最开始的那个例子中的app对象就包含了这些属性和方法。比如想看$data，在控制台输出app.$data就行了。同样调用方法也是如此。

好了，最底层分析完了，回溯到上一层：**src/core/index.js**

	import Vue from './instance/index'
	import { initGlobalAPI } from './global-api/index'
	import { isServerRendering } from 'core/util/env'
	
	initGlobalAPI(Vue)
	
	Object.defineProperty(Vue.prototype, '$isServer', {
	  get: isServerRendering
	})
	
	Vue.version = '__VERSION__'
	
	export default Vue
这个文件通过initGlobalAPI给Vue挂载静态属性和方法，然后给Vue的原型添加了一个$isServer，最后挂载了version属性。

加载静态属性和方法后Vue就变成了这个样子：

	// src/core/index.js / src/core/global-api/index.js
	Vue.config
	Vue.util = util
	Vue.set = set
	Vue.delete = del
	Vue.nextTick = util.nextTick
	Vue.options = {
	    components: {
	        KeepAlive
	    },
	    directives: {},
	    filters: {},
	    _base: Vue
	}
	Vue.use
	Vue.mixin
	Vue.cid = 0
	Vue.extend
	Vue.component = function(){}
	Vue.directive = function(){}
	Vue.filter = function(){}
	
	Vue.prototype.$isServer
	Vue.version = '__VERSION__'

然后继续回溯上一层：**web-runtime.js**

这个文件做了三件事：

1. 覆盖 Vue.config 的属性，将其设置为平台特有的一些方法
2. Vue.options.directives 和 Vue.options.components 安装平台特有的指令和组件
3. 在 Vue.prototype 上定义 __patch__ 和 $mount

最后一层：**web-runtime-with-compiler.js**

目的是给Vue的$mount方法添加compiler编译器，让它支持template。

小结：

构造函数经过四次处理：

第一次被处理时，挂在了Vue原型下的属性和方法；第二次被处理时挂载了Vue的静态属性和方法；第三次处理时添加了web平台特有的配置、组件和指令。第四次处理时让Vue支持template模板。

## 举例分析

	let v = new Vue({
	    el: '#app',
	    data: {
	        a: 1,
	        b: [1, 2, 3]
	    }
	})
这段代码创建了一个Vue对象，从构造函数分析，它在第一次被处理时调用了_init方法。_init方法在源码里面就不列举出来了，它主要做了这几个事：

使用mergeOption方法来处理调用Vue时传入的参数选项options，然后将返回的值赋给this.$options。

可以在控制台输入v.$options查看，里面有el和data选项，此时 vm.$options.data 的值应该是通过 mergeOptions 合并处理后的mergedInstanceDataFn 函数。

**合并策略：合并的策略就是，子组件的选项不存在，才会使用父组件的选项，如果子组件的选项存在，使用子组件自身的。**

_init方法总的来说，一共按顺序调用了这几个方法：

	initLifecycle(vm)
	initEvents(vm)
	callHook(vm, 'beforeCreate')
	initProps(vm)
	initMethods(vm)
	initData(vm)
	initComputed(vm)
	initWatch(vm)
	callHook(vm, 'created')
	initRender(vm)


本例子中只用了initData和initRender，数据绑定和挂载el到#app上。

initData其实就是数据双向绑定，initRender则是Vue的render（渲染）和re-render(重新渲染),双向绑定和渲染毫无疑问是MVVM框架的最最最重点的机制之一，以我的水平理解起来有些困难，但是，硬着头皮也得看下去啊！

