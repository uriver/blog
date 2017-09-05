# Ajax简介
> 准备面试吧！

**Ajax**：异步JavaScript和XML。  

**作用**：向服务器请求额外的数据而无需卸载页面。  

**核心技术**：XMLHttpRequest对象（简称XHR）。

### XHR用法
第一步：创建XHR对象  ```var xhr = new XMLHttpRequest() ```

第二步：调用open方法 ```xhr.open(method,url,async)```

第三部：调用send方法 ```xhr.send(msg)```

注：send没有数据时msg为null。

### readstatechange事件
检测XHR对象的readyState属性，该属性表示请求/响应过程的当前活动阶段。属性值如下（一共五个！）：

	0 － （未初始化）还没有调用send()方法
	1 － （载入）已调用send()方法，正在发送请求
	2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
	3 － （交互）正在解析响应内容
	4 － （完成）响应内容解析完成，可以在客户端调用了 

只要readyState属性的值由一个值变为另一个值，都会触发一次readystatechange事件。通常我们只对readystate值为4的时候感兴趣。  

必须在调用open()之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。

完整代码：
	
	var xhr = new XMLHttpRequest();

	xhr.onreadystatechange = function(){
		if (XHR.readyState == 4 && XHR.status == 200){
			console.log(xhr.responseText);
		}
	}
	
	xhr.open("get",url,true);
	xhr.send(null);


### JQuery Ajax

JQuery的Ajax使用方法是最常见的版本：

	$.ajax({
          url:***,
		  data:{},
          dataType:"json",
          type:'GET',
          async :false,		//async默认为true —— 同步
          success:function (data) {
     	      console.log(data);
          },
          error:function(){
              alert('获取数据失败！')
          },
      });

以上Ajax展示了JQuery常用的API，还有一些不常用的：（只列举自己确实使用过的）

* 获取cookie：在前后端分离的项目中不能直接获取cookie，通过设置ajax的xhrFields API可以获取cookie。

		xhrFields:{
			withCredentials:true
		},

### Axios

Axios是VUE官方推荐的Ajax模块，用法如下：

	this.axios({	//这里使用this是因为VUE将axios方法放入了VUE对象的原型中。
		url:***,
		params:{},
		method: 'get',
	}).then((res)=> {
		console.log(res.data);
	})

axios中，发送get请求传递数据要用参数params；后端接受GET请求要用参数req.query;  
post请求传递数据用参数data，后端接收POST请求要用参数req.body。