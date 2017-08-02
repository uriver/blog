# Promise基本用法
> 异步编程的缺点是callback函数层层嵌套，导致代码不好维护，难以理解。Promise编程是既是ES6的一部分，也是解决异步编程缺陷的方法之一。

## Promise概念
Promise本身是一个构造函数，并且接受一个函数参数。该函数参数又接受两个参数，一个是resolve，另一个是reject。

通过Promise构造出来的对象称为Promise对象。Promise对象可以理解为一次将要执行的操作，并且它能够用链式调用来组织代码，让代码更直观。

resolve 方法可以使 Promise 对象的状态改变成成功，同时传递一个参数用于后续**成功后的操作**。

reject 方法则是将 Promise 对象的状态改变为失败，同时将错误的信息传递到后续**错误处理的操作**。

Promise的三种状态：

* Fulfilled 可以理解为成功的状态

* Rejected 可以理解为失败的状态

* Pending 既不是 Fulfilld 也不是 Rejected 的状态，可以理解为 Promise 对象实例创建时候的初始状态


## 写一个Promise吧
### Promise核心部分
因为Promise对象相当于一段直接执行的代码，所以一般把Promise对象封装太function里面。

首先创建一个简单的Promise对象并调用：

	  function pro(){
	    var p = new Promise(function(resolve,reject){
	      setTimeout(function(){
	        console.log("1");
	      },2000)
	    });
	    return p;
	  }
	  pro();

此时没有体现出Promise的作用。因为没有用到它的链式调用和resolve、reject。

写一个更复杂的Promise对象并链式调用：

	  function pro(){
	    var p = new Promise(function(resolve,reject){
	      setTimeout(function(){
	        var a = 1;
	        console.log(a);
	        resolve(a);
	      },2000)
	    });
	
	    return p;
	  }
	  pro()
	  .then(function(data){
	    data ++;
	    console.log(data);
	  });

这里增加了resolve，resolve里面的值可以作为参数直接传递给then的参数函数。到这一步为止，我们都可以通过callback函数来实现：

	function run(callback){
	    setTimeout(function(){
			a = 1;
	        console.log(a);
	        callback(a);
	    }, 2000);
	}
	
	run(function(data){
		data ++;
	    console.log(data);
	});

但是callback的方式看起来明显要难看。另外，promise的真正的优势在于：

**可以在then方法中继续写Promise对象并返回，然后继续调用then来进行回调操作。**

完整代码如下：

	  //创建三个返回Promise对象的函数
	  function pro1(){
	    var p = new Promise(function(resolve,reject){
	      setTimeout(function(){
	        var a = 1;
	        resolve(a);
	      },2000)
	    });
	    return p;
	  }
	
	  function pro2(){
	    var p = new Promise(function(resolve,reject){
	      setTimeout(function(){
	        var b = 2;
	        resolve(b);
	      },1000);
	    })
	    return p;
	  }
	
	  function pro3(){
	    var p = new Promise(function(resolve,reject){
	      setTimeout(function(){
	        var c = 3;
	        resolve(c);
	      },2000);
	    })
	    return p;
	  }
	
	  //链式调用
	  pro1()
	  .then(function(data){
	    console.log(data);
	    return pro2();
	  })
	  .then(function(data){
	    console.log(data);
	    return pro3();
	  })
	  .then(function(data){
	    console.log(data);
	  });

这样就以一种简单，容易理解的方式依次调用了三个异步函数。

### 错误处理
在上面的例子中，then里面只有一个函数参数，实际上可以有两个，一个对应成功的回调函数，一个对应失败的回调函数。

	function getNumber(){
	    var p = new Promise(function(resolve, reject){
	        //做一些异步操作
	        setTimeout(function(){
	            var num = Math.ceil(Math.random()*10); //生成1-10的随机数
	            if(num<=5){
	                resolve(num);
	            }
	            else{
	                reject('数字太大了');
	            }
	        }, 2000);
	    });
	    return p;            
	}
	getNumber()
	.then(
	    function(data){
	        console.log("数字为：" + data);
	    }, 
	    function(error){
	        console.log(error);
	    }
	);

从http://www.cnblogs.com/lvdabao/p/es6-promise-1.html上copy的例子~

catch同样能达到上述作用。并且catch能更好的处理promise错误问题，还有all（并行执行异步）和race（竞速），具体可以去看http://www.cnblogs.com/lvdabao/p/es6-promise-1.html上面的介绍，写的很详细，不想再重复了~

