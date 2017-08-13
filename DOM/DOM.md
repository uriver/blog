# DOM
> DOM描绘了一个层次化的节点数。允许开发人员添加、移除和修改页面的某一部分。

## 1. DOM基本操作
* getElementById():写入ID名，返回一个对象；

* getElementsByTagName():写入指定标签名，返回一个NodeList（节点列表）；

* getElementsByClassName()：HTML5新增，返回一个NodeList;

* getAttribute()获取属性；

* setAttribute()修改属性；

* childNodes：获取该元素的所有子元素，返回数组；

* firstChild&lastChild：相当于childNodes[0]和childNodes[node.childNodes.length - 1]

* nodeType：节点属性，元素节点属性为1，属性节点为2，文本节点为3；

* nodeValue：获取节点的值；

* innerHTML：插入HTML，只能用于HTML文档，不适用于其他XHTML文档；

* createElement：创建一个未被连接到document的元素节点；

* appendChild：将节点连接到document的节点数；

* createTextNode：有了元素节点还不够，若要放一些文本信息，需要再创建一个文本节点；

* element.style：获取元素样式，但需要将类似border-radius之类中间有'-'的改为驼峰命名：borderRadius，否则将被认为是减号；

* className：className="class"为替换元素的class，className+="class"为追加class；

* parentNode：获取该元素节点的父节点


## 2. DOM操作技巧
### 2.1 动态脚本
动态插入JS脚本：

    function loadscript(url){
      var script = document.createElement("script");
      script.type = "text/javascript";
      script.src = url;
      document.body.appendChild(script);
    }
    loadscript("XXX.js")

### 2.2动态样式

    function loadStyles(url){
      var link = document.createElement("link");
      link.rel = "stylesheet";
      link.type = "text/css";
      link.href = url;
      var head = document.getElementsByTagName("head")[0];
      head.appendChild(link);
    }
    loadStyles("XXX.css")

## 3. DOM扩展
### 3.1 querySelector()方法
结合getElementById()和getElementByTagName()功能。

    //获取body元素
    var body = document.querySelector("body");
    //获取ID为"myDiv"的元素
    var myDiv = document.querySelector("#myDiv");
    //获取类为"selected"的第一个元素
    var selected = document.querySelector(".selected");
    //获取类为"button"的第一个元素
    var img = document.body.querySelector("img.button");

querySelector只能返回第一个元素，而下一种方法能返回一个NodeList。

### 3.2 querySelectorAll()
用法类似querySelector，不过返回的是NodeList。


## 4.DOM0级事件处理和DOM2级事件处理
### 4.1 DOM 0级

主要特点：

1. 标签内写onclick事件；

2. JS内写onclick = function(){}函数；

3. 后面绑定的事件会覆盖前面绑定的事件，激活的时候会执行新绑定的。

```
document.getElementById("btn").onclick = function(){};
```

### 4.2 DOM 1级

1. 使用addEventListener()和removeEventListener()添加和移除事件；

2. 不会覆盖前面的事件，激活的时候所有绑定的时间会依次执行一遍。（可以绑定多个事件）

```
document.getElementById("btn").addEventListener("click", function(){}, false);
```

三个参数：事件类型，事件处理函数，第三个参数默认为false，事件冒泡时调用；为true的时候是事件捕获时调用。
