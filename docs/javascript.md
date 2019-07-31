# JavaScript基础学习

## 变量

es5及之前用`var`申明，es6后建议`let` 和 `const`。

使用`typeof`查看变量类型。

### 1.1 变量类型

> number  小数与整数
> string  字符串(无字符类型)
> boolean  布尔数据类型
> undefined  未定义

```javascript
typeof 10  //number
typeof 3.14  //number
typeof 's'  //string
typeof "sunshine"  //string
typeof true  //boolean
typeof sun  //undefined
```

![截图-1564569518](https://wimg.misiyu.cn/images/20190731/1564569519_f25079a350e7e64.png?x-oss-process=style/first)


### 1.2 变量类型转换

**parseInt()** 转换为数字

```JavaScript
parseInt("123abc123") //123 含非数字字符,将前面的数字字符转换成数字
parseInt("a123") //NaN
parseInt("012") //12 去首部0
parseInt("0x10") //16 以十六进制计算
```

**parseFloat()**  整数字符串仍转换为整数
IsNaN  (is not a muber)不是数字返回true，是数字返回false



## 运算符

```JavaScript
var a = 1;
1+true  //2
"hello"+1  //hello1
10/3  //3.3333333333333335
"sunshine">"sun"  true
10 > "9"  true string-->number
age>18?"成年人":"未成年人" // 三目运算符
```

![截图-1564569868](https://wimg.misiyu.cn/images/20190731/1564569868_396be37972432b0.png?x-oss-process=style/first)

## 控制语句

```JavaScript
if语句
	number  非0为true,0为false
	string  内容非空为true,内容空为false。
	undefined  false
	NaN  false
switch语句
	在javascript中case后可跟常量、变量、表达式
```



## 循环语句

**for-in语句**

(1)遍历数组元素,遍历出的是数组下标

```JavaScript
var arr = [12,13,19,15,16];
for(var index in arr){
	document.write(arr[index]+",");		
}
// 普通for循环遍历数组元素
for(var index = 0 ; index<arr.length ; index++){
	document.write(arr[index]+",");	
}
```

(2)遍历对象的所有属性,遍历出的是对象的属性名

```JavaScript
function Person(id , name){
	this.id = id;
	this.name = name;	
}
var  p = new Person(110,"sunshine");
for(var property in p){
	document.write(p[property]+",");	
}
```

## With语句

使用with语句,在存取对象属性和调用方法时不用重复指定对象。

```JavaScript
with(document){
	write("&nbsp;");
}
function Person(id , name){
	this.id = id;
	this.name = name;
}
var p = new Person(110,"sunshine");
with(p){
	document.write("编号："+ p.id);
	document.write("姓名："+ name);
}
```

![截图-1564570079](https://wimg.misiyu.cn/images/20190731/1564570079_f304ce65dc844d0.png?x-oss-process=style/first)

## 函数

> 注意细节:
> (1)定义形参时是不能使用var关键字声明变量
> (2)函数没有返回值类型，如有需要直接返回即可
> (3)没有函数重载，后定义的同名函数直接覆盖前面定义的同名函数
> (4)任何函数内部都隐式维护了一个arguments数组对象，给函数传递数据的时候，会先传递到arguments对象中，然后再由arguments对象分配数据给形参

```JavaScript
function add(a,b){
	var sum = a+b;	
	//return sum;
}
function add(){
	for(var index = 0 ; index<arguments.length ; index++){
		document.write(arguments[index]+",");	
	}
}
//调用函数
add(11,21,13,14);
```

![截图-1564570165](https://wimg.misiyu.cn/images/20190731/1564570165_6e4621cc8cd0332.png?x-oss-process=style/first)

## Array数组

**创建方式**

```JavaScript
方式1:var 变量名 = new Array(); //创建一个长度为0的数组
方式2:var 变量名 = new Array(长度) //创建一个指定长度的数组对象
方式3:var 变量名 = new Array("元素1","元素2"...) //创建指定元素的数组对象
方式4:var 变量名 = ["元素1","元素2"...];
```

**在javascript中数组长度可变**

```JavaScript
var arr = new Array(3); //创建了一个长度为0的数组对象。
arr[100] = 10;
document.write("arr长度："+arr.length+"<br/>");//101	
var arr2 = new Array("sun1","sun2","sun3");
arr2 = ["sun4","sun5","sun6","sun7"];
document.write("arr2长度："+arr2.length+"<br/>");//4
```

**常见方法/属性**

- **length**：返回数组长度

- sort：排序，传入排序的方法。

  ```javascript
  function sortNumber(num1,num2){
  	return num1-num2;//升序
  }
  ```

- **slice**：指定数组的开始索引值与结束索引值截取数组的元素，**并且返回子数组**
  var subArr = arr1.slice(1,2);

- **reverse**：翻转数组元素。arr1.reverse();

- **join**：使用指定的分隔符把数组中的元素拼装成一个字符串返回。`var string = arr1.join(",");`

- **concat**：把arr1与arr2的数组元素组成一个新的数组返回
  `arr1 = arr1.concat(arr2);`

- **push**：将新元素添加到一个数组中，并返回数组的新长度值`var length = arr1.push("sunshine"); `

- **pop**：移除数组中的最后一个元素并返回该元素
  `var data = arr1.pop();`

- **shift**：移除数组中第一个元素，**并且返回**
  `var data = arr1.shift();`

- **unshift**：在数组的开头增加一个或多个元素，并返回数组的新长度。

- **splice**：第一个参数是开始删除元素的索引值，第二参数是删除元素的个数，往后的数据是插入的元素。

  > 此方法 很重要要理解到位，此方法可删除元素，可添加元素，可替换元素。原理就在于其三个（或更多）的参数的写法。

- Array.from()：从类数组对象或者可迭代对象中创建一个新的数组实例。

- Array.of()：根据一组参数来创建新的数组实例，支持任意的参数数量和类型。

- fill：将数组中指定区间的所有元素的值，都替换成某个固定的值。

  ```javascript
  var array1 = [1, 2, 3, 4];
  
  // fill with 0 from position 2 until position 4
  console.log(array1.fill(0, 2, 4));
  // expected output: [1, 2, 0, 0]
  
  // fill with 5 from position 1
  console.log(array1.fill(5, 1));
  // expected output: [1, 5, 5, 5]
  
  console.log(array1.fill(6));
  // expected output: [6, 6, 6, 6]
  ```

- indexOf()：返回数组中第一个与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。

- lastIndexOf()：返回数组中最后一个（从右边数第一个）与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1。

**迭代方法**

- forEach()：为数组中的每个元素执行一次回调函数。

  ```JavaScript
  let arr = [1,3,4,5,6,7,8];
  
  arr.forEach(item => {
      console.log(item);
  })
  ```

  ![截图-1564571123](https://wimg.misiyu.cn/images/20190731/1564571123_d267dd5489997c7.png?x-oss-process=style/first)

- every()：如果数组中的每个元素都满足测试函数，则返回 `true`，否则返回 `false。`

  ```JavaScript
  let arr = [1,3,4,5,6,7,8];
  
  let brr = arr.every(item => {
      if(item >2) return true;
      return false;
  });
  
  console.log(brr)
  ```

  ![截图-1564571215](https://wimg.misiyu.cn/images/20190731/1564571215_1e1bb6824975cce.png?x-oss-process=style/first)

- some()：如果数组中至少有一个元素满足测试函数，则返回 true，否则返回 false。

  ```JavaScript
  let arr = [1,3,4,5,6,7,8];
  
  let brr = arr.some(item => {
      if(item >2) return true;
      return false;
  });
  
  console.log(brr)
  ```

  ![截图-1564571273](https://wimg.misiyu.cn/images/20190731/1564571273_827b4e144356e0e.png?x-oss-process=style/first)

- filter()：将所有在过滤函数中返回 `true` 的数组元素放进一个新数组中并返回。

  ```JavaScript
  let arr = [1,3,4,5,6,7,8];
  
  let brr = arr.filter(item => {
      if(item > 5) return true;
      return false;
  });
  
  console.log(brr)
  ```

  ![截图-1564571333](https://wimg.misiyu.cn/images/20190731/1564571333_cae9a6575ee44bf.png?x-oss-process=style/first)

- map()：返回一个由回调函数的返回值组成的新数组。

  ```JavaScript
  let arr = [1,3,4,5,6,7,8];
  
  let brr = arr.map(item => {
      return 1;
  });
  
  console.log(brr)
  ```

  ![截图-1564571403](https://wimg.misiyu.cn/images/20190731/1564571404_6c90f148cd2e402.png?x-oss-process=style/first)

- reduce()：从左到右为每个数组元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。

## String对象

```JavaScript
var str1 = new String("hello");
var str2 = new String("hello");
document.write("两个字符串的对象一样吗？"+(str1.toString()==str2.toString()));//true
```

创建一个字符串的方式

> 方式1：new String("字符串的内容");
> 方式2：var str = "字符串的内容";

**长字符串**

有时，你的代码可能含有很长的字符串。你可能想将这样的字符串写成多行，而不是让这一行无限延长或着被编辑器折叠。有两种方法可以做到这一点。

其一，可以使用 + 运算符将多个字符串连接起来，如下所示：

```JavaScript
let longString = "This is a very long string which needs " +
                 "to wrap across multiple lines because " +
                 "otherwise my code is unreadable.";
```

其二，可以在每行末尾使用反斜杠字符（“\”），以指示字符串将在下一行继续。确保反斜杠后面没有空格或任何除换行符之外的字符或缩进; 否则反斜杠将不会工作。 如下所示：

```JavaScript
let longString = "This is a very long string which needs \
to wrap across multiple lines because \
otherwise my code is unreadable.";
```

**字符串比较**

熟练使用 C 语言的开发者经常使用 `strcmp` 函数来比较字符串，但在 JavaScript 中，你只需要

```JavaScript
var a = "a";
var b = "b";
if (a < b) // true
  print(a + " is less than " + b);
else if (a > b)
  print(a + " is greater than " + b);
else
  print(a + " and " + b + " are equal.");
```

**属性**

- length：返回字符串长度

**方法**

- String.prototype.charAt()
  返回特定位置的字符。
- String.prototype.charCodeAt()
  返回表示给定索引的字符的Unicode的值。
- String.prototype.codePointAt()
  返回使用UTF-16编码的给定位置的值的非负整数。
- String.prototype.concat()
  连接两个字符串文本，并返回一个新的字符串。
- String.prototype.includes()
  判断一个字符串里是否包含其他字符串。
- String.prototype.endsWith()
  判断一个字符串的是否以给定字符串结尾，结果返回布尔值。
- String.prototype.indexOf()
  从字符串对象中返回首个被发现的给定值的索引值，如果没有找到则返回-1。
- String.prototype.lastIndexOf()
  从字符串对象中返回最后一个被发现的给定值的索引值，如果没有找到则返回-1。
- String.prototype.localeCompare()
  返回一个数字表示是否引用字符串在排序中位于比较字符串的前面，后面，或者二者相同。
- String.prototype.match()
  使用正则表达式与字符串相比较。
- String.prototype.normalize()
  返回调用字符串值的Unicode标准化形式。
- String.prototype.padEnd()
  在当前字符串尾部填充指定的字符串， 直到达到指定的长度。 返回一个新的字符串。
- String.prototype.padStart()
  在当前字符串头部填充指定的字符串， 直到达到指定的长度。 返回一个新的字符串。
- String.prototype.repeat()
  返回指定重复次数的由元素组成的字符串对象。
- String.prototype.replace()
  被用来在正则表达式和字符串直接比较，然后用新的子串来替换被匹配的子串。
- String.prototype.search()
  对正则表达式和指定字符串进行匹配搜索，返回第一个出现的匹配项的下标。
- String.prototype.slice()
  摘取一个字符串区域，返回一个新的字符串。
- String.prototype.split()
  通过分离字符串成字串，将字符串对象分割成字符串数组。
- String.prototype.startsWith()
  判断字符串的起始位置是否匹配其他字符串中的字符。
- String.prototype.substr()
  通过指定字符数返回在指定位置开始的字符串中的字符。
- String.prototype.substring()
  返回在字符串中指定两个下标之间的字符。
- String.prototype.trim()
  从字符串的开始和结尾去除空格。参照部分 ECMAScript 5 标准。
- String.prototype.trimStart()
  String.prototype.trimLeft() 
  从字符串的左侧去除空格。
- String.prototype.trimEnd()
  String.prototype.trimRight() 
  从字符串的右侧去除空格。

## Number对象

创建Number对象的方式	

> 方式1:var 变量 = new Number(数字)	
> 方式2:var 变量 = 数字; // 十进制	

常用方法	

> toString()  把数字转换成指定进制形式的字符串，默认十进制
> toFixed()   指定保留小数位(四舍五入)

```JavaScript
var num = 10; // 十进制	
num.toString(2); // 二进制
var num2 = 3.455;
num2.toFixed(2); // 保留两位小数
```

![截图-1564572100](https://wimg.misiyu.cn/images/20190731/1564572100_a9d649382572c8d.png?x-oss-process=style/first)

## Date日期对象

**创建**

创建一个新Date对象的唯一方法是通过new 操作符：

```JavaScript
let now = new Date();
// 2019-07-31T11:23:49.162Z
```

**语法**

```JavaScript
new Date();
new Date(value);
new Date(dateString);
new Date(year, monthIndex [, day [, hours [, minutes [, seconds [, milliseconds]]]]]);
```

> **注意 参数**`monthIndex` 是从“0”开始计算的，这就意味着一月份为“0”，十二月份为“11”。

> **注意：**当Date作为构造函数调用并传入多个参数时，如果数值大于合理范围时（如月份为 13 或者分钟数为 70），相邻的数值会被调整。比如 new Date(2013, 13, 1)等于new Date(2014, 1, 1)，它们都表示日期2014-02-01（注意月份是从0开始的）。其他数值也是类似，new Date(2013, 2, 1, 0, 70)等于new Date(2013, 2, 1, 1, 10)，都表示同一个时间：`2013-03-01T01:10:00`。

> **注意：**当Date作为构造函数调用并传入多个参数时，所定义参数代表的是当地时间。如果需要使用世界协调时 UTC，使用 `new Date(Date.UTC(...))` 和相同参数。

```
year
```

表示年份的整数值。 0到99会被映射至1900年至1999年，其它值代表实际年份。参见 [示例](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date#Two_digit_years_map_to_1900_-_1999)。

```
monthIndex
```

**分别提供日期与时间的每一个成员**

> 当至少提供了年份与月份时，这一形式的 `Date() `返回的 `Date `对象中的每一个成员都来自下列参数。没有提供的成员将使用最小可能值（对日期为`1`，其他为`0`）。

表示月份的整数值，从 0（1月）到 11（12月）。

`day` 可选

表示一个月中的第几天的整数值，从1开始。默认值为1。

`hours` 可选

表示一天中的小时数的整数值 (24小时制)。默认值为0（午夜）。

`minutes` 可选

表示一个完整时间（如 01:10:00）中的分钟部分的整数值。默认值为0。

`seconds` 可选

表示一个完整时间（如 01:10:00）中的秒部分的整数值。默认值为0。

`milliseconds` 可选

表示一个完整时间的毫秒部分的整数值。默认值为0。



**方法**

- Date.now()
  返回自 1970-1-1 00:00:00  UTC（世界标准时间）至今所经过的毫秒数。

- Date.parse()

  由于浏览器解析的不一致性，该方法不建议使用。

- Date.UTC()
  接受和构造函数最长形式的参数相同的参数（从2到7），并返回从 1970-01-01 00:00:00 UTC 开始所经过的毫秒数。

**Getter**

- **Date.prototype.getDate()**
  根据本地时间返回指定日期对象的月份中的第几天（1-31）。
- **Date.prototype.getDay()**
  根据本地时间返回指定日期对象的星期中的第几天（0-6）。
- **Date.prototype.getFullYear()**
  根据本地时间返回指定日期对象的年份（四位数年份时返回四位数字）。
- **Date.prototype.getHours()**
  根据本地时间返回指定日期对象的小时（0-23）。
- **Date.prototype.getMilliseconds()**
  根据本地时间返回指定日期对象的毫秒（0-999）。
- **Date.prototype.getMinutes()**
  根据本地时间返回指定日期对象的分钟（0-59）。
- **Date.prototype.getMonth()**
  根据本地时间返回指定日期对象的月份（0-11）。
- **Date.prototype.getSeconds()**
  根据本地时间返回指定日期对象的秒数（0-59）。
- **Date.prototype.getTime()**
  返回从1970-1-1 00:00:00 UTC（协调世界时）到该日期经过的毫秒数，对于1970-1-1 00:00:00 UTC之前的时间返回负值。
- Date.prototype.getTimezoneOffset()
  返回当前时区的时区偏移。
- Date.prototype.getUTCDate()
  根据世界时返回特定日期对象一个月的第几天（1-31）.
- Date.prototype.getUTCDay()
  根据世界时返回特定日期对象一个星期的第几天（0-6）.
- Date.prototype.getUTCFullYear()
  根据世界时返回特定日期对象所在的年份（4位数）.
- Date.prototype.getUTCHours()
  根据世界时返回特定日期对象当前的小时（0-23）.
- Date.prototype.getUTCMilliseconds()
  根据世界时返回特定日期对象的毫秒数（0-999）.
- Date.prototype.getUTCMinutes()
  根据世界时返回特定日期对象的分钟数（0-59）.
- Date.prototype.getUTCMonth()
  根据世界时返回特定日期对象的月份（0-11）.
- Date.prototype.getUTCSeconds()
  根据世界时返回特定日期对象的秒数（0-59）.
- Date.prototype.getYear()【不再建议使用此方法】
  根据特定日期返回年份 (通常 2-3 位数). 使用 getFullYear() .

```JavaScript
let now = new Date();
console.log('getFullYear : '+now.getFullYear())
console.log('getMonth : '+now.getMonth())
console.log('getDate : '+now.getDate())
console.log('getDay : '+now.getDay())
console.log('getHours : '+now.getHours())
console.log('getMinutes : '+now.getMinutes())
console.log('getSeconds : '+now.getSeconds())
console.log('getTime : '+now.getTime()) // 毫秒数 
```

![截图-1564572931](https://wimg.misiyu.cn/images/20190731/1564572931_0bd71995c967df3.png?x-oss-process=style/first)

## Math对象

**常用方法**

- ceil 	  向上取整
- floor()   向下取整
- random()  随机数方法，产生的伪随机数介于0和1之间(含0不含1)
- round     四舍五入

```JavaScript
Math.ceil(3.14) //4
Math.floor(3.14) //3
Math.random()
Math.round(3.75) //4
```

## 自定义对象

> 在ES6之前，js没有类的概念。只要有函数即可创建对象

方式1:使用无参函数创建对象

```JavaScript
function Person(){}
var p = new Person(); //创建了一个Person对象
p.id = 110;
p.name = "sunshine";
```

方式2:使用带参的函数创建对象	

```JavaScript
function Person(id,name){
	this.id = id;
	this.name = name;
	this.say = function(){
		alert(name+"，你好");
	}
}
var p = new Person(110,"sunshine");
```

方式3:使用Object函数创建对象	

```JavaScript
var p = new Object();
p.id = 110;
p.name = "sunshine";
```

方式4:使用字面量的方式创建.

```JavaScript
var p = {
	id:110,
	name:"sunshine",
	say:function(){
		alert(this.name+"，你好");
	}
}
document.write("编号："+ p.id+" 姓名："+ p.name);
p.say();
```

## Prototype

需求:把getMax方法添加到数组对象中

```JavaScript
functoin Array(){
	this.prototype = new Object();
	this.getMax = function(){
	
	}
}
```

注意细节：

> 1.prototype是函数(function)的一个保留属性，任何function都有
> 2.prototype的值是一个对象
> 3.可以任意修改函数的prototype属性的值。
> 4.一个对象会自动拥有prototype的所有成员属性和方法。

```JavaScript
String.prototype.toCharArray = function(){
	var arr = new Array();
	for(var index = 0; index<this.length ;index++){
		arr[index] = this.charAt(index);	
	}
	return arr;
}	
String.prototype.reverse = function(){
	//字符串-->字符数组
	var arr = this.toCharArray();
	arr.reverse();
	return arr.join("");
}
var str = "wudao, I love you";
str = str.reverse();
console.log("翻转后的字符串："+str);
```

![截图-1564573460](https://wimg.misiyu.cn/images/20190731/1564573460_9a4e58c152fed06.png?x-oss-process=style/first)

## 事件

**注册事件方式**

1、在元素内注册：

```html
<body onload="ready()">
    <script>
       function ready(){
           alert('onload');
       }
    </script>
</body>
```

![截图-1564573562](https://wimg.misiyu.cn/images/20190731/1564573562_c1a6b361ef127e6.png?x-oss-process=style/first)

2、先获取DOM，再注册：

```html
<body>

</body>
<script>
    let body = document.querySelector('body');
    body.onload = function () {
        alert('I am load');
    }
</script>
```

![截图-1564573695](https://wimg.misiyu.cn/images/20190731/1564573696_6c4f7a353c1f78a.png?x-oss-process=style/first)

**常用事件**

**鼠标点击相关**

- onclick 单击
- ondblclick 双击
- onmousedown 按下按钮 
- onmouseup 释放按钮

**鼠标移动相关**

 - onmouseout 鼠标指针移出对象边界

 - onmousemove 鼠标划过对象

**焦点相关**

 - onblur 对象失去输入焦点
 - onfocus 对象获得焦点

**其他**

- onchange 对象或选中区的内容改变 
- onload 浏览器完成对象的装载 
- onsubmit 表单将被提交

## BOM浏览器对象模型

javascript组成部分

> EMCAScript ( 基本语法 )
> BOM( Browser Object MOdel) 浏览器对象模型.

浏览器对象模型中把浏览器的各个部分用一个对象进行描述，如果我们要操作浏览器的一些属性，可以通过浏览器对象模型的对象进行操作

> window  代表了一个新开的窗口
> location  代表了地址栏对象
> screen  代表了整个屏幕的对象



### 1.windows窗口对象

window对象常用方法	

- open()   打开一个新的窗口。
- resizeTo() 将窗口的大小更改为指定的宽度和高度值。
- moveBy()  相对于原来的窗口移动指定的x、y值。 
- moveTo() 将窗口左上角的屏幕位置移动到指定的 x 和 y 位置。 
- setInterval() 每经过指定毫秒值后就会执行指定的代码。
- clearInterval() 根据一个任务的ID取消的定时任务。
- setTimeout() 经过指定毫秒值后执行指定的代码一次。

> 注意:使用window对象的任何属性与方法都可以省略window对象不写的。

### 2.Location地址栏对象

- href:设置及获取地址栏对象
- reload():刷新当前页面

具体还有哪些内容，可在浏览器控制台输入`location`查看：

![截图-1564574128](https://wimg.misiyu.cn/images/20190731/1564574128_7be5da970e41077.png?x-oss-process=style/first)

### 3.Screen屏幕对象

**基本被支持的方法/属性**

- Screen.availHeight
  Specifies the height of the screen, in pixels, minus permanent or semipermanent user interface features displayed by the operating system, such as the Taskbar on Windows.

  （指定屏幕高度（以像素为单位）减去操作系统显示的界面，如任务栏）

- Screen.availWidth
  Returns the amount of horizontal space in pixels available to the window.

  （返回窗口可用的水平空间量（以像素为单位）。）

- Screen.colorDepth
  Returns the color depth of the screen.

  （返回屏幕的颜色深度。）

- Screen.height
  Returns the height of the screen in pixels.

  （返回屏幕的高度（以像素为单位）。）

- Screen.width
  Returns the width of the screen.

  （（返回屏幕的高度（以像素为单位）。））

![截图-1564574371](https://wimg.misiyu.cn/images/20190731/1564574371_70f39a0d958dd0b.png?x-oss-process=style/first)

## DOM文档对象模型	

html页面被浏览器加载时，浏览器会对整个html页面上所有标签创建对应的对象进行描述，用户看到的信息是这些html对象的属性信息。只要能找到对应的对象操作对象的属性，则可以改变浏览器当前显示的内容。

```html
var allNodes = document.all; //获取html文件中的所有标签节点
for(var i = 0; i < allNodes.length ; i++){
	alert(allNodes[i].nodeName); //标签的名字 nodeName
}
function writeUrl(){
	var links = document.links; //获取文档中含有href的属性的标签
	for(var i = 0; i<links.length ; i++){
		links[i].href = "http://www.misiyu.cn";
	}
}
```

## 节点查找

通过html元素的标签属性找节点

```JavaScript
document.getElementById("html元素的id") 
document.getElementsByTagName("标签名") 
document.getElementsByName("html元素的name")
```

通过关系(父子关系、兄弟关系)找标签

```JavaScript
parentNode	    获取当前元素的父节点
childNodes	    获取当前元素的所有下一级子元素
firstChild	    获取当前节点的第一个子节点
lastChild	    获取当前节点的最后一个子节点
nextSibling		获取当前节点的下一个节点（兄节点）
previousSibling	获取当前节点的上一个节点（弟节点）
```

## 节点创建、插入、删除

创建字节入插入节点、设置节点的属性。

> document.createElement("标签名")		创建新元素节点
> elt.setAttribute("属性名", "属性值")	设置属性
> elt.appendChild(e)						添加元素到elt中最后的位置

```JavaScript
//创建指定标签名的节点
var inputNode = document.createElement("input"); 	
//设置节点的属性
inputNode.setAttribute("type","button");
inputNode.setAttribute("value","按钮");
//定义新节点的父节点
var bodyNode = document.getElementsByTagName("body")[0];
//appendChild添加子节点到父结点末尾
bodyNode.appendChild(inputNode);
```

插入目标元素的位置	 

> elt.insertBefore(newNode, oldNode);	添加到elt中，child之前，elt必须是oldNode的直接父节点
> elt.removeChild(child)			    删除指定的子节点，elt必须是child的直接父节点

```JavaScript
var newNode = document.createElement("span");
var bodyNode = document.getElementsByTagName("body")[0];
var oldNode = document.getElementsByTagName("script")[1];
bodyNode.insertBefore(newNode,oldNode);
bodyNode.removeChild(oldNode);
```

## 操作CSS样式

```JavaScript
var spanNode = document.getElementById("code");
spanNode.innerHTML = code;
spanNode.style.fontSize ="24px";
spanNode.style.color = "red";
spanNode.style.backgroundColor="gray";
spanNode.style.textDecoration = "line-through";
```

现在还有dom.classList.*

比如dom.classList.add()，dom.classList.remove()

## 正则表达式

正则表达式的创建方式

> 方式1: /正则表达式/模式
> 方式2: new RegExp("正则表达式",模式);

正则表达式对象常用方法

> test()  使用正则对象去匹配字符串，如果匹配成功返回ture，否则返回false
> exec()  根据正则表达式去查找字符串符合规则的内容

模式

> g （全文查找出现的所有pattern） 	
> i （忽略大小写）

```JavaScript
var str = "hello123";
var reg = /^[A-Z0-9]+$/i;
alert(reg.test(str));	
```

```JavaScript
var str = "hello123";
var reg = /^[A-Z0-9]+$/i;
alert(reg.test(str));	
//查找出三到四个字符组成的单词。
var str  ="sun jian feng sunshine studio";
var reg = /\b[a-z]{3,4}\b/gi;
var line ="";
while((line = reg.exec(str))!=null){
	document.write(line+"<br/>")
}
```