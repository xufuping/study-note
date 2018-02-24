> 目录
>
> 1.延迟脚本和异步脚本
> 
> 2.Array类型的方法 

### 1.延迟脚本和异步脚本

延迟脚本：只适用于外部脚本文件，脚本会被延迟到整个页面都解析完毕后再运行。（相当于告诉浏览器立即下载，但延迟执行）

    <script type="text/javascript" defer="defer" src="js/defer.js"></script>

异步脚本：只适用于外部脚本文件，告诉浏览器立即下载文件，但是并不保证按照指定这些脚本的先后顺序执行。(建议异步脚本不要在加载期间修改DOM。)

    <script type="text/javascript" async="async" src="js/defer.js"></script>

### 2.Array类型的方法


方法 | 类别 | 功能
---|---| ---
▲ push() | 栈方法 | 将参数添加到数组末尾
pop() | 栈方法 | 移除数组末尾最后一项
shift() | 队列方法 | 移除数组第一项
unshift() | 队列方法 | 在数组前端添加项
reverse() | 重排序方法 | 反转数组项顺序
sort() | 重排序方法 | 从小到大排列数组，可接收比较函数作为参数
▲ slice() | 操作方法 | 接收数组起始和结束位置并返回相应的数组
splice() | 操作方法 | 删除，插入，替换
indexOf() | 位置方法 | 查找某项(值必须完全相等)在数组中的位置

以下方法接受两个参数，前五个方法参数是函数和作用域对象：

方法 | 类别 | 功能
---|---| ---
every() | 迭代方法 | 如果该函数对每一项都返回true，则返回true。
some() | 迭代方法 | 如果该函数对某一项返回true，则返回true。
▲ filter() | 迭代方法 | 返回该函数会返回true的项组成的数组。
▲ forEach() | 迭代方法 | 对每一项执行某些操作，没有返回值。
▲ map() | 迭代方法 | 返回每次函数调用的结果组成的数组。
reduce() | 归并方法 | 从头到尾逐个遍历，迭代数组中的所有项（见第7点）


1.栈方法：后进先出
- push():接收任意数量的参数，将它们逐个添加到数组末尾，返回修改后的数组长度。
- pop():从数组末尾移除最后一项，减少数组的length值，返回移除的项。
     
2.队列方法：先进先出
- shift():移除数组第一项并返回该项，将数组长度减1。
- unshift():在数组前端添加任意项并返回新数组的长度。
     
3.重排序方法
- reverse():反转数组项的顺序。
- sort():比较数组转型的字符串，从小到大排列。可以接收比较函数作为参数。
    
        function compare(v1, v2) {
			if (v1 < v2) {
				return -1;
			} else if (v1 > v2) {
				return 1;
			} else {
				return 0;
			}
		}

		var numArray = [11, 24, 55, 90, 67];
		numArray.sort(compare);
		console.log(numArray); // 11,24,55,67,90
     
4.操作方法
- slice():接收一个或两个参数，即返回项的起始和结束位置。不会影响原始数组。

        var num = [1, 2, 3, 4, 5];
        var numFind_1 = num.slice(1);
        var numFind_2 = num.slice(1,4);
        
        console.log(numFind_1); // 2, 3, 4, 5 返回从指定位置开始到数组末尾所有项。
        console.log(numFind_2); // 2, 3, 4 返回从指定位置开始到指定结束项。

- splice():接收多个参数。
        
    删除：删除任意数量的项，指定前两个参数，删除的第一项的位置和要删除的项数。
        
        var numArray = [11, 24, 55, 90, 67];
		numArray.splice(0,3); // 删除第一项开始的前三项
		console.log(numArray); // 90, 67

    插入：插入任意数量的项，起始位置，0(删除的项数)，以及插入的项。
    
        var numArray = [11, 24, 55, 90, 67];
		numArray.splice(1, 0, 88); // 在第二项的位置插入88
		console.log(numArray); // 11, 88, 24, 55, 90, 67
        
    替换：指定位置，删除某项，插入替换项。
    
        var numArray = [11, 24, 55, 90, 67];
		numArray.splice(1, 1, 88); // 先删除第二项，再在第二项的位置插入88
		console.log(numArray); // 11, 88, 55, 90, 67
     
5.位置方法
- indexOf():返回查找的项在数组中的位置，但是查找的项的值必选严格相等。

        var numArray = [11, 24, 55, 90, 67];
		console.log(numArray.indexOf(90)); // 3，表示90为数组中第三位，第四项
     
6.迭代方法：

1.每个迭代方法都会接收两个参数：要在==数组中的每一项==都运行的函数和（可选的）运行该函数的作用域对象-影响this的值。

2.传入这些方法中的函数会接收三个参数：数组某项的值(item)，该项在数组中的位置(index)，数组对象本身(array)。

- every():如果该函数对每一项都返回true，则返回true。
- some():如果该函数对某一项返回true，则返回true。
- filter():返回该函数会返回true的项组成的数组。

    
    var numArray = [1, 2, 3, 4, 5, 6, 7];
		
	var everyResult = numArray.every(function(item, index, array){
		return (item > 3);
	});

	console.log(everyResult); // false

	var someResult = numArray.some(function(item, index, array){
		return (item > 3);
	});

	console.log(someResult); // true

	var filterResult = numArray.filter(function(item, index, array){
		return (item > 3);
	});

	console.log(filterResult); // [4, 5, 6, 7]

- forEach():对每一项执行某些操作，没有返回值。
- map():返回每次函数调用的结果组成的数组。

    
    var numArray = [1, 2, 3, 4, 5, 6, 7];
		
	var a = numArray.map( function(item, index, array) {
		return item + 10;
	});

	console.log(a); // [11, 12, 13, 14, 15, 16, 17]
	
	numArray.forEach(function(item, index, array) {
		// 执行某些操作
	});
     
7.归并方法
- reduce():从头到尾逐个遍历，迭代数组中的所有项，然后构建一个最终返回的值。
    
`1.接收两个参数：一个在每一项上调用的函数和(可选的)作为归并基础的初始值。`

`2.传入的函数接收四个参数：前一个值(prev)，当前值(cur)，项的索引(index)，数组对象(array)。这个函数返回的任何值都会作为第一个参数自动传给下一项。`

    var numArray = [1, 2, 3, 4, 5, 6, 7];
		
	var sum = numArray.reduce(function(prev, cur, index, array){
		return prev + cur;
	});
	
	console.log(sum); // 28