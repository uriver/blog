# underscore源码解读笔记
> 网上有很多underscore的源码分析，所以不重复那些文章，而是记录一下学习underscore源码过程中学到的知识。

## 小知识点

### 1. void 0
void 运算符能对给定的表达式进行求值，然后返回 undefined。也就是说无论void后面接什么都是返回undefined。而0是最精短的。

为什么用void代替undefined？因为**undefined不是保留字**。虽然在ES5之后undefined只能读不能写，但是在局部作用域中undefined还是能写：

	(function() {
	  var undefined = 10;
	  alert(undefined);
	})();
而void是不能重写的，另外，相对于undefined，void 0更简短，可以用来压缩JavaScript。

### 2. apply()和call()
apply()和call()虽然作用和用法基本差不多，但实际上call的速度更快。

因为call里面直接输入参数，而apply还要对数组进行检验和深拷贝，所以效率要低。

### 3. 数组去重
使用双重循环的方式有点Low，可以用indexOf：
	
	function unique(arr){
		res = arr.filter(function(item.index,array){
			return array.indexOf(item) === index;
		})
		return res;
	}
	
	var arr = [1, 1, '1', '2', 1];
	var newArr = unique(arr);

使用ES6的set可以直接去重。


## 与知识点无关的小技巧
### 1. 三目运算符
源码中使用了大量的三目运算符，使代码更加精简。