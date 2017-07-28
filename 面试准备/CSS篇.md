# 面试准备之CSS
> 废话不多说，加油吧骚年！

## 知识点

### 盒模型
1. W3C盒模型：现在主流的
2. IE盒模型：box-sizing里面的border-box

CSS3引入box-sizing属性，有三个选项：content-box（符合W3C盒模型），border-box（符合IE盒模型），padding-box（只支持主流浏览器）

### 选择符
1. id选择器（ # myid） 
2. 类选择器（.myclassname） 
3. 标签选择器（div, h1, p） 
4. 相邻选择器（h1 + p） 
5. 子选择器（ul > li） 
6. 后代选择器（li a） 
7. 通配符选择器（ * ） 
8. 属性选择器（a[rel = "external"]） 
9. 伪类选择器（a:hover, li:nth-child）

### 优先级
* 优先级就近原则，同权重情况下样式定义最近者为准;
* 载入样式以最后载入的定位为准;优先级为: !important > id > class > tag 
* important 比 内联优先级高

### 定位机制
1. 标准文档流：从上到下，从左到右；
2. 浮动：左右漂移，知道遇到其他浮动或者父元素边框阻挡；
3. 绝对定位：通过top,left,right,bottom对其相对父元素进行地位；

position:static/relatice/absolute/fixed

### 布局
相对于定位机制，布局也有：
1. 流动布局
2. 浮动布局
3. 绝对定位布局

除此之外，还有
1. flex弹性布局：display:flex，主要属性：flex-flow:row/column和flex：number，方向和比例。  
2. 响应式布局：通过媒体查询使用不同的CSS样式，达到兼容设备的效果。

### 浏览器兼容
1. -moz-：代表FireFox浏览器私有属性  
  
2. -ms-：代表IE浏览器私有属性  
  
3. -webkit-：代表safari、chrome浏览器私有属性  
  
4. -o-：代表opera浏览器私有属性  

### 浏览器内核
1. trident IE
2. gecko Firefox
3. webkit Safari、曾经的Chrome
4. blink Chrome、Opera

渲染引擎(layout engineer或Rendering Engine)和JS引擎。   
（1）渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。  
（2）JS引擎则：解析和执行javascript来实现网页的动态效果。  
最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

### CSS reset
重置浏览器自带CSS样式，保持浏览器一致性。

### CSS hacks（待学习）
不同的浏览器对CSS的解析结果不同，导致相同的CSS输出的页面效果不同。CSS Hack解决浏览器局部兼容问题，实现原理是针对不同浏览器写不同的CSS代码。

### CSS sprite
雪碧图。把零碎的图片整合到一张图片中。
能够减少文件体积和服务器请求次数，提高开发效率。

### iconfont字体
自定义文字，通过@font-face引入
icon图标通过class="icon"引入

### 常见CSS3
* @font-face
* border-radius
* word-wrap：浏览器单词内断句
* text-overlow：配合overflowhidden使用，溢出部分加上...
* ……


## 技巧
### CSS 创建三角形（待学习）
把上、左、右三条边隐藏掉（颜色设为 transparent）

	#demo { 
		width: 0; height: 0; border-width: 20px; 
		border-style: solid; 
		border-color: transparent transparent red transparent;}

### 





## 兼容性
1. Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示, 以前可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决。现在无解。可以调整浏览器设置。
2. 初始化CSS（reset）不同浏览器的标签默认的外间距和内间距不同
