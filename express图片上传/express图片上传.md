# Express图片上传
> 写博客系统的时候需要上传图片，使用Markdown需要一个链接，所以得先把图片上传到服务器。

## 图片上传的方式

### 转化为base64
	将图片转化为base64编码，然后直接存储在数据库中。
这是最简单的，导入的编辑器可以直接做到这个功能。这样做的好处是方便，把文章和图片都往数据库一扔就完事了。  

缺点：  
1. 转化后占用的消耗的空间变大；  
2. 不能缓存。

### 上传到服务器
就跟字面上的意思一样，直接上传到服务器。这是这篇文章主要分析的内容。

### 上传到云服务器
图片比较占空间，如果是大型网站，一般会把图片扔到云服务器中。

## 上传图片到服务器
> 上传的时候踩过一些坑，先把坑给列举出来~

### form标签的enctype属性
enctype 属性规定在发送到服务器之前应该如何对表单数据进行编码。  
**默认地，表单数据会编码为 "application/x-www-form-urlencoded"**。就是说，在发送到服务器之前，所有字符都会进行编码（空格转换为 "+" 加号，特殊符号转换为 ASCII HEX 值）。

	enctype="multipart/form-data"	
不对字符编码。  
在使用包含文件上传控件的表单时，必须使用该值。

### formdata对象
通过FormData对象可以组装一组用 XMLHttpRequest发送请求的键/值对。它可以更灵活方便的发送表单数据，因为可以独立于表单使用。如果你把表单的编码类型设置为multipart/form-data ，则通过FormData传输的数据格式和表单通过submit() 方法传输的数据格式相同

个人理解是：有了formdata可以不用设置enctype属性了。

测试结果：删除掉`enctype="multipart/form-data"	`依旧可以正常上传图片。

### 上代码

#### Web页面上传图片
在express中view文件夹的ejs文件中加入表单：

	<form id='myform'>   
	    <input type='file' name='myImg' id='myImg'/><br/>  
	    <input onclick="send()" type='button' value='确认' id='btn'/>  
	</form>  

然后在script中编写send方法：

	function send(){
		var myform=document.getElementById('myform');  
        var myData=new FormData(myform);  
		$.ajax({
			url:"/users/img",
			data:myData,
			processData: false,
        	contentType: false,
			method:"post",
			success:function(data){
				console.log(data.newPath);
			}
		})
	}

其中有两个需要注意的地方：

	processData:必须false才会避开jQuery对 formdata 的默认处理。
	默认值: true。
	默认情况下，通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型 "application/x-www-form-urlencoded"。
	如果要发送 DOM 树信息或其它不希望转换的信息，请设置为 false。

	contentType:必须false才会自动加上正确的Content-Type (multipart/form-data)。
	默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。
	默认值适合大多数情况。如果你明确地传递了一个 content-type 给 $.ajax() 那么它必定会发送给服务器（即使没有数据要发送）。
	
ejs/html文件处理完毕，接下来处理服务器接受图片了。

#### 服务器接收图片

在处理post请求的js文件中加入一段post请求代码。

	var formidable = require('formidable'),
	    fs = require('fs'),
	    TITLE = 'formidable上传示例',
	    AVATAR_UPLOAD_FOLDER = '/images/',
	    domain = "http://localhost:3000";

	router.post('/img',function(req,res){
	  var form = new formidable.IncomingForm();
	  form.encoding = 'utf-8';        //设置编辑
	  form.uploadDir = 'public' + AVATAR_UPLOAD_FOLDER;     //设置上传目录
	  form.keepExtensions = true;     //保留后缀
	  form.maxFieldsSize = 2 * 1024 * 1024;   //文件大小
	
	  form.parse(req,function(err,fields,files){
	    if (err) {
	      throw err;
	      return;
	    }
	    console.log(files);
	
	    var extName = '';  //后缀名
	    switch (files.myImg.type) {
	      case 'image/pjpeg':
	        extName = 'jpg';
	        break;
	      case 'image/jpeg':
	        extName = 'jpg';
	        break;
	      case 'image/png':
	        extName = 'png';
	        break;
	      case 'image/x-png':
	        extName = 'png';
	        break;
	    }
	
	     if(extName.length == 0){
	      res.locals.error = '只支持png和jpg格式图片';
	      res.render('index', { title: TITLE });
	      return;
	    }
	
	    var avatarName = Math.random() + '.' + extName;
	    //图片写入地址；
	    var newPath = form.uploadDir + avatarName;
	    //显示地址；
	    var showUrl = domain + AVATAR_UPLOAD_FOLDER + avatarName;
	    console.log("newPath",newPath);
	    fs.renameSync(files.myImg.path, newPath);  
	    res.json({
	      "newPath":showUrl
	    });
	  })
	})

这段代码是网上找到的，实测有效。  
需要安装依赖包：formidable。

做完这些之后，打开服务器，在页面上上传文件，确定后，服务器的public/images文件夹就会出现重新命名的该文件！