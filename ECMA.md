## 基础

- 严格模式：`"use strict";`，优先写在js代码第一行
- 导入：`<script>code;</script>`和`<script src="xxx.js"></script>`。如果你想先加载内容再渲染请把js文件放到*body*下面；先渲染再加载内容请放到*head*下面，依据情况而定
- 简写
	```JavaScript
	// 变量    对象     值       数组      属性      函数
	   val    obj     value    arr      attr      fun
	```
	

### 暴露

```JavaScript
//path.js
export const add(a,b)=>{                  //字面量函数、变量
    return a+b;
}
export default {                          //对象
    name:'wangluqi',
    drop:function (a,b){
        return a-b;
    }
}
export function fun() {                   //构造函数、函数
    this.name = "wang"
}
fun.prototype.sex = function () {
    return this.name+'19'
}
export {fun,val,obj}                      //一次性暴露多个



//简写(不能导入default)
import {add,fun} from './path.js';
//只导入default
import a from './path.js';
//升级版(导入path后执行函数)       指定标识符(必须)
import('./path.js').then(({add,default:_,fun})=>{
  console.log(add(1,2));
    console.log(_.drop(5,3))
    const a=new fun();                    //使用构造函数前必须实例化
    console.log(fun.name);
}).catch(err => console.log(err))
```


### 传址

- 基本数据类型(Number、String 、Boolean、Null和Undefined)是传值
	```JavaScript
	var a = 1;
	var b = a;
	a = 10;
	console.log(a, b);     //10 1
	```
	
- 引用数据类型(Object、Function、Array、Date、RegExp)等是传址
	```JavaScript
	var e = {name:"haha"};
	var f = e;
	e.name = "hehe"; 
	console.log(e, f);     //{name:"hehe"} {name:"hehe"}
	```
	

## 变量

### 定义

> 变量|常量名不能以数字开头，后跟字母、数字、下划线和$符，用驼峰命名法且区分大小写。变量的类型根据值的变化而变化；常量名优先用大写，常量的值不可修改且必须初始化


> const定义对象的属性值、数组的值可以修改。==定义数组或对象尽量用const==


```JavaScript
const ARR = [1, 2, 3];
ARR[1] = 10;
console.log(ARR);                //[1,10,3]
```


### 作用域

> var为全局作用域，在函数作用域内为块级；let|const为块作用域，每个`{}`都是一个块；不定义直接赋值的为隐式全局


```JavaScript
//块             全局              隐式全局           定义多个块
let a=1;        var b=2;          c=3;             let a,b,c=10;

{
    块级作用域         //只要{}包裹的都属于块级作用域，if(){}、function fun(){}等等
}

```


### 类型

> `typeof value`或`typeof(value)`可以测试数据类型。类型分为number、string、boolean、undefined、object、function，==array和null的类型为object==


> 基本数据类型已定义，但未初始化是undefined；引用数据类型未初始化是null

<img src="ESMA.assets/ScreenShot_20210902072156.webp" style="zoom: 50%;" />

- **Number**
	
	```JavaScript
	// 整型     浮点型    科学计数法     Not a Number    正无穷      二进制      八进制   十六进制
	   123     123.4    1.23e2        NaN            Infinity    0b10       010     0x10
	```
	
	|方法|描述|
	|---|---|
	|Number(value)|转换为数值型。非数字文本、undefined➞NaN；true➞1；false、null➞0|
	|parseInt|parseFloat(value)|转换为整型|浮点数|
	|isNaN(value)|判断是否是数字，不是数字为true。`NaN!==NaN`|
	|val.toFixed(n)|保留小数位数，会进行四舍五入|
	
- **String**
	> 模板字符串(反引号)
	
	```JavaScript
	let a=123;
	console.log(`我是${a}数字`);              //花括号内可以是任何值(如表达式)
	-------------------------------------------------
	a.innerHTML=`文本
	             换行
	             书写`
	```
	
	<img src="ESMA.assets/30-20-58-33-2018061413391620.png" style="zoom:80%;" />
	
	|方法|描述|
	|---|---|
	|String(value)|转换为字符串|
	|val.toString()|转换为字符串，null和undefined不可用|
	|str.length|字符个数|
	|str.substring\|slice(start,end)|截取字符串，后者也能截取数组|
	|str.substr(start,length)|截取字符串|
	|arr.join(符号)|将数组合并成字符串，并用符号连接|
	|str.toUpperCase\|toLowerCase()|字符串大小写|
	|str.trim()|去除两端空格|
	|str.charAt(index)|返回该索引号的值|
	|str.replace(oldStr,newStr)|替换字符，可用正则|
	|str.indexOf\|search(value,start)| 返回查找字符的索引号，没有则返回-1。后者可添加正则 |
	
	
	```JavaScript
	let str="Hello World1";
	console.log(str.search(/hello/i));            //0，属性
	
	let two=str.replace(/\d/g,"你好");
	console.log(two);                             //Hello World你好，元字符
	
	let three=str.replace(/(hello)/ig,"你好");
	console.log(three);                          //你好 World1，字符串
	```
	
- **Math**
	|方法|描述|
	|---|---|
	|Math.max\|min(value)|最大\|最小值|
	|Math.abs(value)|绝对值|
	|Math.PI|圆周率|
	|Math.pow(m,n)|m的n次方|
	|Math.sqrt(value)|平方根|
	|Math.ceil\|floor(value)|往大\|小取整|
	|Math.round(value)|四舍五入(.5往大取)|
	|Math.random(value)|随机数[0,1)|
	
	
	```JavaScript
	console.log(Math.floor(Math.random()*(m-n))+n;)      //取[n,m)
	console.log(Math.floor(Math.random()*(m-n+1))+n;)    //取[n,m]
	console.log(Math.ceil(Math.random()*(m-n))+n;)       //取(n,m]
	```
	
- **Date**
	> 月份是从0开始，所以要加1；星期日返回0
	
	```JavaScript
	var val=new Date();                 //定义日期对象
	
	Date.now()                          //总毫秒数，不需要实例化
	parseInt(总秒数/60/60/24);           //天数
	parseInt(总秒数/60/60%24);           //小时
	parseInt(总秒数/60%60);              //分
	parseInt(总秒数%60);                 //秒
	```
	
	|方法|描述|
	|---|---|
	|val.getTime()|总毫秒数|
	|val.getFullYear()|当前年份|
	|val.getMoth()|当前月份|
	|val.getDate()|日期|
	|val.getDay()|星期|
	|val.getHours()|小时|
	|val.getMinutes()|分钟|
	|val.getSeconds()|秒数|
	
- **Map|Set**
	
	> Set和Map主要的应用场景在于**数组去重**和**数据存储**
	
	```js
	var map = new Map();
	map.set("attr",value);
	let a = map.get('attr');
	map.delete('attr');
	map.has('attr');
	console.log(map.size);
	
	map.forEach(function(k){
	　　console.log("attr:",k)           //遍历map自带的值
	})
	
	let arr = [1,2,4,5];
	const newArr = arr.map(x => {
	    if (x == 4) {
	        return x * 2;
	    }
	    return x;
	});
	console.log(newArr);               //[1,2,8,5]，遍历array
	--------------------------------------------------------
	var set = new Set();
	set.add(value);
	set.delete(value);
	set.clear();                      //删除所有值
	set.has(value);
	console.log(set.size);
	
	let arr = [1,2,5,2,8];
	arr.forEach(x => set.add(x));
	console.log(set);                 //[1,2,5,8],array去重
	```
	
- **Other**

  |方法|描述|
  |---|---|
  |Boolean(value)|转换为布尔型。""、0、NaN、null、undefined➞false，其他为true|
  |sele.selectedIndex|获取下拉列表被选中option的索引号|


  ```JavaScript
  btn.onclick=function(){
    console.log(sele.selectedIndex);
  }
  ```


### 运算符

- 关系
	```JavaScript
	// 非      与      或       赋值      等于      绝对等于      不等于       绝对不等于(类型、值都不等于)
	   !      &&      ||       =         ==       ===         !=          !==
	```
	
- 算数
	```JavaScript
	+ - * / %
	
	a = "123" + 4        //"1234"
	b = "100" - 1        //99
	a += 1               //a=a+1
	a = 1++              //前置++先加1再运算；后置++先运算在加1
	```
	

## 数组

### 定义

```JavaScript
定义：let arr=[value，obj.attr，[fun,value2]...]
     let arr=new Array(value...)
     
获取：arr[i]=value和val=arr[i]
```


### 方法

|方法|描述|
|---|---|
|Array.isArray(value)|是否是数组|
|let val=arr1.concat(arr2)|合并数组|
|let val=[...arr1,...arr2]|合并数组，...也适用于形参\|实参，还可以==将伪数组、对象转换为真数组==|
|arr.includes(value)|判断该值是否存在于数组中|
|arr.splice(index,length,value)|添加\|删除\|替换数组元素。length为0是添加，不为0是删除的个数|
|arr.unshift\|push(value)|从开头\|末尾添加数组|
|arr.shift\| pop()| 从开头\|末尾删除数组                                         |
|arr.reverse(value)|翻转数组|
|str.split('符号')|将字符串分割成数组|

```JavaScript
let set=new Set(arr);            //数组去重
console.log(...set);
```


```JavaScript
arr[1]='hello'
arr.splice(2,0,'hi')              //a[2]='hi'
arr.splice(1,1)                   //删除数组a[1]
arr.splice(1,1,'world')           //arr[1]='world'
```


```JavaScript
let arr="111\r
       222\r"
console.log(arr.split('\r'));      //a['111','222']
```


### 遍历

- for...of语句用于遍历可迭代对象(array、string、map、set、arguments等)，不适用遍历js对象
	```JavaScript
	let arr = [1, 2, 3, 4];
	let obj = {
	    name: 'wangluqi',
	    age: 18,
	    sex: function () {
	        return 'man'
	    }
	};
	for (let i in obj) {            //遍历对象名、数组索引
	    console.log(i)
	}
	for (let i of arr) {            //遍历数组值
	    console.log(i)
	}
	```
	
- forEach语句用于遍历数组(伪数组除外)，但不能使用break、continue和return
	```JavaScript
	arr.forEach(function(value,index,arr){    //数组的值 索引号 数组
	    console.log(value,index);
	})
	```
	

### 其他

- 筛选
	```JavaScript
	arr=arr.filter(function(n){       //把数组的值遍历给n
	    return value;                 //当value为true时返回n，false会过滤
	})
	```
	
- 修改
	```JavaScript
	arr=[1,2,3]
	arr=arr.map(function(n){
	    return n*2;              
	})
	console.log(arr);                    //2,4,6，return返回值为表达式
	```
	
- 排序
	```JavaScript
	arr=[1,2,3];
	arr.sort(function(a,b){
	    return a-b;                      //a-b升序，b-a降序
	});
	```
	
- 查找
	```JavaScript
	let arr=[1,2,3];
	let one=arr.find|findIndex(function(value,index,arr){
	  return value>2;
	})
	console.log(one);                 //3，查找满足条件的数组|索引
	```
	
- 汇总
	```JavaScript
	arr=[1,2,3];
	arr=arr.reduce(function(prevalue,n){
	    return prevalue+n;               //0+1 1+2 3+3
	},0)                                 //0赋值给prevalue
	```
	
- 测试
	```JavaScript
	arr.some(function(value,index,arr){  //数组中的一个>10,则返回为true
	  return value>10;
	}
	
	arr.every(function(value,index,arr){ //数组中所有的值>10,则返回为true
	  return value>10;
	}
	```
	

---

## 函数

### 定义

1. 形参只能是变量或函数名，实参可以是常量值或变量。`函数名()===window.函数名()`
2. 形参多于实参，多余的形参为undefined；实参多于形参，多余的实参被忽略

```JavaScript
function fun(a,...b){                 //函数声明式
    console.log(b);                        
}
fun('liu',20,'女');
------------------------------------------------------------------
let fun=function(a,b){                //函数字面量式，只能先定义再调用
    return a+b;                       
};
let c = fun('wangluqi',18);
------------------------------------------------------------------
let fun=(a,b)=>{                      //箭头函数
    return a+b;
}                
let c = fun('wangluqi',18);

let fun = x => x;                      //只包含一个形参()可省略;只包含一条语句时{}、return可省略
let c = fun('hello')
------------------------------------------------------------------
(function (a, b) { console.log(a + b) })(10, 20);     //自调用(一次性)

(function(w){                                         //自调用并暴露a(一次性)
  let a=10;
  w.a=a;
})(window)
```


### 箭头函数

1. 箭头函数下this始终指向函数声明时作用域下的顶级对象
2. 不能作为实例化对象，也不能使用arguments变量
3. 普通函数适用于对象的方法、事件回调；箭头函数适用于定时器、数组
	```JavaScript
	btn.onclick = function () {
	    setTimeout(function () {
	        console.log(this)              //window
	    }, 1000)
	    setTimeout(() => {
	        console.log(this)               //btn
	    }, 2000)
	}
	```
	

### 函数返回

> **Arguments**


1. arguments以数组方式获取所有实参的值，但它是一个类型为对象的伪数组(包含索引和length属性)
2. `arugments.length`获取实参的个数；`函数|变量.length`获取形参的个数
3. arguments数组的值会随形参变化，形参也随arguments的值变化(看谁后赋值)
	```JavaScript
	function argu(a,b){
	  a=10;
	  console.log(arguments[0]);              //10
	  arguments[1]=12;
	  console.log(b);                         //12
	}
	argu(1,2);
	```
	

> **Return**


1. return可以中止函数并返回值，可以返回任何类型(常量、变量、函数或表达式)，且只能返回一个
2. `return +value;`返回值变为数值型
	```JavaScript
	function fun(){
	    code;
	    return value;             //终止函数并把返回值给调用它的变量
	}
	let a=fun();  
	```
	

### 提升

> 预解析：执行代码前都会提前进行解析和提升。如存在错误就会报错，下面的代码则不执行


> 提升分为变量提升和函数提升，声明式函数|var变量(==const、let没有变量提升==)会提升到代码最顶层。优先级为：函数声明式>函数字面量式>变量声明


```JavaScript
console.log(a, b);            // a为函数，b为undefined
function a() {};
let b = function() {};
--------------------------------------------------------
let a=1;
function b(){
    console.log(a);        //undefined，优先使用函数内的变量
    var a;
}
```

## 类

### This

1. 在使用类的属性|方法前必须加this，`window.方法()===方法()`、`window.fun()===fun()`
2. 对象中属性的this指向window；对象中方法的this指向该对象
3. 普通函数的this指向window；箭头函数的this指向定义时所属的对象，且无法通过指向来改变

```JavaScript
var name = "wangluqi";
var obj = {
    name: this.name,
    fun: function () {
        console.log(this);
    }
}
console.log(obj.name);                //wangluqi
obj.fun();                            //obj{}     
---------------------------------------------------------
function funs(){
  console.log(this);
}
funs();                                //window

let func=()=>{
  console.log(this);
}
btn.addEventListener("click",funs);    //btn元素，普通函数指向绑定事件的元素
btn.addEventListener("click",func);    //window
```


> **指向**


用于修改this的默认指向，该方法的实参优于调用时的实参。call能接收任何类型的实参，apply只能接收数组

```JavaScript
var one={
  name:"wang",
  age:18
}
var two={
  name:"liu",
  age:20,
  fun:function(a,b){
    return this.name+this.age+a+b;
  }
}

var c=two.fun.call(one,"+haha",12);       //one对象的属性和方法给two并优先与自身的属性和方法
var c=two.fun.apply(one,["haha",12]);
var c=two.fun.bin(one)("haha",12);
console.log(c);                          //wang18+haha12
```


### 定义

1. 类是对象的模板，本质是构造方法，并且没有提升
2. 类必须先定义再调用，类名首字母大写。类内的属性和方法相当于原型对象，属性、方法的this始终指向该类
3. 类内的constructor相当于构造函数，类内的属性和函数相当于原型对象。类内始终添加constructor函数，且无需调用自动执行
4. 类可以使用constructor、prototype、\__proto__等

```JavaScript
class 类名{
    constructor(形参1){ 
      this.name = value;
      this.sex = function(形参2){
      code;
    }
    this.方法(实参2);        //变量调用和构造函数内调用一样，但后者调用自身方法|其他函数会hosting(提升)优先执行
    this.函数(实参3);
  }
    age=value;
    dun(形参3){             //普通函数可直接使用constructor的属性|方法，普通函数要使用另一个普通函数的数据时要提前调用
      code;    
    }          
}                             

let 变量=new 类名(实参1);
console.log(变量.name);
变量.sex(实参2);
变量.fun(实参3);
```


### 继承

> 子类可继承所有父类的属性和方法。super优先调用父类函数，==且super必须在this之前==


```JavaScript
class Father{
    constructor(形参1){
      code;
    }
    sex(形参2){
      code;
  }
}
------------------------------------------------------------------
calss Child extends Father{
    constructor(形参3){
      super(实参1);                 //调用父类中的constructor
      super.sex(实参2);             //优先调用父类函数（函数名相同时默认调用子类）
      this.fun(实参4);              //调用子类函数
    }
    fun(形参4){
      code;
  }
}
let 变量=new Child(实参3);
变量.sex(实参2)|fun(实参4);
```

## 对象

### 定义

> 对象是由属性(常量、变量、数组)和方法(函数)组成


```JavaScript
const obj = {
    name:value,
    sex:function(){
    code;
    }
};
console.log(obj.name|obj['name']); 
obj.sex(); 
------------------------------------------------------------------
var obj = new Object();           //定义一个空对象
obj.name=value;                   //向空对象中添加属性和方法
obj.sex=function(){
    code;
}

```


### 对象访问器

> 以属性的方式访问对象中的函数，语法更加简洁`let 变量=对象名.方法名() === 对象名.函数名`


```JavaScript
let obj={                       //get获取对象属性
  name:value,
  get fun(){
    return this.name;
  }
}
console.log(obj.fun);


let obj={                       //set设置对象属性
  name:value,
  set fun(形参){
    this.name = 形参;
  }
}
obj.fun=实参;
console.log(obj.fun);

```


### 构造函数

> 构造函数类型为对象，构造函数名首字母大写，由属性和方法组成且this始终指向该构造函数


> 构造函数可以通过多个实例化对象来进行多次调用


```JavaScript
function 构造函数(形参1){ 
    this.name=value;
    this.sex=function(形参2){
    code;
  }
}

let 变量=new 构造函数(实参1);
console.log(变量.name);
变量.sex(实参2);
```


### 原型对象

> 原型对象是给普通函数|构造函数用prototype添加属性和方法（构造函数加属性，原型对象加方法）


> \__proto__对象指向对象(两个下划线)、prototype函数指向对象、constructor对象指向函数


```JavaScript
function fun(形参1) {code};
fun.prototype.name = value;               //把原型属性|方法添加给该函数
fun.prototype.sex = function (形参2) {
    code;
}
----------------------------------------
fun.prototype={
    name:value,
    sex:function (形参2){
        code;
    } 
}

let 变量 = new fun(实参1);                  //把该函数变成实例化对象
console.log(变量.name);   
变量.sex(实参2);

```


### 实例化对象

> 多个实例化对象的构造函数的方法不相等，属性相等；原型对象的方法和属性都相等


```JavaScript
function fun(){
  this.name="wang";
};
fun.prototype.age=18;
let a=new fun();
let b=new fun();

//更改原型对象的值
a.__proto__.age=20;        
console.log(b.name);                //20,不加proto只会修改当前实例化对象的属性|方法

//更改构造函数的值
a.__proto__.name="liu" || fun.prototype.name="liu"
console.log(b.__proto__.name);      //liu,这只是另辟蹊径地调用原型对象，真正要修改构造函数只能通过实例化对象的实参来更改
```

<img src="ESMA.assets/1335963-20190703171735078-283801655.webp" style="zoom: 67%;" />

<img src="ESMA.assets/201884161614326.webp" style="zoom: 80%;" />

### 方法

|方法|描述|
|---|---|
|let val={..obj1,...obj2}|合并对象|
|Object.assign(arr1,arr2)|合并对象arr2到arr1|
|Object.entries(obj)|对象名转换为数组|
|Object.values(obj)|对象的值转换为数组|
|Object.defineProperty(obj,"属性名",{value:"值"})|添加\|修改对象的属性、方法|
|更多对象方法请到json| |

```JavaScript
let obj = {
    name: "John",
    age: 30
};

let arr = Object.values(obj);
let ars = Object.entries(oibj); 
console.log(ars[1],arr[1]);           //age 30

```


```JavaScript
let one={
  name:"wang",
  age:18
}
let default={         //设置默认属性的类型，以下分别为可更改、可枚举、可重新配置
  writable : true 
  enumerable : true   
  configurable : true 
}

Object.defineProperty(one,"name",{value:"liu"});
Object.defineProperty(one,"age",{get: function(){return this.age+10}});
console.log(one.name,one.age);                 //liu 28

```

## JSON

1. json是一种存储数据格式，用于接收web服务端的数据，比xml更小更快。
2. json属性名必须用单引号|双引号(优先)包裹，属性值可以是number、String、boolean、array、object、undefined、null。==json内的数组和对象可以互相嵌套==
3. json有两种结构：json字符串与json对象
	```JavaScript
	var jsonStr = "{StudentID:'100',Name:'tmac',Hometown:'usa'}";
	
	var jsonObj = { StudentID: "100", Name: "tmac", Hometown: "usa" };
	
	```
	

### 定义

```JavaScript
var JsonObject = {              
    'names': [
        {
            'name': ['阿宣','法海'],
            'age': 18,
            'data': true
        },
        {
            'name': {'man':'小白','men':'小青'},
            'age': 25,
            'data': false
        }
    ],
    'rush':null
}
```


> **调用**


```JavaScript
数组：JsonObject.names[0].name[1]               //法海
对象：JsonObject.names[1].name.men              //小青
null：JsonObject.rush                          //null
```


> **删除**


```JavaScript
数组：delete JsonObject.names[0].name[1]          //删除法海
对象：delete JsonObject.names[1].name.man         //删除属性man
```


### 遍历

```JavaScript
//数组
for(k in JsonObject.names[0].name){
   console.log(JsonObject.names[0].name[k])     //循环数组的值
   console.log(k)                               //循环数组的索引号
}

//对象|json
for(k in JsonObject.names[1].name){
   console.log(k);                              //man  men，循环输出属性名
   console.log(JsonObject.names[1].name[k])     //小白 小青，循环输出属性值
}
```


### **方法**

```JavaScript
let JsonObject=JSON.parse('{ "names":"白蛇传" }')       //json字符串转换为json对象
               JSON.stringify(JsonObject)              //json对象转换为json字符串
```

## 其他

### RegExp

- 属性：i(忽略大小写)，g(全局匹配)，m(多行匹配)
- 元字符
	```JavaScript
	// [0-9A-z]      [^\w]      [0-9]      [^\d]      [\t\n\r\f\v]      [^\s]    [^\r\n]
	    \w            \W         \d        \D         \s                \S        \r 
	```
	
- 字符串
	```JavaScript
	// 包含      不包含        或                 多个        开头       结尾
	  [text]    [^text]      (text1|text2)    text{n,m}    ^text      text$
	```
	
	```JavaScript
	let a=/字符串|元字符/属性;         
	let a=new RegExp("字符串|元字符","属性");
	------------------------------------------------------------------
	let str="hello world";
	a.test(str)                         //str是否匹配a
	str.match(a)                        //返回匹配a正则的数组
	```
	

### 语句

- **分支**
	
	> **轻量条件用三元；范围条件用if；多分支且值确定用switch**
	
	```JavaScript
	a > b？表达式二:表达式三             //三元表达式
	
	if(条件){
	    code;
	}else if(条件){
	    code;
	}else{
	    code;
	}
	
	switch(条件){
	    case 常量：code;break；
	    default:code；
	}
	```
	
	简写
	
	```js
	if (a == 10 || 20) alert('yes');          //适用于等于|不等于，当if|else|for只包含一条语句时{}可省略
	else alert('no');
	
	for(let i in arr) === for(let i=0 ; i<arr.length ; i++)
	```
	
- **循环**

  > break跳出循环，也可==跳出代码块==；continue开始下一次循环

  > `label:`是一个标记语句，搭配break|continue使用

  > 条件确定用for，反之用while；for内的i不可再使用；while内的i可再使用

  ```JavaScript
  for(let i=0;条件;i++){               //定义i优先用let
      code;
  }
  
  while(i<10){code;i++;}
  
  do{code;i++}while(i<10)
  ```

  ```JavaScript
  one:{                      //块内可以访问外部的let变量，块内的let变量外部则不可访问
    function(){
      code;
      break one;
    }
  }
  
  label: for(……){
    code;
    break|continue label;
  }
  ```

- 异常
  > try测试代码错误，发生错误时执行catch语句，finally有无异常都会执行

  ```JavaScript
  try {
      code;
      throw value;                //当代码出现错误时抛出value传给err
  } catch(err){
      console.log(err.message);   //输出错误信息
  } finally {
      code;
  }
  ```


### 解构

- **变量**
	
	```JavaScript
	let [a,b,c]=[1,2,3];
	[a,b]=[b,a];
	console.log(a,b,c);               //2 1 3
	```
	
- **数组**
	```JavaScript
	let arr=[1,2,3];
	let [a,b,c]=arr;
	```
	
- **函数**
	
	```JavaScript
	function fun({ name, age, sex }) {
	    console.log(name);                //wang
	    sex();                            //men
	}
	
	fun({
	    name: "wang",
	    sex: function () {
	        console.log("men")
	    }
	})
	------------------------------------------------
	obj = {
	    name: 'wangluqi',
	    age: 18,
	}
	
	function fun({ name, age }) {
	    console.log(name, age);          //wangluqi 18
	}
	fun(obj);
	```
	
- **对象**
	```JavaScript
	let obj = {
	    name: 'wang',
	    age: 1,
	    sex: function () {
	        console.log('men');
	    }
	};
	
	let { name, age, sex } = obj;           //变量=对象名.属性名|对象名.方法名，要一一对应
	console.log(name, age);
	sex();
	```
	



