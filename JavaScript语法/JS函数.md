# JS常用函数和操作符
> JS的部分常用函数和操作符列举。

## Array类型
### 1. 检测数组
* **Array.isArray(value)**：判断value是否为数组类型。


### 2. 转换方法
* **arr.toString()**：把数组变成字符串，每一项用逗号隔开；
* **arr.join("|")**：把数组变成字符串，每一项用join里面的值隔开，默认是逗号。

### 3. 栈和队列方法
* **arr.push()**：在数组末尾添加括号里面的值，并**返回修改后的数组长度**；
* **arr.pop()**：获取数组最后一项并返回，数组长度减1；
* **arr.shift()**：获取数组第一项并返回，数组长度减1；
* **arr.unshift()**：在数组前端添加任意项（为括号里面的值），并返回修改后的长度，类似push。  

		使用push()和pop()方法模拟栈；   
		使用push()和shift()方法模拟队列；  
		使用unshift()和shift()方法模拟方向队列。

### 4. 重排序方法
* **arr.reverse()**：数组倒序排列；
* **arr.sort()**：升序排列数组；自动转换为字符串排序，所以默认情况下[1,5,10].sort()为[1,10,5]；所以我们使用sort的时候需要接受比较参数。

**sort比较参数**：

	function compare(a,b){
		if(a < b){
			return -1;
		}else if(a > b){
			return 1;
		}else{
			return 0;
		}
	}

	arr.sort(compare);

如果参数a应该位于b之前，则返回负数，所以此处是升序排列。

对于数值类型比较更简单：

	function compare(a,b){
		return a - b; //升序，倒序则为b - a
	}

### 5. 操作方法
* **arr.concat()**：concat内部无参数时，返回一个arr的副本（而不是引用），有参数时，返回arr+参数所组成的**新的数组**；
* **arr.slice()**：基于当前数组的一项或多项**创建一个新数组**。接受1或2个参数，即返回项的起始和结束位置。只有1个参数时，返回该参数指定位置到数组末尾的所有项，两个参数时返回起始和结束位置中间的项，包括起始项，不包括结束项；若结束位置小于起始位置，返回空数组；若参数为负数，则用数组长度加改参数确定相应的位置；
* **arr.splice()**

splice()方法不会创建新的数组，是直接在原数组上进行操作。它有三个参数，起始项、结束项、插入项。  
1. **删除项**：arr.splice(0,2)会删除arr[0]和arr[1]并**返回删除的数组**，arr的值也会改变为删除后的数组；  
2. **插入项**：arr.splice(2,0,"test")在arr[2]的位置插入test，原来的arr[2]后移；  
3. **替换项**：arr.splice(2,1,"test")替换原来的arr[2];

### 6. 位置方法
* **arr.indexOf()**：两个参数，必须的查找项参数和可选的索引位置参数。查找时使用全等运算符"==="，没有找到的情况下返回-1；
* **arr.lastIndexOf()**：从末尾的位置开始找；

### 7. 迭代方法
以下所有方法都接收两个参数：  
必选的每一项上运行的函数 和 可选的运行改函数的作用于对象。  
传入这些方法的函数会接收三个参数：数组项的值、该项索引、数组对象本身。  
**不会影响数组本身。**

* **arr.every()**:对数组中每一项运行给定函数，若每一项都返回true，则返回true，否则返回false；
* **arr.some()**:对数组中每一项运行给定函数，若有任意一项返回true，则返回true，否则返回false；

用法：

    var numbers = [1,2,3,4,5];

    var everyResult = numbers.every(function(item, index, array){
        return (item > 2)
    }) ;
    alert(everyResult);     // false

    var someResult = numbers.some(function(item, index, array){
        return (item > 2)
    });
    alert(someResult);      // true

* **arr.filter()**：对数组中的每一项运行给定的函数，返回该函数会返回true的项组成的数组；

用法：

    var numbers = [1,2,3,4,5];

    var filterResult = numbers.filter(function(item, index, array){
        return (item > 2)
    }) ;
    alert(filterResult);     // [3,4,5]

* **arr.forEach()**：对数组中的每一项运行给定函数，没有返回值；本质上和for循环迭代数组一样；
* **arr.map()**:对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
