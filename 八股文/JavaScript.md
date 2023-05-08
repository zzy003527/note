# JavaScript

## 一、类型及检测方式

### 1.1 JS内置类型

- JavaScript 的数据类型有下图所示

  - ![](https://s.poetries.work/images/20210414100319.png)

- `JavaScript`一共有8种数据类型，其中有7种基本数据类型：`Undefined`、`Null`、`Boolean`、`Number`、`String`、`Symbol`（`es6`新增，表示独一无二的值）和`BigInt`（`es10`新增）；

- 1种引用数据类型——`Object`

  （Object本质上是由一组无序的名值对组成的）。里面包含`function、Array、Date`等。JavaScript不支持任何创建自定义类型的机制，而所有值最终都将是上述 8 种数据类型之一。

  - **引用数据类型:** 对象`Object`（包含普通对象-`Object`，数组对象-`Array`，正则对象-`RegExp`，日期对象-`Date`，数学函数-`Math`，函数对象-`Function`）

> 因为各种 JavaScript 的数据类型最后都会在初始化之后放在不同的内存中，因此上面的数据类型大致可以分成两类来进行存储：

- **原始数据类型**：基础类型存储在栈内存，被引用或拷贝时，会创建一个完全相等的变量；占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。
- **引用数据类型**：引用类型存储在堆内存，存储的是地址，多个引用指向同一个地址，这里会涉及一个“共享”的概念；占据空间大、大小不固定。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。





#### 1.1.1 **JavaScript 中的数据是如何存储在内存中的？**

- **在 JavaScript 中，原始类型的赋值会完整复制变量值，而引用类型的赋值是复制引用地址**
  - 在 JavaScript 的执行过程中， 主要有三种类型内存空间，分别是`代码空间`、`栈空间`、`堆空间`。其中的**代码空间主要是存储可执行代码的**，**原始类型**(`Number、String、Null、Undefined、Boolean、Symbol、BigInt`)的数据值都是直接**保存在“栈”中**的，**引用类型(Object)的值是存放在“堆”中的**。因此**在栈空间中(执行上下文)，原始类型存储的是变量的值，而引用类型存储的是其在"堆空间"中的地址**，当 JavaScript 需要访问该数据的时候，是通过栈中的引用地址来访问的，相当于多了一道转手流程。
  - 在编译过程中，**如果 JavaScript 引擎判断到一个闭包，也会在堆空间创建换一个`“closure(fn)”`的对象**（这是一个内部对象，JavaScript 是无法访问的），用来保存闭包中的变量。所以**闭包中的变量是存储在“堆空间”中的**。
  - JavaScript 引擎需要用栈来维护程序执行期间上下文的状态，如果栈空间大了话，所有的数据都存放在栈空间里面，那么会影响到上下文切换的效率，进而又影响到整个程序的执行效率。**通常情况下，栈空间都不会设置太大**，主要用来存放一些原始类型的小数据。而引用类型的数据占用的空间都比较大，所以这一类数据会被存放到堆中，堆空间很大，能存放很多大的数据，不过缺点是分配内存和回收内存都会占用一定的时间。因此需要“栈”和“堆”两种空间。





#### 1.1.2 题目

- 题目一

  - ```js
    let a = {
      name: 'lee',
      age: 18
    }
    let b = a;
    console.log(a.name);  //第一个console
    b.name = 'son';
    console.log(a.name);  //第二个console
    console.log(b.name);  //第三个console
    ```

  - 第一个 console 打出来 name 是 'lee'，这应该没什么疑问；但是在执行了 b.name='son' 之后，结果你会发现 a 和 b 的属性 name 都是 'son'，第二个和第三个打印结果是一样的，这里就体现了引用类型的“共享”的特性，即这两个值都存在同一块内存中共享，一个发生了改变，另外一个也随之跟着变化。

- 题目二

  - ```js
    let a = {
      name: 'Julia',
      age: 20
    }
    function change(o) {
      o.age = 24;
      o = {
        name: 'Kath',
        age: 30
      }
      return o;
    }
    let b = change(a);     // 注意这里没有new
    console.log(b.age);    // 第一个console
    console.log(a.age);    // 第二个console
    ```

  - 这道题涉及了 `function`，你通过上述代码可以看到第一个 `console` 的结果是 `30`，`b` 最后打印结果是 `{name: "Kath", age: 30}`；第二个 `console` 的返回结果是 `24`，而 `a` 最后的打印结果是 `{name: "Julia", age: 24}`。

  - 原因在于：**函数传参进来的 `o`，传递的是对象在堆中的内存地址值，通过调用 `o.age = 24`（第 7 行代码）确实改变了 `a` 对象的 `age` 属性**；但是第 12 行代码的 **`return` 却又把 `o` 变成了另一个内存地址**，将 `{name: "Kath", age: 30}` 存入其中，最后返回 `b` 的值就变成了 `{name: "Kath", age: 30}`。而如果把第 12 行去掉，那么 `b` 就会返回 `undefined`





### 1.2 数据类型检测

#### 1.2.1 typeof

- typeof 对于原始类型来说，除了 null 都可以显示正确的类型

- ```js
  console.log(typeof 2);               // number
  console.log(typeof true);            // boolean
  console.log(typeof 'str');           // string
  console.log(typeof []);              // object     []数组的数据类型在 typeof 中被解释为 object
  console.log(typeof function(){});    // function
  console.log(typeof {});              // object
  console.log(typeof undefined);       // undefined
  console.log(typeof null);            // object     null 的数据类型被 typeof 解释为 object
  ```

- `typeof` 对于对象来说，除了函数都会显示 `object`，所以说 **`typeof` 并不能准确判断变量到底是什么类型,所以想判断一个对象的正确类型**，这时候可以考虑使用 `instanceof`



#### 1.2.2 instanceof

- `instanceof` 可以正确的判断对象的类型，因为**内部机制是通过判断对象的原型链中是不是能找到类型的 `prototype`**

- ```js
  console.log(2 instanceof Number);                    // false
  console.log(true instanceof Boolean);                // false 
  console.log('str' instanceof String);                // false  
  console.log([] instanceof Array);                    // true
  console.log(function(){} instanceof Function);       // true
  console.log({} instanceof Object);                   // true    
  // console.log(undefined instanceof Undefined);
  // console.log(null instanceof Null);
  ```

  - `instanceof` 可以准确地判断复杂引用数据类型，但是**不能正确判断基础数据类型**；
  - 而 `typeof` 也存在弊端，它虽然可以判断基础数据类型（`null` 除外），但是引用数据类型中，除了 `function` 类型以外，其他的也无法判断

- 手写instanceof

  - ```js
    // 我们也可以试着实现一下 instanceof
    function _instanceof(left, right) {
        // 由于instance要检测的是某对象，需要有一个前置判断条件
        //基本数据类型直接返回false
        if(typeof left !== 'object' || left === null) return false;
    
        // 获得类型的原型
        let prototype = right.prototype
        // 获得对象的原型
        left = left.__proto__
        // 判断对象的类型是否等于类型的原型
        // 递归遍历left的原型链
        while (true) {
        	if (left === null)
        		return false
        	if (prototype === left)
        		return true
        	left = left.__proto__
        }
    }
    
    console.log('test', _instanceof(null, Array)) // false
    console.log('test', _instanceof([], Array)) // true
    console.log('test', _instanceof('', Array)) // false
    console.log('test', _instanceof({}, Object)) // true
    ```



#### 1.2.3 constructor

- ```js
  console.log((2).constructor === Number); // true
  console.log((true).constructor === Boolean); // true
  console.log(('str').constructor === String); // true
  console.log(([]).constructor === Array); // true
  console.log((function() {}).constructor === Function); // true
  console.log(({}).constructor === Object); // true
  ```

- 这里有一个坑，如果我创建一个对象，更改它的原型，`constructor`就会变得不可靠了

  - ```js
    function Fn(){};
     
    Fn.prototype=new Array();
     
    var f=new Fn();
     
    console.log(f.constructor===Fn);    // false
    console.log(f.constructor===Array); // true 
    ```





#### 1.2.4  Object.prototype.toString.call()

- `toString()` 是 `Object` 的原型方法，调用该方法，可以统一返回格式为 `“[object Xxx]”` 的字符串，其中 `Xxx` 就是对象的类型。对于 `Object` 对象，直接调用 `toString()` 就能返回 `[object Object]`；而对于其他对象，则需要通过 `call` 来调用，才能返回正确的类型信息。我们来看一下代码。

  - ```js
    Object.prototype.toString({})       // "[object Object]"
    Object.prototype.toString.call({})  // 同上结果，加上call也ok
    Object.prototype.toString.call(1)    // "[object Number]"
    Object.prototype.toString.call('1')  // "[object String]"
    Object.prototype.toString.call(true)  // "[object Boolean]"
    Object.prototype.toString.call(function(){})  // "[object Function]"
    Object.prototype.toString.call(null)   //"[object Null]"
    Object.prototype.toString.call(undefined) //"[object Undefined]"
    Object.prototype.toString.call(/123/g)    //"[object RegExp]"
    Object.prototype.toString.call(new Date()) //"[object Date]"
    Object.prototype.toString.call([])       //"[object Array]"
    Object.prototype.toString.call(document)  //"[object HTMLDocument]"
    Object.prototype.toString.call(window)   //"[object Window]"
    
    // 从上面这段代码可以看出，Object.prototype.toString.call() 可以很好地判断引用类型，甚至可以把 document 和 window 都区分开来。
    ```

- 实现一个全局通用的数据类型判断方法，来加深你的理解，代码如下

  - ```js
    function getType(obj){
      let type  = typeof obj;
      if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
        return type;
      }
      // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
      return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1');  // 注意正则中间有个空格
    }
    /* 代码验证，需要注意大小写，哪些是typeof判断，哪些是toString判断？思考下 */
    getType([])     // "Array" typeof []是object，因此toString返回
    getType('123')  // "string" typeof 直接返回
    getType(window) // "Window" toString返回
    getType(null)   // "Null"首字母大写，typeof null是object，需toString来判断
    getType(undefined)   // "undefined" typeof 直接返回
    getType()            // "undefined" typeof 直接返回
    getType(function(){}) // "function" typeof能判断，因此首字母小写
    getType(/123/g)      //"RegExp" toString返回
    ```





#### 1.2.5 小结

- `typeof`
  - 直接在计算机底层基于数据类型的值（二进制）进行检测
  - `typeof null`为`object` 原因是对象存在在计算机中，都是以`000`开始的二进制存储，所以检测出来的结果是对象
  - `typeof` 普通对象/数组对象/正则对象/日期对象 都是`object`
  - `typeof NaN === 'number'`
- `instanceof`
  - 检测当前实例是否属于这个类的
  - 底层机制：只要当前类出现在实例的原型上，结果都是true
  - 不能检测基本数据类型
- `constructor`
  - 支持基本类型
  - constructor可以随便改，也不准
- `Object.prototype.toString.call([val])`
  - 返回当前实例所属类信息

> 判断 `Target` 的类型，单单用 `typeof` 并无法完全满足，这其实并不是 `bug`，本质原因是 `JS` 的万物皆对象的理论。因此要真正完美判断时，我们需要区分对待:

- 基本类型(`null`): 使用 `String(null)`
- 基本类型(`string / number / boolean / undefined`) + `function`: - 直接使用 `typeof`即可
- 其余引用类型(`Array / Date / RegExp Error`): 调用`toString`后根据`[object XXX]`进行判断





### 1.3 数据类型转换

- 在 `JS` 中类型转换只有三种情况，分别是：
  - 转换为布尔值
  - 转换为数字
  - 转换为字符串
  - ![img](https://s.poetries.work/gitee/2020/07/1.png)





#### 1.3.1  **转Boolean**

- 在条件判断时，除了 `undefined`，`null`， `false`， `NaN`， `''`， `0`， `-0`，其他所有值都转为 `true`，包括所有对象

- ```js
  Boolean(0)          //false
  Boolean(null)       //false
  Boolean(undefined)  //false
  Boolean(NaN)        //false
  Boolean(1)          //true
  Boolean(13)         //true
  Boolean('12')       //true
  ```

- 



#### 1.3.2 对象转原始类型

- 对象在转换类型的时候，会调用内置的 `[[ToPrimitive]]` 函数，对于该函数来说，算法逻辑一般来说如下

  - 如果已经是原始类型了，那就不需要转换了
  - 调用 `x.valueOf()`，如果转换为基础类型，就返回转换的值
  - 调用 `x.toString()`，如果转换为基础类型，就返回转换的值
  - 如果都没有返回原始类型，就会报错

- 也可以重写 `Symbol.toPrimitive`，该方法在转原始类型时调用优先级最高。

  ```js
  let a = {
    valueOf() {
      return 0
    },
    toString() {
      return '1'
    },
    [Symbol.toPrimitive]() {
      return 2
    }
  }
  1 + a // => 3
  ```





#### 1.3.3 **四则运算符**

- 特点：

  - **运算中其中一方为字符串，那么就会把另一方也转换为字符串**
  - **如果一方不是字符串或者数字，那么会将它转换为数字或者字符串**

- ```js
  1 + '1' // '11'
  true + true // 2
  4 + [1,2,3] // "41,2,3"
  ```

  - 对于第一行代码来说，触发特点一，所以将数字 `1` 转换为字符串，得到结果 `'11'`
  - 对于第二行代码来说，触发特点二，所以将 `true` 转为数字 `1`
  - 对于第三行代码来说，触发特点二，所以将数组通过 `toString`转为字符串 `1,2,3`，得到结果 `41,2,3`

- 另外对于加法还需要注意这个表达式 `'a' + + 'b'`

  - ```js
    'a' + + 'b' // -> "aNaN"
    ```

  - 因为 `+ 'b'` 等于 `NaN`，所以结果为 `"aNaN"`，你可能也会在一些代码中看到过 `+ '1'`的形式来快速获取 `number` 类型。

- 那么对于除了加法的运算符来说，只要其中一方是数字，那么另一方就会被转为数字

  - ```js
    4 * '3' // 12
    4 * [] // 0
    4 * [1, 2] // NaN
    ```





#### 1.3.4 比较运算符

- 如果是对象，就通过 `toPrimitive` 转换对象
- 如果是字符串，就通过 `unicode` 字符索引来比较

- ```js
  let a = {
    valueOf() {
      return 0
    },
    toString() {
      return '1'
    }
  }
  a > -1 // true
  ```

- 在以上代码中，因为 `a` 是对象，所以会通过 `valueOf` 转换为原始类型再比较值。





#### 1.3.5 强制类型转换

- 强制类型转换方式包括 `Number()`、`parseInt()`、`parseFloat()`、`toString()`、`String()`、`Boolean()`，这几种方法都比较类似

- `Number()` 方法的强制转换规则

  - 如果是布尔值，`true` 和 `false` 分别被转换为 `1` 和 `0`；
  - 如果是数字，返回自身；
  - 如果是 `null`，返回 `0`；
  - 如果是 `undefined`，返回 `NaN`；
  - 如果是字符串，遵循以下规则：如果字符串中只包含数字（或者是 `0X / 0x` 开头的十六进制数字字符串，允许包含正负号），则将其转换为十进制；如果字符串中包含有效的浮点格式，将其转换为浮点数值；如果是空字符串，将其转换为 `0`；如果不是以上格式的字符串，均返回 NaN；
  - 如果是 `Symbol`，抛出错误；
  - 如果是对象，并且部署了 `[Symbol.toPrimitive]` ，那么调用此方法，否则调用对象的 `valueOf()` 方法，然后依据前面的规则转换返回的值；如果转换的结果是 `NaN` ，则调用对象的 `toString()` 方法，再次依照前面的顺序转换返回对应的值。

- ```js
  Number(true);        // 1
  Number(false);       // 0
  Number('0111');      //111
  Number(null);        //0
  Number('');          //0
  Number('1a');        //NaN
  Number(-0X11);       //-17
  Number('0X11')       //17
  ```







#### 1.3.6 Object的转换规则

- 对象转换的规则，会先调用内置的 `[ToPrimitive]` 函数，其规则逻辑如下：

  - 如果部署了 `Symbol.toPrimitive` 方法，优先调用再返回；
  - 调用 `valueOf()`，如果转换为基础类型，则返回；
  - 调用 `toString()`，如果转换为基础类型，则返回；
  - 如果都没有返回基础类型，会报错。

- ```js
  var obj = {
    value: 1,
    valueOf() {
      return 2;
    },
    toString() {
      return '3'
    },
    [Symbol.toPrimitive]() {
      return 4
    }
  }
  console.log(obj + 1); // 输出5
  // 因为有Symbol.toPrimitive，就优先执行这个；如果Symbol.toPrimitive这段代码删掉，则执行valueOf打印结果为3；如果valueOf也去掉，则调用toString返回'31'(字符串拼接)
  // 再看两个特殊的case：
  10 + {}
  // "10[object Object]"，注意：{}会默认调用valueOf是{}，不是基础类型继续转换，调用toString，返回结果"[object Object]"，于是和10进行'+'运算，按照字符串拼接规则来，参考'+'的规则C
  [1,2,undefined,4,5] + 10
  // "1,2,,4,510"，注意[1,2,undefined,4,5]会默认先调用valueOf结果还是这个数组，不是基础数据类型继续转换，也还是调用toString，返回"1,2,,4,5"，然后再和10进行运算，还是按照字符串拼接规则，参考'+'的第3条规则
  ```





#### 1.3.7 **'==' 的隐式类型转换规则**

- 如果类型相同，无须进行类型转换；

- 如果其中一个操作值是 `null` 或者 `undefined`，那么另一个操作符必须为 `null` 或者 `undefined`，才会返回 `true`，否则都返回 `false`；

- 如果其中一个是 `Symbol` 类型，那么返回 `false`；

- 两个操作值如果**为 `string` 和 number 类型**，那么就会**将字符串转换为 `number`；**

- 如果一个**操作值是 `boolean`，那么转换成 `number`；**

- 如果一个操作值为 `object` 且另一方为 `string`、`number` 或者 `symbol`，就会**把 `object` 转为原始类型再进行判断**（调用 `object` 的 `valueOf/toString` 方法进行转换）。

- ```js
  null == undefined       // true  规则2
  null == 0               // false 规则2
  '' == null              // false 规则2
  '' == 0                 // true  规则4 字符串转隐式转换成Number之后再对比
  '123' == 123            // true  规则4 字符串转隐式转换成Number之后再对比
  0 == false              // true  e规则 布尔型隐式转换成Number之后再对比
  1 == true               // true  e规则 布尔型隐式转换成Number之后再对比
  var a = {
    value: 0,
    valueOf: function() {
      this.value++;
      return this.value;
    }
  };
  // 注意这里a又可以等于1、2、3
  console.log(a == 1 && a == 2 && a ==3);  //true f规则 Object隐式转换
  // 注：但是执行过3遍之后，再重新执行a==3或之前的数字就是false，因为value已经加上去了，这里需要注意一下
  ```





#### 1.3.8 **'+' 的隐式类型转换规则**

- ‘+' 号操作符，不仅可以用作数字相加，还可以用作字符串拼接。**仅当 '+' 号两边都是数字时，进行的是加法运算**；**如果两边都是字符串，则直接拼接**，无须进行隐式类型转换。

  - 如果其中有一个是字符串，另外一个是 `undefined`、`null` 或布尔型，则调用 `toString()` 方法进行字符串拼接；如果是纯对象、数组、正则等，则默认调用对象的转换方法会存在优先级，然后再进行拼接。
  - 如果其中有一个是数字，另外一个是 `undefined`、`null`、布尔型或数字，则会将其转换成数字进行加法运算，对象的情况还是参考上一条规则。
  - 如果其中一个是字符串、一个是数字，则按照字符串规则进行拼接

- ```js
  1 + 2        // 3  常规情况
  '1' + '2'    // '12' 常规情况
  // 下面看一下特殊情况
  '1' + undefined   // "1undefined" 规则1，undefined转换字符串
  '1' + null        // "1null" 规则1，null转换字符串
  '1' + true        // "1true" 规则1，true转换字符串
  '1' + 1n          // '11' 比较特殊字符串和BigInt相加，BigInt转换为字符串
  1 + undefined     // NaN  规则2，undefined转换数字相加NaN
  1 + null          // 1    规则2，null转换为0
  1 + true          // 2    规则2，true转换为1，二者相加为2
  1 + 1n            // 错误  不能把BigInt和Number类型直接混合相加
  '1' + 3           // '13' 规则3，字符串拼接
  ```

- 整体来看，如果数据中有字符串，JavaScript 类型转换还是更倾向于转换成字符串，因为第三条规则中可以看到，在字符串和数字相加的过程中最后返回的还是字符串，这里需要关注一下





#### 1.3.9 **null 和 undefined 的区别？**

- 首先 `Undefined` 和 `Null` 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 `undefined` 和 `null`。
- **`undefined` 代表的含义是未定义， `null` 代表的含义是空对象**（其实不是真的对象，请看下面的注意！）。一般变量声明了但还没有定义的时候会返回 `undefined`，`null` 主要用于赋值给一些可能会返回对象的变量，作为初始化。

> 其实 null 不是对象，虽然 typeof null 会输出 object，但是这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象，然而 null 表示为全零，所以将它错误的判断为 object 。虽然现在的内部类型判断代码已经改变了，但是对于这个 Bug 却是一直流传下来。

- undefined 在 js 中不是一个保留字，这意味着我们可以使用 `undefined` 来作为一个变量名，这样的做法是非常危险的，它会影响我们对 undefined 值的判断。但是我们可以通过一些方法获得安全的 `undefined` 值，比如说 `void 0`。
- 当我们对两种类型使用 typeof 进行判断的时候，Null 类型化会返回 “object”，这是一个历史遗留的问题。当我们使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。









## 二、This

### 2.1 什么是this

- this提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将API设计得更加简洁并且易于复用

- this的上下文取决于**函数调用**时的各种条件，**<u>this的绑定</u>**和**函数声明的位置**没有任何关系，<u>只取决于函数的**调用方式**</u>

- 当一个函数被调用时，会创建一个活动记录（执行上下文），这个记录会包含函数的调用位置（函数调用栈），函数的调用调用方式和传入的参数。this 就是这个记录的一个属性，会在函数执行的过程中用到。

  

### 2.2 this的绑定规则

#### 2.2.1 默认绑定

- 如果直接调用函数，this指向全局对象
- 如果把null或undefined作为this的绑定对象传入call或apply，这些值在调用时会被忽略。实际应用默认绑定规则
- **如果使用严格模式（strict mode），则不能将全局对象用于默认绑定，因此this会绑定到undefined**



#### 2.2.2 隐式绑定

- 当**函数引用有上下文对象**时，**隐式绑定规则会把函数调用中的this绑定到这个上下文对象**。

- 如：

  ```js
  function foo() { 
   console.log( this.a ); 
  } 
   
  
  var obj = { 
   a: 2, 
   foo: foo 
  }; 
   
  obj.foo(); // 2
  ```

  - 调用位置会使用 obj 上下文来引用函数，因此你可以说函数被调用时 obj 对象“拥 有”或者“包含”它。
  - 因为调用 foo() 时 this 被绑定到 obj，因此 this.a 和 obj.a 是一样的

- 对象属性引用链中只有上一层或者说最后一层在调用位置中起作用

  - 如：

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

  - 输出的a是最后一层调用（obj2）的a

- **总结一下就是，当一个函数以`方法调用模式`被调用时，this指向直接对象**

- **隐式丢失**

  - 一个最常见的 this 绑定问题就是被隐式绑定的函数会丢失绑定对象，也就是说它会应用默认绑定，从而把 this 绑定到全局对象或者 undefined 上，取决于是否是严格模式。

  - 原因1：函数别名

    - 把一个隐式绑定的函数赋给了另一个变量，对这个变量进行操作造成隐式丢失，this指向window

    - ```js
      var a = 0;
      function fn(){
          console.log(this.a);
      }
      var obj = {
          a: 1,
          myFn: fn
      }
      // 隐式绑定，this.a指向直接对象obj下的属性a
      obj.myFn();         // 1
      
      //obj.myFn被赋值给bar,相当于把console.log(this.a);这句语句赋值给bar，bar与obj没有直接关系
      var bar = obj.myFn;
      // bar()相当于单纯的执行打印a这个操作，bar的作用域是全局，那么this就是window
      // 隐式丢失，this.a指向window下的变量a
      bar();              // 0
      ```

  - 原因2：参数传递

    - 把一个隐式绑定的函数作为参数传入另一个函数中，在另一个函数内部被调用时造成隐式丢失，this指向window

    - ```js
      var a = 0;
      function foo(){
          console.log(this.a);
      }
      function bar(fn){
          fn();
      }
      var obj = {
          a: 1,
          myFn: foo
      }
      // obj.myFn作为参数传入到bar函数中，在该函数内被调用，相当于在bar函数内部调用了foo()，与obj无关
      // 隐式丢失，this.a指向window下的变量a
      bar(obj.myFn);      // 0
      ```

  - 原因3：内置函数

    - 如内置函数setTimeout和setInterval，它俩的第一个参数是回调函数，这个回调函数中this指向window

    - ```js
      setTimeout(function(){
          console.log(this);      // window
      });
      
      var a = 0;
      function fn(){
          console.log(this.a);
      }
      var obj = {
          a: 1,
          myFn: fn
      }
      // obj.myFn作为内置函数setTimeout的回调函数，内置函数执行时造成隐式丢失，this.a指向window下的变量a
      setTimeout(obj.myFn,1000);   // 0;
      ```

  - 原因4：间接调用

    - 将一个隐式绑定的函数用call()，apply()等方法间接调用时，造成隐式丢失，this指向call、apply中首个参数，若为空则指向window

    - ```js
      var a = 0;
      function fn(){
          console.log(this.a);
      }
      var obj = {
          a: 1,
          myFn: fn
      }
      var p = {
          a: 2
      }
      
      fn.call();          // 0
      fn.call(obj);       // 1
      p.myFn = obj.myFn;
      fn.call(p);         // 2
      ```

  - 其他原因

    - 比如，自执行函数中this指向window

    - ```js
      var a = 0;
      function fn(){
          console.log(this.a);
      }
      var obj = {
          a: 1,
          myFn: fn
      }
      var p = {
          a: 2
      }
      // 相当于fn函数的立即调用，this指向window
      (p.myFn = obj.myFn)()
      ```

  

#### 2.2.3 显式绑定

- 使用`call(...)`、` apply(...) `方法

- *如果你传入了一个原始值（字符串类型、布尔类型或者数字类型）来当作this的绑定对象，这个原始值会被转换成它的对象形式（也就是newString(…)、new Boolean(…)或者newNumber(…)）。这通常被称为“装箱”*

- 硬绑定

  - **绑定后就不能二次更改绑定对象，一次绑定永久有效**

  - bind() 返回一个硬编码的新函数，它会把指定的参数设置为this的上下文并调用原始函数

  - 如：

    ```js
    function foo() { 
     console.log( this.a ); 
    } 
     
    
    var obj = { 
     a:2 
    }; 
     
    
    var bar = function() { 
     foo.call( obj ); 
    }; 
     
    bar(); // 2 
    setTimeout( bar, 100 ); // 2 
     
    // 硬绑定的 bar 不可能再修改它的 this 
    bar.call( window ); // 2
    ```

    - 创建了**函数 bar()**，并在它的内部手动调用 了 foo.call(obj)，因此强制把 foo 的 this 绑定到了 obj。无论之后如何调用函数 bar，它 总会手动在 obj 上调用 foo。
    - **这个函数bar()的绑定是一种显式的强制绑定，我们称之为硬绑定**



#### 2.2.4 new绑定

- 在JavaScript中，构造函数只是一些使用new操作符时被调用的函数。它们并不会属于某个类，也不会实例化一个类。实际上，它们甚至都不能说是一种特殊的函数类型，它们只是被new操作符调用的普通函数而已。

- new调用函数的步骤：

  - 创建一个新对象
  - 这个新对象会被执行 [[Prototype]] 连接
  - 这个新对象会绑定到函数调用的 this
  - 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象

- 比如：

  ```js
  function foo(a) { 
   this.a = a; 
  } 
   
  var bar = new foo(2); 
  console.log( bar.a ); // 2
  ```

  - 使用 new 来调用 foo(..) 时，我们会构造一个新对象并把它绑定到 foo(..) 调用中的 this上。new 是最后一种可以影响函数调用时 this 绑定行为的方法，我们称之为 new 绑定。



#### 2.2.5 优先级

-  **<u>new绑定 > 显式绑定 > 隐式绑定 > 默认绑定</u>**
-  判断this，现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的 顺序来进行判断：
   1. 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。`var bar = new foo()`
   2. .函数是否通过 call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是 指定的对象。`var bar = foo.call(obj2)`
   3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上 下文对象。`var bar = obj1.foo()`
   4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定 到全局对象。`var bar = foo()`
-  ![image.png](https://s.poetries.work/gitee/2020/07/2.png)





### 2.3 绑定例外

#### 2.3.1 被忽略的this

- 如果你把 null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind，这些值 在调用时会被忽略，实际应用的是默认绑定规则

- 比如：

  ```js
  function foo() { 
   console.log( this.a ); 
  } 
   
  var a = 2; 
   
  foo.call( null ); // 2
  ```

  - 当我们的call()函数不想要绑定什么时，可以传入null，让其绑定window（或严格模式下的undefined）



#### 2.3.2 软绑定

- 软绑定

  - 硬绑定会大大降低函数的灵活性，**使用硬绑定之后就无法使用隐式绑定或者显式绑定来修改 `this`。**
  - 如果可以**给默认绑定指定一个全局对象和 `undefined` 以外的值**，那就可以实现和硬绑定相同的效果，同时**保留隐式绑定或者显式绑定修改 this 的能力**。可以通过一种被称为**软绑定**的方法来实现我们想要的效果

- 软绑定代码：

  - ```js
    if(!Function.prototype.softBind){
        Function.prototype.softBind=function(obj){
            var fn=this;
            var args=Array.prototype.slice.call(arguments,1);
            var bound=function(){
                return fn.apply(
                    (!this||this===(window||global))?obj:this,
                    args.concat.apply(args,arguments)
                );
            };
            bound.prototype=Object.create(fn.prototype);
            return bound;
        };
    }
    ```

  - 测试代码：

    ```js
    function foo(){
        console.log("name: "+this.name);
    }
    
    var obj1={name:"obj1"},
        obj2={name:"obj2"},
        obj3={name:"obj3"};
    
    var fooOBJ=foo.softBind(obj1);
    fooOBJ();//"name: obj1" 在这里软绑定生效了，成功修改了this的指向，将this绑定到了obj1上
    
    obj2.foo=foo.softBind(obj1);
    obj2.foo();//"name: obj2" 在这里软绑定的this指向成功被隐式绑定修改了，绑定到了obj2上
    
    fooOBJ.call(obj3);//"name: obj3" 在这里软绑定的this指向成功被硬绑定修改了，绑定到了obj3上
    
    setTimeout(obj2.foo,1000);//"name: obj1"
    /*回调函数相当于一个隐式的传参，如果没有软绑定的话，这里将会应用默认绑定将this绑定到全局环
    境上，但有软绑定，这里this还是指向obj1*/
    ```

  - 在第一行，先通过判断，如果函数的原型上没有`softBind()`这个方法，则添加它，然后通过`Array.prototype.slice.call(arguments,1)`获取传入的外部参数，这里这样做其实为了函数柯里化，也就是说，允许在软绑定的时候，事先设置好一些参数，在调用函数的时候再传入另一些参数。

  - 最后返回一个`bound`函数形成一个闭包，这时候，在函数调用`softBind()`之后，得到的就是`bound`函数，例如上面的`var fooOBJ=foo.softBind(obj1)`。

  - 在`bound`函数中，首先会判断调用软绑定之后的函数（如fooOBJ）的调用位置，或者说它的this的指向，如果`!this`（this指向undefined）或者`this===(window||global)`（this指向全局对象），那么就将函数的this绑定到传入`softBind`中的参数obj上。

  - 如果此时this不指向undefind或者全局对象，那么就将this绑定到现在正在指向的函数（即隐式绑定或显式绑定）。`fn.apply`的第二个参数则是运行`foo`所需要的参数，由上面的args（外部参数）和内部的arguments（内部参数）连接成，也就是上面说的柯里化。



#### 2.3.3 this词法

- **箭头函数不适用this的标准规则,而是根据外层(函数或全局)作用域来决定this**,箭头函数最常用于**回调函数**中，例如事件处理器或者定时器, **相当于self=this**

- 箭头函数的词法作用域：

  - ```js
    function foo() { 
     // 返回一个箭头函数 
     return (a) => { 
     //this 继承自 foo() 
     console.log( this.a ); 
     }; 
    } 
     
    var obj1 = { 
     a:2 
    }; 
    
    var obj2 = { 
     a:3 
    }; 
    
    var bar = foo.call( obj1 ); 
    bar.call( obj2 ); // 2, 不是 3 ！
    ```

  - foo() 内部创建的箭头函数会捕获调用时 foo() 的 this。由于 foo() 的 this 绑定到 obj1，bar（引用箭头函数）的 this 也会绑定到 obj1，箭头函数的绑定无法被修改。（new 也不行）







## 三、onclick和addEventListent()的区别

- 区别：

  - onclick添加事件不能绑定多个事件，后面绑定的会覆盖前面的。而addEventListener能添加多个事件绑定，按顺序执行。
  - onclick只能冒泡，addEventListener()可以得到捕获or冒泡。
  - addEventListener方式，不支持低版本的IE。（attachEvent 支持IE）。
  - 普通方式绑定事件后，不可以取消。addEventListener绑定后则可以用 removeEvenListener 取消。
  - addEventListener 是W3C DOM 规范中提供的注册事件监听器的方法。
  - addEventListener对任何DOM都是有效的，而onclick仅限于HTML。
  - on 事件解绑需要将其置为none，而addEventListener需要使用removeEventListener。
- **addEventListener用法：**

  - **语法：** element.addEventListener( type , listener , useCapture ) - - 添加事件监听 - - **type**: 事件类型字符串，不使用“on”前缀 - - **callback**：事件处理程序（回调函数） - - **useCapture**：可选参数，是否使用事件捕获的方式处理事件。不传递时，默认为false，表示不使用事件捕获（使用事件冒泡），如果需要显示事件捕获，则显示传递true。
  - **示例：**

    ```js
    document.getElementById("item").addEventListener( 'click' , (event) => {
    	console.log(event)
    }, false )
    ```

    - addEventListener 第三个参数： 为 true 时，浏览器采用Capture（捕捉） 为 false 时，浏览器采用bubbing（冒泡）-- 建议使用！
- **addEventListener兼容写法：**

  - IE9之前浏览器使用element.attachEvent(type, callback)

  ```js
  attachEvent(type, callback)
  ```

  - type:事件类型字符串，使用“on”前缀 
  - callback：事件处理程序（回调函数） 注意：因为IE9之前只有事件捕获，没有事件冒泡，所有attachEvent没有第三个参数。











## 四、apply/call/bind 原理

- ![img](https://s.poetries.work/images/20210414155100.png)
- `call、apply` 和 `bind` 是挂在 `Function` 对象上的三个方法，调用这三个方法的必须是一个函数。

  - ```js
    func.call(thisArg, param1, param2, ...)
    func.apply(thisArg, [param1,param2,...])
    func.bind(thisArg, param1, param2, ...)
    ```
  - 在浏览器里，在全局范围内this 指向window对象；
  - 在函数中，this永远指向最后调用他的那个对象；

  - 构造函数中，this指向new出来的那个新的对象；

  - `call、apply、bind`中的this被强绑定在指定的那个对象上；

  - 箭头函数中this比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来；

  - `apply、call、bind`都是js给函数内置的一些API，调用他们可以为函数指定this的执行,同时也可以传参。

  - ![img](https://s.poetries.work/gitee/2020/09/6.png)





### 4.1 方法的应用场景

#### 4.1.1 判断数据类型

- 用 `Object.prototype.toString` 来判断类型是最合适的，借用它我们几乎可以判断所有类型的数据

  ```js
  function getType(obj){
    let type  = typeof obj;
    if (type !== "object") {
      return type;
    }
    return Object.prototype.toString.call(obj).replace(/^$/, '$1');
  }
  ```





#### 4.1.2 类数组借用方法

- 类数组因为不是真正的数组，所有没有数组类型上自带的种种方法，所以我们就可以利用一些方法去借用数组的方法，比如借用数组的 `push` 方法，看下面的一段代码。

  ```js
  var arrayLike = { 
    0: 'java',
    1: 'script',
    length: 2
  } 
  Array.prototype.push.call(arrayLike, 'jack', 'lily'); 
  console.log(typeof arrayLike); // 'object'
  console.log(arrayLike);
  // {0: "java", 1: "script", 2: "jack", 3: "lily", length: 4}
  ```

- 用 `call` 的方法来借用 `Array 原型链上的 push` 方法，可以实现一个`类数组的 push` 方法，给 `arrayLike` 添加新的元素







#### 4.1.3 获取数组的最大/最小值

- 我们可以用 apply 来实现数组中判断最大 / 最小值，`apply` 直接传递数组作为调用方法的参数，也可以减少一步展开数组，可以直接使用 `Math.max、Math.min` 来获取数组的最大值 / 最小值，请看下面这段代码。

  ```js
  let arr = [13, 6, 10, 11, 16];
  const max = Math.max.apply(Math, arr); 
  const min = Math.min.apply(Math, arr);
   
  console.log(max);  // 16
  console.log(min);  // 6
  ```







### 4.2 手写call函数

- ```js
  function call(fn, obj, ...args) {
       console.log('call')
  
       // 如果obj是undefined或者是null，则将this指定为window
       if (obj === undefined || obj === null) {
           obj = window;
       }
  
       // 给obj添加一个临时方法，方法指向的函数就是fn
       obj.tempFn = fn;
       // 通过obj来调用这个方法，这样子就执行了fn函数，并且执行的fn函数中的this为obj
       const result = obj.tempFn(...args);
       // 删除obj上的临时方法
       delete obj.tempFn;
       // 返回fn执行的结果
       return result
   }
  ```

- 使用：

  ```js
  let obj = {
       c: 15
   }
   const AA = function (a, b) {
       console.log(this.c);
       console.log(a + b + this.c);
       return a + b;
   }
  
   call(AA, obj, 1, 2);
  
  // 15，18
  ```



### 4.3 手写apply函数

- ```js
  function apply(fn, obj, args) {
       console.log('apply');
  
       // 如果obj是undefined或者null，则将this指定为window
       if (obj === undefined || obj === null) {
           obj = window;
       }
  
       // 给obj添加一个临时方法，方法指向的函数就是fn
       obj.tempFn = fn;
       // 通过obj来调用这个方法：则会执行fn函数，并且执行的fn函数中的this为obj
       const result = obj.tempFn(...args);
       // 删除obj上的临时方法
       delete obj.tempFn;
       // 返回fn执行的结果
       return result;
   }
  ```

- 使用：

  ```js
   let obj = {
       c: 15
   }
   const AA = function (a, b) {
       console.log(this.c);
       console.log(a + b + this.c);
       return a + b;
   }
  
   apply(AA, obj, [1, 2]);
  
  // 15，18
  ```



### 4.4 手写bind函数

- ```js
  import {
      call
  } from './call';
  
  export function bind(fn, obj, ...args) {
      console.log('bind');
      // 返回一个新函数
      return (...args2) => {
          // 通过call调用原函数，并指定this为obj，实参为args与args2之和
          return call(fn, obj, ...args, ...args2); 
      }
  }
  ```

- 使用：

  ```js
  let obj = {
      c: 15
  }
  const AA = function (a, b) {
      console.log(a, b, this.c, this);
      return a + b;
  }
  
  const fun = bind(AA, obj);
  console.log(fun(1, 2));
  
  // 1 2 15 { c: 15, tempFn: [Function: AA] }
  // 3
  ```









## 五、变量提升

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- d