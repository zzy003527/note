## async函数与await

- ES2017 标准引入了 async 函数，使得异步操作变得更加方便。

  async 函数是什么？一句话，它就是 Generator 函数的语法糖。

  前文有一个 Generator 函数，依次读取两个文件。

  ```js
  const fs = require('fs');
  
  const readFile = function (fileName) {
    return new Promise(function (resolve, reject) {
      fs.readFile(fileName, function(error, data) {
        if (error) return reject(error);
        resolve(data);
      });
    });
  };
  
  const gen = function* () {
    const f1 = yield readFile('/etc/fstab');
    const f2 = yield readFile('/etc/shells');
    console.log(f1.toString());
    console.log(f2.toString());
  };
  
  //上面代码的函数gen可以写成async函数，就是下面这样。
  
  const asyncReadFile = async function () {
    const f1 = await readFile('/etc/fstab');
    const f2 = await readFile('/etc/shells');
    console.log(f1.toString());
    console.log(f2.toString());
  };
  
  //一比较就会发现，async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已。
  ```

- `async`函数对 Generator 函数的改进，体现在以下四点。

  - 内置执行器。

    - Generator 函数的执行必须靠执行器，所以才有了`co`模块，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。

    - ```js
      asyncReadFile();
      
      //上面的代码调用了asyncReadFile函数，然后它就会自动执行，输出最后结果。这完全不像 Generator 函数，需要调用next方法，或者用co模块，才能真正执行，得到最后结果。
      ```

  - 更好的语义。

    - `async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。

  - 更广的适用性。

    - `co`模块约定，`yield`命令后面只能是 Thunk 函数或 Promise 对象，而`async`函数的`await`命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。

  - 返回值是 Promise。

    - `async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。
    - 进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。

  

- 基本用法

  - `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到`await`就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

    ```js
    例子1：
    async function getStockPriceByName(name) {
      const symbol = await getStockSymbol(name);
      const stockPrice = await getStockPrice(symbol);
      return stockPrice;
    }
    
    getStockPriceByName('goog').then(function (result) {
      console.log(result);
    });
    //上面代码是一个获取股票报价的函数，函数前面的async关键字，表明该函数内部有异步操作。调用该函数时，会立即返回一个Promise对象。
    
    例子2(指定多少毫秒后输出一个值)：
    function timeout(ms) {
      return new Promise((resolve) => {
        setTimeout(resolve, ms);
      });
    }
    
    async function asyncPrint(value, ms) {
      await timeout(ms);
      console.log(value);
    }
    
    asyncPrint('hello world', 50);
    //上面代码指定 50 毫秒以后，输出hello world。
    //由于async函数返回的是 Promise 对象，可以作为await命令的参数。所以，上面的例子也可以写成下面的形式。
    async function timeout(ms) {
      await new Promise((resolve) => {
        setTimeout(resolve, ms);
      });
    }
    
    async function asyncPrint(value, ms) {
      await timeout(ms);
      console.log(value);
    }
    
    asyncPrint('hello world', 50);
    ```

  - async 函数有多种使用形式。

    ```js
    // 函数声明
    async function foo() {}
    
    // 函数表达式
    const foo = async function () {};
    
    // 对象的方法
    let obj = { async foo() {} };
    obj.foo().then(...)
    
    // Class 的方法
    class Storage {
      constructor() {
        this.cachePromise = caches.open('avatars');
      }
    
      async getAvatar(name) {
        const cache = await this.cachePromise;
        return cache.match(`/avatars/${name}.jpg`);
      }
    }
    
    const storage = new Storage();
    storage.getAvatar('jake').then(…);
    
    // 箭头函数
    const foo = async () => {};
    ```

    

- 语法

  `async`函数的语法规则总体上比较简单，难点是错误处理机制。

  - 返回Promise对象

    - `async`函数返回一个 Promise 对象。

    - `async`函数内部`return`语句返回的值，会成为`then`方法回调函数的参数。

      ```js
      async function f() {
        return 'hello world';
      }
      
      f().then(v => console.log(v))
      // "hello world"
      
      //上面代码中，函数f内部return命令返回的值，会被then方法回调函数接收到。
      ```

    - `async`函数内部抛出错误，会导致返回的 Promise 对象变为`reject`状态。抛出的错误对象会被`catch`方法回调函数接收到。

      ```js
      async function f() {
        throw new Error('出错了');
      }
      
      f().then(
        v => console.log('resolve', v),
        e => console.log('reject', e)
      )
      //reject Error: 出错了
      ```

  - Promise对象的状态变化

    - `async`函数返回的 Promise 对象，必须等到内部所有`await`命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到`return`语句或者抛出错误。也就是说，**只有`async`函数内部的异步操作执行完，才会执行`then`方法指定的回调函数。**

    - ```js
      例子：
      async function getTitle(url) {
        let response = await fetch(url);
        let html = await response.text();
        return html.match(/<title>([\s\S]+)<\/title>/i)[1];
      }
      getTitle('https://tc39.github.io/ecma262/').then(console.log)
      // "ECMAScript 2017 Language Specification"
      
      //上面代码中，函数getTitle内部有三个操作：抓取网页、取出文本、匹配页面标题。只有这三个操作全部完成，才会执行then方法里面的console.log。
      ```

  - await命令

    - 正常情况下，`await`命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。

      ```js
      async function f() {
        // 等同于
        // return 123;
        return await 123;
      }
      
      f().then(v => console.log(v))
      // 123
      
      //上面代码中，await命令的参数是数值123，这时等同于return 123。
      ```

    - 另一种情况是，`await`命令后面是一个`thenable`对象（即定义了`then`方法的对象），那么`await`会将其等同于 Promise 对象。

      ```js
      class Sleep {
        constructor(timeout) {
          this.timeout = timeout;
        }
        then(resolve, reject) {
          const startTime = Date.now();
          setTimeout(
            () => resolve(Date.now() - startTime),
            this.timeout
          );
        }
      }
      
      (async () => {
        const sleepTime = await new Sleep(1000);
        console.log(sleepTime);
      })();
      // 1000
      
      //上面代码中，await命令后面是一个Sleep对象的实例。这个实例不是 Promise 对象，但是因为定义了then方法，await会将其视为Promise处理。
      
      //这个例子还演示了如何实现休眠效果。JavaScript 一直没有休眠的语法，但是借助await命令就可以让程序停顿指定的时间。下面给出了一个简化的sleep实现。
      function sleep(interval) {
        return new Promise(resolve => {
          setTimeout(resolve, interval);
        })
      }
      
      // 用法
      async function one2FiveInAsync() {
        for(let i = 1; i <= 5; i++) {
          console.log(i);
          await sleep(1000);
        }
      }
      
      one2FiveInAsync();
      ```

    - `await`命令后面的 Promise 对象如果变为`reject`状态，则`reject`的参数会被`catch`方法的回调函数接收到。

      ```js
      async function f() {
        await Promise.reject('出错了');
      }
      
      f()
      .then(v => console.log(v))
      .catch(e => console.log(e))
      // 出错了
      
      //注意，上面代码中，await语句前面没有return，但是reject方法的参数依然传入了catch方法的回调函数。这里如果在await前面加上return，效果是一样的。
      ```

    - 任何一个`await`语句后面的 Promise 对象变为`reject`状态，那么整个`async`函数都会中断执行。

      ```js
      async function f() {
        await Promise.reject('出错了');
        await Promise.resolve('hello world'); // 不会执行
      }
      
      //上面代码中，第二个await语句是不会执行的，因为第一个await语句状态变成了reject。
      ```

    - 有时，我们希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个`await`放在`try...catch`结构里面，这样不管这个异步操作是否成功，第二个`await`都会执行。

      ```js
      async function f() {
        try {
          await Promise.reject('出错了');
        } catch(e) {
        }
        return await Promise.resolve('hello world');
      }
      
      f()
      .then(v => console.log(v))
      // hello world
      ```

    - 另一种方法是`await`后面的 Promise 对象再跟一个`catch`方法，处理前面可能出现的错误。

      ```js
      async function f() {
        await Promise.reject('出错了')
          .catch(e => console.log(e));
        return await Promise.resolve('hello world');
      }
      
      f()
      .then(v => console.log(v))
      // 出错了
      // hello world
      ```

  - 错误处理

    - 如果`await`后面的异步操作出错，那么等同于`async`函数返回的 Promise 对象被`reject`。

      ```js
      async function f() {
        await new Promise(function (resolve, reject) {
          throw new Error('出错了');
        });
      }
      
      f()
      .then(v => console.log(v))
      .catch(e => console.log(e))
      // Error：出错了
      
      //上面代码中，async函数f执行后，await后面的 Promise 对象会抛出一个错误对象，导致catch方法的回调函数被调用，它的参数就是抛出的错误对象。具体的执行机制，可以参考后文的“async 函数的实现原理”。
      ```

    - 防止出错的方法，也是将其放在`try...catch`代码块之中。

      ```js
      async function f() {
        try {
          await new Promise(function (resolve, reject) {
            throw new Error('出错了');
          });
        } catch(e) {
        }
        return await('hello world');
      }
      ```

    - 如果有多个`await`命令，可以统一放在`try...catch`结构中。

      ```js
      async function main() {
        try {
          const val1 = await firstStep();
          const val2 = await secondStep(val1);
          const val3 = await thirdStep(val1, val2);
      
          console.log('Final: ', val3);
        }
        catch (err) {
          console.error(err);
        }
      }
      ```

    - 下面的例子使用`try...catch`结构，实现多次重复尝试。

      ```js
      const superagent = require('superagent');
      const NUM_RETRIES = 3;
      
      async function test() {
        let i;
        for (i = 0; i < NUM_RETRIES; ++i) {
          try {
            await superagent.get('http://google.com/this-throws-an-error');
            break;
          } catch(err) {}
        }
        console.log(i); // 3
      }
      
      test();
      
      //上面代码中，如果await操作成功，就会使用break语句退出循环；如果失败，会被catch语句捕捉，然后进入下一轮循环。
      ```

  - 使用注意点

    - 第一点，前面已经说过，`await`命令后面的`Promise`对象，运行结果可能是`rejected`，所以最好把`await`命令放在`try...catch`代码块中。

      ```js
      async function myFunction() {
        try {
          await somethingThatReturnsAPromise();
        } catch (err) {
          console.log(err);
        }
      }
      
      // 另一种写法
      
      async function myFunction() {
        await somethingThatReturnsAPromise()
        .catch(function (err) {
          console.log(err);
        });
      }
      ```

    - 

    - 

    - 

    - 

    - 

    - 

    - 

    - 

    - d

  - 

  - 

  - 

  - 

  - 

  - d

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

- 

- 

- 

- 

- d