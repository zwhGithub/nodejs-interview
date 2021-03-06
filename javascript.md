## javascript面试题整理

* [ES6有哪些新特性](#ES6有哪些新特性)
* [常用js类定义的方法有哪些？](#常用js类定义的方法有哪些)
* [js类继承的方法有哪些](#js类继承的方法有哪些)
* [js类多重继承的实现方法是怎么样的?](#js类多重继承的实现方法是怎么样的)
* [apply, call和bind有什么区别?](#apply-call和bind有什么区别)
* [defineProperty, hasOwnProperty, propertyIsEnumerable都是做什么用的？](#defineProperty-hasOwnProperty-propertyIsEnumerable都是做什么用的)
* [js常用设计模式的实现思路，单例，工厂，代理，装饰，观察者模式等](#js常用设计模式的实现思路单例工厂代理装饰观察者模式等)
* [简述一下js的闭包？](#简述一下js的闭包)
* [列举数组相关的常用方法](#列举数组相关的常用方法)
* [列举字符串相关的常用方法](#列举字符串相关的常用方法)
* [Math相关的常用方法](#Math相关的常用方法)
* [Object相关的常用方法](#Object相关的常用方法)
* [实现以下函数，使得输入的字符串逆序输出](#实现以下函数使得输入的字符串逆序输出)
* [实现throttle节流函数](#实现throttle节流函数)
* [实现debounce防抖函数](#实现debounce防抖函数)
* [数组去重有哪些方法？](#数组去重有哪些方法)
* [扁平化数组](#扁平化数组)
* [new操作符都做了什么？](#new操作符都做了什么)
* [实现一个new](#实现一个new)
* [手写一个JS深拷贝](#手写一个JS深拷贝)
* [手写一个instanceof](#手写一个instanceof)
* [实现一个JSON.stringify](#实现一个JSONstringify)
* [实现sleep函数](#实现sleep函数)
* [在JavaScript中,(a==1 && a==2 && a==3)是否有可能为true?](#在JavaScript中a1--a2--a3是否有可能为true)
* [变量对象与活动对象](#变量对象与活动对象)
* [commonjs和es6在模块引用上的区别？](#commonjs和es6在模块引用上的区别)
* [ES6模块与CommonJS模块的差异](#ES6模块与CommonJS模块的差异)


### ES6有哪些新特性

1. 类的支持
2. 模块化
3. 箭头函数
4. let/const块作用域
5. 模板字符串
6. 解构
7. 参数默认值/不定参数/扩展参数
8. for-of遍历
9. generator
10. Map/Set
11. Promise
12. 延展操作符

``async/await`` 在es8中加入


### 常用js类定义的方法有哪些？

主要有构造函数原型和对象创建两种方法。原型法是通用老方法，对象创建是ES5推荐使用的方法.目前来看，原型法更普遍.

1、构造函数方法定义类

```
function Person(){
	this.name = 'michaelqin';
}
Person.prototype.sayName = function(){
	alert(this.name);
}

var person = new Person();
person.sayName();
```

2、对象创建方法定义类

```
var Person = {
	name: 'michaelqin',
	sayName: function(){ alert(this.name); }
};

var person = Object.create(Person);
person.sayName();
```

### js类继承的方法有哪些

原型链法，属性复制法和构造器应用法. 另外，由于每个对象可以是一个类，这些方法也可以用于对象类的继承．

1、原型链法

```
function Animal() {
	this.name = 'animal';
}
Animal.prototype.sayName = function(){
	alert(this.name);
};

function Person() {}
Person.prototype = Animal.prototype; // 人继承自动物
Person.prototype.constructor = 'Person'; // 更新构造函数为人
```

2、属性复制法

```
function Animal() {
	this.name = 'animal';
}
Animal.prototype.sayName = function() {
	alert(this.name);
};

function Person() {}

for(prop in Animal.prototype) {
	Person.prototype[prop] = Animal.prototype[prop];
} // 复制动物的所有属性到人量边
Person.prototype.constructor = 'Person'; // 更新构造函数为人
```

3、构造器应用法

```
function Animal() {
	this.name = 'animal';
}
Animal.prototype.sayName = function() {
	alert(this.name);
};

function Person() {
	Animal.call(this); // apply, call, bind方法都可以．细微区别，后面会提到．
}
```


### js类多重继承的实现方法是怎么样的?

就是类继承里边的属性复制法来实现．因为当所有父类的prototype属性被复制后，子类自然拥有类似行为和属性．


### apply, call和bind有什么区别?

三者都可以把一个函数应用到其他对象上，注意不是自身对象．apply,call是直接执行函数调用，bind是绑定，执行需要再次调用．apply和call的区别是apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表，

```
function Person() {
}
Person.prototype.sayName() { alert(this.name); }

var obj = {name: 'michaelqin'}; // 注意这是一个普通对象，它不是Person的实例
1) apply
Person.prototype.sayName.apply(obj, [param1, param2, param3]);

2) call
Person.prototype.sayName.call(obj, param1, param2, param3);

3) bind
var sn = Person.prototype.sayName.bind(obj);
sn([param1, param2, param3]); // bind需要先绑定，再执行
sn(param1, param2, param3); // bind需要先绑定，再执行
```

### defineProperty, hasOwnProperty, propertyIsEnumerable都是做什么用的？

``Object.defineProperty(obj, prop, descriptor)``用来给对象定义属性,有``value``,``writable``,``configurable``,``enumerable``,``set/get``等.``hasOwnProerty``用于检查某一属性是不是存在于对象本身，继承来的父亲的属性不算．``propertyIsEnumerable``用来检测某一属性是否可遍历，也就是能不能用``for..in``循环来取到.


### js常用设计模式的实现思路，单例，工厂，代理，装饰，观察者模式等

1) 单例：　任意对象都是单例，无须特别处理

```
var obj = {name: 'michaelqin', age: 30};
```

2) 工厂: 就是同样形式参数返回不同的实例

```
function Person() { this.name = 'Person1'; }
function Animal() { this.name = 'Animal1'; }

function Factory() {}
Factory.prototype.getInstance = function(className) {
	return eval('new ' + className + '()');
}

var factory = new Factory();
var obj1 = factory.getInstance('Person');
var obj2 = factory.getInstance('Animal');
console.log(obj1.name); // Person1
console.log(obj2.name); // Animal1
```

3) 代理: 就是新建个类调用老类的接口,包一下

```
function Person() { }
Person.prototype.sayName = function() { 
	console.log('michaelqin'); 
}
Person.prototype.sayAge = function() { console.log(30); }

function PersonProxy() {
	this.person = new Person();
	var that = this;
	this.callMethod = function(functionName) {
		console.log('before proxy:', functionName);
		that.person[functionName](); // 代理
		console.log('after proxy:', functionName);
	}
}

var pp = new PersonProxy();
pp.callMethod('sayName'); // 代理调用Person的方法sayName()
pp.callMethod('sayAge'); // 代理调用Person的方法sayAge()
```

4) 观察者: 就是事件模式，比如按钮的onclick这样的应用.

```
function Publisher() {
	this.listeners = [];
}
Publisher.prototype = {
	'addListener': function(listener) {
		this.listeners.push(listener);
	},

	'removeListener': function(listener) {
		delete this.listeners[this.listeners.indexOf(listener)];
	},

	'notify': function(obj) {
		for(var i = 0; i < this.listeners.length; i++) {
			var listener = this.listeners[i];
			if (typeof listener !== 'undefined') {
				listener.process(obj);
			}
		}
	}
}; // 发布者

function Subscriber() {

}
Subscriber.prototype = {
	'process': function(obj) {
		console.log(obj);
	}
};　// 订阅者


var publisher = new Publisher();
publisher.addListener(new Subscriber());
publisher.addListener(new Subscriber());
publisher.notify({name: 'michaelqin', ageo: 30}); // 发布一个对象到所有订阅者
publisher.notify('2 subscribers will both perform process'); // 发布一个字符串到所有订阅者

```

### 简述一下js的闭包？

闭包就是能够读取其他函数内部变量的函数。

由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。

所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

```
function f1(){
   var n = 999;
   function f2(){
       alert(n);
   }
   return f2;
}

var result = f1();
result();//999
```
f2()就是闭包

### 列举数组相关的常用方法

下面这些方法会改变数组自身的值：
	
1. ``Array.prototype.pop()`` 删除数组的最后一个元素，并返回这个元素
2. ``Array.prototype.push()`` 在数组的末尾增加一个或多个元素，并返回数组长度
3. ``Array.prototype.shift()`` 删除数组的第一个约束，并返回这个元素
4. ``Array.prototype.unshift()`` 在数组的开头增加一个或多个元素，并返回数组长度
5. ``Array.prototype.sort()`` 对数组进行排序，并返回当前数组。
6. ``Array.prototype.reverse()`` 颠倒数组中元素的排列顺序。
7. ``Array.prototype.splice()`` 在任意位置给数组添加或删除任意个元素。

下面这些方法不会改变数组自身的值，只会返回一个新的数组或者其他值：

1. ``Array.prototype.concat()`` 返回当前数组和其他若干数组或者若干个费数组值组合而成的新数组
2. ``Array.prototype.includes()`` 判断当前数组是否包含某指定的值。
3. ``Array.prototype.join()`` 连接所有数组元素组成一个字符串
4. ``Array.prototype.slice()`` 抽取当前数组中的一段元素组合成一个新数组
5. ``Array.prototype.toString()`` 返回由数组元素组成的字符串
6. ``Array.prototype.indexOf()`` 返回数组中第一个与指定值相等的元素的索引，如果找不到返回-1
7. ``Array.prototype.lastIndexOf()`` 返回数组中最后一个与指定值相等的元素的索引，如果找不到返回-1

遍历方法:

1. ``Array.prototype.forEach()`` 为数组中每一个元素执行一次回调。
2. ``Array.prototype.every()`` 如果数组中每个元素都满足测试函数，则返回true，否则返回false；
3. ``Array.prototype.some()`` 如果数组中至少有一个元素满足测试函数，则返回true，否则返回false
4. ``Array.prototype.filter()`` 将所有在过滤函数中返回true的数组元素放假一个新数组中并返回。
5. ``Array.prototype.find()`` 找到第一个满足测试函数的元素并返回那个元素的值，如果找不到，返回undefined
6. ``Array.prototype.map()`` 返回一个由回调函数的返回值组成的新数组
7. ``Array.prototype.reduce()`` 从左到右为每个元素执行一次回调函数，并把上次回调函数的返回值放在一个暂存器中传给下次回调函数，并返回最后一次回调函数的返回值。

### 列举字符串相关的常用方法

1. ``String.prototype.indexOf()`` 从字符串对象中返回首个被发现的给定值的索引，没找到返回-1
2. ``String.prototype.lastIndexOf()`` 从字符串对象中返回最后一个被发现的给定值的索引，没找到返回-1
3. ``String.prototype.charAt()`` 返回特定位置的字符
4. ``String.prototype.split()`` 将字符串分割为数组
5. ``String.prototype.match()`` 使用正则表达式
6. ``String.prototype.slice()`` 摘取一个字符串区域，返回一个新字符串
7. ``String.prototype.substring()`` 返回在字符串中指定两个下标之间的字符
8. ``String.prototype.substr()`` 通过指定字符数返回在指定位置开始的字符串中的字符。
9. ``String.prototype.trim()`` 从字符串的开始和结尾去除空格
10. ``String.prototype.concat()`` 连接两个字符串，返回新字符串
11. ``String.prototype.includes()`` 判断一个字符串是否包含某个字符

### Math相关的常用方法

1. ``Math.abs(x)`` 返回绝对值
2. ``Math.ceil(x)`` 返回x向上取整后的值
3. ``Math.floor(x)`` 返回x向下取整后的值
4. ``Math.min(x)``  返回最小值
5. ``Math.max(x)`` 返回最大值
6. ``Math.pow(x, y)`` 返回x的y次幂
7. ``Math.random(x)`` 返回0-1之间的伪随机数
8. ``Math.round(x)`` 返回四舍五入后的整数
9. ``Math.sign(x)`` 返回x的符号函数，判断x是正数、负数还是0 

### Object相关的常用方法

1. ``Object.assign()`` 通过复制一个或多个对象来创建一个新对象
2. ``Object.create()`` 使用指定的原型对象和属性创建一个新对象
3. ``Object.defineProperty()`` 给对象添加一个属性并指定该属性的配置
4. ``Object.keys()`` 返回一个包含所有给定对象自身可枚举属性名称的数组
5. ``Object.values()`` 返回给定对象自身可枚举值的数组


### 实现以下函数，使得输入的字符串逆序输出


```
'hello world!'.reverse();  // output: '!dlrow olleh'
```

这个问题需要给String的原型上添加方法，但是要考虑有一些改造：

```
String.prototype.reverse = function reverse() {
  let res = this.split('');
  return res.reverse().join('');
}
```

### 实现throttle节流函数

用法：
	
```
const throFun = () => console.log('hello');
const thro = throttle(throFun, 300);
document.body.onscroll = () => {
  thro();  // 调用至少间隔 300 毫秒才会触发一次
}
```

```
/**
- 频率控制，返回函数连续调用时，action 执行频率限定为 1次 / delay
- @param delay  {number}    延迟时间，单位毫秒
- @param action {function}  请求关联函数，实际应用需要调用的函数
- @return {function}    返回客户调用函数 
*/
function throttle(action, delay) {
  var previous = 0;
  // 使用闭包返回一个函数并且用到闭包函数外面的变量previous
  return function() {
    var _this = this;
    var args = arguments;
    var now = new Date();
    if(now - previous > delay) {
        action.apply(_this, args);
        previous = now;
    }
  }
}
```

### 实现debounce防抖函数

用法：
	
```
const throFun = () => console.log('hello');
const thro = debounce(throFun, 300);
document.body.onscroll = () => {
  thro();  // 若一直调用则不会执行，空闲时间大于 300 毫秒才会执行
}
```

```
/**
- 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 delay，action 才会执行
- @param delay   {number}    空闲时间，单位毫秒
- @param action {function}  请求关联函数，实际应用需要调用的函数
- @return {function}    返回客户调用函数 */
function debounce(action, delay) {
  var timer; // 维护一个 timer
  return function () {
      var _this = this; // 取debounce执行作用域的this
      var args = arguments;
      if (timer) {
          clearTimeout(timer);
      }
      timer = setTimeout(function () {
          action.apply(_this, args); // 用apply指向调用debounce的对象，相当于_this.action(args);
      }, delay);
  };
}
```

### 数组去重有哪些方法？

```
const arr = [1,2,3,4,4,3,2,1];
// 方法一：new Set ES6
return [...new Set(arr)]; // 注意...的用法

// 方法二：双层for循环 (这是一种方法，但是性能不好)
function unique(arr){
  var res=[];
  for (var i = 0; i < arr.length; i++) {
    for (var j = i+1; j < arr.length; j++) {
      if (arr[i] === arr[j]) {
        ++ i;
        j = i;
      }
    }
    res.push(arr[i]);
  }
  return res;
}

// 方法三：单层for循环 + indexOf
function unique(array){
    var res = [];
    for(var i = 0; i < array.length; i++) {
        //如果当前数组的第i项在当前数组中第一次出现的位置是i，才存入数组；否则代表是重复的
        if (array.indexOf(array[i]) === i) {
            res.push(array[i])
        }
    }
    return res;
}
//或者这样
function unique(array){
    let res = [];
    for(var i = 0; i < array.length; i++) {
        if (res.indexOf(array[i]) === -1) {
            res.push(array[i]);
        }
    }
    return res;
}

// 方法四：如果可以容忍改变原有数组的情况下，这样性能更好
function unique(array){
    // 注意这里一定要倒叙for循环，否则会出现问题
    for(var i = array.length - 1; i > 0; i--) { 
        if (array.indexOf(array[i]) !== i) {
            array.splice(i, 1);
        }
    }
    // 因为少声明一个变量，节省了内存空间
    return array;
}

```


### 扁平化数组

完成一个函数,接受数组作为参数,数组元素为整数或者数组,数组元素包含整数或数组,函数返回扁平化后的数组
	
如：[1, [2, [ [3, 4], 5], 6]] => [1, 2, 3, 4, 5, 6]

```
function flat(array, result = []) {
	for (let i = 0; i < array.length; i++) {
		const value = array[i];
		if (Array.isArray(value)) {
			flat(value, result);
		} else {
			result.push(value);
		}
	}
	return result;
}
```

```
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]]

const flatten = function (arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr)
    }
    return arr
}

console.log(flatten(arr))
```

```
const flattenDeep = (arr) => Array.isArray(arr)
? arr.reduce( (a, b) => [...a, ...flattenDeep(b)] , [])
: [arr]

flattenDeep([1, [[2], [3, [4]], 5]])
```

一下方法扁平化数组的同时去重并排序：
```
Array.from(new Set(arr.toString().split(',').sort((a,b)=>{return a-b;}).map(Number)));
```

```
Array.from(new Set(arr.flat(Infinity))).sort((a,b)=>{return a-b});
```


### new操作符都做了什么？

四大步骤：
1. 创建一个空对象，并且 this 变量引用该对象，// `let target = {}`;
2. 继承了函数的原型。// `target.proto = func.prototype;`
3. 属性和方法被加入到 this 引用的对象中。并执行了该函数func// `func.call(target);`
4. 新创建的对象由 this 所引用，并且最后隐式的返回 this 。// 如果`func.call(target)`返回的res是个对象或者function 就返回它

```
function new(func) {
	lat target = {};
	target.__proto__ = func.prototype;
	let res = func.call(target);
	if (typeof(res) == "object" || typeof(res) == "function") {
		return res;
	}
	return target;
}
```

### 实现一个new？

```
function new(fn, ...arg){
	const obj = Object.create(fn.prototype);
	fn.apply(obj, arg);
	return obj;
}
```

### 手写一个JS深拷贝

1、乞丐版

```
consr netObj = JSON.parse(JSON.stringify(oldObj));
```

2、升级版

```javascript
function cloneDeep(obj){
	let ret;
	if(typeof obj === 'object'){
		ret = obj.constructor === Array ? [] : {};
		for (let i in obj){
			ret[i] = typeof obj[i] === 'object' ? cloneDeep(obj[i]) : obj[i];
		}
	} else {
		ret = obj;
	}
	return obj;
}
```

### 手写一个instanceof

```
function _instanceof(left, right){
	let proto = left.__proto__;
	const prototype = right.prototype;
	while(true){
		if(proto===null) return false;
		if(proto===prototype) return true;
		proto = proto.__proto__;
	}
}
```

### 实现一个JSON.stringify


* Boolean | Number| String 类型会自动转换成对应的原始值。
* undefined、任意函数以及symbol，会被忽略（出现在非数组对象的属性值中时），或者被转换成 null（出现在数组中时）。
* 不可枚举的属性会被忽略
* 如果一个对象的属性值通过某种间接的方式指回该对象本身，即循环引用，属性也会被忽略。

```
function jsonStringify(obj) {
    let type = typeof obj;
    if (type !== "object") {
        if (/string|undefined|function/.test(type)) {
            obj = '"' + obj + '"';
        }
        return String(obj);
    } else {
        let json = []
        let arr = Array.isArray(obj)
        for (let k in obj) {
            let v = obj[k];
            let type = typeof v;
            if (/string|undefined|function/.test(type)) {
                v = '"' + v + '"';
            } else if (type === "object") {
                v = jsonStringify(v);
            }
            json.push((arr ? "" : '"' + k + '":') + String(v));
        }
        return (arr ? "[" : "{") + String(json) + (arr ? "]" : "}")
    }
}
jsonStringify({x : 5}) // "{"x":5}"
jsonStringify([1, "false", false]) // "[1,"false",false]"
jsonStringify({b: undefined}) // "{"b":"undefined"}"
```


### 实现sleep函数

要求用法：
	
```
/**
- 使当前运行的异步操作（promise 或者 async）停止等待若干秒
- 
- @param ms */
(async () => {
    console.log('hello')
    await sleep(2000) // 等待两秒
    console.log('world')
})()
```

```
const sleep = (ms) => {
    new Promise(resolve, reject) {
    setTimeOut(() => {
        resolve();
    }, ms)
    }
}

```

### 在JavaScript中,(a==1 && a==2 && a==3)是否有可能为true?

```
const a = {
    i: 1,
    toString: function () {
        return a.i++;
    }
}
if(a == 1 && a == 2 && a == 3) {
    console.log('Hello World!');
}// Hello World!
```

利用松散相等运算符 == 的工作原理.使用松散相等时，如果其中一个操作数与另一个类型不同，则 JS 引擎将尝试将一个操作转换为另一个类型。在左边对象、右边的数字的情况下，它会尝试将对象转换为一个数，首先通过调用 valueOf 如果是可调用的。否则，它会调用toString方法。我使用toString仅仅是因为它是我的第一反应，valueOf 会更合理。如果我不从toString返回一个字符串（而是返回数字），JS 引擎会尝试将字符串转换为一个数字，虽然有一个稍长的路径，但它仍然会给我们同样的结果。


### 变量对象与活动对象

当调用一个函数时（激活），一个新的执行上下文就会被创建。而一个执行上下文的生命周期可以分为两个阶段。

* 创建阶段：在这个阶段中，执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向。
* 代码执行阶段：创建完成之后，就会开始执行代码，这个时候，会完成变量赋值，函数引用，以及执行其他代码。

变量对象VO在执行上下文的创建阶段，而活动对象AO在执行上下文的执行阶段。

变量对象在函数被调用的时候被创建，VO（变量对象）包含：函数的形参（arguments）、函数声明（FunctionDeclaration, FD）、变量声明（VariableDeclaration，var）三个内容。

当函数开始执行时进入执行上下文的执行阶段，变量对象就会被激活，初始化成为活动对象AO。


### commonjs和es6在模块引用上的区别？

* CommonJS模块规范使用require语句导入模块，module.exports导出模块，输出的是值的拷贝，模块导入的也是输出值的拷贝，也就是说，一旦输出这个值，这个值在模块内部的变化是监听不到的。

* ES6模块的规范是使用import语句导入模块，export语句导出模块，输出的是对值的引用。ES6模块的运行机制和CommonJS不一样，遇到模块加载命令import时不去执行这个模块，只会生成一个动态的只读引用，等真的需要用到这个值时，再到模块中取值，也就是说原始值变了，那输入值也会发生变化。


###  ES6模块与CommonJS模块的差异

1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

* CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。
* ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的import有点像 Unix 系统的“符号连接”，原始值变了，import加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

2. CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

* 运行时加载: CommonJS 模块就是对象；即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。
* 编译时加载: ES6 模块不是对象，而是通过 export 命令显式指定输出的代码，import时采用静态命令的形式。即在import时可以指定加载某个输出值，而不是加载整个模块，这种加载称为“编译时加载”。

CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。
