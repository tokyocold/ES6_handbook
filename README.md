# ES6_handbook

## 0.概念

ES6 表示 ECMAScript5.1后的JavaScript标准，涵盖了ES2015，ES2016，ES2017等等。

传统的JavaScript可以看作ECMAScript5.1。

## 1.基本

### 1.1 let&const

#### let

let类似于var，不同点：

> let声明的变量只在声明所在的块级作用域内有效

```js
var a=[];
for(let i=0;i<10;i++){
	a[i]=funcion(){
		console.log(i)
	}
}
a[6]();//6
#let声明的i只在当前循环有效。
for(let i=0;i<10;i++)
{

}
console.log(i);//error
#var声明的则全局有效
for(var i=0;i<10;i++)
{
    
}
console.log(i)//10
```



必须要先声明再使用

```
console.log(a);//error
let a=1;
#如果是
console.log(a);//undefined
var a=1;
```



#### const

const声明一个`只读`常量。一旦声明，不能改变。

> const和let只在声明所在的块级作用域内有效。

```js
{
    const MAX=1
    console.log(MAX) //1
}
console.log(MAX) //error
```



### 1.2 解构赋值

ES6允许变量从数组，对象中提取值。

```js
let [x,y='b'] = ['a']
console.log(x,y)//a b
```

从对象中提取，变量必须与属性同名。

```js
let {a,b,c} = {a:"A",b:"B"}
console.log(a)//A
console.log(b)//B
console.log(c)//undefined
```

如果变量名，属性名不同：

```js
let {length : l} = 'hello';
console.log(l)//5,hello是一个字符串对象，包括一个length属性，赋值给了l,length为undefined	
```

用途：

1. 交换变量值

   ```js
   let x=1;
   let y=2;
   [x,y]=[y,x]
   ```

2. 函数返回多个值

   ```js
   function a(){
   return [1,2,3];
   }
   let [a,b,c] = a();
   ```

3. 函数参数定义

   ```js
   function test({x,y,z}){
       return [x+y+z,x*y*z]
   }
   
   let [sum,product] = test({y:10,x:5,z:3})
   console.log(sum,product)
   ```

4. 提取JSON

   ```js
   let jsonData = {
     id: 42,
     status: "OK",
     data: [867, 5309]
   };
   
   let { id, status, data: number } = jsonData;
   ```

   

## 2.数据类型

### 2.1 字符串

#### 模板字符串

用反引号（`)标识，可以表示普通字符串，多行字符串，同时嵌入变量。

```js
let name="Ash",age="18";
`my name is ${name},i am ${age}`
```

也可以调用函数

```js
function fn() {
  return "Hello World";
}

`foo ${fn()} bar` //foo hello world bar	
```

#### includes(),startsWith(),endsWith()方法

判断一个字符串是否包含在另一个字符串中，第二个参数表示搜索位置

```js
"hello world!".startsWith("hello")//true
"hello world!".endsWith("hello",5)//true
"hello world!".includes("hello")//true
```

#### repeat(n)

将字符串重复n次

```js
"hello".repeat(n)//hellohello
```

#### padStart(),padEnd()

将字符串补全到指定长度

```js
'x'.padStart(5,"abc")//abcax
```

可以用作数字补全，或者字符串格式

```js
'1'.padStart(2,'0')//01
'12'.padStart(10,'YYYY-mm-dd')//YYYY-mm-12
```

#### trimStart(),trimEnd()

行为与trim()一致，前者消除头部空格，后者尾部。

#### replaceAll()

replace()只能替换第一个，如果要全部替补，必须使用正则表达式g修饰符，因此引入replaceAll()方法。

```js
'abcabc'.replace('a','x')//xbcabc
'abcabc'.replaceAll('a','x')//abcabc
```

#### at()

返回指定位置的字符

```js
const STR="a我特么"
STR.at(1)//我
```



### 2.2数值

#### Number.isInteger()
判断数值是否是整数

```js
Number.isInteger(25) // true
Number.isInteger(20.1) //false
```

#### Math对象

`Math.trunc()`：去除小数部分，返回整数

```js
Math.trunc(5.1) //5
Math.trunc(-3.1) // -3
```

`Math.sign()`:判断数字是正数、负数、零。

+ 正数：返回+1
+ 负数：返回-1
+ 0：返回0

### 2.3函数

#### 默认值

ES6支持函数参数默认值

```js
function log(x,y='world')
{
	console.log(x,y)
    let x=1;//error 默认参数，不能再声明
}
```

通常，定义了默认值的参数应该是尾参数。

```js
function f(a,b,c=5)
{
return [a,b,c];
}
f()//[undefined,undefined,5]
```

#### rest参数（...变量名）

获取多余的所有参数。

```js
function add(...values)
{
	let sum=0;
	for (var a of values)
	{
		sum+=a;
	}
	return a;
}
add(1,2,3,4,5) //15
```

#### 箭头函数

基本用法

```js
let f=a=>a+1; //1个参数
let f=(a,b)=>a+b; //多个参数
let f=()=>5;//不需要参数
```

可以简化回调函数

```js
[1,2,3].map(function(x){
return x*x
})
//简化
[1,2,3].map(x=>x*x)
```

**关于箭头函数的this注意点**

**笼统的说：箭头函数的this跟外层function的this一致，外层function的this指向谁，箭头函数的this就指向谁，如果外层不是function则指向window。**

1. 默认绑定外层的this，如果外层是对象，指向windows。

   ```js
   //不使用箭头函数
   const obj = {
       a: function() { console.log(this) } 
   }
   obj.a() //打出的是obj对象
   
   //使用箭头函数
   const obj = {
    a: () => {
    console.log(this)
    }
   }
   obj.a() //打出来的是window
   ```

2. 不能用call方法修改里面的this。永远指向调用该函数时对象的作用域。

   ```js
   //不使用箭头函数：
   let obj={
       "user":"A",
       "display":function (){
           console.log(this.user)  //this的指向会随着外层调用改变
       }
   }
   
   obj.display()  //this为obj
   obj.display.call({"user":"B"})  //this为{"user":"B"}
   
   //使用箭头函数，外层为对象，始终指向windows
   let obj1={
       "user":"A",
       "display":()=>{
           console.log(this.user)  //this的指向不会改变
       }
   }
   
   obj1.display()  //this为 windows
   obj1.display.call({"user":"B"})  //this为 windows
   
   //使用箭头函数，外层为function,则指向外层function中的this。
   let obj2={
       "user":"A",
       "display":function (){
           console.log(this)
           let func=function(){console.log(this.user)}  //这里不使用箭头函数
           return func 
       },
       "display2":function (){
           console.log(this)
           let func=()=>{console.log(this.user)}  //这里使用箭头函数
           return func
       }
   }
   
   let func2=obj2.display();
   func2(); //undefined，调用func2()时，func2并不是箭头函数，直接剥离出这个函数调用，作用域变成了windows
   func2.call({"user":"B"})  //B,这里 this随着传入的对象变为了{"user":"B"}
   
   let func3=obj2.display2();
   func3();//A func3实质上是箭头函数，其外层的function 也就是上一行，已经“永远”指向的 obj2
   func3.call({"user":"B"})  //A  func3的this，已经永远指向obj2
   
   //!!! 接下来 来点好玩的
   let func4=obj2.display2.call({"user":"joker"})  //此时，display2中的this对象已经变为了{"user":"joker"}
   func4()	//joker 箭头函数中this的指向已经为外层this的{"user":"joker"}
   func4.call({"user":"B"}) //joker 显而易见，call绑定不会生效
   
   ```

   

   



### 2.4 数组

#### 扩展运算符 ...

类似rest参数的逆运算，将数组转为逗号分隔的参数序列。

```js
console.log(...[1,2,4])// 1 2 3
```

#### Array.from()

将对象转换为真正的数组，同时拥有第二个参数，对每个元素进行处理，处理后的值放入返回的数组。

```js
Array.from([1, , 2, , 3], (n) => n*n || 0)//[1,0,4,0,9]
```

#### Array.of()

将一组值，转换为数组。

```js
Array.of(1,2,3)//[1,2,3]
```

#### copyWithin()

将指定位置的数组成员复制到其他位置

```js
[1,2,3].copyWithin(0,1)//[2,2,3]
```

#### find()/findIndex()

找出第一个符合条件的数组成员，参数是一个回调函数。区别`find()`返回数组成员，`findIndex()`返回数组位置

```js
[1,2,3,4].find(n=>n>2) //3
[1,2,3,4].findIndex(n=>n>2) //2
```

#### fill()

给定值填充数组，已有的元素会被抹去。

```js
[1,2,3].fill(0) \\[0,0.0]
new Array(3).fill(0) \\[0,0,0]
```

#### includes()

返回布尔值，表示数组是否包含给定值，取代`IndexOf()`,indexOf()找不到是返回-1

```js
[1,2,3].includes(4) // false
[1,2,3].indexOf(4) //-1
```

#### flat(),flatMap()

flat(n) 将嵌套数组拉平，参数为拉平层数，flatMap()对每个成员执行函数，然后执行flat()方法，只能展开一层。

```js
[1,[2,[3]]].flat() //[1,2[3]]
[1,[2,[3]]].flat(2) // [1,2,3]
[1,2,3].flatMap(x=>[x,x*2])//[1,2,2,4,3,6]
```

#### at()

返回对应位置的数组成员，支持负索引。

```js
const a=[1,2,3];
a.at(1) // 2
a.at(-1) //3
```

### 2.5 对象

#### 属性简洁表示法

允许直接在大括号里直接写入变量和函数，作为对象的属性和方法

```js
const name="Ash";
const user={name}
user //{name:"Ash"}

const o={
    // 等同于hello: function ()...
    hello(){return "hello"}
}

```

#### 扩展运算符

(...)用于取出参数对象所有可遍历的属性，拷贝到当前对象中。

```js
let z={a:3,b:4}
let n={...z}
n //{a:3,b:4}
```

#### Object.assign()

用于对象的合并，将源对象所有属性复制到目标对象。

```js
const A={a:1}
const B={b:1,a:2}
Object.assign(A,B)  //同名属性，后面的值会覆盖
A//{a:1,b:1}
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]  数组会当作对象来处理
```

用途

```js
//为对象添加属性
class User{
    constructor(name,age){
        Object.assign(this,{name,age})
    }
}

let u1 = new User("Ash",18)
console.log(u1)  //{name:"Ash",age:18}

//克隆对象
let u2=Object.assign({},u1)
console.log(u2) //{name:"Ash",age:18}
```

#### Object.keys()

返回数组，成员为对象自身所有可遍历属性的键名

```js
var obj={name:"Ash",age:20}
Object.keys(obj) // ["name","age"]
```

#### Object.values()

返回数组，成员为对象自身所有可遍历属性的键值

```js
var obj={name:"Ash",age:20}
Object.values(obj) // ["Ash",20]
```

#### Object.entries()

返回数组，成员为对象自身所有可遍历属性的`键值对`数组

```js
var obj={name:"Ash",age:20}
Object.entries(obj) // [["name","Ash"],["age",20]]
```

#### Object.fromEntries()

Object.entries()的逆操作。

```js
Object.fromEntries([
["name","age"],
["Ash",20]
])
```

### 2.6 运算符

#### 指数运算符

```js
2**3 // 2*2*2
let a=2
a **= 3 // a=a*a*a
```

#### 链判断运算符

获取对象内部的属性，需要逐级判断是否存在。

```js
// 错误的写法
const  firstName = message.body.user.firstName || 'default';

// 正确的写法
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
//采用链判断运算符，可以简化
const firstName = message?.body?.user?.firstName || 'default';

```

#### Null判断运算符

如果某个值为null或者undefined,如果要指定默认值，可以使用`??`Null判断运算符。

```js
const u=User.name.firstname??"Ash"

let b
let a=b??"hello" //hello
```

#### 逻辑赋值运算符

```js
// 或赋值运算符
x ||= y
// 等同于
x || (x = y)

// 与赋值运算符
x &&= y
// 等同于
x && (x = y)

// Null 赋值运算符
x ??= y
// 等同于
x ?? (x = y)

//作用，可以用来为变量设置默认值
//旧写法
user.name = user.name || "Ash"
//新写法
user.name||="Ash"
```

### 2.7 Symbol

简单来说，Symbol是一个独一无二的标识，可以在扩展对象属性时防止属性名出现冲突。

```
let obj={
    "a":"abc"
}
let a=Symbol("a"); 
let b=Symbol("a");
a==b // false 参数只是一个描述，即便参数一样，两个变量也不相等
obj[a] = "abc";
obj[b] = 'abc';
console.log(obj) //{ a: 'abc', [Symbol(a)]: 'abc', [Symbol(a)]: 'abc' }
```



## 3.数据结构

### 3.1 Set

Set类似于数组，但是成员是唯一的，没有重复值。

Set函数可以接受数组（具有iterable接口的其他数据结构）作为参数。

```
const set = new Set([1,2,3,4,4]) 
...set //1,2,3,4
const set = new Set(document.querySelectorAll("p"))

//去除数组重复成员
[...new Set([array])]
//去除字符串重复字符
[...new Set("abcab")].join("") //abc
```

操作方法：

```js
set.add(1) //添加某个值
set.delete(2) // 删除
set.has(1) // 是否存在
set.clear() //清除
```

遍历方法

```
//set.keys()和set.values()行为是一样的
for(let item of set.keys())
{
	console.log(item) 
}

```

### 3.2 Map

类似于Object类型，区别是Object的键是字符串，而Set的键不限于字符串，对象也可以作为键。

```
const m=new Map([
['name','Ash'],['age',20] //可以使用数组作为参数
]);
m.set({},"content")  //可以使用set设置成员

//属性和方法
m.size //3
m.set("job","worker")
m.get("name")
m.has("name")
m.delete(undefined)
m.clear()
```

## 4.Proxy

proxy可以看作对目标对象的操作访问之前做一次“拦截”。这个对象可以是对象，方法。

```js
let person={
    "name":"Ash",
    "age":20
};

person = new Proxy(person,{
    get(target, p, receiver) {
        if (p in target) {
            console.log('get property:'+p)
            return target[p];
        } else {
            throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
        }
    },
    set:function (target, p, value, receiver){
        console.log('set property:'+p)
        target[p]=value
    }
})

person.name  // get property:name
person.age=15  // set property:age
console.log(person)  //{name:"Ash",age:15}
person.job // error
```

proxy对象虽然是通过`new Proxy`创建，但其本质仍然对是person对象的实例化，操作。

#### 对方法设置拦截

```js
let f = (x,y)=>x+y
let proxy = new Proxy(f,{
    apply(target, thisArg, argArray) {
        let sum=0;
        for (let i of argArray)
            sum+=i
        sum = sum>100?0:sum
        return sum
    }
})  //对f进行了拦截
console.log(proxy(1,99)) //100
console.log(proxy(1,100)) //0
```

#### 支持的拦截操作：

```js
get(target, p, receiver) {} // target:操作的目标对象，p为属性， receiver为操作的对象，即proxy实例自身

set(target, p, value, receiver) {} //同上

apply(target, thisArg, argArray) {}//apply拦截对函数的调用

has(target,p){}//拦截判断对象是否有某个属性，例如 in 操作 property in proxy

construct (target, args, newTarget) {}//拦截 new 命令

defineProperty (target, key, descriptor) {  //拦截设置属性操作
    return false;  //proxy.name='a'; //不会再生效
}

....
```

## 5.Reflect

作用：

- 将对象内部的属性方法通过Reflect暴露给外部
- 将Object的操作变成函数行为，例如 `name in obj` 和 `delete obj[name]`，可以改写为 `Reflect.has(obj,name)` `Reflect.deleteProperty(obj,name)`

```js
let f = (x,y)=>x+y
let proxy = new Proxy(f,{
    apply(target, thisArg, argArray) {
       // let sum=0;
       // for (let i of argArray)
       //     sum+=i
        let sum=Reflect.apply(...arguments)  //直接使用Reflect.apply()调用原始的方法
        sum = sum>100?0:sum
        return sum
    }
})  //对f进行了拦截
console.log(proxy(1,99)) //100
console.log(proxy(1,100)) //0
```

`Reflect`的方法大部分与`Object`对象的同名方法作用相同，参数与`Proxy`方法是一一对应的。

```js
Reflect.apply(target, thisArg, args) 
Reflect.construct(target, args) //等同于 new target()写法
Reflect.get(target, name, receiver) //返回target的name属性
Reflect.set(target, name, value, receiver) //设置target的name属性为value
Reflect.defineProperty(target, name, desc)
Reflect.deleteProperty(target, name) //对应delete obj[name]
Reflect.has(target, name) // 对应name in obj
Reflect.ownKeys(target)
Reflect.isExtensible(target)
Reflect.preventExtensions(target)
Reflect.getOwnPropertyDescriptor(target, name)
Reflect.getPrototypeOf(target)
Reflect.setPrototypeOf(target, prototype)
```

## 6.Promise——异步编程实现之一

### 6.1 Promise含义

核心：解决回调层层嵌套的问题。

特点：

1. 三类状态：`pending` `fulfiled` `rejected` ，只有异步操作结果可以影响状态
2. 一旦状态改变，就不会再变。只有两种改变可能：`peding`改变为`fulfiled` 和  `pending` 改变为 `rejected` 
2. Promise 一旦新建就会立即执行，无法中途取消。

### 6.2 基本用法：

Promise构造函数接受一个函数作为参数，该函数两个参数分别是`resolve`和`reject`，这是两个js内部提供的函数。

```js
const p = new Promise((resolve, reject) => {
    console.log("promise") //promise在创建后就会立即执行
    // resolve("执行")  // 将状态改变为fulfilled
    reject("rejected") //将状态改变为rejected
    console.log("finish")
}).then(value => {
    console.log('正确',value)  //fulfilled时调用 
},value=>{
    console.log('错误',value)  //rejected时调用
})

//输出结果：
//promise
//finish
//错误 rejected
```

一般来说，调用resolve或reject后，Promise就应该完成了。后续的操作应该放到then方法里，不应该写在resolve或者reject后面。

### 6.3 相关方法：

#### Promise.prototype.then()

为Promise实例添加状态改变时的回调函数。第一个参数是resolve时的回调函数，第二个参数是reject()的回到函数。

`then`返回的是一个新的Promise实例。这个Promise的状态默认也是fulfilled。

```js
const p = new Promise((resolve, reject) => {
    console.log("promise")
    // resolve("执行")
    reject("rejected")
}).then(value => {
    console.log('正确',value)
    return "正确"
},value=>{
    console.log('错误',value)
    return "错误"   //此处返回的Promise是fulfilled状态
}).then(value => {
    console.log('正确2',value)  //触发这个回调函数
}).catch((value)=>{
    console.log('错误2',value)
})
//输出结果：
//promise
//错误 rejected
//正确2 错误
```

---

#### Promise.prototype.catch()

`catch()`就是`then`函数的第二个参数别名。指定发生错误时的回调函数。或者捕获reject()的错误

```js
const p = new Promise((resolve, reject) => {
    console.log("promise")
    // resolve("执行")
    reject("rejected")
}).then(value => {
    console.log('正确',value)
    return "正确"
},value=>{
    console.log('错误',value)
    //可以通过返回一个rejected的Promise
    return new Promise((resolve, reject) => {
        reject("rejected")
    })
    //也可以触发一个错误或者抛出错误
    a(); 
}).then(value => {
    console.log('正确2',value)
}).catch((value)=>{
    console.log('错误2',value)
})
//输出结果：
//promise
//错误 rejected
//错误2 rejected  or 错误2 ReferenceError: a is not defined

```

---

#### Promise.prototype,finally()

`finally`指定不管Promise对象状态为何，都会最终执行的操作。在执行完`then`或者`catch`指定的回调函数后，都会执行`finally`指定的回调函数。

---

#### Promise.all()

`all()`将多个Promise实例，包装成一个新的Promise实例。

```js
const p = Promise.all([p1,p2,p3])
```

1. p1,p2,p3状态都变为fulfilled，p的状态才会变成fulfilled。触发p的then。
2. p1,p2,p3只要有一个被rejected，p的状态就会是rejected。触发p的catch。

```js
let p1 = new Promise((resolve, reject) => {
    setTimeout(resolve,1000)
})
let p2 = new Promise((resolve, reject) => {
    setTimeout(resolve,3000)
})
let p3 = new Promise((resolve, reject) => {
    setTimeout(resolve,2000)
})

let p = Promise.all([p1,p2,p3]).then(value => {
    console.log('all finished');
}).catch(value=>{
    console.log('some one error');
})
```

---

#### Promise.race()

类似与all()，将多个Promise包装成一个新的promise。

不同点，只要有一个promise状态改变，则p的状态跟着其改变。

```js
let p1 = new Promise((resolve, reject) => {
    setTimeout(resolve,1000)  //率先改变状态，触发回调
})
let p2 = new Promise((resolve, reject) => {
    setTimeout(reject,3000)
})
let p3 = new Promise((resolve, reject) => {
    setTimeout(resolve,2000)
})

let p = Promise.race([p1,p2,p3]).then(value => {
    console.log('someone has finished');
}).catch(value=>{
    console.log('someone has errored');
})
```

---

### 6.4 应用

#### ajax连续回调

```js
/*
* 假设业务场景，先获取用户信息，根据返回的用户信息，在获取用户的订单信息，根据返回的订单信息，再获取商品信息。
* 传统的方式，则需要ajax层层回调来触发
* 采用Promise方式，则可以避免层层回调	
*/

let userId=1
let p=new Promise((resolve, reject) => {
    $.ajax({
        url:"/ES6/user.json",
        data: {userId},
        dataType:"json",
        success:function (j){
            let userInfo=j[userId]
            resolve(userInfo)   // 原本需要嵌套的ajax调用，提取到外部then方法中，通过resolve来触发下一步。参数也通过resolve来传递
        }
    })
}).then((userInfo)=>{		//then中函数的参数来接受上一步参数
    console.log(userInfo)
    return new Promise((resolve, reject) => {
        $.ajax({
            url:"/ES6/order.json",
            data:{'order':userInfo['order']},
            dataType:"json",
            success:function (j){
                let orderInfo = j[userInfo['order']]
                resolve(orderInfo)
            }
        })
    })
}).then(orderInfo => {
    console.log(orderInfo)
    return new Promise((resolve, reject) => {
        $.ajax({
            url:"/ES6/goods.json",
            data:{'good':orderInfo['good']},
            dataType:"json",
            success:function (j){
                let goodInfo = j[orderInfo['good']]
                resolve(goodInfo)
            }
        })
    })
}).then(value => {
    console.log(value)
})
//输出结果：
//{name: 'Ash', order: 1}
//{date: '2020-1-1', good: 1}
//{date: 'bicycle', price: 200}
```

传统的ajax嵌套调用，一般是ajax执行完成，在success:中再次添加新的ajax。这样会一层一层嵌套下去。

可以通过promise，将success内的ajax调用抽出到then回调中，改为resolve(j)触发并传参。

#### 加载图片

```js
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    const image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```

----

## 7.迭代器

### 7.1 概念

实现：

1. 创建一个类指针对象，指向需要遍历的数据结构的起始位置；
2. 第一次调用next()方法，指向数据结构第一个成员；
3. 每次调用next()，指针向后移动一次，直到最后一个成员；
4. 每次调用next()方法一个{value,done}对象。

模拟：

```js
let obj={
    "user":[
        {name:"Ash"},{name: "lily"},{name: "George"}
    ],
    "index":0,
    "next":function(){
        return this.index<this.user.length?this.user[this.index++]:{"done":true}
    }

}
console.log(obj.next());  \\{name: 'Ash'}
console.log(obj.next());  \\{name: "lily"}
console.log(obj.next());  \\{name: "George"}
console.log(obj.next());  \\{done: true}
```



### 7.2 Iterator默认接口：Symbol.iterator

Array,Map,Set等数据结构，之所以可以使用`for of`方法迭代，是因为其内部对象 拥有`Symbol.iterator`属性。

对象默认没有这个属性，因而不能`for of`遍历，如果要使得对象可以迭代，或者说，对象可以使用`for of`方法，则可以添加一个`Symbol.iterator`属性，则对象可以使用`for of`来遍历。

只要

1. 这个属性返回的对象拥有next()方法
2. next方法返回一个{value,done}对象

满足这两点，则这个对象就是可迭代的对象。

```js
let obj2={
    "user":[
        {name:"Ash"},{name: "lily"},{name: "George"}
    ],
    index: 0,
    [Symbol.iterator](){ //注意 Symbol.iterator是一个表达式，因此要使用[]来添加属性。
        let _this=this;
        return {
            next(){
                return _this.index<_this.user.length?{value:_this.user[_this.index++],"done":false}:{"done":true}
            }
        }
    }
}
for (var o of obj2)
{
    console.log(o)  
}
//结果：
//{name: 'Ash'}
//{name: 'lily'}
//{name: 'George'}
```



### 7.3 应用

迭代器在各种语言中都有实现，除了以上的`for of`作用外：

1. 通过迭代器，在读取超大文件，或者遍历超大文件夹时，迭代器不同于普通的for 需要将迭代对象提前整体存入内存中。理论上内存只要够迭代一次，就可以遍历无限的内容
2. 通过对象实现迭代器，可以指定外部在遍历对象时得到的结果。类似上个例子，直接使用`for in` 遍历obj.user 虽然可以实现结果，但是这种模式不符合面向对象的原则，这种获取方式是一种侵入式的获取方式。



---

## 8. 生成器——异步编程实现之二

### 8.1 基本形式

1. function 关键字和函数名之间有一个*
2. 函数内部使用yield表达式，定义不同的状态。

```js
function * hello()
{
    console.log("hello world")
    yield "hello"
    yield "world"
}
let h = hello();  //不会执行
h.next(); // 执行next后，才会执行函数 hello world
		  //{value: 'hello', done: false}
h.next(); //{value: 'world', done: false}
h.next(); //{value: undefined, done: true}
```

调用Generator函数，**函数不会立即执行,**会返回一个`遍历器`对象，代表Generator函数的内部指针。以后，每次调用遍历器对象的`next`方法，会返回一个{value,done}的对象。

value是当前内部状态值，是`yeild`后面的表达式的值；

done表示是否遍历结束。

### 8.2 与Iterator关系

任何一个对象，只要部署了`Symbol.iterator`方法，等于该对象拥有遍历器生成函数，该对象就可以遍历。

因此可以把Generator赋值给对象的Symbol.iterator属性，从而使得对象可以遍历

> 7.2的示例，采用Generator实现：

```js
let obj2={
    "user":[
        {name:"Ash"},{name: "lily"},{name: "George"}
    ],
    index: 0,
    [Symbol.iterator]:function *(){
        for (let i=0;i<this.user.length;i++)
            yield this.user[i]
    }
}
for (var o of obj2)
{
    console.log(o)
}
```

### 8.3 next()的参数

`yield`本身没有返回值。`next`可以带一个参数，作为上一个`yield`表达式的返回值。

注意，由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的。只有从第二次`next`方法开始，参数才是有效的。

> 6.4的例子，采用Generator实现

```js
let getUser=function (userId){
    $.ajax({
        url:"/ES6/user.json",
        data: {userId},
        dataType:"json",
        success:function (j){
            let userInfo=j[userId]
            g.next(userInfo);  //将userinfo作为next参数传递进去，则可以理解为：本次yield的返回值就是userInfo。在外部可以接收到，并作为参数传入下个yield中
        }
    })
}
let getOrder = function (userInfo){
    $.ajax({
        url:"/ES6/order.json",
        data:{'order':userInfo['order']},
        dataType:"json",
        success:function (j){
            let orderInfo = j[userInfo['order']]
            g.next(orderInfo)
        }
    })
}
let getGood = function (orderInfo){
    $.ajax({
        url:"/ES6/goods.json",
        data:{'good':orderInfo['good']},
        dataType:"json",
        success:function (j){
            let goodInfo = j[orderInfo['good']]
            g.next(goodInfo)
        }
    })
}

let get = function * (userId){
    let userInfo = yield getUser(userId); //userInfo通过第一个next()的参数返回。下面也是类似
    let orderInfo = yield getOrder(userInfo);
    let goodInfo = yield getGood(orderInfo);
    console.log(goodInfo)
}

let g=get(2);
g.next();
```

---

## 9.async函数——异步编程实现之三

### 9.1 基本形式

`async`和`await`两种语法结合可以让异步代码像同步代码一样。

本质是Promise的语法糖。

```js
const fn=async function () {
    try{
        var res1 = await new Promise((resolve, reject)=>{
            reject("步骤1错误")
        })
        }catch(e)
     {
        console.warn(e)
     }
    var res2 = await new Promise((resolve, reject)=>{
        resolve("步骤2")
    })
    return "执行完成"
}
let a=fn()
a.then((res)=>{
    console.log(res)  //执行完成
})
```

#### async函数

1. `async`返回值是Promise对象。可以用`then`指定下一步操作；
2. `async`函数自带执行器。不需要像Generator函数一样，调用`next`方法；
3. `async`函数return的返回值，会被外部then方法接受到。

#### await函数

1. await必须写在async函数中；
2. await后面一般是一个`Promise`对象；
3. await的Promise对象变为`reject`状态，那么整个async函数就中断执行，如果希望前一个异步失败也不影响后面的异步操作，可以把await放在`try`..`catch`中。

### 9.2 实例

> 6.4的例子，采用async函数实现

```js
let getUser=function (userId){
    return new Promise((resolve,reject)=>{
        $.ajax({
            url:"/user.json",
            data: {userId},
            dataType:"json",
            success:function (j){
                let userInfo=j[userId]
                resolve(userInfo);
            }
        })	
    })

}
let getOrder = function (userInfo){
    return new Promise((resolve,reject)=>{
        $.ajax({
            url:"/order.json",
            data:{'order':userInfo['order']},
            dataType:"json",
            success:function (j){
                let orderInfo = j[userInfo['order']]
                resolve(orderInfo)
            }
        })
    })
}
let getGood = function (orderInfo){
    return new Promise((resolve,reject)=>{

        $.ajax({
            url:"/good.json",
            data:{'good':orderInfo['good']},
            dataType:"json",
            success:function (j){
                let goodInfo = j[orderInfo['good']]
                resolve(goodInfo)
            }
        })
    })
}

let func = async function getGoodByUid(userId) {
    try{
        var userInfo = await getUser(userId);	//await返回的都是Promise
    }catch(e){
        console.warn(e)
    }
    try{
        var orderInfo = await getOrder(userInfo); //await返回的都是Promise
    }catch(e){
        console.warn(e)
    }
    try{
        var goodInfo = await getGood(orderInfo); //await返回的都是Promise
    }catch(e){
        console.warn(e)
    }
    console.log(goodInfo)
}

func(1);
```

如果 await函数的异步操作，不存在继发关系，可以让其同时触发，上面的是一个继发的例子。

并发执行：

```js
let func = async function getGoodByUid(userId) {
	let[userInfo,orderInfo,goodInfo] = await Promise.all([getUser(1),getOrder(1),getGood(1)])
}
func(1); //采用这种形式，三个请求会同时发送。
```

---

## 10. Class

### 10.1 基本形式

JavaScript/ES5中，实现对象是使用`function`

```js
function phone(brand,price) {
    this.brand=brand
    this.price=price
}
phone.prototype.print=function(){  //添加对象使用原型链
    console.log(`phone:${this.brand} price:${this.price}`)
}
let huawei = new phone('Huawei',2999)
huawei.print();
```

ES6引入了`class`概念。通过class来定义类，class可以看作只是一个语法糖，目的是使得对象表示更清晰。

```js
class mobile{	
    constructor(brand,price){
        this.brand=brand
        this.price=price
    }

    print(){   //使用方法名()来定以方法
        console.log(`phone:${this.brand} price:${this.price}`)
    }

    //print2:function (argument) {  //类中的方法 不能使用这种形式
    // body...
    //}
}

let iphone = new mobile('apple','2000')
iphone.print()
```

#### getter 和 setter

类内部，可以使用`get`和`set`关键字，对某个属性设置存取函数，拦截其存取行为。

```js
class phone{
    get color(){
        console.log("get color")
        //return this.color  //Maximum call stack size exceeded
        return this.abc
    }

    set color(value)
    {
        console.log("set color",value)
        //this.color=value //Maximum call stack size exceeded
        this.abc=value
    }	
}
let iphone = new phone()
iphone.color="red";
```

以上写法会报错，陷入栈溢出。

**原因分析：**

在设置get color()后，phone对象就已经拥有了color属性。可以通过实例化Phone后，来设置color属性值。

这个时候，如果color()中重复在对this.color进行存取，将会再次触发setter/getter函数，陷入死循环。因此，这里可以使用其他属性名等操作。

setter/getter本质上可以看作是在对属性操作后自动触发的hook。其目的意义并不是对需要操作的属性的值进行直接操作。

---

#### 静态属性 和 静态方法

静态属性，静态方法相当于直接定义在类的原型上。不会被实例继承，而是直接通过类来调用。

```js
class mobile{	
	static category = "3C"
	
	 constructor(brand,price){
	 	this.brand=brand
	 	this.price=price
	 }

	 print(){
	 	console.log(`phone:${this.brand} price:${this.price}`)
	 }
    
    static print(){ 
        console.log(`category:${mobile.category}`)
    }
}
mobile.print()
```

可以看出，静态属性/方法 可以非静态重名。

---

#### 实例属性新写法

```js
class mobile{	
    manufacturer = ["China","Vietnam"]
    netType = "5G"
}
```

### 10.2 继承

Class可以通过`extends`关键字来实现继承。

```js
class phone{	

    constructor(brand,price){
        this.brand=brand
        this.price=price
    }

    takePhoto(){}
}

class smartPhone extends phone{
    constructor(brand,price,network)
    {
        super(brand,price) //调用父类构造函数
        this.network=network;
    }

    takePhoto(){
        super.takePhoto() //super代表父类对象
        console.log("i can take photo")
    }
}
let iphone = new smartPhone('apple','2000','4g')
```

---

## 11 Module

### 11.1 概述

**模块化的好处：**

1. 防止命名冲突
2. 代码复用
3. 高维护性

ES6之前的模块加载方案主要有`CommonJS`和`AMD`两种。

- CommonJS => NodeJs、Browserify  主要用于服务器
- AMD=>requireJS 主要用于浏览器

ES6的模块实现了编译时加载/静态加载，即ES6可以在编译时完成模块加载。而不是CommonJS和AMD的运行时加载。

---

### 11.2 语法

`export`和`import`

- export规定模块的对外接口
- import输入其他模块提供的功能

> export

```js
//方式一
export let brand='huawei'
export let price='2999'
export function call() {
	console.log("make a call!")
}

//方式二
let brand='huawei'
let price='2999'
function call() {
    console.log("make a call!")
}
export { brand,price,call}

```

`export`规定的是对外的接口，必须与模块内部的变量建立一一对应关系。与其对应的值是动态绑定关系，通过这个接口，可以实施取到内部的值。

```js
//模块内部
let brand='huawei'
let price='2999'
function call() {
    console.log("make a call!")
}
export { brand,price,call}
setTimeout(() => brand = 'iphone', 500); //500ms后动态改变
//模块外部
import * as phone from './phone.js'
console.log(phone.brand) //huawei
setTimeout(()=>console.log(phone.brand),500) //iphone  
```

`export`不能出现在块级作用域中。



> import

```js
//方式一 解构赋值形式
import {brand,call} from './phone.js'
//方式二 整体引入
import * as phone from './phone.js'
console.log(phone.brand)
phone.call()
//方式三 简便形式，只针对默认输出export default
import phone from './phone.js'
```



> export default 默认输出

默认输出可以为模块指定一个暴露给外界的接口，外界不需要知晓模块内部的属性方法，直接使用。

**一个模块只可能有一个默认输出**

```js
//phone.js
let brand='huawei'
let price='2999'
function call() {
    console.log("make a call!")
}
export default function(){
    console.log("i am a phone")
}
export { brand,price,call} //只有一个默认输出，但同时也可以普通暴露其他属性方法
//外部使用默认输出
import  phone from './phone.js'
phone() //i am a phone  并不知晓内部是什么属性
//整体输入
import * as phone from "./phone.js"
console.log(phone.brand)
phone.call()
phone.default()  // i am a phone 事实上，默认输出的接口绑定到了接口的default属性上。
```

---

### 11.3 加载

浏览器中使用ES6

```html
//方式一
<script type="module">
import utils from "./utils.js"
</script>

//方式二  等于把上面script标签中的内容封装了一个新的文件
<script type="module" src="./app.js"></script>
```

浏览器对于使用了`type="module"`的`<script>`标签使用异步加载，即等到整个页面渲染完再执行脚本。

`Jquery`模块加载

```html
<script type="module">
  import $ from "./jquery/src/jquery.js";
  $('#message').text('Hi from jQuery!');
</script>
```

---

## 12. 最佳实践

1. **使用let取代var**

   `let`可以完全取代`var`；在`let`和`const`之间优先使用`const`

   ```js
   let x=1;
   const [a,b,c] = [1,2,3]
   ```

2. **字符串使用`单引号`或者`反引号`，不使用`双引号`,动态字符串使用`反引号`**

   ```js
   const a='hello'
   const b = `${a} world`
   ```

3. **优先使用`解构赋值`**

   使用数组成员对变量赋值；

   ```js
   const arr=[1,2,3]
   let [a,b] = arr;
   ```

   函数的参数是对象的成员；

   ```js
   function print({name,age})
   {
       console.log(name,age)
   }
   
   let userInfo={"name":"ash","age":20,"nationality":"China"};
   print(userInfo)
   ```

   如果函数返回多个值，优先使用对象的解构赋值：便于添加返回值，调整返回值顺序

   ```js
   function returnUser()
   {
   	return {name,age,nationality,sex}
   }
   let {name,age} = returnUser()
   ```

4. **对象**

   对象属性名是动态时，使用**属性表达式**定义；尽量使用简洁表达法。

   ```js
   let id=1
   const obj={
   	id,
       name:"Ash",
       [getCompany()]:true,
       print(){
           console.log(this.id,this.name)
       }
   }
   ```

5. **使用`...`来拷贝数组**

   ```js
   const itemsCopy = [...items];
   ```

6. **使用`class`取代ES5的`function`**

   


