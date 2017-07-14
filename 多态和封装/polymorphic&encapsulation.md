# JS多态和封装
> 对象的三大特性：封装,继承,多态。javascript 是基于原型继承的面向对象编程语言，要学会使用这三个特效。继承比较复杂，到时候单独写笔记。

## 多态
同一操作作用于不同的对象，可以有不同的解释，产生不同的执行结果。  
JS语法本身没有多态，但可以自己实现多态。    


看到了一个很形象的类比：家里养了一只鸡，一只鸭。当主人向他们发出‘叫’的命令时。鸭子会嘎嘎的叫，而鸡会咯咯的叫。  
对应的有一个程序：  
	
	var makeSound = function(animal) {
	    animal.sound();
	}
	
	var Duck = function(){}
	Duck.prototype.sound = function() {
	    console.log('嘎嘎嘎')
	}
	var Chiken = function() {};
	Chiken.prototype.sound = function() {
	    console.log('咯咯咯')
	}
	
	makeSound(new Chicken());
	makeSound(new Duck());


多态最常见的2种实现方式：

- 覆盖
- 重载

覆盖指子类重新定义父类方法，正好就是基于prototype继承的用法。
重载是指多个同名但参数不同的方法，这个JavaScript没有，但是可以通过判断参数类型或者数量，用switch来实现多态。

## 封装
通过私有化变量和私有化方法达到封装的效果。  
可以通过**特权方法**和**闭包实现封装**。  
特权方法：

 	 function myObject(msg){
         //特权属性(公有属性)
	     this.myMsg = msg; //只在被实例化后的实例中可调用
	     this.address = '上海';
	     
	     //私有属性
	     var name = '豪情';
	     var age = 29;
	     var that = this;
	     
	     //私有方法
	     function sayName(){
	         alert(that.name);
	     }
	     //特权方法(公有方法)
	     //能被外部公开访问
	     //这个方法每次实例化都要重新构造
	     this.sayAge = function(){
	         alert(name); //在公有方法中可以访问私有成员
	     }
	     //私有和特权成员在函数的内部，在构造函数创建的每个实例中都会包含同样的私有和特权成员的副本，
	     //因而实例越多占用的内存越多
	}

	//公有方法
	//向prototype中添加成员将会把新方法添加到构造函数的底层中去
	myObject.prototype.sayHello = function(){
		 alert('hello everyone!');
	}
	//静态属性
	//适用于对象的特殊实例，就是作为Function对象实例的构造函数本身
	myObject.name = 'china';
    //静态方法
	myObject.alertname = function(){
		 alert(this.name);
	}
	 //实例化
	var m1 = new myObject('111');

闭包实现封装：

	function fun1(){
		var n=1;
		var add = function(){
			n += 1;
	  		alert(n);
		}
	    return add;
	}
	
	var res = fun1();
	res();	 //alert n=2;
	res();	// alert n=3;