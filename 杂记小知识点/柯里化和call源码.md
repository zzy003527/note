## 2.4 柯里化

### 2.4.1 什么是柯里化

在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。

而对于Javascript语言来说，我们通常说的柯里化函数的概念，与数学和计算机科学中的柯里化的概念并不完全一样。

在数学和计算机科学中的柯里化函数，一次只能传递一个参数；而我们Javascript实际应用中的柯里化函数，可以传递一个或多个参数。

```js
//普通函数
function fn(a,b,c,d,e) {
  console.log(a,b,c,d,e)
}
//生成的柯里化函数
let _fn = curry(fn);

_fn(1,2,3,4,5);     // print: 1,2,3,4,5
_fn(1)(2)(3,4,5);   // print: 1,2,3,4,5
_fn(1,2)(3,4)(5);   // print: 1,2,3,4,5
_fn(1)(2)(3)(4)(5); // print: 1,2,3,4,5

```

对于已经柯里化后的 _fn 函数来说，当接收的参数数量与原函数的形参数量相同时，执行原函数； 当接收的参数数量小于原函数的形参数量时，**返回一个函数用于接收剩余的参数**，**直至接收的参数数量与形参数量一致，执行原函数**。

### 2.4.2 柯里化的用途

柯里化的本质是降低通用性，提高适用性

#### 例子1

我们工作中会遇到各种需要通过正则检验的需求，比如校验电话号码、校验邮箱等， 这时我们会封装一个通用函数 checkByRegExp ,接收两个参数，校验的正则对象和待校验的字符串

```js
function checkByRegExp(regExp,string) {
    return regExp.test(string);  
}

checkByRegExp(/^1\d{10}$/, '18642838455'); // 校验电话号码
checkByRegExp(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/, 'test@163.com'); // 校验邮箱

```

当当需要验证多个电话号码时，我们就得需要一直传入正则表达式，造成代码冗长

这时，可以利用柯里化对函数进行封装，以简便代码书写，提高代码可读性:

```js
//进行柯里化
let _check = curry(checkByRegExp);
//生成工具函数，验证电话号码
let checkCellPhone = _check(/^1\d{10}$/);
//生成工具函数，验证邮箱
let checkEmail = _check(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/);

checkCellPhone('18642838455'); // 校验电话号码
checkCellPhone('13109840560'); // 校验电话号码
checkCellPhone('13204061212'); // 校验电话号码

checkEmail('test@163.com'); // 校验邮箱
checkEmail('test@qq.com'); // 校验邮箱
```

经过柯里化后，我们生成了两个函数 checkCellPhone 和 checkEmail， checkCellPhone 函数只能验证传入的字符串是否是电话号码， checkEmail 函数只能验证传入的字符串是否是邮箱， 它们与 原函数 checkByRegExp 相比，从功能上通用性降低了，但适用性提升了。 柯里化的这种用途可以被理解为：**参数复用**

### 2.4.3 简单封装柯里化函数

柯里化函数会等参数数量足够的时候才会调用原函数，所以要先存储参数，判断参数数量

```js
// length是js函数对象的一个属性值，表示有多少个必须要传入的参数， 
// 形参的数量不包括剩余参数个数，仅包括 “第一个具有默认值之前的参数个数”

function curry(fn, len = fn.length) {
    return _curry.call(this, fn, len)
}

function _curry(fn, len, ...args) {
    return function(...parms) {
        let _args = [...args, ...parms]
        if(_args.length >= len) {
            return fn.call(this, ..._args)
        } else {
            return _curry(fn, len, ..._args)
        }
    }
}


let _fn = curry(function(a,b,c,d,e){
    console.log(a,b,c,d,e)
});

_fn(1)
const fn1 = _fn(1, 2)
fn1(5, 6, 7) // 1, 2, 5, 6, 7

```

## 2.5 call,apply,bind源码解析

三者都可以改变调用函数的this指向，让this指向第一个参数，其实现原理是通过在该参数中手动添加该函数，通过默认绑定原则，通过该参数调用这个函数，自然this就是指向该参数了

* 在函数的原型上添加这三者，当调用fn.call()时，实际上是调用了call函数

### 2.4.1 call

```js
Function.prototype.myCall = function(context) {
	
    let content = context || window
    content.fn = this  // this是调用该函数的对象，即fn,隐式绑定
    let arg = [...arguments].slice(1)  // 将参数中的第一个去掉
    content.fn(...arg)
    delete content.fn  // 要删除，不然会给obj添加函数
}

fn.myCall(obj, 1, 2)

```

### 2.4.2 apply

```js
Function.prototype.myApply = function(context) {
    let content = context || window
    content.fn = this
    let arg = arguments[1]
    content.fn(...arg)
    // let arg = [...arguments].slice(1)
    // content.fn(arg)
    delete content.fn
}

add.myApply(obj1, [4, 6])
add.myApply(obj1, [5, 5])
```

### 2.4.3 bind

```js
Function.prototype.myBind = function(context, ...args) {
  	const self = this
    const content = context || window
    return function(...params) {
        self.apply(context, [...args, ...params])
    }
}
```





参考博客：https://juejin.cn/post/6844903882208837645

