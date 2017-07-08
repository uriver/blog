# Express学习

> 最近需要学习node.js,而现在node.js用的最广的就是写后台了。Express是后台框架最火的一个，而本次我用Express的目的是为了搭建一个留言板。

## 安装并生成应用骨架
http://www.expressjs.com.cn/starter/installing.html
官网上写的很清楚，应用骨架就是搭建一个Express开发的模板，使用应用骨架生成的模板目录如下：
![](IMG1.png)



## 更换模板引擎
	模板引擎（这里特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档。
Express默认使用jade模板引擎，对前端程序猿来说并不友好,这意味着我们需要按照jade的编程方式写前端界面。
对于前端来说，使用HTML是自己最熟悉的，而jade模板引擎并不能通过路由打开HTML文件。

	但是可以通过静态资源打开HTML文件，比如把HTML扔在public文件夹内。
	应用骨架中有一句 app.use(express.static(path.join(__dirname, 'public'))); 可以帮助服务器在public文件夹找到HTML。
	然后通过 url/文件名.html 打开该HTML文件。
显然这种打开HTML文件的方式不优雅（也不好用）。我需要通过路由打开HTML文件，所以我需要更换模板引擎。

通过查找资料，我知道了ejs和swig这两个比较有名的模板引擎可以在路由中使用HTML。所以我决定用ejs替换jade。

#### 第一步：下载ejs
npm install ejs --save

#### 第二步：修改模板引擎
模板引擎的配置位于app.js中：
![](IMG2.png)
把这段代码改为配置ejs模板引擎：
```
  app.set('views',path.join(__dirname , 'views') );
  app.engine('.html', require('ejs').__express);  
  app.set('view engine', 'html');
```

#### 第三步：测试
先在views文件夹创建一个可以正常使用的HTML文件test.html；
然后修改routes文件夹中的index.js中的路由：
![](IMG3.png)
最后启动服务器，发现主页确实变成了test.html中的内容！

