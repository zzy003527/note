# 《你不知道的JavaScript》（上卷）阅读笔记

## 一.作用域和闭包

### 1.作用域

#### 1.1 演员表

-  引擎

  从头到尾负责整个 JavaScript 程序的编译及执行过程。

- 编译器

  引擎的好朋友之一，负责语法分析及代码生成等脏活累活。

-  作用域

  引擎的另一位好朋友，负责收集并维护由所有声明的标识符（变量）组成的一系列查 询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。



#### 1.2 编译过程

- 例子：

  - 当进行`var a = 2`时，会发生这样的事：
    - 遇到 `var a`，编译器会询问作用域是否已经有一个该名称的变量存在于同一个作用域的 集合中。如果是，编译器会忽略该声明，继续进行编译；否则它会要求作用域在当前作 用域的集合中声明一个新的变量，并命名为` a`。
    - 接下来编译器会为引擎生成运行时所需的代码，这些代码被用来处理` a = 2 `这个赋值 操作。引擎运行时会首先询问作用域，在当前的作用域集合中是否存在一个叫作 `a `的 变量。如果是，引擎就会使用这个变量；如果否，引擎会继续查找该变量
    - 如果引擎最终找到了 a 变量，就会将 2 赋值给它。否则引擎就会举手示意并抛出一个异 常
  - 变量的赋值操作会执行两个动作，首先编译器会在当前作用域中声明一个变量（如果之前没有声明过），然后在运行时引擎会在作用域中查找该变量，如果能够找到就会对 它赋值。

- **RHS查询 （赋值操作的左侧）**

  **简单地找到某个变量**

- **LHS查询（赋值操作的右侧）**

  **试图找到变量地容器本身，从而可以对其赋值**

- **在概念上最 好将其理解为“赋值操作的目标是谁（LHS）”以及“谁是赋值操作的源头 （RHS）**

- 例子：

  - ```js
    function foo(a) { 
     console.log( a ); // 2 
    } 
     
    foo( 2 );
    ```

  - 上方代码中最后一行 foo(..) 函数的调用需要**对 foo** 进行 **RHS 引用**，意味着“去找到 foo 的值，并把 它给我

  - 在 2 被当作参数传递给foo(..) 函数时，2 会被分配给参数 a。**为了给参数 a（隐式地）分配值，需要进行一次**

    **LHS 查询**

- 练习：

  - ```js
    function foo(a) { 
     var b = a; 
     return a + b; 
    } 
    
    var c = foo( 2 );
    ```

  - 上面代码中，在`function foo(a)`的时候，`执行b = a`的时候、`var c  = foo(2)`的时候都进行了**LHS查询，共3次**

  - 在`调用foo(2)`时、`b=a时查询a的值`、`a+b时查询a和b的值`的时候都进行了**RHS查询，共4次**



#### 1.3 作用域嵌套

- **当一个块或函数嵌套在另一个块或函数中时，就发生了作用域的嵌套。因此，在当前作用域中无法找到某个变量时，引擎就会在外层嵌套的作用域中继续查找，直到找到该变量，或抵达最外层的作用域（也就是全局作用域）为止。**

  

#### 1.4 异常

- **ReferenceError**
  - RHS查询在所有嵌套的作用域中遍寻不到所需的变量，即未声明
  - *（非严格模式下）当引擎执行LHS查询时，如果在全局作用域中也无法找到目标变量，全局作用域中就会创建一个具有该名称的变量*
- **TypeError**
  - 可以在作用域中找到变量，但是对结果的操作是非法的
- **ReferenceError 同作用域判别失败相关，而 TypeError 则代表作用域判别成功了，但是对 结果的操作是非法或不合理的**



#### 1.5 小结

- **作用域是一套规则，用于确定在何处以及如何查找变量（标识符）。如果查找的目的是对变量进行赋值，那么就会使用LHS查询；如果目的是获取变量的值，就会使用RHS查询。**

  



### 2. 词法作用域

#### 2.1 词法阶段

- 词法阶段：大部分标准语言编译器的第一个工作阶段叫作词法化（也叫单词化）。回忆一下，词法化的过程会对源代码中的字符进行检查，如果是有状态的解析过程，还会赋予单词语义。

- 词法作用域就是定义在词法阶段的作用域，是由写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变

- 词法作用域是在写代码或者定义时确定的，而动态作用域是在运行时确定的（this也是！）。**词法作用域关注函数在何处声明，而动态作用域关注函数从何处调用**

- 简单地说，**词法作用域就是定义在词法阶段的作用域**。换句话说，**词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的**，因此当词法分析器处理代码时会保持作用域不变

  

#### 2.2 欺骗词法

- eval

  - JavaScript中的eval(…)函数可以接受一个字符串为参数，并将其中的内容视为好像在书写时就存在于程序中这个位置的代码

  - 例子：

    ```js
    function foo(str, a) {
    	eval(str); // 欺骗
        console.log(a,b);
    }
    
    var b = 2;
    foo("val b =3", 1); // 1 3
    
    //eval(..) 调用中的 "var b = 3;" 这段代码会被当作本来就在那里一样来处理
    ```

  - **在严格模式的程序中，eval(…)在运行时有其自己的词法作用域，意味着其中的声明无法修改所在的作用域。（在严格模式中eval不可用）**

- with

  - with通常被当作重复引用同一个对象中的多个属性的快捷方式，可以不需要重复引用对象本身

  - 例子：

    ```js
    var obj = {
        a: 1,
        b: 2,
        c: 3
    };
    // 重复obj
    obj.a = 2;
    // 快捷方式
    with(obj) {
        a = 3;
        b = 4;
        c = 5;
    }
    ```

  - *eval(…)函数如果接受了含有一个或多个声明的代码，就会修改其所处的词法作用域，而with声明实际上是根据你传递给它的对象凭空创建了一个全新的词法作用域*

  

  #### 2.3 小结

  - 词法作用域意味着作用域是由书写代码时函数声明的位置来决定的。编译的词法分析阶段 基本能够知道全部标识符在哪里以及是如何声明的，从而能够预测在执行过程中如何对它 们进行查找。
  - 欺骗词法的副作用是**引擎无法在编译时对作用域查找进行优化，因为引擎只能谨慎地认 为这样的优化是无效的**。使用这其中任何一个机制都将导致代码运行变慢。**不要使用它们**。



### 3.函数作用域和块作用域

#### 3.1 函数作用域

- 在任意代码片段外部添加包装函数，可以将内部的变量和函数定义“隐藏”起来，外部作用域无法访问包装函数内部的任何内容

- 匿名与具名

  - 匿名函数

    - 如：

      ```js
      setTimeout( function() { 
       console.log("I waited 1 second!"); 
      }, 1000 );
      ```

    - 这叫作匿名函数表达式，因为 function().. 没有名称标识符

  - 具名函数

    - 如：

      ```js
      setTimeout( function timeoutHandler() { // <-- 快看，我有名字了！ 
       console.log( "I waited 1 second!" ); 
      }, 1000 );
      ```

    - 行内函数表达式非常强大且有用——匿名和具名之间的区别并不会对这点有任何影响。

- 立即执行函数表达式

  - IIFE，代表立即执行函数表达式 （Immediately Invoked Function Expression）

  - 例子：

    ```js
    var a = 2; 
     
    (function foo() { 
     
     var a = 3; 
     console.log( a ); // 3 
     
    })(); 
     
    console.log( a ); // 2
    ```

  - 由于函数被包含在一对 ( ) 括号内部，因此成为了一个表达式，通过在末尾加上另外一个( ) 可以立即执行这个函数，比如 `(function foo(){ .. })()`

  - 第一个 ( ) 将函数变成表 达式，第二个 ( ) 执行了这个函数

  - 第二种形式：`(function(){ .. }())`

 

#### 3.2 块作用域

- 块作用域是一个用来对之前的最小授权原则进行扩展的工具，将代码从在函数中隐藏信息扩展为在块中隐藏信息

- 具有块级作用域

  - with

  - try/catch

    - JavaScript的ES3规范中规定try/catch的catch分句会创建一个块作用域，其中声明的变量仅在catch内部有效

  - let

    - let关键字可以将变量绑定到所在的任意作用域中（通常是{ … }内部）。换句话说，let为其声明的变量隐式地劫持了所在的块作用域。使用let进行的声明不会在块作用域中进行提升。声明的代码被运行之前，声明并不“存在”

      ```js
      {
          console.log(bar); // ReferenceError
          let bar = 2;
      }
      ```

  - const

    - 除了 let 以外，ES6 还引入了 const，同样可以用来创建块作用域变量，但其值是固定的 （常量）。之后任何试图修改值的操作都会引起错误。

  



### 4.提升

#### 4.1 什么是提升

- 回顾关于编译器的内容：引擎会在解释Javascript代码之前进行编译，将找到所有的声明并用合适的作用域将它们关联起来。

  因此, 包括变量和函数声明都会在任何代码被执行之前首先被处理。

  **这个过程就好像变量和函数声明从它们的代码中出现的位置被“移动”到了最上面。这个过程叫做提升。**

- **一个普通块内部的函数声明通常会被提升到所在作用域的顶部**

  - 当你看到var a = 2；时，可能会认为这是一个声明。但JavaScript实际上会将其看成两个声明：var a；和a = 2;。第一个定义声明是在编译阶段进行的。第二个赋值声明会被留在原地等待执行阶段

- **函数声明会被提升，函数表达式不会被提升**

  ```js
  foo(); // TypeError
  
  var foo = function bar() {
  }
  ```

- **只有声明本身会被提升，而赋值或其他运行逻辑会留在原地**。如果提升改变了代码执行的顺序，会造成非常严重的破坏

  ```js
  foo();
  
  function foo() {
  	console.log(a); // undefined
      var a = 2;
  }
  
  // 相当于
  
  function foo() {
  	var a；
      console.log(a); // undefined    
      a = 2;
  }
  
  foo();
  ```



#### 4.2 提升顺序

- <u>**函数会首先被提升，然后才是变量。**</u>

  ```js
  foo();
  var foo;
  function foo() {
  	console.log(1);
  }
  foo = function() {
      console.log(2);
  }
  
  // 相当于
  function foo() {
  	console.log(1);
  }
  foo(); // 1
  foo = function() {
      console.log(2);
  }
  ```

  



### 5.作用域闭包

#### 5.1 什么是闭包

- 闭包是基于词法作用域书写代码时所产生的自然结果。

  **当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数实在当前词法作用域之外执行。**

- 例子：

  ```js
  function foo() {
    var a = 2;
    function bar() {
     console.log(a)
    }
    return bar;
  }
  
  var baz = foo();
  baz(); // 2 --这就是闭包的效果
  
  //bar依然保持对这个作用域的引用，这个引用就叫做闭包
  ```

- 定时器的回调函数也是闭包

  - ```js
    function wait(message) { 
     
     setTimeout( function timer() { 
     console.log( message ); 
     }, 1000 ); 
     
    } 
     
    wait( "Hello, closure!" );
    ```

  - 将一个内部函数（名为 timer）传递给 setTimeout(..)。timer 具有涵盖 wait(..) 作用域 的闭包，因此还保有对变量 message 的引用

  - wait(..) 执行 1000 毫秒后，它的内部作用域并不会消失，timer 函数依然保有 wait(..)作用域的闭包

  - **只要使 用了回调函数，实际上就是在使用闭包**

- 循环和闭包：

  - ```js
    for(var i=1; i<=5; i++) {
     setTime(function timer(){
        console.log(i);
     }, i*1000)
    }
    ```

    - 以上代码： 我们的期望是：分别输出1-5，每秒一次，每次一个

    - 实际：每秒输出一个6

  - 方法1：

    - 使用IIFE的方式，每次迭代中创建的作用域封闭起来

      ```js
      for(var i=1; i<=5;i++) {
        (function(j){
         setTimeout(function timer() {
          console.log(j)
         }, j*1000)
        })(i)
      }
      ```

  - 方法2：

    - 块级作用域与闭包联合使用

      ```js
      for(let i=1;i<=5;i++) {
        setTimeout(function timer(){
         console.log(i);
        }, i*1000)
      }
      ```

    - for 循环头部的 let 声明还会有一 个特殊的行为。这个行为指出变量在循环过程中不止被声明一次，每次迭代都会声明。随 后的每个迭代都会使用上一个迭代结束时的值来初始化这个变量

  

#### 5.2 模块

- 如：

  - ```js
    function CoolModule() { 
     var something = "cool"; 
     var another = [1, 2, 3]; 
     
     function doSomething() { 
     console.log( something ); 
     } 
     
     function doAnother() { 
     console.log( another.join( " ! " ) ); 
     } 
     
     return { 
     doSomething: doSomething, 
     doAnother: doAnother 
     }; 
    } 
     
    
    var foo = CoolModule(); 
     
    foo.doSomething(); // cool 
    foo.doAnother(); // 1 ! 2 ! 3
    ```

  - 这个模式在 JavaScript 中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露

  - 形成条件：

    1. CoolModule() 只是一个函数，必须要通过调用它来创建一个模块实例。

       

- **<u>模式模块需要具备的两个条件：</u>**

  1. 必须有外部的封闭函数，该函数必须至少被调用一次。（每次调用都会创建一个新的模块实例）
  2. 封闭函数必须返回至少一个内部函数，这样函数才能在私有作用与中形成闭包，并且可以访问或者改变私有的状态。

- 当只需要一个实例的时候，可以对这个模式进行简单的改进来实现**单例模式**。

  - ```js
    var foo = (function CoolModule(){
      var something = "cool";
      var anthor = [1,2,3];
      function doSomething() {
       console.log(something)
      }
      function doAnther() {
       console.log(another.join("!"))
      }
      
      return {
       doSomthing,
       doAnother,
      }
    })();
    foo.doSomthing();
    foo.doAnother();
    ```

- 模块模式的另一个简单但强大的用法是 命名将要作为公共API返回的对象。



#### 5.3 小结

- 什么是闭包？
  - *闭包就是能够读取其他函数内部变量的函数*
- 闭包有什么作用？
  - 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。
  - 函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。
- 模块的两个主要特征
  1. 必须有外部的封闭函数，该函数必须至少被调用一次。（每次调用都会创建一个新的模块实例）
  2. 封闭函数必须返回至少一个内部函数，这样函数才能在私有作用与中形成闭包，并且可以访问或者改变私有的状态。



### 6.附录

#### 6.1 显式作用域

- 下面这种 let 的使用方法，它被称作 let 作用域或 let 声明

  ```js
  let (a = 2) { 
   console.log( a ); // 2 
  } 
   
  console.log( a ); // ReferenceError
  ```

  - 同隐式地劫持一个已经存在的作用域不同，let 声明会创建一个**显示的作用域**并与其进行绑定











## 二.this和对象原型

### 1.this

#### 1.1 什么是this

- this提供了一种更优雅的方式来隐式“传递”一个对象引用，因此可以将API设计得更加简洁并且易于复用

- this的上下文取决于**函数调用**时的各种条件，**<u>this的绑定</u>**和**函数声明的位置**没有任何关系，<u>只取决于函数的**调用方式**</u>

- 当一个函数被调用时，会创建一个活动记录（执行上下文），这个记录会包含函数的调用位置（函数调用栈），函数的调用调用方式和传入的参数。this 就是这个记录的一个属性，会在函数执行的过程中用到。

  

#### 1.2 this的绑定规则

##### 1.2.1 默认绑定

- 如果直接调用函数，this指向全局对象
- 如果把null或undefined作为this的绑定对象传入call或applu，这些值在调用时会被忽略。实际应用默认绑定规则
- **如果使用严格模式（strict mode），则不能将全局对象用于默认绑定，因此this会绑定到undefined**



##### 1.2.2 隐式绑定

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

  

##### 1.2.3 显式绑定

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



##### 1.2.4 new绑定

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



##### 1.2.5 优先级

-  **<u>new绑定 > 显式绑定 > 隐式绑定 > 默认绑定</u>**
- 判断this，现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的 顺序来进行判断：
  1. 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。`var bar = new foo()`
  2. .函数是否通过 call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是 指定的对象。`var bar = foo.call(obj2)`
  3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上 下文对象。`var bar = obj1.foo()`
  4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定 到全局对象。`var bar = foo()`





#### 1.3 绑定例外

##### 1.3.1 被忽略的this

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



##### 1.3.2 软绑定

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



##### 1.3.3 this词法

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

    





### 2.对象

#### 2.1 语法

- 对象可以通过两种形式定义：声明（文字）形式和构造形式

- 声明形式：

  ```js
  var myObj = { 
   key: value 
   // ... 
  };
  ```

- 构造形式：

  ```js
  var myObj = new Object(); 
  myObj.key = value;
  ```

- 构造形式和文字形式生成的对象是一样的。唯一的区别是，在文字声明中你可以添加多个 键 / 值对，但是在构造形式中你必须逐个添加属性



#### 2.2 类型

- 在 JavaScript 中一共有六种主要类型（术语是“语言类型”）：

  -  string

  -  number

  - boolean

  - null

  - undefined

  - object

  - 注意，简单基本类型（string、boolean、number、null 和 undefined）本身并不是对象。

    **null 有时会被当作一种对象类型，但是这其实只是语言本身的一个 bug**，即对 null 执行

    typeof null 时会返回字符串 "object"。实际上，null 本身是基本类型。

- 内置对象

  - String

  - Number

  - Boolean

  - Object

  - Function

  - Array

  - Date

  - RegExp

  - Error

  - 这些内置对象从表现形式来说很像其他语言中的类型（type）或者类（class），比如 Java中的 String 类。

  - 但是在 JavaScript 中，它们实际上只是一些内置函数。这些内置函数可以当作构造函数 来使用，从而可以构造一个对应子类型的新对象。

    

#### 2.3 内容

##### 2.3.1 属性引用

- 对象的内容是由一些存储在特定命名位置的（任意类型的）值组成的， 我们称之为属性。

- 如：

  - ```js
    var myObject = { 
     a: 2 
    }; 
     
    myObject.a; // 2 
     
    myObject["a"]; // 2
    ```

  - 如果要访问 myObject 中 a 位置上的值，我们需要使用 . 操作符或者 [] 操作符。

  - .a 语法通 常被称为“属性访问”

  - ["a"] 语法通常被称为“键访问”

- **这两种语法的主要区别在于 . 操作符要求属性名满足标识符的命名规范，而 [".."] 语法 可以接受任意 UTF-8/Unicode 字符串作为属性名。**举例来说，如果要引用名称为 "SuperFun!" 的属性，那就必须使用 ["Super-Fun!"] 语法访问，因为 Super-Fun! 并不是一个有效 的标识符属性名。

- **在对象中，属性名永远都是字符串。**



##### 2.3.2 可计算属性名

- 如果需要通过表达式来计算属性名，那么我们刚刚讲到的 myObject[..] 这种属性访问语法就可以派上用场了，如**可以使用 myObject[prefix + name]**。但是使用文字形式来声明对 象时这样做是不行的。

- 例子

  ```js
  var prefix = "foo"; 
   
  
  var myObject = { 
   [prefix + "bar"]:"hello", 
   [prefix + "baz"]: "world" 
  }; 
   
  myObject["foobar"]; // hello 
  myObject["foobaz"]; // world
  ```

  

##### 2.3.3 数组

- 数组也支持 [] 访问形式，但是**数组期望的是数值下标**，也就是说值存储的位置（通 常被称为索引）是非负整数，比如说 0 和 42：

  ```js
  var myArray = [ "foo", 42, "bar" ]; 
   
  myArray.length; // 3 
   
  myArray[0]; // "foo" 
   
  myArray[2]; // "bar"
  ```

- **数组也是对象**，所以虽然每个下标都是整数，你仍然可以给数组添加属性：

  ```js
  var myArray = [ "foo", 42, "bar" ]; 
   
  myArray.baz = "baz"; 
   
  myArray.length; // 3 
   
  myArray.baz; // "baz"
  //虽然添加了命名属性（无论是通过 . 语法还是 [] 语法），数组的 length 值并未发生变化
  ```

- 注意：**如果你试图向数组添加一个属性，但是属性名“看起来”像一个数字，那它会变成 一个数值下标（因此会修改数组的内容而不是添加一个属性）**：

  ```js
  var myArray = [ "foo", 42, "bar" ]; 
   
  myArray["3"] = "baz"; 
   
  myArray.length; // 4 
   
  myArray[3]; // "baz"
  ```

  

##### 2.2.4 复制对象

- **ES6定义了`Object.assign(…)`方法来实现浅复制**。Object.assign(…)方法的第一个参数是目标对象，之后还可以跟一个或多个源对象。它会遍历一个或多个源对象的所有可枚举（enumerable，参见下面的代码）的自有键（owned key，很快会介绍）并把它们复制**（使用=操作符赋值）**到目标对象

- ```js
  var newObj = Object,assign({},myObject);
  ```

- **注意：由于 Object.assign(..) 就是使用 = 操作符来赋值，所 以源对象属性的一些特性（比如 writable）不会被复制到目标对象。**



##### 2.2.5 属性描述符

- 从 ES5 开始，所有的属性都具备了属性描述符

  - 如

    ```js
    ar myObject = { 
     a:2 
    }; 
     
    Object.getOwnPropertyDescriptor( myObject, "a" ); 
    // { 
    // value: 2, 
    // writable: true, 
    // enumerable: true, 
    // configurable: true 
    // }
    ```

  - 这个普通的对象属性对应的属性描述符（也被称为“数据描述符”，因为它 只保存一个数据值）可不仅仅只是一个 2。它还包含另外三个特性：writable（可写）、enumerable（可枚举）和 configurable（可配置）

- **Object.defineProperty(…)来添加一个新属性或者修改一个已有属性（如果它是configurable）并对特性进行设置。**

  - writable决定是否可以修改值
  - Configurable决定是否可以使用defineProperty方法来修改属性描述符（把configurable修改成false是单向操作，无法撤销），configurable:false还会禁止删除
    - 注意有一个小小的例外：即便属性是 conﬁgurable:false， 我们还是可以 把 writable 的状态由 true 改为 false，但是无法由 false 改为 true
  - Enumerable决定属性是否可枚举（是否出现在for…in循环中）
  - 对象的不变性：使用writable:false和configurable:false创建常量属性； 使用Object.prevent Extensions()来禁止扩展； Object.seal(…)密封对象（密封后不能添加新属性，也不能重新配置或删除现有属性，前两者的结合）； Object.freeze(…)冻结对象（seal+writable:false）

  

#####  2.2.6 Getter和Setter以及存在性

- getter 是一个隐藏函数，会在获取属性值时调用。

- setter 也是一个隐藏 函数，会在设置属性值时调用。

- 存在性

  - 如 myObject.a 的属性访问返回值可能是 undefined，但是这个值有可能 是属性中存储的 undefined，也可能是因为属性不存在所以返回 undefined。那么如何区分 这两种情况呢？

  - 可以在不访问属性值的情况下判断对象中是否存在这个属性：

    ```js
    var myObject = { 
     a:2 
    }; 
     
    ("a" in myObject); // true 
    ("b" in myObject); // false 
     
    myObject.hasOwnProperty( "a" ); // true 
    myObject.hasOwnProperty( "b" ); // false
    ```

  - **in操作符会检查属性是否在对象及其[[Prototype]]原型链中**（参见第5章）。相比之下，**hasOwnProperty(…)只会检查属性是否在myObject对象中**，不会检查[[Prototype]]链。

  - 看起来in操作符可以检查容器内是否有某个值，但是**它实际上检查的是某个属性名是否存在**。对于数组来说这个区别非常重要，**4in [2, 4, 6]的结果并不是你期待的True**，**因为[2, 4, 6]这个数组中包含的属性名是0、1、2，没有4**。

  - **访问属性时，引擎实际上会调用内部的默认[[Get]]操作（在设置属性值时是[[Put]]）, [[Get]]操作会检查对象本身是否包含这个属性，如果没找到的话还会查找[[Prototype]]链**

    

##### 2.2.7 遍历

- forEach(..) 会遍历数组中的所有值并忽略回调函数的返回值。

- every(..) 会一直运行直到回调函数返回 false（或者“假”值）

- some(..) 会一直运行直到回调函数返回 true（或者 “真”值）。

- **every(..) 和 some(..) 中特殊的返回值和普通 for 循环中的 break 语句类似，它们会提前 终止遍历。**

- 使用 for..in 遍历对象是无法直接获取属性值的，因为它实际上遍历的是对象中的所有可 枚举属性，你需要手动获取属性值

- ES6 增加了一种用来遍 历数组的 for..of 循环语法（如果对象本身定义了迭代器的话也可以遍历对象）

  - for..of 循环首先会向被访问对象请求一个迭代器对象，然后通过调用迭代器对象的next() 方法来遍历所有返回值。
  - 数组有内置的 @@iterator，因此 for..of 可以直接应用在数组上。

  





### 3.混合对象"类"

####  3.1什么是类

- **类是一种设计模式**。许多语言提供了对于面向类软件设计的原生语法。JavaScript 也有类似的语法，但是和其他语言中的类完全不同

- **构造函数**：类实例是由一个特殊的类方法构造的，这个方法名通常和类名相同，被称为构造函数。这 个方法的任务就是初始化实例需要的所有信息（状态）。

- **类构造函数属于类**，而且通常和类同名。此外，构造函数大多需要用 new 来调，这样语言 引擎才知道你想要构造一个新的类实例

- **类的继承**

  - 在面向类的语言中，你可以先定义一个类，然后定义一个继承前者的类。后者通常被称为“子类”，前者通常被称为“父类”

  

#### 3.2  混入(继承)

##### 3.2.1 显式混入

- 现在我们来分析一下mixin(…)的工作原理。**它会遍历sourceObj（本例中是Vehicle）的属性，如果在targetObj（本例中是Car）没有这个属性就会进行复制**

  ```js
  function mixin(sourceObj, targetObj) {
      for(var key in sourceObj) {
          // 只会在不存在的情况下复制，子类对父类属性的重写
          if(! (key in target)) {
              tergetObj[key] = sourceObj[key];
          }
      }
      return targetObj;
  }
  ```

  - 例子：

    ```js
    var Vehicle = { 
     engines: 1, 
     
     ignition: function() { 
     console.log( "Turning on my engine." ); 
     }, 
     
     drive: function() { 
     this.ignition(); 
     console.log( "Steering and moving forward!" ); 
     } 
    }; 
     
    
    var Car = mixin( Vehicle, { 
     wheels: 4, 
     
     drive: function() { 
     Vehicle.drive.call( this ); 
     console.log( 
     "Rolling on all " + this.wheels + " wheels!" 
     ); 
     } 
    } );
    ```

    - 现在 **Car 中就有了一份 Vehicle 属性和函数的副本**了。
    - **因为Car 已经有了 drive 属性**（函数），所以**这个属性引用并没有被 mixin 重写**，从而**保留了Car 中定义的同名属性**，实现了“子类”对“父类”属性的重写

- 寄生继承

  - 显式混入模式的一种变体被称为“寄生继承”，它既是显式的又是隐式的

  - ```js
    // “传统的 JavaScript 类”Vehicle
    function Vehicle() {
    	this.engines = 1;
    }
    Vehicle.prototype.igition = function() {};
    Vehicle.prototype.drive = function() {};
    
    // 寄生类 Car
    function Car() {
      var car = new Vihicle();
      car.wheels = 4;
      var vehDrive = car.drive;
      car.drive = funciton() {
    	vehDrive.call(this);
      };
      return car;
    }
    ```

  - 首先我们复制一份 Vehicle 父类（对象）的定义，然后混入子类（对象）的定 义（如果需要的话保留到父类的特殊引用），然后用这个复合对象构建实例

    

##### 3.2.2 隐式混入

- 通过在构造函数调用或者方法调用中使用Something.cool.call(this)，我们实际上“借用”了函数Something.cool()并在Another的上下文中调用了它（通过this绑定；参见第2章）。最终的结果是Something.cool()中的赋值操作都会应用在Another对象上而不是Something对象上

- ```js
  var Something = {
      cool: function() {
  		this.greeting = "Hello World";
           this.count = this.count ? this.count+1 : 1;
      }
  }
  
  Something.cool();
  
  var Another = {
      cool: function() {
  		// 隐式把Something混入Another
          Something.cool.call(this);
      }
  }
  Another.cool();
  ```

  





### 4.原型

#### 4.1 什么是原型

- **所有普通对象都有内置的 Object.prototype，指向原型链的顶端（比如说全局作用域），如 果在原型链中找不到指定的属性就会停止。**

- 使用for…in遍历对象时原理和查找[[Prototype]]链类似，任何可以通过原型链访问到（并且是enumerable，参见第3章）的属性都会被枚举。**使用in操作符来检查属性在对象中是否存在时，同样会查找对象的整条原型链（无论属性是否可枚举）**

- **所有普通的[[Prototype]]链最终都会指向内置的Object.prototype**

- **属性设置和屏蔽**

  - ```js
    myObject.foo = "bar";
    ```

  - foo直接存在于myObject中，直接赋值

  - 在原型链上存在foo，直接在myObject中添加一个名为foo的新属性，它是屏蔽属性

  - 若原型链上的foo被标记为只读(writable:false),严格模式下报错，否则这条语句会被忽略

  - 若原型链上的foo是一个setter,那就一定会调用这个setter,不会屏蔽

  - d

  

#### 4.2 构造函数

- 换句话说，**在JavaScript中对于“构造函数”最准确的解释是，所有带new的函数调用**。函数不是构造函数，但是当且仅当使用new时，函数调用会变成“构造函数调用”

- **constructor并不表示被构造**

  ```js
  function Foo() {}
  Foo.prototype = {}
  
  var a = new Foo();
  a.constructor === Foo; // fasle
  a.constructor === Object; // true
  ```

  - a并没有.constructor属性，所以它会委托[[Prototype]]链上的Foo.prototype。但是这个对象也没有．constructor属性（不过默认的Foo.prototype对象有这个属性！），所以它会继续委托，这次会委托给委托链顶端的Object.prototype。这个对象有.constructor属性，指向内置的Object(…)函数

- **调用Object.create(…)会凭空创建一个“新”对象并把新对象内部的[[Prototype]]关联到你指定的对象**

  ```js
  if(! Object.create) {
      Object.create = function(o) {
          function F() {}
          F.prototype = o;
          return new F();
      }
  }
  ```

- 检查一个实例（JavaScript中的对象）的继承祖先（JavaScript中的委托关联）通常被称为内省（或者反射）

  - 检查`a instanceof Foo`，结果是`true`

    - instanceof操作符的左操作数是一个普通的对象a，右操作数是一个函数Foo()。instanceof回答的问题是：**在a的整条[[Prototype]]链中是否有指向Foo.prototype的对象**

  - 第二种判断 [[Prototype]] 反射的方法：

    - ```js
      Foo.prototype.isPrototypeOf( a ); // true
      ```

    - 在本例中，我们实际上并不关心（甚至不需要）Foo，我们只需要一个可以用来判断的对象（本例中是 Foo.prototype）就行。isPrototypeOf(..) 回答的问题是：**在 a 的整 条 [[Prototype]] 链中是否出现过 Foo.prototype**



#### 4.3 对象关联

- ```js
  var foo = { 
   something: function() { 
   console.log( "Tell me something good..." ); 
   } 
  }; 
   
  
  var bar = Object.create( foo ); 
   
  bar.something(); // Tell me something good...
  
  //Object.create(..) 会创建一个新对象（bar）并把它关联到我们指定的对象（foo），这样 我们就可以充分发挥 [[Prototype]] 机制的威力（委托）并且避免不必要的麻烦（比如使用 new 的构造函数调用会生成 .prototype 和 .constructor 引用）
  ```

  - Object.create(null)会创建一个拥有空（或者说null）[[Prototype]]链接的对象，这个对象无法进行委托。由于这个对象没有原型链，所以instanceof操作符（之前解释过）无法进行判断，因此总是会返回false。这些特殊的空[[Prototype]]对象通常被称作“字典”，它们完全不会受到原型链的干扰，因此非常适合用来存储数据。

  



### 5.行为委托

#### 5.1 两种设计模式

- “类”设计模式

  - 面向对象三大特性：【封装】、【继承】、【多态】。

    1. 所谓封装，即把客观事物封装成抽象的类。
    2. 所谓继承，即子类继承父类的能力。
    3. 所谓多态，即子类可以用更特殊的行为重写所继承父类的通用行为。

  - **【类】描述了一种代码的组织结构形式，它是软件中对真实世界中问题领域的建模方法。**

  - 例子：

    - 就好比我们现实中修房子，你要修一个“写字楼”、或者一个“居民楼”、或者一个“商场”，你就得分别找到修“写字楼”、“居民楼”、“商场”的【设计蓝图】。

      但是设计蓝图只是一个建筑计划，并不是真正的建筑。要想在真实世界实现这个建筑，就得由建筑工人将设计蓝图的各类特性（比如长宽高、功能）【复制】到现实世界来。

      **这里的【设计蓝图】就是【类】，【复制】的过程就是【实例化】，【实例】就是【对象】**。

  - **“类的设计模式”** 意味着对【设计蓝图】的【复制】，在 JS 各种函数调用的场景下基本看不到它的痕迹。

- “原型”设计模式

  - JS 也是能做到【继承】和【多态】的！只不过它不是通过类**复制**的方式，而是通过原型链**委托**的方式！
  - ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1675f6c23cca4593b1418fdbd18b459f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)
    - 注意：
      - **一个对象的显示原型的构造函数指向对象本身**
      - **一个对象的隐式原型指向构造这个对象的函数的显示原型**
  - **JS 不是通过在类里面写同名构造函数的方式来进一步实现的实例化，它的构造函数在原型上**
  - **<u>JS new 操作会发生什么？</u>**
    - JS 访问一个对象的属性或方法的时候，先在对象本身中查找，如果找不到，则到原型中查找，如果还是找不到，则进一步在原型的原型中查找，一直到原型链的最末端。复制不是它所做的，这种查找的方式才是！对象之间的关系更像是一种委托关系，就像找东西，你在我这找不到？就到有委托关系的其它人那里找找看，再找不到，就到委托委托关系的人那里找......直至尽头，最后还找不到，指向 null。
  - 所以：JavaScript 和面向对象的语言不同，它并没有类来作为对象的抽象模式或者设计蓝图。JavaScript 中只有对象，对象直接定义自己的行为。对象之间的关系是委托关系，这是一种极其强大的设计模式。**在你的脑海中对象并不是按照父类到子类的关系垂直组织的，而是通过任意方向的委托关联并排组织的！**

- 小结：

  - “类设计模式”的构造函数挂在同名的类里面，类的继承意味着复制，多态意味着复制 + 自定义。
  - “原型设计模式”的构造函数挂在原型上，原型的查找是一种自下而上的委托关系。
  - “类设计模式”的类定义之后就不支持修改。
  - “原型设计模式”讲究的是一种动态性，任何对象的定义都可以修改，这和 JavaScript 作为脚本语言所需的动态十分契合！

  

#### 5.2 委托理论

- ```js
  Task = { 
   setID: function(ID) { this.id = ID; }, 
   outputID: function() { console.log( this.id ); } 
  }; 
   
  // 让 XYZ 委托 Task 
  XYZ = Object.create( Task ); 
   
  XYZ.prepareTask = function(ID,Label) { 
   this.setID( ID ); 
   this.label = Label; 
  }; 
   
  XYZ.outputTaskDetails = function() { 
   this.outputID(); 
   console.log( this.label ); 
  }; 
   
  // ABC = Object.create( Task ); 
  // ABC ... = ...
  
  //在这段代码中，Task 和 XYZ 并不是类（或者函数），它们是对象。XYZ 通过Object.
  //create(..) 创建，它的 [[Prototype]] 委托了 Task 对象
  ```

- **委托行为意味着某些对象（XYZ）在找不到属性或者方法引用时会把这个请求委托给另一 个对象（Task）**

- **行为委托认为对象之间是兄弟关系，互相委托，而不是父类和子类的关系。**

- JavaScript 的[[Prototype]] 机制本质上就是行为委托机制。

