# #目录

>[TOC]

## 一、浏览器执行JS过程
浏览器分为两部分：渲染引擎和JS引擎
##### 渲染引擎:用来解析HTML与CSS，俗称内核，如chrome浏览器的blink,老版本的webkit
##### JS引擎:也成为JS解释器，用来读取网页中的JS代码，对其处理后运行，比如chorme浏览器的V8
浏览器本身并不会执行JS代码，而是通过内置的JS引擎(解释器)来执行JS代码，逐行解释每一句源码(转换为机器语言)，然后用计算机去执行，所以JS语言归于脚本语言，会逐行解释执行

## 二、JS三部分组成:ECMAScript DOM BOM
1、ECMAScript：JS语法
2、DOM-文档对象模型:是W3C组织推荐的处理可扩展标记语言的标准编程接口，通过DOM提供的接口可以对页面上的各种元素进行操作(大小、位置、颜色等)
3、BOM-浏览器对象模型:提供了独立于内容的、可以与浏览器窗口进行互动的对象结构，通过BOM可以操作浏览器的窗口，比如弹出框、控制浏览器跳转、获取分辨率等

## 三、JS三种书写位置
1、行内式JS：一般更推荐使用单引号；可以将单行或少量JS代码写在HTML标签的实践属性中，以on开头的元素，如:onclick
2、内嵌式
3、外部JS文件：引用外部JS文件的script标签中间不可以写代码！

## 四、JS输入输出语句
1、alert(msg)：浏览器弹出警示框（输出）
2、console(msg)：浏览器控制台打印输出信息（一般程序员查看）

```
var age = 18; //声明变量同时赋值为18(一般命名为初始化)
var myname = '小蓝'; //一般使用单引号
console.log(age); //在调试台输出变量，一般需要使用log文件日志
```
3、prompt(info)：浏览器弹出框，用户可以输入
&emsp;取过来的数值是字符串型的，比如我输入数字18，实际上会变成字符串’18‘，因此无法进行加减操作

## 五、变量
变量本质就是程序在**内存**中申请的一块用来存放数据的**空间**

JS式一种弱类型或者是动态语言，变量的数据类型是由JS引擎 **根据 = 右边变量值的数据类型** 来判断的
也就意味着相同的变量可以用作不同的类型，比如上面定义的是int
但是下面写了myname = 'pink'就变成了字符串类型

### 1、简单数据类型（基本数据类型）
(1)Number:数字型，包含整值型和浮点型
&emsp;  八进制前面加0,如010代表十进制里面的8；十六进制前面加0x，例如0xa代表十进制里面的10
&emsp; infinity代表无穷大，大于任何数值;-infinity代表无穷小，小于任何数值；NaN代表一个非数值
&emsp; 可以用isNaN判断一个变量是否为非数字类型，如果是则返回false，如果不是则返回true

```js
console.log(isNaN(undefined)); //true
console.log(isNaN(12)) //false
console.log(isNaN(null));  //false --> 可以转换为0
console.log(isNaN(Boolean));  //true
console.log(isNaN(NaN))  //true
console.log(isNaN(String))  //true

函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。NaN会转换为0
函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。

```

(2)字符串类型：可以是引号中的任意文本，语法为双引号和单引号
&emsp; 2.1 字符串转义符：\n 换行符&emsp;  \t缩进&emsp;  \b空格
&emsp; 2.2 字符串长度：通过**length**属性可以获取整个字符串的长度

```
var str = 'my name is zhujingxian' ;  
console.log(str.length);  
```
 &emsp;2.3 字符串拼接：多个字符串之间可以只用 + 进行拼接，拼接方式为字符串 + 任何类型 = 拼接之后的新字符串（**数值相加，字符相连**） --> undefined,null,Number,boolean等和字符串拼接之后仍然是字符串
 ```
 console.log(12+12); //24
 console.log('12'+12) //'1212'
 ```
 &emsp;2.4 字符串拼接加强：经常会将字符串和变量来拼接，但是变量不能加引号，因为加了会变成字符串，可以使用‘引引加加’:删掉数字，变量写中间
 ```
 var age = 18;
 console.log('我今年' + age + 岁'); //显示我今年18岁
 ```
 &emsp;2.5 综合例子：
 ```
 var age = prompt('请输入你的年龄：');
 var str = '你今年已经'+age+'岁了';
 alert(str);
 ```
 (3) boolean：布尔值类型，如true、false等价于1和0

布尔值为false的情况：undefined，null, ''(空字符串)，0，NaN

数组在传入Boolean值的时候是true,包括空数组--> Boolean([]) = true

 ```
 console.log(flag+1);  //等于2
 ```
 (4) undefined:例如var a，上面了变量a但是没有给值，此时a=undefined
 ```
 var variable;
 console.log(variable + 'pink'); // 'variablepink' ps:任何拼接的和字符串相加都是字符串
 console.log(variable + 1);  //NaN undefined类型和数字相加最后是NaN
 
 undefined的情况：
 　　（1）声明变量，但未赋值
 　　（2) 函数无返回值(无return)，执行后返回undefined
 　　（3) 函数中可选参数，没有传参时返回undefined(如果实参数小于形参数则输出NaN,但是多余的形参是undefined)
 　　（4）对象中不存在或未赋值的属性
 ```
 (5)null:空值，typrof null是返回object的,主要用于赋值给一些可能会返回对象的变量，作为初始化
 ```
 var space = null;
 console.log(space + 'pink'); //输出'nullpink'
 console.log(space + 1); //输出1
 ```

（6）补充：NaN(但它不属于简单数据类型)：NaN 指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。

```
typeof NaN = number
NaN !== NaN //true
NaN == NaN //false
NaN === NaN  //false
```



 ### 2、获取检测变量的数据类型：typeof

数组、对象、null都会被判断为object，其他判断都正确

 ```
 var num = 18;
 console.log(typeof num); //这里typeof和变量之间是空格隔开
 
 console.log(typeof 2);               // number
 console.log(typeof true);            // boolean
 console.log(typeof 'str');           // string
 console.log(typeof []);              // object    
 console.log(typeof function(){});    // function
 console.log(typeof {});              // object
 console.log(typeof undefined);       // undefined
 console.log(typeof null);            // object
 ```

### 3、转换字符串类型

 使用表单、prompt获取过来的数据默认是字符串类型的，此时就不能直接进行简单的加法运算，而是需要转换变量的数据类型，就是把一种数据类型的变量转换成另外一种数据类型
 通常会有三种方式的转换：
 3.1 转换为字符串类型：
 &emsp;3.1.1 变量名.toString() 转换为字符串类型

 ```
 var num = 10;
 var str = num.toString()
 console.log(str);  //输出字符串'10'
 ```
&emsp;3.1.2 String((变量名))强制转换
```
console.logo(String(num));  //输出字符串
```
&emsp;3.1.3 **加号拼接字符串**（最常用）：和字符串拼接的结果都是字符串
```
console.log(num + '');
```
**3.2转换为数字型**（重点）
&emsp;3.2.1 parseInt(string)函数：可以将字符型的转换为数字型，得到的是整数，若识别不出来则输出NaN

```
var age = prompt('请输入你的年龄');
consple.log(parseInt(age));
console.log(parseInt('3.14')) //3取整，没有四舍五入，即使是‘3.94’也是3
console.log('120px'); //120会去掉px这个单位，因为先识别到数字
console.log('rem120px'); //输出NaN
```
&emsp;3.2.2 parseFloat(string)函数，可以将字符型的转换为浮点型，得到的是浮点型数值
```
console.log(parseInt('3.14')) //3.14，也会去掉单位和输出NaN
```
&emsp;3.2.3 Number()强制转换函数：将string类型转换为数值型,转换后整数型和浮点型都可以
```
var str = '123';
console.log(Number(str)); //123数字
console.log(Number('12'));  //12
```
&emsp;3.2.4 js隐式转换（- *  /）：利用算数运算隐式转换为数值型
```
console.log('12' - 0);  //12
console.log('123' - '120');  //3
console.log('123' * 1); //123
ps:console.log('123' + '123'); //'123123' 只有字符串数数字相加才会变成字符串，减乘除不会
```
3.3转换为布尔型:用Boolean()函数
代表**空、否定的值**都会被转换为false,如''、0、NaN、null、undefined;其余的值都会被转换成true
```
console.log(Boolean('')); //false
console.log(Boolean(12)); //ture
```
### 4、变量名称类型
&emsp;4.1 标识符：就是指开发人员为变量、属性、函数、参数取的名字（ps:标识符不能是关键字或保留字）
&emsp;4.2 关键字：指JS本身已经使用的字，不能用他们在充当变量名、方法名

>例如:break case catch continue default delete do else finally for function if in instanceof new return switch this throw try typeof var void while with等

&emsp;4.3保留字：实际上是预留的关键字，也就是将来可能是关键字，同样不能用作变量名或方法名
>例如:boolean byte char class const debugger double enum export extends fimal float goto implements import int interface long mative package private protected public short static super synchronized throws transient volatile等

## 六、运算符
### 1、算术运算符概述：+ - * / %(取余)

ps:浮点数有精度问题，因为其最高精度是17位小数
例如：0.1 + 0.1 == 0.2; //false，是0.200000000004

浮点数精度解决方法：

使用toFixed():`toFixed(num)` 方法可把 Number 四舍五入为指定小数位数(num)的数字;例如(0.1+0.2).toFixed(2)就是保留两位小数。另一个方法是设置一个误差范围，通常称为“机器精度”。对JavaScript来说，这个值通常为2-52，在ES6中，提供了`Number.EPSILON`属性，而它的值就是2-52，只要判断`0.1+0.2-0.3`是否小于`Number.EPSILON`，如果小于，就可以判断为0.1+0.2 ===0.3。

```
function numberepsilon(arg1,arg2){                   
  return Math.abs(arg1 - arg2) < Number.EPSILON;        
}        

console.log(numberepsilon(0.1 + 0.2, 0.3)); // true
```

**取余常见用法：**
判断一个数能被整除（余数为0就代表能被整除）

### 2、递增递减运算符
&emsp;递增运算符：++变量
&emsp;**(先自加1，后返回值)**
如果需要反复给数字变量增加或者减去1，可以使用++和--来实现;可以放在变量的前面（前置递增运算符），可以放在后面（后置递增/递减运算符）
&emsp;递减运算符:例如age++
**先返回原值，后自加1**
在单独使用是一样的，比如age=11;++age=age++(都是11)；但是在和别的加减时则不一样
**递增和递减运算符必须和变量配合使用**

```
var p=10;
console.log(++p + 10);  //输出21
console,log(p++ +10);  //输出20，但是此时的p是11，因为输出20后p再自加1
```
### 3、比较运算符：比较运算后会返回一个布尔值作为比较运算的结果
&emsp;ps:JS里面的等于号会转型，也就是会把字符串类型转换为数字型再进行比较；=== 或者 !==则是全等要求值和数值类型一致

```
console.log(18=='18'); //ture
consolee.log(18 === '18');  //false

转换规则：
(1) 一边是布尔类型，会转换为数值类型进行比较
(2) 一边是字符串类型，会转换为数值类型进行比较
(3) 一边是对象，将对象转换为原始值  -->原始值：原始数据类型的值(原始数据类型：Undefined、Null、Boolean、Number和String) 复杂值：Object(对象、数组、函数)
(4) null == undefined 
(5) NaN不管和谁比较都返回 false Boolean(NaN == NaN)//false,即使NaN === NaN也是false
```
一个等于号是赋值，两个等于号是判断，三个等于号是全等

### 4、逻辑运算符
&&与&emsp;||或&emsp;！非
！又叫取反符，用来取一个布尔值相反的值，如!ture=false
&emsp;3.1 逻辑中断（短路运算）：当有多个表达式(值)时，左边的表达式可以确定结果时，就不再继续运行右边的表达式的值
&emsp;&emsp;3.1.1 逻辑与短路运算：如果表达式1结果为真，则返回表达式2;如果表达式1为假，则返回表达式1(只有否定或空值才为假,在数字里面除了0其他都是真值)

```
|| 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先强制转换为布尔类型，然后再执行条件判断。
console.log(123 && 456); //456
console.log(0 && 123);  //0
console.log(0 && 1 +2 && 123* 4566);  //第一个为假其他都不需要看直接返回表达式1(即0值)
```
&emsp;&emsp;3.1.2 逻辑或短路运算：
语法：表达式1 || 表达式2
如果第一个表达式值为真，则返回表达式1；如果第一个表达式值为假，则返回表达式2

```
var num = 0;
console.log(123 || num++);
console.log(num++); //0,因为上面的num++实际没有运行,num一开始先返回了自己的值然后再++
```

### 5、赋值运算符
+=&emsp;-=&emsp;*=等

```
var age = 10;
age+=5; //相当于age=age+5;
```

### 6、运算符优先级

| 优先级 | 运算符 | 顺序 |
| :----:| :----: | :----: |
| 1 | 小括号 | （） |
| 2 | 一元运算符 | ++ -- ！ |
| 3 | 算术运算符 | 先*/后+- |
| 4 | 关系运算符 | >&emsp;>=&emsp;<&emsp;<=|
| 5 | 相等运算符 | == != === !==|
| 6 | 逻辑运算符 | 先&&后或值 |
| 7 | 赋值运算符 | = |
| 8 | 逗号运算符 | , |
ps:一元逻辑符里面的非逻辑优先级很高
一般比较使用逻辑运算符进行分割，因为逻辑运算符优先级较低

## 七、流程控制
### 1、流程控制主要有三种结构：顺序结构、分支结构、循环结构
&emsp;1.1 分支语句：if语句和switch语句
if语句：条件成立时执行代码，否则什么也不做
语法：
1.1.1if(条件表达式) {
 &emsp;&emsp;&emsp;&emsp;//条件成立时执行的代码语句
}
else {
 &emsp;&emsp;&emsp;&emsp;//条件不成立时执行的代码语句
}
else后面没有条件语句，直接写代码语句

1.1.2 if else语句
语法：
if(条件表达式1) { 
 &emsp;&emsp;&emsp;语句1
}
if else (条件表达式2) {
 &emsp;&emsp;&emsp;语句2
}
if else (条件表达式3) {
 &emsp;&emsp;&emsp;语句3
}
else {
 &emsp;&emsp;&emsp;最后的语句
}
执行思路：（1）如果条件表达式1满足条件则执行，执行完毕后退出整个分支语句
（2）如果条件表达式1不满足，则判断条件表达式2，满足的化执行语句2，以此类推
（3）如果上面的所有条件表达式都不成立，则执行else里面的语句
**注意点：**
（1）多分支语句还是多选1，最后只能有一个语句执行
（2）else if里面的条件理论上是可以任意多个的，且else if 后面记得加空格

1.1.3 switch语句
switch语句也是多分支语句，它用于基于不同的条件执行不同的代码；当要针对变量设置一系列的特定值的选项时，就可以使用switch
语法：
switch(表达式) {
&emsp;&emsp;case value1:
&emsp;&emsp;执行语句1;
&emsp;&emsp;break;
&emsp;&emsp;case value2:
&emsp;&emsp;执行语句2;
&emsp;&emsp;break;
···
&emsp;&emsp;default:
&emsp;&emsp;执行最后的语句;
}
执行思路：
利用表达式的值和case后面的选项值像匹配，如果匹配上，就执行该case里面的语句，如果都没有匹配上，那么执行default里面的语句（执行时是直接将表达式里面的值来和value进行对比，如果符合第二个条件就直接跳到2，实际上并不经过1）
常见问题：
（1）开发里面的表达式一般写成变量
（2）表达式里面的值和case里面的值匹配时是全等，也就是值和数据类型一致才可以
（3）break如果当前case里面没有break，则不会退出switch，而实继续执行下一个case

1.2 三元表达式
语法：条件表达式？ 表达式1：表达式2;
如果条件表达式结构为真，则返回表达式1的值；如果表达式结构为假，则返回表达式2的值

## 八、循环
### 1、for循环
for(初始化变量; 条件表达式; 操作表达式) {
&emsp;&emsp;&emsp;循环体
}
（1）初始化变量：就是用var声明一个普通变量，通常用于作为计数器使用
（2）条件表达式：就是用来决定每次循环是否继续执行，是终止的条件
（3）操作表达式：是每次循环最后执行的代码，常用于计数器变量的更新（递增或递减）

```
for (var i=1; i<=100; i++>) {
    console.log('你好')
}
```
操作流程：
（1）首先执行里面的计数器变量 var i=1，但是这句话在for里面只执行一次
（2）去i<=100判断是否满足条件，若满足则执行循环体，不满足则退出循环
&emsp;&emsp;**而不是直接执行i++，是先执行循环体console语句**
（3）最后执行i++，i++是单独写的代码 之后第一轮结束
（4）第二轮开始去执行i<=100，如果满足条件则执行循环体，不满足则退出循环

**双重for循环：**
当外层的循环一次，里层的for循环完整个设定的次数；外层循环控制行数，里层循环控制一行输出多少个（相当于列数）
```
//打印五行五列星星
    var str = '';
    for(var i=1;1<=5;i++){ //外层循环负责打印五行
        for(var j=1;j<=5;j++){ //里层循环负责打印一行的五个星星
            str = str + '★';
        }
        str = str + '\n';
    }
    console.log(str); 
```

### 2、while循环
while(条件表达式){
&emsp;&emsp;&emsp;循环体
}
执行思路：当条件表达式结果为true的时候则执行循环体；否则退出循环
ps:里面应该也有计数器和初始化变量；里面应该也有操作表达式完成计数器的更新防止死循环
while循环可以做一些更复杂的条件判断（与for对比的情况下）

```
var num = 1;
while (num <= 100){
    console.log('abc');
    num++;
}
```

### 3、do while循环
该循环会先执行一次代码块，然后对条件表达式进行判断，如果条件为真就会重复执行循环体，否则退出循环
do {
&emsp;&emsp;循环体代码块-条件表达式为true时重复执行循环体代码
}while(条件表达式)

```
var i = 1;
do {
    console.log('how are you');
    i++
}while(i <= 100)
```

### 4、continue：退出本次（当前词的循环），继续执行剩余次数循环

```
//求1-100之间除了能被7整除以外的整数和
var sum = 0;
for (var i =1; i <= 100; i++){
    if (i % 7 == 0){
        continue;  //在这里是退出这个循环
    }
    sum += i;
}
```

### 5、break:立即跳出整个循环

### 6、标识符命名规范
变量、函数的命名必须有意义
变量的名称一般是名词
函数的名词一般是动词

## 九、数组
数组是一组数据的集合，其中的每个数据被称为元素，在数组中可以存放任意类型的元素。数组是一种将一组数据存储在单个变量名下的优雅方式
### 1、创建数组方式
（1）利用new创建数组

```
var 数组名 = new Array();
例如：var arr = new Array();  //创建了一个新的空数组
```
（2）利用数组字面量创建数组
```
var 数组名 = [];
例如：var 数组名 = ['小白',3,'黑色'];  //里面的数组元素类型不受限制
```
### 2、访问数组元素
通过数组名[索引]的形式来进行访问，注意索引值是从0开始的

### 3、遍历数组元素
遍历就是把数组中的每个元素从头到尾都访问一次，使用循环来实现这个功能

```
var arr = ['red','green','blue'];
for (var i = 0; i < 3; i++){ 
    //这里的i必须为0,因为索引号是从0开始的；i必须小于3不能等于，因为最后一个元素的索引号是2
    console.log(arr[i]);
}
```
### 4、数组的长度
用数组名.length来访问数组元素的数量（数组长度） 字符串也是用length来访问string长度

```
for (var i = 0; i < arr.length; i++){
    console.log(arr[i]);
}
```
arr[i]是数组元素第i个数组的元素
索引号是从0开始的，数组长度是元素个数

### 5、修改数组长度
5.1 通过修改length长度新增数组元素

```
var arr=['red','green','blue'];
arr.length=6;  //索引号3，4，5的变量是未赋值的，默认是undefined
```
5.2 通过修改数组索引新增数组元素
**ps:不能给数组名赋值，否则会覆盖以前的数据**

```
var arr=['red','green','blue'];
arr[4]='pink';
arr='这是不被允许的'; //这样子arr就没有了以前的数组元素，直接是字符串里面的内容了
```
 **6、冒泡排序**
是一种算法，把一系列的数据按照一定的顺序进行排列显示（从小到大或者从大到小）
它重复地走访过要排序地数列，一次比较两个元素，如果他们地顺序错误就把他们交换过来。走访数列地工作就是重复地进行直到没有再现需要交换，也就是该数列已经排序完成

```
                //冒泡排序
                    var arr = [5, 4, 3, 2, 1];
                    for (var i = 0;i <= arr.length - 1;i++) { //i和j必须等于0，因为索引号是从1开始的 外层循环管趟数
                        for (var j = 0;j <= arr.length - i - 1;j++) { //里面循环过每一趟的交换次数
                            //内部交换两个变量的值 注意一定是前一个和后一个数组元素相比较
                            if (arr[j] > arr[j + 1]) {
                                var temp = arr[j];
                                arr[j] = arr[j + 1];
                                arr[j + 1] = temp;
                            }
                        }
                    }
                    console.log(arr);
```
## 十、函数
函数就是封装了一段可以被重复执行调用的代码块，让大量的代码重复使用
### 1、函数使用：声明函数和调用函数
声明函数语法：
function 函数名(形参1，形参2...) { //在声明函数的小括号里面的是 形参
&emsp;&emsp;函数体
}
函数名(实参1，实参2...);  //在函数调用的小括号里面的是实参
ps:function是声明函数的关键字，全都是小写
函数是做某件事情，一般函数名是动词
函数不调用自己不执行，调用方式：函数名();

**实参和形参：**
（1）实参等于形参个数时：输出正常结果
（2）实参多于形参个数时：只取到形参的个数
**（3）实参个数小于形参个数：多的形参定义为undefined,结果为NaN**

return返回值：
function 函数名(){
&emsp;&emsp;return 需要返回的结果;
}
函数只是实现某种功能麻醉后的结果需要返回给函数的调用函数名，通过return实现
即相当于函数名 - return后面的结果
（1）但是return后面的代码不会被执行
（2）return后面只能输出一个值，返回的结果是最后一个值；如果想要实现多个值输出可以使用数组
**(3)如果没有return则输出的是undefined**
return不仅可以退出循环，还能够返回return语句中的值，同时还可以结束当前函数体内的代码

```
function getMax(num1,num2){
    if(num1>num2){
        return(num1);
    }
    else {
    return(num2);
    }
    或者函数里面直接用三元运算符：
    return num1>num2? num1:num2;
}
console.log(getMax(1,5));
```
利用函数求数组最大值
```
function getArrMax(arr) {
    var max = arr[0];//这里一定要记得把第一个值赋值给max
    for(var i=1;i<=arr.length;i++>){
        if(max<arr[i]){
            max=arr[i];
        }
    }
    return max;
}
getArrSum([3,67,98,34,87,23]);
```

### 2、arguments

当不确定有多少个参数传递时可以用arguments来获取；实际上是当前函数的一个内置参数，存储了传递的所有实参
但是只有函数才有arguments对象，输出的结果是一个**伪数组**，伪数组并不是真正意义上的数组：
（1）具有数组的length属性
（2）按照索引的方式进行存储
（3）它没有真正数组的一些方法 例如pop() push()等

### 3、函数的两种命名方式
（1）利用函数关键字自定义函数（命名函数）
function fn() {

}
fn();
（2）函数表达式（匿名函数）
var fun = function(aru){
    console.log('我是函数表达式')
    console.log(aru);
}
fun('这里实参传入形参aru')

## 十一、作用域
### 1、JS作用域

就是代码名字（变量）在某个范围内起作用和效果，目的是为了提高程序的可靠性以及减少命名冲突
JS的作用域（es6之前）：全局作用域 局部作用域
全局作用域：整个script标签或者是一个单独的js文件

局部作用域（函数作用域）：在函数内部就是局部作用域，这个代码的名字只在函数内部起效果和作用

### 2、全局变量和局部变量
全局变量：全局变量在JS的任何位置都可以使用（在函数外部定义的变量）
在全局作用域下var声明的变量是 全局变量
特殊情况下，在**函数**内不使用var声明的变量也是全局变量（不建议使用）

局部变量：在局部作用域下声明的变量叫做局部变量（在函数内部定义的变量）
局部变量只能在该函数内部使用
在函数内部var声明的变量是局部变量,如果没有var直接赋值例如a=1就会变成全局变量
函数的形参实际上就是局部变量

从执行效率来看：
全局变量只有在浏览器关闭的时候才会销毁，比较占内存资源
局部变量：当我们程序执行完毕之后就会销毁，比较节约内存资源

### 3、作用域链
（1）只要是代码就至少有一个作用域
（2）写在函数内部的是局部作用域
（3）如果函数中还有函数，那么在这个作用域中又诞生一个作用域
（4）根据作用域在**内部函数可以访问外部函数变量**的这种机制，用链式查找决定哪些数据能被内部函数访问，就称作作用域链（相当于就近原则）

## 十二、预解析和对象
### 1、JS引擎运行JS主要分为两部：预解析和代码执行
（1）**预解析：JS引擎会把JS里面的var还有function提升到当前作用域的最前面**
（2）代码执行：按照代码书写的顺序从上往下执行

预解析分为变量预解析（变量提升）和函数预解析（函数提升）
（1）**变量提升：就是把所有的变量声明提升到当前作用域的最前面，不提升赋值操作;例如var a=3,只提升var a;a=3仍然在原本的位置**
（2）函数提升：就是把所有的函数声明提升到当前作用域的最前面，不调用函数

### 2、对象
对象是一组无序的相关**属性和方法的集合**，所有的事物都是对象，例如字符串、数值、数组等
对象是由属性和方法组成的：
（1）属性：事物的特征，在对象中用属性来表示（常用名词）
（2）方法：事物的行为，在对象中用方法来表示（常用动词）

### 3、创建对象的方法
（1）对象字面量：就是花括号{}里面包含了表达这个具体事物（对象）的属性和方法
&emsp;&emsp;&emsp;{}里面采取键值对的形式展示；多个属性或方法中间用逗号隔开
键：相当于属性名；  值：相当于属性值，可以是任意类型的值（数字类型、布尔类型等）

使用对象：
调用对象的属性采取对象名.属性名，理解为console.log(obj.uname);
还可以使用对象名['属性值']的方法调用，即console.log(obj['age]);
调用对象的方法（即里面是个函数）：对象名.方法名()  千万别忘记加括号,例如obj.sayHi()

```
var obj = {
    uname: '张三'；
    age: 18;
    sex: '男';
    sayHi: function(){ //sayHi在这里叫做方法
        console.log('hi');  
    }
}
//属性是在对象里面的，不需要声明var
```
(2)利用new Object创建对象
var obj = new Object(); //创建了一个空的对象
```
var obj = new Object();
obj.uname='张三';
obj.sex=''男;
obj.sayHi=function(){
    console.log('hi');
}
```

**（3）利用构造函数创建对象**

语法：function 构造函数名（） {
    this.属性 = 值;
    this.方法 = function(){}
}
new 构造函数名(); //这里是调用函数的方法
ps:
（1）构造函数名字首字母要大写
（2）构造函数不需要return就可以返回结果
（3）调用构造函数必须使用new

```
//new构造函数名();
function Star(uname,age,sex){
    this.name=uname;
    this.age=age;
    this.sex=sex;
}
var ldh=new Star('刘德华',18,'男');
console.log(ldh.name);
```
**变量、属性、函数、方法的不同点和相同点：**
相同点：变量和属性都是用来存储数据的；函数和方法都是实现某种功能或者做某件事情
不同点：
（1）变量 单独声明并赋值，使用的时候直接写 变量名 单独存在
（2）属性 在对象里面不需要声明 使用的时候必须是对象.属性

（1）函数是单独声明并且调用的，使用时函数名()
（2）方法是在对象里面调用的时候使用对象.方法()

构造函数和对象;
（1）构造函数，例如Star()，抽象了对象的公共部分，封装到了函数里面，是泛指的某一大类
（2）创建对象，如new Stars(),是特指的一个具体的事物
利用构造函数创建对象的过程称为对象的实例化

new关键字执行：
（1）在内存中创建一个新的空对象
（2）让this指向这个新的对象
（3）执行构造函数里面的代码，让这个新对象添加属性和方法
（4）返回这个新对象（所以构造函数里面不需要return）

遍历对象：使用for in
```
for (var k in obj) {
    console.log(k); //k变量直接输出，得到的是属性名
    console.log(obj[k]); //obk[k]得到的是属性值
}
```

### 4、内置对象
**JS中的对象分为三种：自定义对象、内置对象、浏览器对象**
前两种对象是JS基础内容，属于ECMAScript；第三个浏览器对象独属于JS
内置对象就是JS语言自带的一些对象，这些对象共开发者使用，并提供了一些常见的或是最基本而必要的功能（属性和方法）

JS有哪些内置对象？（有标准内置对象）

```
全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在 全局作用域里的对象。全局作用域中的其他对象可以由用户的脚本创建或由宿主程序提供。

回答答案：
总结：
js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般经常用到的如全局变量值 NaN、undefined，全局函数如 parseInt()、parseFloat() 用来实例化对象的构造函数如 Date、Object 等，还有提供数学计算的单体内置对象如 Math 对象。

标准内置对象的分类：
（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。例如 Infinity、NaN、undefined、null 字面量
（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。例如 eval()、parseFloat()、parseInt() 等
（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等
（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date
（5）字符串，用来表示和操作字符串的对象。例如 String、RegExp
（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括数组和类型数组，以及类数组结构的对象。例如 Array
（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。
例如 Map、Set、WeakMap、WeakSet
（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。
例如 SIMD 等
（9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等
（10）控制抽象对象
例如 Promise、Generator 等
（11）反射。例如 Reflect、Proxy
（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等
（13）WebAssembly
（14）其他。例如 arguments

```



4.1 Math数学对象
不是一个构造函数，所以不需要new来调用，而是直接使用里面的属性和方法即可
例如：console.log(Math.PI);  console.log(Math.max(1,5,7,890));
(1)Math.abs:绝对值(但是遇到字符串会隐式转换)
console.log(Math.abs('-1'); //输出-1
(2)Math.ceil():向上取整  Math.floor():向下取整
(3)Math.round():四舍五入 就近取整 但是0.5特殊，一般往大了取值
console.log(Math.round(-1.5)); //结果是-1  
console.log(Math.round(1.5));  //结果是2
(4)random随机数方法：例如取随机的一个范围的整数

```
function getRandom(min, max){
    return Math.floor(Math.random()* (max-min+1))+min;  //这个公式是MDN查到的
}
```

4.2 Date对象：是一个构造函数，必须实例化new以后才能使用；主要用来处理日期和时间
使用方法：（1）获取当前时间必须实例化
```
var now = new Date();
console.log(now);
```
(2)Date()构造函数的参数：如果括号里面里有时间就返回参数里面的时间；例如日期格式字符串为'2019-4-5'可以写成new Date('2019-4-5')或者new Date('2019/4/5')

(3)格式化日期 年月日
```
var date = new Date();
var year = date.getFullYear(); //Data对象里面的方法getFullYear()
var month = date.getMonth + 1; //月份是从0开始算的
var dates = date.getDate();
var arr = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六']  
//星期是从周日开始算的
var day = getDay();
console.log('今天是：' + year + '年' + month + '月' + day + '日' + arr[day]); 
```

(4)格式化时间 时分秒
```
//例如封装一个函数返回当前的时分秒
function getTimer() {
    var time = new Date();
    var h = time.getHours();
    h=h<10? '0'+h:h;
    var m = time.getMinutes();
    m=m<10? '0'+m:m;    
    var s = time.getSeconds();
    s=s<10? '0'+s:s;
} 
console.log(getTimer());
```

(5)获得总的毫秒数（时间戳） 不是当前时间的毫秒数，而是距离1970年1月1日过了多少毫秒数:主要通过valueOf() 和 getTimer()获得
```
var date = new Date();
console.log(date.valueOf()); //第一种写法
console.log(date.getTime()); //第二种写法

var date1 = +new Date(); //等于date1=0+new Date()隐式转换
console.log(date1); //简单的写法也是最常用的写法

console.log(Date.now()); //H5新增的写法
```

经典案例：倒计时
```
1、核心算法：输入的时间减去现在的时间就是剩余的时间，即倒计时；但是不能拿时分秒相减，结果会是负数
2、用时间戳来做，用户输入时间总和毫秒数减去现在时间的总的毫秒数，得到的就是剩余时间的毫秒数
3、把剩余时间总的毫秒数转换为天、时、分、秒
转换公式如下：
d = parseInt(总秒数/60/60/24); //计算天数
h = parseInt(总秒数/60/60%24); //计算小时
m = parseInt(总秒数/60%60); //计算分数
s = parseInt(总秒数%60); //计算当前秒数

function countDown(time) {
    var nowTime = +new Date();  //返回当前时间总的毫秒数
    var inputTime = +new Date(time);  //返回用户输入的时间总的毫秒数
    var times = (inputTime - nowTime) /1000; //time时剩余时间总的秒数
    var d= parseInt(times/60/60/24);  //天
    d = d<10? '0' + d : d; 
    var h= parseInt(times/60/60%24);  //时
    h = h<10? '0' + h : h;     
    var m= parseInt(times/60%60);  //分
    m = m<10? '0' + m : m; 
    var s= parseInt(times%60);  //秒
    s = s<10? '0' + s : s; 
    return d+'天'+h+ '时'+m+'分'+s+'秒';
}
console.log(countDown('2019-5-1 18:00:00')); //一定要用标准格式写
var date = new
```
### 5、数组元素
5.1 创建数组元素的方法：前面提到的字面量和利用new Array()
利用new Array():
var arr1 = new Array(); //代表创建了一个空的数组
例如var arr1 = new Array(2); //这个2表示 数组长度为2，里面有两个空的数组元素，直接输出为undefined
var arr1 = new Array(2,3); //等价于[2,3] 表示里面有两个数组元素2和3

5.2 检测是否为数组
（1）instanceof 运算符 可以用来检测是否是数组,如果直接typeof则会返回object; `instanceof`**只能正确判断引用数据类型**，而不能判断基本数据类型。`instanceof` 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype` 属性

```
var arr = []; //数组
var obj = {}; //对象
console.log(arr instanceof Array); //返回ture
console.log(obj instanceof Array); //返回false

console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```
（2）Array.isArray(参数); H5新增的方法，ie9以上版本支持
```
console.log(Array.isArray(arr));  //ture
console.log(Array.isArray(obj));  //false
```

**5.3 添加数组元素**：

(1) push:在数组的末尾添加一个或者多个数组元素

```
var arr = [1,2,3];
arr.push(4,5);
console.log(arr); //输出数组[1,2,3,4,5]
console.log(arr.push(4,5)); //这样写的话就只会输出数组的长度5
```
push是可以直接给数组后面添加新的元素
push()参数直接写数组元素就可以了
push完毕之后,返回的结果是 新数组的长度,而不是整个数组
原数组也会发生变化

(2) unshift:在数组的开头添加一个或者多个数组元素
```
var arr = [1,2,3];
arr.unshift(4,5);
console.log(arr); //输出数组[4,5,1,2,3]
console.log(arr.push(4,5)); //这样写的话就只会输出数组的长度5
```
unshift是可以直接给数组前面添加新的元素
push()参数直接写数组元素就可以了
push完毕之后,返回的结果是 新数组的长度,而不是整个数组
原数组也会发生变化

5.4 删除数组元素
(1) pop():可以删除数组最后一个元素

```
var arr = [1,2,3,4,5];
arr.pop();
console.log(arr); //输出[1,2,3,4];
console.log(arr.pop()); //返回值是数组长度
```
pop()是可以删除数组的最后一个元素,但是一次只能删除一个元素
pop()没有参数
pop完毕之后,返回的结果是数组长度
原数组也会发生变化

(2) shift():可以删除数组第一个元素
```
var arr = [1,2,3,4,5];
arr.shift();
console.log(arr); //输出[2,3,4,5];
console.log(arr.shift()); //返回值是数组长度
```
shift()是可以删除数组的第一个元素,但是一次只能删除一个元素
shift()没有参数
pop完毕之后,返回的结果是数组长度
原数组也会发生变化

5.5 翻转数组reverse

```
var arr = [1,2,3,4];
arr.reverse();
console.log(arr); //输出[4,3,2,1]
```

5.6 数组排序(冒泡排序):sort,但是只对个位的排序有效,对非个位的使用function(先记住)

```
var arr1 = [13,4,77,2,5];
arr.sort(function(a,b)) {
    return a-b; //升序的顺序排序
    return b-a; //降序的顺序排序
}
```

**5.7 获取数组元素索引**
(1) indexOf(数组元素):返回该数组元素的索引号,从前面开始查找
它只返回第一个满足条件的索引号(如果有需要查找的数组元素在数组里面有多个相同的数组元素)
如果在该数组里面找不到元素,则返回-1，这里的-1是数字不是字符串

```
var arr = [1,2,3,5,67,78];
console.log(arr.indexOf(1));  //输出0
```
(2) lastindexOf(数组元素):稍微了解即可,作用是返回该数组的索引号,从后面开始查找
```
var arr = [1,2,3,5,67,78];
console.log(arr.lastindexOf(1));  //输出0(从后面开始查找但是索引号是不变的)
```

**重要案例:**
数组去重,要求去除数组中重复的元素
思路:把旧数组里面不重复的元素选取出来放进新数组中,重复的元素只保留一个,放到新数组中去重
核心算法:遍历旧数组,然后拿旧数组元素去查询新数组,如果该元素在新数组里面没有出现过,则添加,否则就不添加
Q:怎么知道该元素是否存在?利用新数组indexOf(数组元素),如果返回是-1则说明没有重复元素

```
//封装一个去重的函数 unique
function unique(arr){
    var newArr = [];
    for(i=0; i<arr.length; i++){
        if(newArr.indexOf(arr[i]) === -1) {
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
```

5.8 数组转化为字符串
(1) toString()

```
var arr= [1,2,35,5];
console.log(arr.toString()); //输出'1,2,35,3'
```
(2) join(分隔符)
```
var arr= [1,2,3];
console.log(arr.join()); //输出'1,2,3' 默认分隔符是逗号
console.log(arr.join('')); //输出'123'
console.log(arr.join('&'));  //输出'1&2&3'
```

5.9 额外补充 
(1) concat():连接多个或者两个数组,不影响原数组,返回一个新的数组
(2) splice():数组删除splice(第几个开始,要删除个数),返回被删除项目的新数组,但是会影响原数组

#### **5.10 总结（数组的原生方法）**

```
1. 数组和字符串的转换方法：toString()、toLocalString()、join() 其中 join() 方法可以指定转换为字符串时的分隔符。
2. 数组尾部操作的方法:
	2.1 pop()-> 删除数组最后一个元素
	2.2 push()-> 在数组末尾添加一个或者多个数组元素，push 方法可以传入多个参数。
3. 数组首部操作的方法:
	3.1 shift() -> 删除数组的第一个元素,但是一次只能删除一个元素 
	3.2 unshift() -> 在数组的开头添加一个或者多个数组元素 
	3.3 重排序的方法： 
		3.3.1 reverse() -> 翻转数组
		3.3.2 sort()-> 可以传入一个函数来进行比较，传入前后两个值，如果返回值为正数，则交换两个参数的位置(非个位)
			(sort是数组排序，但是只对个位的排序有效,对非个位的使用function)
4. 数组连接的方法 concat() -> 返回的是拼接好的数组，不影响原数组。
5. 数组截取办法 slice() -> 用于截取数组中的一部分返回，不影响原数组。
6. 数组插入方法 splice() -> splice(第几个开始,要删除个数),返回被删除项目的新数组,但是会影响原数组
7. 影响原数组查找特定项的索引的方法:
	7.1 indexOf() -> 返回该数组元素的索引号,从前面开始查找;它只返回第一个满足条件的索引号(如果有需要查找的数组元素在数组					里面有多个相同的数组元素),如果在该数组里面找不到元素,则返回-1，这里的-1是数字不是字符串
	7.2 lastIndexOf() -> 返回该数组的索引号,从后面开始查找
	7.3 迭代方法 
		7.3.1 every()
		7.3.2 some()
		7.3.3 filter() -> 创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素
		7.3.4 map() 
		7.3.5 forEach() 
8. 数组归并方法
	8.1 reduce() 
    8.2 reduceRight() 



总结：
方法：push(在数组末尾添加一个或者多个数组元素)，unshift，pop(删除数组最后一个元素)，shift，reverse,splice,concat
对原数组发生改变：push,unshift,pop,shift,reverse,splice
对原数组不发生改变：concat,slice(一般就要var 新变量来接收新数组),filter


具体容易弄错：
slice:从某个已有的数组返回选定的元素（可以用来从数组提取指定元素）
	-该方法不会改变元素数组，而是将截取到的元素封装到一个新数组中返回
	-参数：1.截取开始的位置索引，包含开始索引
		  2.截取结束的位置的索引，不包含结束索引（第二个参数可以省略不写，此时会截取从开始索引往后的所有元素）
		  3.索引可以传递一个负值，负值意味着从后往前计算（例如-1是倒数第一个，-2倒数第二个）
	-例子：
	var arr = ['a','b','c','d','e']
	var res = arr.slice(1,3) //['b','c']
	var res = arr.slice(2)  //['c','d','e']
	var res = arr.slice(1,-1) //['b','c','d']
	
	
splice:删除元素，并向数组添加新元素（可以用于删除数组中的指定元素）
	-使用splice()会影响到原数组，会将指定元素从数组中删除并将删除的元素作为返回值返回
	-参数：1.第一个表示开始位置的索引
		  2.第二个表示删除的数量
		  3.第三个表示如果要添加新元素时添加的元素
	-例子：
var arr = ['a','b','c','d','e']
var res = arr.splice(1,2)
console.log(res) //['b','c']
console.log(arr) //['a','d','e']
var res = arr.splice(1,1,'z','q') //arr = ['a','z','q']
var res = arr.splice(3,0,'z','q') 
console.log(res) //[]
console.log(arr)  //['a', 'b', 'c', 'z', 'q', 'd', 'e']
		 
		 
split('分隔符'):字符转换为数组(join是把数组转换为字符串)
例子：
    var str = 'red,pink,blue';
    console.log(str.split(,));  //输出[red,pink,blue]
    var str = 'red&pink&blue';
    console.log(str.split(&));  //输出[red,pink,blue]


filter:创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素
	-filter() 不会对空数组进行检测
	-Array.filter(function(currentValue, indedx, arr), thisValue)
		-参数：1.function 为必须，数组中的每个元素都会执行这个函数;且如果返回值为 true，则该元素被保留;
			  2.函数的第一个参数 currentValue 也为必须，代表当前元素的值
	-例子：返回数组中大于5的数
let nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let res = nums.filter((num) => {
  return num > 5;
});
console.log(res);  // [6, 7, 8, 9, 10]

```

 6、字符串元素
将简单数据类型（字符串元素）包装成复杂数据类型，这样基本数据类型就有了属性和方法
（因为基本数据类型没有属性和方法，对象才有属性和方法）

6.1 字符串的不可变：指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间

6.2 查找字符位置：str.indexOf('要查找的字符',[起始的位置])
ps:起始位置可写可不写

```
var str = '测试是否可以测量位置';
console.log(str.indexOf('可'));  //输出可字符的位置4
console.log(str.indexOf('测',2));  //从索引号是2的位置开始往后查找
```

**重要案例：**
```
查找字符串'abcdefghohseodhwo'中所有o出现的位置以及次数
核心算法：先查找第一个o出现的位置，然后indexOf返回的结果不是-1就继续往后查找
因为indexOf只能查找到第一个，所以后面的查找一定是当前索引+1，再继续查找
var str = 'abcdefghohseodhwo';
var index = str.indexOf('o');
var num = 0;
while (index !== -1){
    console.log(index);
    num++;
    index = str.indexOf('o',index+1);
} 
console.log('o出现的次数是：' + num);
```

**6.3 根据位置返回字符**
（1）charAt(index)：返回指定位置的字符（index字符串的索引号）

```
var str = 'andy';
console.log(str.charAt(3)); //输出y
//遍历所有字符
for(i=0;i<str.length'i++){
    console.log(str.charAt(i));
}
```
（2）charCodeAt(index):返回指定位置处字符的ASCII码（index索引号）
目的：判断用户是否按下了那个键
```
console.log(str.charCodeAt(0)); //97（0的ASCII码是97）
```
（3）str[index]:获取指定位置处字符（HTML5,IE8+支持,H5新增的）
```
console.log(str[0]); //返回a andy的第一个
```

6.4 截取字符串
substr('截取的起始位置','截取几个字符');
```
var str = '这世界真的很';
console.log(str.substr(1,2)); //输出 世界
```

6.5 替换字符
（1）replace('被替换的字符', '替换为的字符') ps:指挥替换第一个字符
```
var str = 'everybody';
console.log(str.replace('e','a')); //输出averybody
```

字符转换为数组：ps(前面的join是把数组转换为字符串)
（2）split('分隔符')

```
var str = 'red,pink,blue';
console.log(str.split(,));  //输出[red,pink,blue]
var str = 'red&pink&blue';
console.log(str.split(&));  //输出[red,pink,blue]
```

## 十三、数据类型
简单类型又叫做基本数据类型或者**值类型**，复杂类型又叫做**引用类型**
（1）值类型：在存储时变量存储的是值本身，因此叫做值类型
例如：string,number,boolean,undefined,null
（2）引用类型：在存储时变量存储的仅仅是地址（引用），真正的对象实例存放在堆空间中
通过new关键字创建的对象（系统对象、自定义对象），如Object、Array、Date

堆栈空间分配区别：
（1）栈（操作系统）：由操作系统自动分配释放存放函数的参数值、局部变量的值等，里面直接开辟一个空间存放（存放的是变量名和变量值）**简单数据类型存放在栈里面**，占据空间小、大小固定，属于被频繁使用数据(值与值之间是独立存在的，修改一个变量不会影响其他变量)

（2）堆（操作系统）：存储复杂类型（对象），一般由程序员分配释放，若程序员不释放则由垃圾回收机制回收**复杂数据类型存放到堆里面**，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。--> 对象是保存到堆内存中的，每创建一个新的对象，就会在堆内存中开辟一个新的空间；而变量保存的是对象的内存地址(对象的引用)，实例对象创建的方法和属性是保存在堆中的，如果两个变量保存的是同一个对象引用(同一地址)，当一个通过另一个变量修改属性时，另一个也会受到影响



```
var a = 123;
var b = a;
a++; //输出a=124,b=123;因为当b=a赋值时,在栈空间直接存入b变量名和直接复制a的值123，两个是相互独立的;a++直接124覆盖123

比较区别：
（1）当比较两个基本数据类型的值时，就是在比较值
（2）当比较两个引用数据类型时，它是比较的对象的内存地址--> 如果两个对象是一模一样的，但是地址不同，他也会返回false

var a = 1
bar b = 1
console.log(a === b) //true

var obj3 = new Object()
var obj4 = new Object()
obj3.name = 'zjx'
obj4.name = 'zjx'
console.log(obj3 === obj4)  //false
console.log(obj3.name === obj4.name)  //true
```



ps:简单数据类型有一个例外
null 返回的是一个空对象 object 
因此如果有个变量以后打算存储为对象，但是暂时还没有想好放什么可以先给null





# 十四、补充

## 1、对象

1.1 对象的分类：

1. 内建对象：由ES标准中定义的对象，在任何的ES的实现中都可以使用
(例如 Math String Number Boolean Functrion Object...)
2. 宿主对象：由JS的运行环境提供的对象，目前来讲主要是指由浏览器提供的对象
(例如BOM,DOM; console.log()中的console和document.write()中的document都是宿主对象)
3. 自定义对象：由开发人员自己创建的对象


如果读取对象中没有的属性，不会报错而是会返回undefined



1.2 函数: **函数也是一个对象**

1. 函数中可以封装一些功能（代码），在需要时可以执行这些功能（代码）
2. 使用typeof检查一个函数对象时，会返回function

## 2、this

解析器在调用函数每次都会向函数内部传递进一个隐含的参数，这个隐含的参数就是this,this指向的是一个对象，这个对象称为函数执行的上下文对象

根据函数的调用方法不同，this会指向不同的对象：
	1. 以函数的形式调用时，this永远都是window(例如调用fun())
	2. 以方法的形式调用时，this都是调用方法的那个对象
```
var name = '全局对象'
function fun(){
	console.log(this.name)
}

var obj = {
	name = '孙悟空'
	sayName:fun //调用上面的fun函数
}

var obj2 = {
	name = '沙和尚'
	sayName:fun //调用上面的fun函数
}

fun(); //输出的是全局对象
obj.fun() //输出'孙悟空'
obj2.fun() //输出'沙和尚'
```



## 3、原型对象

我们创建的每一个函数，解析器都会向函数中添加一个属性prototype，这个属性对应着一个对象，这个对象就是原型对象

1. 如果函数作为普通函数调用prototype没有任何作用
2. 当函数以构造函数的形式调用时，它所创建的对象中都会有一个隐含的属性指向该构造函数的原型对象，可以通过```__proto__```来访问该属性

原型对象就相当于一个公共区域，所有同一个类的实例都可以访问到这个原型对象；因此可以将对象中的共有内容统一设置到原型对象中

当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果找到则直接使用，如果没有则会去原型对象中寻找

以后创建构造函数时，可以将这些对象共有的属性和方法统一添加到构造函数的原型对象中，这样就不用分别为每一个对象添加，也不会影响到全局作用域，就可以使每个对象都具有这些属性和方法

```
function MyClass(){

}

var mc = new MyClass();
var mc2 = new MyClass();

console.log(mc.__proto__ == MyClass.prototype)
console.log(mc2.__proto__ == MyClass.prototype)
```



**检查是否有某个属性**

1. 使用in检查对象中是否含有某个属性时，如果对象中没有但是原型中有，也会返回true
2. 使用对象的hasOwnProperty()来检查对象自身中是否含有该属性，使用该方法只有当对象自身中含有属性时，才会返回true



## 4、call&apply

call()和apply()
- 这两个方法都是**函数对象**的方法，需要通过函数对象来调用
- 当对函数调用call()和apply()都会调用函数执行
- 当调用call()和apply()可以将一个对象指定为第一个参数，此时这个对象将会成为函数执行时的this
- call()方法可以将实参在对象之后依次传递
- apply()方法需要将实参封装到一个数组中统一传递

```
function fun(){
	alert('fun函数')
}

fun.call()
fun.apply() //fun()是函数，没有括号的fun是函数对象
fun() //这三者是相等的

//这里是第三点的例子
function fun(){
	alert(this)
}
var obj = {}
fun.call(obj)  //object
fun.apply(obj)  //object
fun() //windows

//第四点例子
function fun(a,b){
	alert("a="+ a + "b" + b)
}
fun.call(obj,1,2)  //a=1,b=2
fun.apply(obj,[2,3]) //a=2,b=3
```

### 4.1 **this的情况总结**

1. 以函数形式调用时，this永远都是window
2. 以方法的形式调用时，this是调用方法的对象
3. 以构造函数的形式调用时，this是新创建的那个对象
4. 使用call和apply调用时，this是指定的那个对象

### 4.2 arguments

arguments是封装实参的对象，它是一个类数组对象
- 在调用函数时，传递的实参都会在arguments中保存

- arguments.length可以用来获取实参的长度

- 即使不定义形参，也可以通过arguments来使用实参(例如arguments[0]表示第一个实参)

- 还有callee()对象，返回的时整个函数及函数内容

```
function fun(){
	console.log(arguments instanceof Array) //false
	console.log(Array.isArray(arguments)) //false
	console.log(arguments.length)  //2
	console.log(arguments[0]) //'test'
}

fun('test','hello')
```

## 5、JSON

1. JS中的对象只有JS自己认识，其他语言都不认识
2. JSON就是一个特殊格式的字符串，这个字符串可以被任意的语言所识别，并且可以转换为任意语言中的对象，JSON在开发中主要用来进行数据的交互
2. JSON和JS对象的格式一样，只不过JSON字符串中的属性名必须加双引号，其他和JS语法一致

```
JSON分类：
	1. 对象{} (JSON对象)
	2. 数组[] (JSON数组)
	
JSON中允许的值：
	1.字符串
	2.数值
	3.布尔值
	4.null
	5.对象 //只允许普通对象，不允许函数对象
	6.数组
	
将JSON字符串转换为js对象：(在JS中提供了一个工具类JSON)可以使用JSON.parse()将JSON字符串转换为js对象
将js对象转换为JSON：使用JSON.stringify()
	-需要一个js对象作为参数，会返回一个JSON字符串
	
	
	
var json = '{name:'zjx',age:18}'
var o = JSON.parse(json)
console.log(o) //object
console.log(o.name) //'zjx'

var obj = {name:'zjx',age:18}
var str = JSON.stringify(obj)
console.log(typeof str) //string
