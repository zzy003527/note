## 分享

- let有没有变量提升？

  - 之前认为let没有变量提升，因为这个例子，在var的时候是undefined，但是var换成let就报错了

    ```js
    function fn(){
        console.log(a) // undefined
        let a = 12
    }
    fn()
    
    //但是现在看来，这个例子只能说明var有变量提升，但不能说明let没有变量提升
    ```
  
- 一个变量被声明发生了什么
  
  - 先**创建变量**，然后**初始化变量**，然后对**变量赋值**
  
  - var
  
    - ```js
        // 例2：
        function fn(){
          var x = 1
          var y = 2
        }
        fn()
        
        // 进入 fn，为 fn 创建一个环境。
        //找到 fn 中所有用 var 声明的变量，在这个环境中「创建」这些变量（即 x 和 y）。
        //将这些变量「初始化」为 undefined。
        //开始执行代码
        //x = 1 将 x 变量「赋值」为 1
        //y = 2 将 y 变量「赋值」为 2
        ```
  
    - 所以var的变量提升是经历了**创建变量和初始化变量**，所以例1的时候a会输出undefined
  
  - 函数function
  
    - ```js
        fn2()
        
        function fn2(){
          console.log(2)
        }
        
        //找到所有用 function 声明的变量，在环境中「创建」这些变量。
        //将这些变量「初始化」并「赋值」为 function(){ console.log(2) }。
        //开始执行代码 fn2()
        ```
  
    - 由此看来，函数的变量提升是经历了**创建变量，初始化变量和变量赋值**的
  
  - let
  
    - ```js
        {
          let x = 1
          x = 2
        }
        
        //找到所有用 let 声明的变量，在环境中「创建」这些变量
        //开始执行代码（注意现在还没有初始化）
        //执行 x = 1，将 x 「初始化」为 1（这并不是一次赋值，如果代码是 let x，就将 x 初始化为 undefined）
        //执行 x = 2，对 x 进行「赋值」
        ```
  
    - 由此可见，let是有变量提升的。
  
    - 但是let的变量提升只经历了**创建变量**的阶段，并没有初始化，因此在初始化之前使用let出来的变量会报错
  
  - 总结：
  
    - let 的`创建`过程被提升了，但是初始化没有提升。
      - var 的`创建`和`初始化`都被提升了。
      - function 的`创建、初始化和赋值`都被提升了。
      - const只有创建阶段和初始化阶段，没有赋值阶段（const不能重新赋值）
  
  

#### 解构赋值

- 解构赋值的默认值

  - 解构赋值允许赋值默认值，但是如果对有默认值的变量赋值，那么最后变量的值是默认值还是赋给的值呢？

  - 如：

    ```js
    let [a,b = 2] = [1]                    // 这样子就是有默认值，a=1，b=2
     
    let [a,b = 2] = [1,undefined]           //这样子同上，a=1，b=2
    
    let [a,b = 2] = [1,null]               //这样子不同了，因为null不严格等于undefined，所以是a=1，b=null
    ```

  - 因此，只有在后面赋给的值**严格等于undefined或为空时**，默认值才会生效（对象解构赋值和数组解构赋值都一样）

- 对象的解构赋值

  - 对象的解构赋值是怎么匹配的，需要按照次序排列吗？

    ```js
    let {foo,bar} = { bar:'aaa', foo:'bbb' }              //此处的foo和bar都是变量，此处用变量匹配属性
    // foo是bbb，bar是aaa
    // 说明对象的解构赋值不需要按照次序排列
    
    let {foo,baar} = { bar:'aaa', foo:'bbb' }
    // foo是bbb，而baar是undefined，因为baar没有对应的同名属性
    // 说明对象的解构赋值是按照相应属性名对应赋值的？
    
    // 那如果变量名和属性名不一样，但又要给变量赋对应值，要咋办呢？
    let { foo: foo, bar: baar} = { bar:'aaa', foo:'bbb' }
    //此时foo是bbb，baar是aaa，但是bar是没有被赋值的（它只是用来匹配属性的）
    ```

  - 因此对象的解构赋值是先找到同名的属性，然后再将属性的值赋给变量。变量指的是后者（baar），而不是前者（bar）。

  - 不需要按照次序排列

  

- 函数参数默认值与解构赋值默认值的混合使用

  - ```js
    function foo({x, y = 5}) {
      console.log(x, y);
    }
    
    foo({}) // undefined 5
    foo({x: 1}) // 1 5
    foo({x: 1, y: 2}) // 1 2
    foo() // TypeError: Cannot read property 'x' of undefined
    
    // 上方代码使用了对象的解构赋值默认值，当参数是对象的时候，x和y才会生成，如果没有参数，那么就会报错
    ```

  - 所以，如果要不报错，那就要让<u>函数参数默认值</u>与<u>解构赋值默认值</u>一起使用

    ```js
    function foo({x, y = 5} = {}) {
      console.log(x, y);
    }
    
    foo() // undefined 5
    // 前一个大括号是解构赋值默认值，后一个大括号是函数参数默认值
    // 当没有参数传入时，函数参数默认值是一个空对象{}，然后进入解构赋值默认值，让x为undefined，y为5
    ```

  - **注意：如果函数参数默认值和解构赋值默认值同时存在，那么最终结果是解构赋值默认值**

    ```js
    // 写法一 （这是解构赋值默认值存在，但函数参数默认值不存在的情况）
    function m1({x = 0, y = 0} = {}) {
      return [x, y];
    }
    
    // 写法二  （这是函数参数默认值和解构赋值默认值同时存在的情况）
    function m2({x, y} = { x: 0, y: 0 }) {
      return [x, y];
    }
    
    // 函数没有参数的情况
    m1() // [0, 0]  ，m1的x，y值是解构赋值默认值给的
    m2() // [0, 0]  ，m2的x，y值是函数参数默认值给的
    
    // x 和 y 都有值的情况
    m1({x: 3, y: 8}) // [3, 8]  ，x，y都有值，那么默认值无效，一切以给的值为准
    m2({x: 3, y: 8}) // [3, 8]  ，同上
    
    
    // x 和 y 都无值的情况
    m1({}) // [0, 0];  m1先有函数参数默认值为一个空对象{}，然后解构赋值默认值产生作用，x，y都为0
    m2({}) // [undefined, undefined]  ，m2先是函数参数默认值让x，y为0，然后解构赋值默认值产生作用，让x，y变为undefined
    
    ```

  - 






#### 尾调用优化

- 什么是尾调用？

  - 尾调用是一个 `return` 函数调用的语句，除了调用后返回其返回值之外没有任何其他动作。

  - 这个优化只在 `strict` 模式下应用。

  - 如：

    ```js
    function b () {
        return 1;
    }
    
    //这是一个尾调用
    function a1() {
        return b();
    }
    
    //这不是尾调用
    function a2() {
        b();        //这里看似是尾调用了
        //但实际上这里还藏着一个return undefined
    }
    
    //这也不是尾调用
    function a3() {
        let i = 1;
        return i + b();        //因为再这里除了调用b()之外还使用了a3的变量i
    }
    ```

- 调用栈

  - 程序运行到一个函数，它就会将其添加到调用栈中，当从这个函数返回的时候，就会将这个函数从调用栈中删掉。
  - 每个进入到调用栈中的函数，都会分配到一个单独的栈空间，称为“调用侦”。
  - 当函数嵌套的层级比较深了，调用栈中的调用侦比较多的时候，这些信息对内存消耗是非常大的，很容易出现“爆栈”问题(stack overflow)。
  - 尾调用优化就是用来**删除外层无用的调用侦**，只保留内层函数的调用侦，来节省浏览器的内存，避免爆栈

- 尾调用优化

  - 即只保留内层函数的调用帧

  - 例如递归，递归是非常消耗内存的，因为需要保存很多个调用帧，所以很容易爆栈。但如果使用了尾调用优化（尾递归），那就使保存的调用帧永远只有最内层那一个，避免了爆栈。

  - ```js
    function Fibonacci (n) {
      if ( n <= 1 ) {return 1};
    
      return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
    
    Fibonacci(10) // 89
    Fibonacci(100) // 超时
    Fibonacci(500) // 超时
    //上方是一个递归，当递归层数过多时，就会出现爆栈
    
    //下方是使用尾调用优化后的递归（尾递归）
    function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
      if( n <= 1 ) {return ac2};
      return Fibonacci2 (n - 1, ac2, ac1 + ac2);     //此处最后返回调用Fibonacci2，是尾调用，因此删除了外层的调用帧，只保存了这个return的调用帧，避免爆栈
    }
    
    Fibonacci2(100) // 573147844013817200000
    Fibonacci2(1000) // 7.0330367711422765e+208
    Fibonacci2(10000) // Infinity
    ```

    



#### Promise

- promise对象的3个状态

  - 初始化状态（等待状态）：pending
  - 成功状态：fulfilled
  - 失败状态：rejected
  - **<u>Promise 的状态一旦改变，就永久保持该状态，不会再变了。</u>**

- Promise基本用法

  - **<u>Promise 新建后就会立即执行。(先于同步）</u>**

  - ES6 规定，`Promise`对象是一个构造函数 ，用来生成`Promise`实例。

    下面代码创造了一个`Promise`实例。

    ```js
    const promise = new Promise(function(resolve, reject) {
      // ... some code
    
      if (/* 异步操作成功 */){
        resolve(value);
      } else {
        reject(error);
      }
    });
    ```

  - `Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

  - `resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 fulfilled），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去

  - `reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

    - `reject()`方法的作用，等同于抛出错误。
    - 如果 Promise 状态已经变成`resolved`，再抛出错误是无效的。



- Promise.prototype.then()

  - Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。

  - then方法的第一个参数是resolved状态的回调函数，第二个参数是rejected状态的回调函数，它们都是可选的。

  - `then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

    ```js
    getJSON("/posts.json").then(function(json) {
      return json.post;
    }).then(function(post) {
      // ...
    });
    
    //上面的代码使用then方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
    ```

  - 采用链式的`then`，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个`Promise`对象（即有异步操作），这时后一个回调函数，就会等待该`Promise`对象的状态发生变化，才会被调用。

    ```js
    getJSON("/post/1.json").then(function(post) {
      return getJSON(post.commentURL);
    }).then(function (comments) {
      console.log("resolved: ", comments);
    }, function (err){
      console.log("rejected: ", err);
    });
    
    //上面代码中，第一个then方法指定的回调函数，返回的是另一个Promise对象。这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化。如果变为resolved，就调用第一个回调函数，如果状态变为rejected，就调用第二个回调函数。
    ```





- Promise.prototype.catch()

  - `Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

    ```js
    getJSON('/posts.json').then(function(posts) {
      // ...
    }).catch(function(error) {
      // 处理 getJSON 和 前一个回调函数运行时发生的错误
      console.log('发生错误！', error);
    });
    
    //上面代码中，getJSON()方法返回一个 Promise 对象，如果该对象状态变为resolved，则会调用then()方法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调用catch()方法指定的回调函数，处理这个错误。另外，then()方法指定的回调函数，如果运行中抛出错误，也会被catch()方法捕获。
    ```

  - Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个`catch`语句捕获。

    ```js
    getJSON('/post/1.json').then(function(post) {
      return getJSON(post.commentURL);
    }).then(function(comments) {
      // some code
    }).catch(function(error) {
      // 处理前面三个Promise产生的错误
    });
    
    //上面代码中，一共有三个 Promise 对象：一个由getJSON()产生，两个由then()产生。它们之中任何一个抛出的错误，都会被最后一个catch()捕获。
    ```

  - 一般来说，不要在`then()`方法里面定义 Reject 状态的回调函数（即`then`的第二个参数），总是使用`catch`方法。

  - 理由是使用catch可以捕获前面`then`方法执行中的错误**(捕获前面多个then的错误）**，也更接近同步的写法（`try/catch`）。因此，建议总是使用`catch()`方法，而不使用`then()`方法的第二个参数。



##### 手写promise

- 首先，定义一个类

  ```js
  class MyPromise {}
  ```

- 然后进行初始化

  - 首先是constructor

    ```js
    constructor(executor) {
                    // 初始化值
                    this.initValue()
                    // 初始化this指向
                    this.initBind()
        
                    // 执行传进来的函数
                    // 这里用try...catch是因为在原生promise中如果在then中抛出错误，是会执行reject函数的，所以这里要捕捉错误
                    try {
                        // 执行传进来的函数
                        executor(this.resolve, this.reject)
                    } catch (e) {
                        // 捕捉到错误直接执行reject
                        this.reject(e)
                    }    
                }
    ```

    - executor是promise后面的那个函数
    - 在传入参数时立即在constructor里面调用executor执行传进来的函数，并捕捉错误

  - 接下来是initValue

    ```js
    initValue() {
                    // 初始化值
                    this.result = null // 终值
                    this.state = 'pending' // 状态
                    // 为了promise中的异步，需要等异步完成后再判断执行哪一个回调函数，所以要先把回调函数保存起来
                    this.onFulfilledCallbacks = [] // 保存成功回调
                    this.onRejectedCallbacks = [] // 保存失败回调
                }
    ```

    - result是终值，即resolve或者reject中的值
    - state是当前promise的状态，有pending、fulfilled和rejected
    - onFulfilledCallbacks和onRejectedCallbacks是用来保存回调函数的，是下面进行异步处理用到，下面再讲

  - 最后是initbind

    ```js
    initBind() {
                    // 初始化this
                    this.resolve = this.resolve.bind(this)
                    this.reject = this.reject.bind(this)
                }
    ```

    - 如果没有绑定this，那么在执行resolve或者reject时是在外部环境下执行的，此时会因为外部没有state而报错

- 然后写resolve

  - ```js
    resolve(value) {
                    // state是不可变的
                    if (this.state === 'pending') {
                        // 如果执行resolve，状态变为fulfilled
                        this.state = 'fulfilled'
                        // 终值为传进来的值
                        this.result = value
                        // 当promise的异步结束，就执行保存的成功回调
                        this.onFulfilledCallbacks.forEach(fn => fn());
                    }
                }
    ```

  - 因为state是不可变的，所以在刚开始进入的时候要加一个if判断，如果是pending，那就变化状态

  - 然后将resolve中的值赋给result

  - 最下面那行是回调函数，遍历下面保存的回调函数数组，在promise的异步结束之后，执行所有回调

- 然后是reject

  - ```js
      reject(reason) {
                    // state是不可变的
                    if (this.state === 'pending') {
                        // 如果执行reject，状态变为rejected
                        this.state = 'rejected'                   
                        // 终值为传进来的reason
                        this.result = reason
                        // 当promise的异步结束，就执行保存的成功回调
                        this.onRejectedCallbacks.forEach(fn => fn());
                    }
                }
    ```

  - 同上

- 然后是then

  - ```js
    then(onFulfilled,onRejected) {
                    // 为了链式调用，返回一个promise实例
                    let thenPromise =  new MyPromise((resolve,reject) => {
                         // 判断传入参数是不是函数
                        onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : () => {}
                        onRejected = typeof onRejected === 'function' ? onRejected : () => {}
    
                        if (this.state === 'fulfilled') {
                            // 为什么要setTimeout?
                            // 因为如果setTimeout是异步操作,如果没有setTimeout,那么下方的是同步代码,而此时thenPromise还没有完全生成,导致resolvePromise不执行
                            setTimeout(() => {
                                // 如果当前为成功状态，执行第一个回调
                                // 用x接收第一个回调的返回值
                                try {
                                    let x = onFulfilled(this.result)
                                    resolvePromise(x,resolve,reject,thenPromise)
                                } catch(e) {
                                    reject(e)
                                }
                            })                     
                            } else if (this.state === 'rejected') {
                                setTimeout(() => {
                                    try {
                                        // 如果当前为失败状态，执行第二个回调
                                        let x = onRejected(this.result)
                                        resolvePromise(x,resolve,reject,thenPromise)
                                    } catch(e) {
                                        reject(e)
                                    }
                                })
                            }  else if (this.state === 'pending') {
                                // 如果状态为待定状态(promise还在等待执行)，暂时保存两个回调
                                // onFulfilled传入到成功数组
                                this.onFulfilledCallbacks.push(() => {
                                    //异步
                                    setTimeout(() => {
                                        try {
                                            let x = onFulfilled(this.result);
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch (e) {
                                            reject(e)
                                        }
                                    })
                                })
                                // onRejected 传入到失败数组
                                this.onRejectedCallbacks.push(() => {
                                    // 异步
                                    setTimeout(() => {
                                        try {
                                            let x = onRejected(this.result);
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch (e) {
                                            reject(e)
                                        }
                                    })
                                })
                            }
                    })
    
                    return thenPromise
                }
    ```
    
  - 基础功能：
  
    - then函数有两个参数
      - 第一个参数onFulfilled，是promise状态为fulfilled时执行的回调函数
      - 第二个参数onRejected，是promise状态为rejected时执行的回调函数
    - 注意：在刚进入then时，要判断传入的参数onFulfilled和onRejected是否为函数，如果不是，则返回一个空函数
    - 进入then之后，通过判断当前promise的状态，执行不同的函数
    - 在这段代码中有三个判断，如果是fulfilled，则执行onFulfilled；如果是rejected，则执行onRejected
    - 如果是pending，则进行异步处理
  
  - 异步处理：
  
    - 为什么状态会是pending？
  
      - ```js
        // 例1：
        const test = new MyPromise((resolve, reject) => {
                    setTimeout(() => {
                        resolve('成功') // 1秒后输出 成功
                    },1000)
                }).then(res => console.log(res), err => console.log(err))
        ```
  
      - 如上方代码，promise内部是一个定时器，一秒后执行
  
      - 所以当调用then的时候，函数尚未执行resolve，此时promise状态为pending
  
    - ```js
        else if (this.state === 'pending') {
                                  // 如果状态为待定状态(promise还在等待执行)，暂时保存两个回调
                                  // onFulfilled传入到成功数组
                                  this.onFulfilledCallbacks.push(() => {
                                      //异步
                                      setTimeout(() => {
                                          try {
                                              let x = onFulfilled(this.result);
                                              resolvePromise(x,resolve,reject,thenPromise)
                                          } catch (e) {
                                              reject(e)
                                          }
                                      })
                                  })
                                  // onRejected 传入到失败数组
                                  this.onRejectedCallbacks.push(() => {
                                      // 异步
                                      setTimeout(() => {
                                          try {
                                              let x = onRejected(this.result);
                                              resolvePromise(x,resolve,reject,thenPromise)
                                          } catch (e) {
                                              reject(e)
                                          }
                                      })
                                  })
                              }
      ```
  
      - 当判断状态为pending时，就要将then中的函数储存起来，等到异步任务执行的时候再使用
      - 其中第一个参数onFulfilled储存进onFulfilledCallbacks数组； 第二个参数onRejected储存进onRejectedCallbacks数组
      - 在promise异步完成之后再调用储存在数组中的函数（在上面的resolve和reject中）
      - 关于为什么在push的时候要加一层setTimeout，下面讲
  
  - 链式调用：
  
    - then函数是可以链式调用的，而要实现链式调用，则需要：
      - 上一个then返回一个promise对象
      - 下一个then的参数要拿到上一个then的回调返回值
  
    - 为了实现第一个要求：所以上方then代码中，要新建一个promise实例thenPromise，最后再将其return
  
    - 为了实现第二个要求，获取上一个promise的返回值，如下代码：
  
      - ```js
         // 这是then方法的部分代码
        if (this.state === 'fulfilled') {
                                setTimeout(() => {
                                    // 如果当前为成功状态，执行第一个回调
                                    // 用x接收第一个回调的返回值
                                    try {
                                        let x = onFulfilled(this.result)
                                        resolvePromise(x,resolve,reject,thenPromise)
                                    } catch(e) {
                                        reject(e)
                                    }
                                })                     
                                }
        ```
  
      - 在判断完状态之后，声明一个x变量接受上一个promise的返回值
  
      - 然后再使用resolvePromise函数处理x，resolvePromise函数下面讲
  
      - ```js
        // 例2：
        const promise = new MyPromise((resolve,reject) => {
                    resolve(500)
                })
        
        promise.then(res => {
            console.log(res);
            return 200
        }).then(data => {
            console.log(data);
        })
        
        // 如上面例子，promise连续调用了两个then，输出了500 200
        // 在第一次调用then的时候，其参数是onFulfilled，当其进入then函数，便执行onFulfilled并将其返回值（200）赋给x，然后通过resolvePromise处理x
        ```
  
      - 
  
    - 下面是resolvePromise函数
  
      - ```js
        // 完成 resolvePromise函数（处理链式调用的函数）
                // x是前一个then的返回值,thenPromise是当前的promise
                function resolvePromise(x,resolve,reject,thenPromise) {
                        // 判断是否是自身,避免循环调用(见例3)
                        if(x === thenPromise) {
                            const err = new TypeError('Uncaught (in promise) TypeError: detected for promise #<Promise>');
                            return reject(err)
                        }
                        //判断x是否是一个promise
                        if(x instanceof MyPromise) {
                            // 等他成功或失败,执行resolve或reject
                            x.then((value) => {
                                resolve(value)
                            },(err) =>{
                                reject(err)
                            })
                        } else {
                            // 普通值
                            resolve(x)
                        }
                    }
        ```
  
      - resolvePromise函数共有四个参数
  
        - 第一个参数是x，即前面保存的上一个promise的返回值
        - resolve和reject是promise的两个参数
        - thenPromise是当前的promise
  
      - 进入resolvePromise，首先判断x（上一个then的返回值）是否与thenPromise（当前promise）相等。如果相等，则报错，因为如果相等的话，会造成循环调用（即自己等待自己完成）
  
        ```js
        // 例3：
                const promise2 = new MyPromise((resolve,reject) => {
                            resolve(500)
                        })
        
                // p就是resolvePromise中的thenPromise（当前promise）,return的p就是resolvePromise中的x（返回值）
                const p = promise2.then(res => {
                    console.log(res);
                    return p;
                })
        
                p.then(null,err => {
                    console.log(err);
                })
        // 输出：
        //TypeError: Uncaught (in promise) TypeError: detected for promise #<Promise>
        //at resolvePromise (手写Promise.html:201:33)
        //at 手写Promise.html:145:33
        
        // 在上面的例子中，promise2的第一个then的返回值是p（它自身）
        // 所以在第二个then中，x接收到的值与thenPromise相等，所以报错
        ```
  
      - 接下来判断x是否为一个promise
  
        - 如果不是promise，则直接成功（resolve(x)）
  
        - 如果是promise，则等待x成功或失败后执行resolve或reject
  
        - ```js
          // 例4：
          const promise1 = new MyPromise((resolve,reject) => {
                      resolve(500)
                  })
          
                  promise1.then(res => {
                      console.log(res);
                      return new MyPromise((resolve,reject) => {
                          resolve(200)
                      })
                  }).then(data => {
                      console.log(data);
                  })
          
          // 最后输出500 200
          // 如上例子：第一个then的时候，promise的状态是fulfilled，此时console.log(res)之后，返回了一个promise
          // 在第二个then的时候，x的值是一个promise，因为x内部resolve（200），所以状态为fulfilled，调用then中的data的值是200
          ```
  
      - 为什么在if判断promise状态的时候要加一个setTimeout包起来？
  
        - 因为如果没有setTimeout，则如下代码：
  
          - ```js
             let thenPromise =  new MyPromise((resolve,reject) => { 
            if (this.state === 'fulfilled') {
                                    // 为什么要setTimeout?
                                    // 因为如果setTimeout是异步操作,如果没有setTimeout,那么下方的是同步代码,而此时thenPromise还没有完全生成,导致resolvePromise不执行
                                        // 如果当前为成功状态，执行第一个回调
                                        // 用x接收第一个回调的返回值
                                        try {
                                            let x = onFulfilled(this.result)
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch(e) {
                                            reject(e)
                                        }                 
                            }
             })
            ```
  
          - 此时if中的代码是同步执行的，而resolvePromise中的参数有thenPromise，但是if是包含在thenPromise中的，此时thenPromise还没有完全生成，因此无法执行resolvePromise
  
        - 而如果有了setTimeout，则：
  
          - ```js
             if (this.state === 'fulfilled') {
                                    // 为什么要setTimeout?
                                    // 因为如果setTimeout是异步操作,如果没有setTimeout,那么下方的是同步代码,而此时thenPromise还没有完全生成,导致resolvePromise不执行
                                    setTimeout(() => {
                                        // 如果当前为成功状态，执行第一个回调
                                        // 用x接收第一个回调的返回值
                                        try {
                                            let x = onFulfilled(this.result)
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch(e) {
                                            reject(e)
                                        }
                                    })                     
                            }
            ```
  
          - 因为setTimeout是一个异步任务，所以在执行resolvePromise的时候，thenPromise已经完全生成，可以作为参数，resolvePromise可以正常执行
  
- 然后是catch

  - ```js
    catch(onRejected) {
                    // 为了链式调用，返回一个promise实例
                    let thenPromise =  new MyPromise((resolve,reject) => {
                         // 判断传入参数是不是函数
                        onRejected = typeof onRejected === 'function' ? onRejected : () => {}
    
                        if (this.state === 'rejected') {
                                setTimeout(() => {
                                    try {
                                        // 如果当前为失败状态，执行第二个回调
                                        let x = onRejected(this.result)
                                        resolvePromise(x,resolve,reject,thenPromise)
                                    } catch(e) {
                                        reject(e)
                                    }
                                })
                            }  else if (this.state === 'pending') {
                                // 如果状态为待定状态(promise还在等待执行)，暂时保存两个回调
                                // onFulfilled传入到成功数组
                                // 为啥要包一层箭头函数:因为如果没有箭头函数,直接push会丢失参数this.result
                                this.onFulfilledCallbacks.push(() => {
                                    //异步
                                    setTimeout(() => {
                                        try {
                                            let x = onFulfilled(this.result);
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch (e) {
                                            reject(e)
                                        }
                                    })
                                })
                                // onRejected 传入到失败数组
                                this.onRejectedCallbacks.push(() => {
                                    // 异步
                                    setTimeout(() => {
                                        try {
                                            let x = onRejected(this.result);
                                            resolvePromise(x,resolve,reject,thenPromise)
                                        } catch (e) {
                                            reject(e)
                                        }
                                    })
                                })
    
                            }
    
                            
                    })
    
                    return thenPromise
                }
    ```

  - catch和then差不多，只不过少了一个参数



- Promise.prototype.finally()

  - `finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

    ```js
    promise
    .then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
    
    //上面代码中，不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。
    ```

  - `finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是`fulfilled`还是`rejected`。这表明，`finally`方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

  - 手写Promise.prototype.finally()

    ```js
    finally(callback) {
                    return this.then(
                        // callback()执行完可能返回一个Promise
                        // 也可能不返回一个Promise，为了兼容，嵌套一层Promise.resolve保证向下传递
                        // finally前面是成功，就执行成功的回调，并把前面的参数向下传递
                        value  => MyPromise.resolve(callback()).then(() => value),
                        // finally前面是失败，就执行失败的回调，并把前面的参数向下传递
                        reason => MyPromise.resolve(callback()).then(() => { throw reason })
                        );
                    };
    ```

    - finally接受一个参数，为一个回调函数
    - finally不管promise是什么状态，直接调用then
    - 因为callback()执行完不一定是一个promise，所以用resolve保证向下传递，resolve方法在下面写



- Promise.resolve()

  - 有时需要将现有对象转为 Promise 对象，`Promise.resolve()`方法就起到这个作用。

  - `Promise.resolve()`方法的参数分成四种情况。

    - 如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。
    - 如果参数是参数是一个`thenable`对象，`Promise.resolve()`方法会将这个对象转为 Promise 对象，然后就**立即执行`thenable`对象的`then()`方法。**
    - 如果参数是一个原始值，或者是一个不具有`then()`方法的对象，则`Promise.resolve()`方法返回一个新的 Promise 对象，**状态为`resolved`。**
    - `Promise.resolve()`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。

  - 手写resolve

    - ```js
      MyPromise.resolve = function (param) {
                      if (param instanceof Promise) {
                          return param;
                      }
                      return new MyPromise((resolve, reject) => {
                          if (param && typeof param === 'object' && typeof param.then === 'function') {
                              setTimeout(() => {
                                  param.then(resolve, reject);
                              });
                          } else {
                              resolve(param);
                          }
                      });
                  }
      ```

    - resolve首先判断参数是否为promise对象，如果是，则直接返回

    - 接下来新建一个promise对象并返回

    - 在其中判断参数是否具有then方法，如果具有then方法，则执行then方法

    - 如果不具有then方法，则直接resolve返回

      

- Promise.reject()

  - `Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

  - Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。

  - 手写reject

    - ```js
       MyPromise.reject = function(val) {
                      return new MyPromise((resolve,reject) => {
                          reject(val)
                      })
                  }
      ```

    - 就直接返回一个新的promise实例，将传入的参数作为promise的参数

      

- Promise.race()

  - `Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

    ```js
    const p = Promise.race([p1, p2, p3]);
    
    //上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
    ```

  - 手写race

    - ```js
      // race方法(谁先满足条件，就先被then处理，其他的忽略)
                  // 返回最快的结果
                  MyPromise.race = function(promises) {
                      return new Promise((resolve,reject) => {
                          //遍历promises，看谁先满足条件，就让外层的promise resolve
                          // promise的状态只能被修改一次，只要第一个改了，就成功，后续再改就无效了
                          for(let i = 0;i < promises.length;i++) {
                              MyPromise.resolve(promises[i]).then((value) => {
                                  resolve(value)
                              },(err) => {
                                  reject(err)
                              })
                          }
                      })
                  }
      ```

    - race方法接受一个参数，是一个数组

    - 在race内部返回一个新的promise实例

    - 遍历传入的数组，将数组元素包装为promise（因为元素有可能不是promise），然后执行then





- Promise.all()

  - `Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

    ```js
    const p = Promise.all([p1, p2, p3]);
    
    //上面代码中，Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用上面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
    ```

  - `p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

    - 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
    - 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

  - 手写all

    - ```js
      // all方法(传入多个promise，等待所有的promise都满足条件，拿到所有的成功的结果)
                  // 获取所有的promise，执行then的结果
                  MyPromise.all = function(promises) {
                      return new MyPromise((resolve,reject) => {
                          let arr = []       // 用来储存结果，将结果返回（按顺序）
                          let count = 0         // 计数器，累计有多少个promise已经有结果了，如果结果个数与参数长度相等，那就完成了
      
                          if(promises.length === 0) {
                              resolve(arr)
                          }
                          for (let [i, p] of promises.entries()) {
                              Promise.resolve(p).then((data) => {
                                  arr[i] = data;
                                  count++;
                                  if (count === promises.length) {
                                      resolve(arr);
                                  }
                              }, (err) => {
                                  reject(err);
                              });
                          }
                      })
                  }
      ```

    - all方法接受一个参数，是一个数组

    - 如果传入数组长度为0，那么直接返回

    - 要用resolve包装数组元素，因为数组元素可能不是promise

- 
