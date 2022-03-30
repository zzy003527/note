## async和await

- 字面理解

  - async是“异步”的意思，而await是等待的意思。所以。async用于申明一个 异步的function（实际上是async function对象），而await是用于等待一个异步任务执行完成的结果。并且await只能出现在async函数中

- async和await的执行

  - async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

  - async 告诉程序这是一个异步操作，**await 是一个操作符，即 await 后面是一个表达式**

  - 当调用一个 async 函数时，会**返回一个 Promise 对象**

  - 当这个 async 函数返回一个值时，Promise 的 resolve 方法会负责传递这个值；当 async 函数抛出异常时，Promise 的 reject 方法也会传递这个异常值。async 函数中可能会有 await 表达式，await表达式会使 async 函数暂停执行，直到表达式中的 Promise 解析完成后继续     执行 async中await 后面的代码并返回解决结果。

  - **注意， await 关键字仅仅在 async function中有效**

  - ```js
    例子:
    var fnPromise1 = function() {
        return new Promise(function(resolve,reject) {
            setTimeout(function() {
                resolve('3秒执行')
            })
        },3000);
    }
    
    var fnPromise2 = function() {
        return new Promise(function(resolve,reject) {
            setTimeout(function() {
                resolve('2秒执行')
            })
        },2000);
    }
    
    var fnPromise3 = function() {
        return new Promise(function(resolve,reject) {
            reject("失败了");
        })
    }
    
    
    async function demo() {
        var result1 = await fnPromise1()
        var result2 = await fnPromise2()
        var result3 = await fnPromise3()
        
        console.log(result1)
        console.log(result2)
        console.log(result3)            //此错误会直接报错，中断
    }
    
    async function demo() {
        try {
             var result1 = await fnPromise1()
             var result2 = await fnPromise2()
             var result3 = await fnPromise3()
        
             console.log(result1)
             console.log(result2)
             console.log(result3)     
             }catch(e) {
                 console.log(e)                //可以输出错误信息
             }       
         }
    
    
    demo()
    ```

  - async语法

    - async函数返回一个 Promise 对象。

    - async函数内部retur语句返回的值，会成为then方法回调函数的参数。

      如：

      ```js
      async function f() {
        return 'hello world';
      }
      
      f().then(v => console.log(v))
      // "hello world"
      
      //上面代码中，函数f内部return命令返回的值，会被then方法回调函数接收到。
      ```

    - async函数内部抛出错误，会导致返回的 Promise 对象变为`reject`状态。抛出的错误对象会被`catch`方法回调函数接收到。

    - async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，**只有async函数内部的异步操作执行完，才会执行`then`方法指定的回调函数。**

  - await语法

    - 正常情况下，await命令后面是一个 Promise 对象，**返回该对象的结果**。如果不是 Promise 对象，就**直接返回对应的值**。
    - await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到。
    - **任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。**

    

- 总结

  - await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。
  - async告诉程序这是一个异步，await会暂停执行async中的代码，等待await表达式后面的结果，跳过async函数，执行后方的同步操作后再回来执行async
  - async函数会返回一个Promise对象，那么当async函数返回一个值时，Promise的resolve方法会负责传递这个值；当async函数抛出异常的时候，Promise的reject方法也会传递这个异常值