# 第一章 关于this

## 1.1 为什么要用this

## 1.2 误解

### 1.2.1 指向自身

```javascript
function foo(num) { 
 console.log( "foo: " + num ); 
 // 记录 foo 被调用的次数
 this.count++; 
} 
foo.count = 0; 
var i; 
for (i=0; i<10; i++) { 
 if (i > 5) { 
 foo( i ); 
 } 
} 
// foo: 6 
// foo: 7 
// foo: 8 
// foo: 9 
// foo 被调用了多少次？
console.log( foo.count ); // 0 -- 什么？！
console.log(count); // NAN  没报错
```

foo函数调用了4次，但foo.count仍然为0， 实际上我们是在全局作用域下创建了count这个变量，但并未赋值，所以是NAN，即此时foo里面的this不是指向自身，而是指向全局window

### 1.2.2 指向它的作用域

```javascript
function foo() { 
 var a = 2; 
 this.bar(); 
} 
function bar() { 
 console.log( this.a ); 
} 
foo(); // ReferenceError: a is not defined
```

报错，说明this不指向它的作用域，所以不能通过作用域来查找或者将this跟作用域联系起来使用

## 1.3 this到底指向什么

* **this是在运行时才被绑定的**
* this的绑定和函数声明的位置没有关系，只取决于函数的调用方式
* 当一个函数被调用时，会创建一个活动记录（执行上下文），this就是这个记录的一个属性，会在函数执行过程中用到

# 第二章 this全面解析

## 2.1 绑定规则

### 2.1.1 默认绑定

当函数是直接使用且不带任何修饰时（独立函数调用），会使用默认绑定规则

```javascript
function foo() { 
 console.log( this.a ); 
} 

var a = 2; 
foo(); // 2
```

如果使用严格模式，则不能将全局对象用于默认绑定，因此this会绑定到undefined

```js
function foo() { 
 "use strict"; 
 console.log( this.a ); 
} 
var a = 2; 
foo(); // TypeError: this is undefined
```

这里有一个比较重要的细节，虽然this的绑定规则完全取决于调用位置，但是只有foo()运行在非严格模式下时，默认绑定才能绑定到全局对象，在严格模式下**调用**foo()则不影响默认绑定

```js
function foo() { 
 console.log( this.a ); 
} 
var a = 2; 
(function(){ 
 "use strict"; 
 foo(); // 2 
})()
```

注意：对于默认绑定来说，决定 this 绑定对象的并不是调用位置是否处于严格模式，而是函数体是否处于严格模式。如果函数体处于严格模式，this 会被绑定到 undefined，否则this 会被绑定到全局对象。

### 2.1.2 隐式绑定

这条规则则是需要考虑函数调用位置是否有上下文对象，

```js
function foo() { 
 console.log( this.a ); 
} 
var obj = { 
 a: 2, 
 foo: foo 
}; 
obj.foo(); // 2   this.a === obj.a
```

调用位置会是用obj的上下文来引用函数，当函数引用有上下文对象时，隐式绑定规则会把函数调用中的this绑定到这个上下文对象中

对象属性链中只有上一层或者最后一层在调用位置中起作用，如

```js
function foo() { 
 console.log( this.a ); 
} 
var obj2 = { 
 a: 42, 
 foo: foo 
}; 
var obj1 = { 
 a: 2, 
 obj2: obj2 
}; 
obj1.obj2.foo(); // 42
```

#### 隐式丢失

即被绑定的函数会丢失绑定对象，就会应用默认绑定

```js
function foo() { 
 console.log( this.a ); 
} 
var obj = { 
 a: 2, 
 foo: foo 
}; 
var bar = obj.foo; // 函数别名！
 
var a = "oops, global"; // a 是全局对象的属性
bar(); // "oops, global"
```

虽然 bar 是 obj.foo 的一个引用，但是实际上，它引用的是 foo 函数本身，因此此时的bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。

一种更微妙、更常见并且更出乎意料的情况发生在传入回调函数时：

```js
function foo() {  
    console.log( this.a ); 
} 
function doFoo(fn) {  
    // fn 其实引用的是 foo  
    fn(); 
    // <-- 调用位置！
}
var obj = {  a: 2,  foo: foo };
var a = "oops, global"; // a 是全局对象的属性
doFoo( obj.foo ); // "oops, global"
```

参数传递其实就是一种隐式赋值，因此我们传入函数时也会被隐式赋值，所以结果和上一个例子一样

### 2.1.3 显示绑定

直接指定this的绑定对象，称为显示绑定，例如可以将函数用call或apply指定this的绑定对象

#### call和apply的使用

```js
function foo() {  console.log( this.a ); } 
var obj = {  a:2 }; 
foo.call( obj ); // 2
```

通过 foo.call(..)，我们可以在调用 foo 时强制把它的 this 绑定到 obj 上。

如果你传入了一个原始值（字符串类型、布尔类型或者数字类型）来当作 this 的绑定对象，这个原始值会被转换成它的对象形式（也就是 new String(..)、new Boolean(..) 或者new Number(..)）。这通常被称为“装箱”

#### 硬绑定

硬绑定可以解决隐式丢失的问题，例子如下：

```js
function foo() {  
    console.log( this.a ); 
}
var obj = {  
    a:2 
};
var bar = function() {  
    foo.call( obj );  // 强制把foo的this绑定在obj上
                     }; 
bar(); // 2 
setTimeout( bar, 100 ); // 2 // 硬绑定的 bar 不可能再修改它的 this 
 bar.call( window ); // 2
```

硬绑定的典型应用就是创建一个包裹函数，负责接受参数并返回值

```js
function foo(something) {  
    console.log( this.a, something );  
    return this.a + something; }
var obj = {  a:2 }; 
var bar = function() {  
    return foo.apply( obj, arguments ); };
var b = bar( 3 ); // 2 3 
console.log( b ); // 5
```

另一种使用方法是创建一个可以重复使用的辅助函数：

```js
function foo(something) {  
    console.log( this.a, something );  
    return this.a + something; 
} // 简单的辅助绑定函数
function bind(fn, obj) {  
    return function() {  
        return fn.apply( obj, arguments );  }; } 
var obj = {  a:2 }; 
var bar = bind( foo, obj ); 
var b = bar( 3 ); // 2 3 console.log( b ); // 5
```

### 2.1.4 new绑定

使用new来调用函数时，会自动执行下面的操作：

* 创建或者说构造一个新对象
* 这个新对象会执行[[prototype]]连接
* 这个新对象会绑定到函数调用的this
* 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象

```js
function foo(a) {  
    this.a = a; 
} 
var bar = new foo(2); 
console.log( bar.a ); // 2
```

使用 new 来调用 foo(..) 时，我们会构造一个新对象并把它绑定到 foo(..) 调用中的 this上。new 是最后一种可以影响函数调用时 this 绑定行为的方法，我们称之为 new 绑定。

## 2.2 优先级

现在可以通过判断函数的调用位置来判断应用哪一条规则，但如果同时可以应用多条规则的话，就需要知道他们的优先级

#### 隐式绑定和显式绑定比较

```js
function foo() {  
    console.log( this.a ); 
} 
var obj1 = {  
    a: 2,  
    foo: foo 
}; 
var obj2 = {  
    a: 3,  
    foo: foo 
}; 
obj1.foo(); // 2 
obj2.foo(); // 3 
obj1.foo.call( obj2 ); // 3 
obj2.foo.call( obj1 ); // 2
```

可以看到，显式绑定优先级更高，也就是说在判断时应当先考虑是否可以存在显式绑定。

#### new绑定与隐式绑定的比较

```js
function foo(something) {  
    this.a = something; 
} 
var obj1 = {  
    a: 2,  
    foo: foo 
}; 
var obj2 = {}; 
obj1.foo( 2 ); 
console.log( obj1.a ); // 2 
obj1.foo.call( obj2, 3 ); 
console.log( obj2.a ); // 3 
// 隐式绑定与new绑定的比较
var bar = new obj1.foo( 4 );  // 即判断此时的this指向谁，
console.log( obj1.a ); // 2 
console.log( bar.a ); // 4
```

可以看到 new 绑定比隐式绑定优先级高

#### new绑定与显示绑定的比较

```js
new foo.call(obj1)  // TypeError: foo.call is not a constructor
```

new 和 call/apply 无法一起使用，因此无法通过 new foo.call(obj1) 来直接进行测试。但是我们可以使用硬绑定来测试它俩的优先级。

```js
function foo(something) {  
    this.a = something; 
} 
var obj1 = {
     a: 2,  
}; 
var bar = foo.bind( obj1 ); 
bar( 2 ); 
console.log( obj1.a ); // 2 
var baz = new bar(3); 
console.log( obj1.a ); // 2 
console.log( baz.a ); // 3
```

bar 被硬绑定到 obj1 上，但是 new bar(3) 并没有像我们预计的那样把 obj1.a 修改为 3。相反，new 修改了硬绑定（到 obj1 的）调用 bar(..) 中的 this。因为使用了new 绑定，我们得到了一个名字为 baz 的新对象，并且 baz.a 的值是 3。因此，new绑定的优先级高于显示绑定

#### 总结

* 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。var bar = new foo()

* 函数是否通过 call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是指定的对象。var bar = foo.call(obj2)

* 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。var bar = obj1.foo()

* 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。var bar = foo()

#### 软绑定

```js
if (!Function.prototype.softBind) {  
    Function.prototype.softBind = function(obj) {  
        var fn = this;  // 捕获所有 curried 参数 
        var curried = [].slice.call( arguments, 1 );  
        var bound = function() {  
            return fn.apply(  (!this || this === (window || global)) ?  obj : this, curried.concat.apply( curried, arguments )  );  
        };  
        bound.prototype = Object.create( fn.prototype );  
        return bound;  }; }
```

## 2.3 箭头函数

箭头函数不使用This的四种标准规则，而是根据外层（函数或者全局）作用域来决定This

箭头函数的词法作用域：

```js
function foo() {  
    // 返回一个箭头函数  
    return (a) => {  
        //this 继承自 foo()  
        console.log( this.a );  
    }; 
} 
var obj1 = {  a:2 }; 
var obj2 = {  a:3    }; 
var bar = foo.call( obj1 ); 
bar.call( obj2 ); // 2, 不是 3 ！
```

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

对于已经柯里化后的 _fn 函数来说，当接收的参数数量与原函数的形参数量相同时，执行原函数； 当接收的参数数量小于原函数的形参数量时，返回一个函数用于接收剩余的参数，直至接收的参数数量与形参数量一致，执行原函数。

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
// length是jss函数对象的一个属性值，表示有多少个必须要传入的参数， 
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





# 第三章 对象

## 3.1 对象定义

* 声明（文字）形式，字面量

  ```js
  var myObj = { 
   key: value 
   // ... 
  };
  ```

* 构造形式

  ```js
  var myObj = new Object(); 
  myObj.key = value;
  ```

唯一的区别是，在文字声明中你可以添加多个键 / 值对，但是在构造形式中你必须逐个添加属性。

**用上面的“构造形式”来创建对象是非常少见的，一般来说你会使用文字语法，绝大多数内置对象也是这样做的**

## 3.2 类型

在 JavaScript 中一共有六种主要类型（术语是“语言类型”）：

* string
* number
* boolean
* null
* undefined
* object

原理是这样的，不同的对象在底层都表示为二进制，在 JavaScript 中二进制前三位都为 0 的话会被判断为 object 类型，null 的二进制表示是全 0，自然前三位也是 0，所以执行 typeof 时会返回“object”。

### 3.2.1 内置对象

js中还有一些对象子类型，称为内置对象。有些内置对象的名字看起来和简单基础类型一样，不过实际上它们的关系更复杂

* String
* Number
* Boolean
* Object
* Function
* Array
* Date
* RegExp
* Error

在 JavaScript 中，它们实际上只是一些内置函数。这些内置函数可以当作构造函数来使用，从而可以构造一个对应子类型的新对象。举例来说：

```js
const str = 'i am you'
console.log(typeof str);
console.log(str instanceof String); // false

const str1 = new String('i am you')
console.log(typeof str1);  // object
console.log(str1 instanceof String); // true
```

原始值 "I am you" 并不是一个对象，它只是一个字面量，并且是一个不可变的值。如果要在这个字面量上执行一些操作，比如获取长度、访问其中某个字符等，那需要将其转换为 String 对象。

幸好，在必要时语言会自动把字符串字面量转换成一个 String 对象，也就是说你并不需要显式创建一个对象。JavaScript 社区中的大多数人都认为能使用文字形式时就不要使用构造形式。

思考下面的代码：

```js
var strPrimitive = "I am a string"; 
console.log( strPrimitive.length ); // 13 
console.log( strPrimitive.charAt( 3 ) ); // "m"
```

使用以上两种方法，我们都可以直接在字符串字面量上访问属性或者方法，之所以可以这样做，是因为引擎自动把字面量转换成 String 对象，所以可以访问属性和方法。

null 和 undefined 没有对应的构造形式，它们只有文字形式。相反，Date 只有构造，没有文字形式。

## 3.3 对象内容

之前我们提到过，对象的内容是由一些存储在特定命名位置的（任意类型的）值组成的，我们称之为属性。

需要强调的一点是，当我们说“内容”时，似乎在暗示这些值实际上被存储在对象内部，但是这只是它的表现形式。在引擎内部，这些值的存储方式是多种多样的，一般并不会存在对象容器内部。存储在对象容器内部的是这些属性的名称，它们就像指针（从技术角度来说就是引用）一样，指向这些值真正的存储位置。

```js
let obj = {
    a : 1
}
// 访问对象属性
console.log(obj.a);  // 称为属性访问
console.log(obj['a']);  // 注意要加引号 称为健访问

这两种语法的主要区别在于 . 操作符要求属性名满足标识符的命名规范，而 [".."] 语法可以接受任意 UTF-8/Unicode 字符串作为属性名。举例来说，如果要引用名称为 "SuperFun!" 的属性，那就必须使用 ["Super-Fun!"] 语法访问，因为 Super-Fun! 并不是一个有效的标识符属性名。
```

在对象中，属性名永远都是字符串。如果你使用 string（字面量）以外的其他值作为属性名，那它首先会被转换为一个字符串。即使是数字也不例外，**虽然在数组下标中使用的的确是数字，但是在对象属性名中数字会被转换成字符串**，所以当心不要搞混对象和数组中数字的用法：

### 3.3.1 可计算属性名

ES6 增加了可计算属性名，可以在文字形式中使用 [] 包裹一个表达式来当作属性名：

```js
let aa = 'bb'
let obj1 = {
    [aa + '123']: 123
}

console.log(obj1.bb123);  // 123
console.log(obj1['bb123']); // 123
```

### 3.3.2  数组

数组也是对象，所以虽然每个下标都是整数，你仍然可以给数组添加属性

```js
var myArray = [ "foo", 42, "bar" ]; 
myArray.baz = "baz"; 
myArray.length; // 3 
myArray.baz; // "baz"
```

**注意：如果你试图向数组添加一个属性，但是属性名“看起来”像一个数字，那它会变成一个数值下标（因此会修改数组的内容而不是添加一个属性 **

```js
var myArray = [ "foo", 42, "bar" ]; 
myArray["3"] = "baz"; 
myArray.length; // 4 
myArray[3]; // "baz"
```

### 3.3.3 复制对象

* 浅复制：即赋值之后的对象跟被复制的对象共用一个内存，两者哪一个改变了属性值，另一个也会改变
* 深复制：简单来说深复制就是当遇到值是对象类型的时候就再运行一遍复制，这样两者哪一个改变了另一个不会跟着改变

#### Object.assign

ES6 定义了 Object.assign(..) 方法来实现浅复制。Object.assign(..) 方法的第一个参数是目标对象，之后还可以跟一个或多个源对象。它会遍历一个或多个源对象的所有可枚举（enumerable，参见下面的代码）的自有键（owned key，很快会介绍）并把它们复制（使用 = 操作符赋值）到目标对象，最后返回目标对象

**由于 Object.assign(..) 就是使用 = 操作符来赋值，所以源对象属性的一些特性（比如 writable）不会被复制到目标对象。**

### 3.3.4 属性描述符

在创建普通属性时属性描述符会使用默认值，我们也可以使用 Object.defineProperty(..)来添加一个新属性或者修改一个已有属性（如果它是 configurable）并对特性进行设置。

```js
var myObject = {}; 
Object.defineProperty( myObject, "a", { 
 value: 2, 
 writable: true, 
 configurable: true, 
 enumerable: true
} ); 
myObject.a; // 2
```

我们使用 defineProperty(..) 给 myObject 添加了一个普通的属性并显式指定了一些特性。然而，一般来说你不会使用这种方式，除非你想修改属性描述符。

#### writable

writable 决定是否可以修改属性的值。

```js
var myObject = {}; 
Object.defineProperty( myObject, "a", { 
 value: 2, 
 writable: false, // 不可写！
 configurable: true, 
 enumerable: true
} ); 
myObject.a = 3;  // 在严格模式下，这样赋值会报错TypeError
myObject.a; // 2 
```

#### configurable

只要属性是可配置的，就可以使用 defineProperty(..) 方法来修改属性描述符：

```js
Object.defineProperty( myObject, "a", { 
 value: 4, 
 writable: true, 
 configurable: false, // 不可配置！
 enumerable: true
} ); 
myObject.a; // 4 
myObject.a = 5; 
myObject.a; // 5 
Object.defineProperty( myObject, "a", { 
 value: 6, 
 writable: true, 
 configurable: true, 
 enumerable: true
} ); // TypeError
```

* 最后一个 defineProperty(..) 会产生一个 TypeError 错误，不管是不是处于严格模式，尝试修改一个不可配置的属性描述符都会出错。注意：如你所见，把 configurable 修改成false 是单向操作，无法撤销！

* 要注意有一个小小的例外：即便属性是 confifigurable:false，我们还是可以把 writable 的状态由 true 改为 false，但是无法由 false 改为 true。

* 除了无法修改，configurable:false 还会禁止删除这个属性

  ```js
  myObject.a; // 2 
  delete myObject.a; 
  myObject.a; // 2
  ```

#### enumerable

从名字就可以看出，这个描述符控制的是属性是否会出现在对象的属性枚举中，比如说for..in 循环。如果把 enumerable 设置成 false，这个属性就不会出现在枚举中，虽然仍然可以正常访问它。相对地，设置成 true 就会让它出现在枚举中。

#### Object.defineProperties()

> 语法： `Object.defineProperties(obj, props)`

> ```
> obj
> ```
>
> 在其上定义或修改属性的对象。
>
> ```
> props
> ```
>
> 要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符

```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```

### 3.3.5 不变性

有时候你会希望属性或者对象是不可改变（无论有意还是无意）的，在 ES5 中可以通过很多种方法来实现。

很重要的一点是，所有的方法创建的都是浅不变性，也就是说，它们只会影响目标对象和它的直接属性。如果目标对象引用了其他对象（数组、对象、函数，等），其他对象的内容不受影响，仍然是可变的

#### 1. 对象常量

结合 writable:false 和 configurable:false 就可以创建一个真正的常量属性（不可修改、重定义或者删除）：

```js
var myObject = {}; 
Object.defineProperty( myObject, "FAVORITE_NUMBER", { 
 value: 42, 
 writable: false, 
 configurable: false 
} );
```

#### 2. 禁止扩展

如 果 你 想 禁 止 一 个 对 象 添 加 新 属 性 并 且 保 留 已 有 属 性， 可 以 使 用 Object.prevent Extensions(..)：

```js
let obj = {
    a: 1
}
Object.preventExtensions(obj)

console.log(obj.a);
obj.b = 2 // 在严格模式下，将会抛出 TypeError 错误
console.log(obj.b); // undefined
```

#### 3. 密封

Object.seal(..) 会创建一个“密封”的对象，这个方法实际上会在一个现有对象上调用Object.preventExtensions(..) 并把所有现有属性标记为 configurable:false。所以，密封之后不仅不能添加新属性，也不能重新配置或者删除任何现有属性（虽然可以修改属性的值）。

#### 4. 冻结

Object.freeze(..) 会创建一个冻结对象，这个方法实际上会在一个现有对象上调用Object.seal(..) 并把所有“数据访问”属性标记为 writable:false，这样就无法修改它们的值。

这个方法是你可以应用在对象上的级别最高的不可变性，它会禁止对于对象本身及其任意直接属性的修改（不过就像我们之前说过的，这个对象引用的其他对象是不受影响的）。

你可以“深度冻结”一个对象，具体方法为，首先在这个对象上调用 Object.freeze(..)，然后遍历它引用的所有对象并在这些对象上调用 Object.freeze(..)。但是一定要小心，因为这样做有可能会在无意中冻结其他（共享）对象。

### 3.3.6 [[ get ]]

属性访问在实现时有一个微妙却非常重要的细节

```js
var myObject = { 
 a: 2 
}; 
myObject.a; // 2
```

在语言规范中，myObject.a 在 myObject 上实际上是实现了 [[Get]] 操作（有点像函数调用：[[Get]]()）。对象默认的内置 [[Get]] 操作首先在对象中查找是否有名称相同的属性，如果找到就会返回这个属性的值。

然而，如果没有找到名称相同的属性，按照 [[Get]] 算法的定义会执行另外一种非常重要的行为。我们会在第 5 章中介绍这个行为（其实就是遍历可能存在的 [[Prototype]] 链，也就是原型链）。

**如果无论如何都没有找到名称相同的属性，那 [[Get]] 操作会返回值 undefined**

```js
var myObject = { 
 a: undefined
}; 
myObject.a; // undefined 
myObject.b; // undefined
```

从返回值的角度来说，这两个引用没有区别——它们都返回了 undefined。然而，尽管乍看之下没什么区别，实际上底层的 [[Get]] 操作对 myObject.b 进行了更复杂的处理。

由于仅根据返回值无法判断出到底变量的值为 undefined 还是变量不存在，所以 [[Get]]操作返回了 undefined。不过稍后我们会介绍如何区分这两种情况

### 3.3.7 [[ put ]]

你可能会认为给对象的属性赋值会触发 [[Put]] 来设置或者创建这个属性。但是实际情况并不完全是这样。

[[Put]] 被触发时，实际的行为取决于许多因素，包括对象中是否已经存在这个属性（这是最重要的因素）。

如果已经存在这个属性，[[Put]] 算法大致会检查下面这些内容。

（1）属性是否是访问描述符（参见 3.3.9 节）？如果是并且存在 setter 就调用 setter。

（2）属性的数据描述符中 writable 是否是 false ？如果是，在非严格模式下静默失败，在

严格模式下抛出 TypeError 异常。

（3）如果都不是，将该值设置为属性的值。

如果对象中不存在这个属性，[[Put]] 操作会更加复杂。我们会在第 5 章讨论 [[Prototype]]

时详细进行介绍。

### 3.3.8 getter和setter

在 ES5 中可以使用 getter 和 setter 部分改写默认操作，但是**只能应用在单个属性上，无法应用在整个对象上。**getter 是一个隐藏函数，会在获取属性值时调用。setter 也是一个隐藏函数，会在设置属性值时调用。

当你给一个属性定义 getter、setter 或者两者都有时，这个属性会被定义为“访问描述符”（和“数据描述符”相对）。对于访问描述符来说，JavaScript 会忽略它们的 value 和writable 特性，取而代之的是关心 set 和 get（还有 configurable 和 enumerable）特性。

```js
let obj1 = {
    get a() {
        return 123
    },
    get b() {
        return 88
    }
}

console.log(obj1.a); // 123
console.log(obj1.b); // 88

Object.defineProperty(obj1, 'c', {
    
    // 可以用箭头函数，但此时this不指向obj1
    // get: () => {
    //     console.log(this);
    //     return this.a * 2
    // }
    get: function() {
        console.log(this);
        return this.a * 2
    },
    enumerable: true  
    //  确保 c 会出现在对象的属性列表中,即打印obj1时会显示出c,不设置为true时不会显示
})

console.log(obj1.c);  // 246
console.log(obj1); // { a: [Getter], b: [Getter], c: [Getter] }
```

不管是对象文字语法中的 get a() { .. }，还是 defineProperty(..) 中的显式定义，二者都会在对象中创建一个不包含值的属性，对于这个属性的访问会自动调用一个隐藏函数，它的返回值会被当作属性访问的返回值

```js
var myObject = { 
 // 给 a 定义一个 getter 
 get a() { 
 return 2; 
 } 
}; 
myObject.a = 3; 
myObject.a; // 2
```

由于我们只定义了 a 的 getter，所以对 a 的值进行设置时 set 操作会忽略赋值操作，不会抛出错误。而且即便有合法的 setter，由于我们自定义的 getter 只会返回 2，所以 set 操作是没有意义的。

为了让属性更合理，还应当定义 setter，和你期望的一样，setter 会覆盖单个属性默认的[[Put]]（也被称为赋值）操作。通常来说 getter 和 setter 是成对出现的（只定义一个的话通常会产生意料之外的行为）：

```js
var myObject = { 
 // 给 a 定义一个 getter 
 get a() { 
 return this._a_; 
 }, 
 // 给 a 定义一个 setter 
 set a(val) { 
 this._a_ = val * 2; 
 } 
}; 
myObject.a = 2; 
myObject.a; // 4
```

### 3.3.9 存在性

前面我们介绍过，如 myObject.a 的属性访问返回值可能是 undefined，但是这个值有可能是属性中存储的 undefined，也可能是因为属性不存在所以返回 undefined。那么如何区分这两种情况呢？

我们可以在不访问属性值的情况下判断对象中是否存在这个属性：

```js
var myObject = { 
 a:2 
}; 
("a" in myObject); // true 
("b" in myObject); // false 
myObject.hasOwnProperty( "a" ); // true 
myObject.hasOwnProperty( "b" ); // false
```

**in 操作符会检查属性是否在对象及其 [[Prototype]] 原型链中。相比之下，hasOwnProperty(..) 只会检查属性是否在 myObject 对象中，不会检查 [[Prototype]] 链。**

有的对象可能没有连接到 Object.prototype（通过 Object.create(null) 来创建——参见第 5 章）。在这种情况下，形如 myObejct.hasOwnProperty(..)就会失败。

这 时 可 以 使 用 一 种 更 加 强 硬 的 方 法 来 进 行 判 断：**Object.prototype.hasOwnProperty.call(myObject,"a")，它借用基础的 hasOwnProperty(..) 方法并把它显式绑定到 myObject 上。**

**看起来 in 操作符可以检查容器内是否有某个值，但是它实际上检查的是某个属性名是否存在**。对于数组来说这个区别非常重要，4 in [2, 4, 6] 的结果并不是你期待的 True，因为 [2, 4, 6] 这个数组中包含的属性名是 0、1、 2，没有 4。

#### 枚举

**“可枚举”就相当于“可以出现在对象属性的遍历中**

```js
let obj2 = {}

Object.defineProperty(obj2, 'a', {
    value: 123,
    enumerable: true
})

Object.defineProperty(obj2, 'b', {
    value: 456,
    enumerable: false  // 不可枚举
})
console.log(obj2); // { a: 123 }

console.log('b' in obj2); // true

for(let k in obj2) {
    console.log(k, obj2[k]); // a 123
}
```

可以通过另一种方式来区分属性是否可枚举：

```js
var myObject = { }; 
Object.defineProperty( 
 myObject, 
 "a", 
 // 让 a 像普通属性一样可以枚举
 { enumerable: true, value: 2 } 
); 
Object.defineProperty( 
 myObject, 
 "b", 
 // 让 b 不可枚举
 { enumerable: false, value: 3 } 
); 
myObject.propertyIsEnumerable( "a" ); // true 
myObject.propertyIsEnumerable( "b" ); // false 
Object.keys( myObject ); // ["a"] 
Object.getOwnPropertyNames( myObject ); // ["a", "b"]
```

propertyIsEnumerable(..) 会检查给定的属性名是否直接存在于对象中（而不是在原型链上）并且满足 enumerable:true

**Object.keys(..)和 Object.getOwnPropertyNames(..) 都只会查找对象直接包含的属性，不会在原型上找**

## 3.4 遍历

ES5 中增加了一些数组的辅助迭代器，包括 forEach(..)、every(..) 和 some(..)。每种辅助迭代器都可以接受一个回调函数并把它应用到数组的每个元素上，唯一的区别就是它们对于回调函数返回值的处理方式不同。

#### Array.prototype.forEach()

> ## [语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#语法)
>
> ```
> arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
> ```
>
> ### [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#参数)
>
> - `callback`
>
>   为数组中每个元素执行的函数，该函数接收一至三个参数：
>
>   `currentValue`数组中正在处理的当前元素。`index` 可选数组中正在处理的当前元素的索引。`array` 可选`forEach()` 方法正在操作的数组。
>
> - `thisArg` 可选
>
>   可选参数。当执行回调函数 `callback` 时，用作 `this` 的值。
>
> ### [返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#返回值)
>
> [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。

```js
let arry = ['a', 'b', 'c', 'd']

arry.forEach((val, index, array) => {
    console.log(val, index); // a, 0 等
    console.log(array); // [ 'a', 'b', 'c', 'd' ]
})
```

当改变遍历的`this`时，注意此时只是this改变了，但遍历的还是原来的对象

```js
let arry1 = ['e', 'f', 'g']
arry.forEach(function(val) {
    console.log(this); // [ 'e', 'f', 'g' ]
    console.log(val);  // a, b, c, d
}, arry1)
```

**注意：如果使用箭头函数来传入函数参数， `thisArg` 参数会被忽略，因为箭头函数在词法上绑定了`this` 值。**

如果数组在迭代的过程中被修改了，则其他元素会被跳过

下面的例子会输出 "one", "two", "four"。当到达包含值 "two" 的项时，整个数组的第一个项被移除了，这导致所有剩下的项上移一个位置。因为元素 "four" 正位于在数组更前的位置，所以 "three" 会被跳过。 `forEach()` 不会在迭代之前创建数组的副本。

```js
var words = ['one', 'two', 'three', 'four'];
words.forEach(function(word) {
  console.log(word);
  if (word === 'two') {
    words.shift();
  }
});
// one
// two
// four
```

#### Array.prototype.every()

`every()` 方法测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。

**注意**：若收到一个空数组，此方法在一切情况下都会返回 `true`。

```js
let arry2 = [12, 34, 56, 37,90]

const tag = arry2.every((val) => {
    console.log(val);
    return val > 12 
})
// 输出 12 后就停止遍历了，因为12=12，即遍历会提前中止

console.log(tag);  // false
```

> ## [语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every#语法)
>
> ```
> arr.every(callback(element[, index[, array]])[, thisArg])
> ```
>
> ### [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every#参数)
>
> - `callback`
>
>   用来测试每个元素的函数，它可以接收三个参数：`element`用于测试的当前值。`index`可选用于测试的当前值的索引。`array`可选调用 `every` 的当前数组。
>
> - `thisArg`
>
>   执行 `callback` 时使用的 `this` 值。
>
> ### [返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every#返回值)
>
> 如果回调函数的每一次返回都为 [truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 值，返回 `**true**` ，否则返回 `**false**`。

`every` 方法为数组中的每个元素执行一次 `callback` 函数，直到它找到一个会使 `callback` 返回 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy) 的元素。如果发现了一个这样的元素，`every` 方法将会立即返回 `false`。否则，`callback` 为每一个元素返回 `true`，`every` 就会返回 `true`。`callback` 只会为那些已经被赋值的索引调用。不会为那些被删除或从未被赋值的索引调用。

`every` 不会改变原数组。

`every` 遍历的元素范围在第一次调用 `callback` 之前就已确定了。在调用 `every` 之后添加到数组中的元素不会被 `callback` 访问到。如果数组中存在的元素被更改，则他们传入 `callback` 的值是 `every` 访问到他们那一刻的值。那些被删除的元素或从来未被赋值的元素将不会被访问到。

#### Array.prototype.some()

`**some()**` 方法测试数组中是不是至少有1个元素通过了被提供的函数测试。它返回的是一个Boolean类型的值。

**注意：**如果用一个空数组进行测试，在任何情况下它返回的都是`false`。

```js
let arry2 = [12, 34, 56, 37,90]
const tag2 = arry2.some((val) => {
    console.log(val);
    return val > 34  
})

// 输出12， 34， 56 就停止了，因为56>34

console.log(tag2);  // true
```

> ## [语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some#syntax)
>
> ```
> arr.some(callback(element[, index[, array]])[, thisArg])
> ```
>
> ### [参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some#parameters)
>
> - `callback`
>
>   用来测试每个元素的函数，接受三个参数：`element`数组中正在处理的元素。`index` 可选数组中正在处理的元素的索引值。`array`可选`some()`被调用的数组。
>
> - `thisArg`可选
>
>   执行 `callback` 时使用的 `this` 值。
>
> ### [返回值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some#返回值)
>
> 数组中有至少一个元素通过回调函数的测试就会返回**`true`**；所有元素都没有通过回调函数的测试返回值才会为false。
>
> ## [描述](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some#description)
>
> `some()` 为数组中的每一个元素执行一次 `callback` 函数，直到找到一个使得 callback 返回一个“真值”（即可转换为布尔值 true 的值）。如果找到了这样一个值，`some()` 将会立即返回 `true`。否则，`some()` 返回 `false`。`callback` 只会在那些”有值“的索引上被调用，不会在那些被删除或从来未被赋值的索引上调用。
>
> `callback` 被调用时传入三个参数：元素的值，元素的索引，被遍历的数组。
>
> 如果一个`thisArg`参数提供给some()，它将被用作调用的 `callback`的 `this` 值。否则， 它的 `this` value将是 `undefined`。`this`的值最终通过callback来观察，根据 [the usual rules for determining the `this` seen by a function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)的this判定规则来确定。
>
> `some()` 被调用时不会改变数组。
>
> `some()` 遍历的元素的范围在第一次调用 `callback`. 前就已经确定了。在调用 `some()` 后被添加到数组中的值不会被 `callback` 访问到。如果数组中存在且还未被访问到的元素被 `callback` 改变了，则其传递给 `callback` 的值是 `some()` 访问到它那一刻的值。已经被删除的元素不会被访问到。

#### for...of

ES6 增加了一种用来遍历数组的 for..of 循环语法,能够直接遍历值而不是数组下标（如果对象本身定义了迭代器的话也可以遍历对象）：

```js
var myArray = [ 1, 2, 3 ]; 
for (var v of myArray) { 
 console.log( v );   // 1 2 3
} 
```

**for..of 循环首先会向被访问对象请求一个迭代器对象，然后通过调用迭代器对象的next() 方法来遍历所有返回值。**

数组有内置的 @@iterator，因此 for..of 可以直接应用在数组上。我们使用内置的 @@iterator 来手动遍历数组，看看它是怎么工作的：

```js
var myArray = [ 1, 2, 3 ]; 
var it = myArray[Symbol.iterator](); 
console.log(it.next());; // { value:1, done:false } 
console.log(it.next());; // { value:2, done:false } 
console.log(it.next());; // { value:3, done:false } 
console.log(it.next()); // { done:true }
```

和数组不同，普通的对象没有内置的 @@iterator，所以无法自动完成 for..of 遍历。

# 第四章 混合对象“类”









