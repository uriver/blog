# 响应式Web设计
> 随着终端种类的增多，网站需要适应不同大小的屏幕，于是响应式Web设计出现了。

## 1. 网格系统
网格系统是响应式设计的重要组成部分，个人认为它更适合归属于自适应这类（自适应是响应式设计的子类）。

在bootstrap中最有名的莫过于它的网格系统了，在这里我们自己尝试去写一个网格系统：

1.先设置所有的元素有box-sizing的border-box属性： 

	* {
		box-sizing:border-box
	}
2.设置每一竖(col)的宽度，一般把一行(row)分为12小份：

	.col-1 {width: 8.33%;}
	.col-2 {width: 16.66%;}
	.col-3 {width: 25%;}
	.col-4 {width: 33.33%;}
	.col-5 {width: 41.66%;}
	.col-6 {width: 50%;}
	.col-7 {width: 58.33%;}
	.col-8 {width: 66.66%;}
	.col-9 {width: 75%;}
	.col-10 {width: 83.33%;}
	.col-11 {width: 91.66%;}
	.col-12 {width: 100%;}
	[class*="col-"]{
		float: left;
		padding: 15px;
		border:1px solid black;
	}
3.设置行：

	.row:after {
	    content: "";
	    clear: both;
	    display: block;
	}

这样就构成了一个自适应的网格系统，比想象中的简单。

## 2. Viewport
**Viewport**：用户网页的可视区域。

在响应式网站源码中，在head中都有一句代码：

	<meta name="viewport" content="width=device-width, initial-scale=1,maximum-scale=1,user-scalable=no">

这句代码是用来设置Viewport的：


    width：控制 viewport 的大小，可以指定的一个值，如果 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
    height：和 width 相对应，指定高度。
    initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
    maximum-scale：允许用户缩放到的最大比例。
    minimum-scale：允许用户缩放到的最小比例。
    user-scalable：用户是否可以手动缩放。

## 3. 媒体查询

@media 媒体查询用来针对不同的媒体类型定义不同的样式。

参数分为两类，一类是**媒体类型**，另一类是**媒体功能**。举个例子：

	@media screen and (max-width: 300px) {}
这句代码中screen是指媒体类型（screen用于电脑屏幕，平板电脑，智能手机等），max-width:300定义输出设备中的页面最大可见区域宽度。

媒体查询有两种用法：

1. 针对不同的媒体类型**定义**不同的样式；
2. 针对不同的媒体类型**引入**不同的样式。

### 3.1 定义样式
在css文件中插入：

	@media only screen and (max-width: 500px) {
	    body {
	        background-color: lightblue;
	    }
	}
即最大可见区域宽度小于等于500时，网站背景会变蓝。

**技巧：**

	/* For mobile phones: */
	[class*="col-"] {
	    width: 100%;
	}
	@media only screen and (min-width: 600px) {
	    /* For tablets: */
	    .col-m-12 {width: 8.33%;}
	    .col-m-2 {width: 16.66%;}
	    .col-m-3 {width: 25%;}
	    .col-m-4 {width: 33.33%;}
	    .col-m-5 {width: 41.66%;}
	    .col-m-6 {width: 50%;}
	    .col-m-7 {width: 58.33%;}
	    .col-m-8 {width: 66.66%;}
	    .col-m-9 {width: 75%;}
	    .col-m-10 {width: 83.33%;}
	    .col-m-11 {width: 91.66%;}
	    .col-m-12 {width: 100%;}
	}
	@media only screen and (min-width: 768px) {
	    /* For desktop: */
	    .col-1 {width: 8.33%;}
	    .col-2 {width: 16.66%;}
	    .col-3 {width: 25%;}
	    .col-4 {width: 33.33%;}
	    .col-5 {width: 41.66%;}
	    .col-6 {width: 50%;}
	    .col-7 {width: 58.33%;}
	    .col-8 {width: 66.66%;}
	    .col-9 {width: 75%;}
	    .col-10 {width: 83.33%;}
	    .col-11 {width: 91.66%;}
	    .col-12 {width: 100%;}
	}
在css中如上定义，意思就是可视区域大于768px时col-1——col-12生效，而可视区域小于768px，大于600px时col-m-1——col-m-12生效，若可视区域小于600,则所有匹配col-的class都是width=100%的属性。

应用举例：
	
	<div class="col-3 col-m-3"></div>



### 3.2 引入样式

    <link rel="stylesheet" href="style/common.css" type="text/css">
    <link rel="stylesheet" href="style/pad.css" type="text/css" media="(min-width:768px)">
    <link rel="stylesheet" href="style/pc.css" type="text/css" media="(min-width:1000px)">
    <link rel="stylesheet" href="style/pc-w.css" type="text/css" media="(min-width:1200px)">
这里有公用css文件的引入，也有根据不同媒体的css文件的引入。

## 4. 图片
### 4.1 图片自适应
图片自适应：

	img {
	    width: 100%;
	    height: auto;
	}
这样做会导致图片宽度等于父容器的宽度，可能会导致图片扩大。如果不想图片扩大，可以改写为：

	img {
	    max-width: 100%;
	    height: auto;
	}
这样父容器再怎么宽，图片只会放大到自己原本的大小。

### 4.2 背景图片
三个不同的方法调整背景图片的大小。

1. background-size 属性设置为 "contain", 背景图片将按比例自适应内容区域。图片保持其比例不变。

2. background-size 属性设置为 "100% 100%" ，背景图片将延展覆盖整个区域。

3. background-size 属性设置为 "cover"，则会把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。注意该属性保持了图片的比例因此 背景图像的某些部分无法显示在背景定位区域中。 