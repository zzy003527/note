## Promise

- 概述：

  - Promise对象：代表了未来某个将要发生的事情（通常是一个异步操作）

  - ES6中的promise对象，可以将异步操作以同步的流程表达出来，很好的解决了回调地狱的问题（避免了层次嵌套的回调函数）。在使用ES5的时候，在多层嵌套回调时，写完的代码层次过多，很难进行维护和二次开发

- 回调地狱的举例
  - 假设买菜，做饭，洗碗都是异步的
  - 现在的流程是：买菜成功之后，才能开始做饭，做饭成功之后，才能开始洗碗。这里面就涉及到了回调的嵌套
  - ES6中的Promise是一个构造函数，用来生成promise实例

- promise对象的3个状态

  - 初始化状态（等待状态）：pending
  - 成功状态：fulfilled
  - 失败状态：rejected
  -  **<u>Promise 的状态一旦改变，就永久保持该状态，不会再变了。</u>**

- Promise基本用法

  - ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

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

  - Promise.prototype.finally()的实现

    ```js
    Promise.prototype.finally = function (callback) {
      let P = this.constructor;
      return this.then(
        value  => P.resolve(callback()).then(() => value),
        reason => P.resolve(callback()).then(() => { throw reason })
      );
    };
    ```

    

- Promise.all()

  - `Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

    ```js
    const p = Promise.all([p1, p2, p3]);
    
    //上面代码中，Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
    ```

  - `p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。

    - 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
    - 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

  - **注意，如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。**

    ```js
    const p1 = new Promise((resolve, reject) => {
      resolve('hello');
    })
    .then(result => result)
    .catch(e => e);
    
    const p2 = new Promise((resolve, reject) => {
      throw new Error('报错了');
    })
    .then(result => result)
    .catch(e => e);
    
    Promise.all([p1, p2])
    .then(result => console.log(result))
    .catch(e => console.log(e));
    // ["hello", Error: 报错了]
    
    //上面代码中，p1会resolved，p2首先会rejected，但是p2有自己的catch方法，该方法返回的是一个新的 Promise 实例，p2指向的实际上是这个实例。该实例执行完catch方法后，也会变成resolved，导致Promise.all()方法参数里面的两个实例都会resolved，因此会调用then方法指定的回调函数，而不会调用catch方法指定的回调函数。
    ```

    

- Promise.race()

  - `Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

    ```js
    const p = Promise.race([p1, p2, p3]);
    
    //上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
    ```

  - 下面是一个例子，如果指定时间内没有获得结果，就将 Promise 的状态变为`reject`，否则变为`resolve`。

    ```js
    const p = Promise.race([
      fetch('/resource-that-may-take-a-while'),
      new Promise(function (resolve, reject) {
        setTimeout(() => reject(new Error('request timeout')), 5000)
      })
    ]);
    
    p
    .then(console.log)
    .catch(console.error);
    
    //上面代码中，如果 5 秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数。
    ```

    

- Promise.allSettled()

  - `Promise.all()`方法只适合所有异步操作都成功的情况，如果有一个操作失败，就无法满足要求。

    ```js
    const urls = [url_1, url_2, url_3];
    const requests = urls.map(x => fetch(x));
    
    try {
      await Promise.all(requests);
      console.log('所有请求都成功。');
    } catch {
      console.log('至少一个请求失败，其他请求可能还没结束。');
    }
    
    //上面示例中，Promise.all()可以确定所有请求都成功了，但是只要有一个请求失败，它就会报错，而不管另外的请求是否结束。
    ```

  - `Promise.allSettled()`方法接受一个数组作为参数，数组的每个成员都是一个 Promise 对象，并返回一个新的 Promise 对象。只有等到参数数组的所有 Promise 对象都发生状态变更（不管是`fulfilled`还是`rejected`），返回的 Promise 对象才会发生状态变更。

    ```js
    const promises = [
      fetch('/api-1'),
      fetch('/api-2'),
      fetch('/api-3'),
    ];
    
    await Promise.allSettled(promises);
    removeLoadingIndicator();
    
    //上面示例中，数组promises包含了三个请求，只有等到这三个请求都结束了（不管请求成功还是失败），removeLoadingIndicator()才会执行。
    ```

  - 该方法返回的新的 Promise 实例，一旦发生状态变更，状态总是`fulfilled`，不会变成`rejected`。状态变成`fulfilled`后，它的回调函数会接收到一个数组作为参数，该数组的每个成员对应前面数组的每个 Promise 对象。

    ```js
    const resolved = Promise.resolve(42);
    const rejected = Promise.reject(-1);
    
    const allSettledPromise = Promise.allSettled([resolved, rejected]);
    
    allSettledPromise.then(function (results) {
      console.log(results);
    });
    // [
    //    { status: 'fulfilled', value: 42 },
    //    { status: 'rejected', reason: -1 }
    // ]
    
    //上面代码中，Promise.allSettled()的返回值allSettledPromise，状态只可能变成fulfilled。它的回调函数接收到的参数是数组results。该数组的每个成员都是一个对象，对应传入Promise.allSettled()的数组里面的两个 Promise 对象。
    ```

  - `results`的每个成员是一个对象，对象的格式是固定的，对应异步操作的结果。

    ```js
    // 异步操作成功时
    {status: 'fulfilled', value: value}
    
    // 异步操作失败时
    {status: 'rejected', reason: reason}
    
    //成员对象的status属性的值只可能是字符串fulfilled或字符串rejected，用来区分异步操作是成功还是失败。如果是成功（fulfilled），对象会有value属性，如果是失败（rejected），会有reason属性，对应两种状态时前面异步操作的返回值。
    ```

  - 返回值用法的例子

    ```js
    const promises = [ fetch('index.html'), fetch('https://does-not-exist/') ];
    const results = await Promise.allSettled(promises);
    
    // 过滤出成功的请求
    const successfulPromises = results.filter(p => p.status === 'fulfilled');
    
    // 过滤出失败的请求，并输出原因
    const errors = results
      .filter(p => p.status === 'rejected')
      .map(p => p.reason);
    ```

    

- Promise.any()

  - 该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。

    ```js
    Promise.any([
      fetch('https://v8.dev/').then(() => 'home'),
      fetch('https://v8.dev/blog').then(() => 'blog'),
      fetch('https://v8.dev/docs').then(() => 'docs')
    ]).then((first) => {  // 只要有一个 fetch() 请求成功
      console.log(first);
    }).catch((error) => { // 所有三个 fetch() 全部请求失败
      console.log(error);
    });
    
    //只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态。
    ```

  - `Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是`Promise.any()`不会因为某个 Promise 变成`rejected`状态而结束，必须等到所有参数 Promise 变成`rejected`状态才会结束。

  - `Promise.any()`抛出的错误，不是一个一般的 Error 错误对象，而是一个 AggregateError 实例。它相当于一个数组，每个成员对应一个被`rejected`的操作所抛出的错误。下面是一个例子：

    ```js
    var resolved = Promise.resolve(42);
    var rejected = Promise.reject(-1);
    var alsoRejected = Promise.reject(Infinity);
    
    Promise.any([resolved, rejected, alsoRejected]).then(function (result) {
      console.log(result); // 42
    });
    
    Promise.any([rejected, alsoRejected]).catch(function (results) {
      console.log(results); // [-1, Infinity]
    });
    ```





- Promise.resolve()

  - 有时需要将现有对象转为 Promise 对象，`Promise.resolve()`方法就起到这个作用。

    ```js
    const jsPromise = Promise.resolve($.ajax('/whatever.json'));
    //上面代码将 jQuery 生成的deferred对象，转为一个新的 Promise 对象。
    
    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))
    ```

  - `Promise.resolve()`方法的参数分成四种情况。

    - 参数是一个 Promise 实例

      - 如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。

    - 参数是一个`thenable`对象

      - `thenable`对象指的是具有`then`方法的对象，比如下面这个对象。

        ```js
        let thenable = {
          then: function(resolve, reject) {
            resolve(42);
          }
        };
        ```

      - `Promise.resolve()`方法会将这个对象转为 Promise 对象，然后就立即执行`thenable`对象的`then()`方法。

        ```js
        let thenable = {
          then: function(resolve, reject) {
            resolve(42);
          }
        };
        
        let p1 = Promise.resolve(thenable);
        p1.then(function (value) {
          console.log(value);  // 42
        });
        
        //上面代码中，thenable对象的then()方法执行后，对象p1的状态就变为resolved，从而立即执行最后那个then()方法指定的回调函数，输出42。
        ```

    - 参数不是具有`then()`方法的对象，或根本就不是对象

      - 如果参数是一个原始值，或者是一个不具有`then()`方法的对象，则`Promise.resolve()`方法返回一个新的 Promise 对象，状态为`resolved`。

        ```js
        const p = Promise.resolve('Hello');
        
        p.then(function (s) {
          console.log(s)
        });
        // Hello
        
        //上面代码生成一个新的 Promise 对象的实例p。由于字符串Hello不属于异步操作（判断方法是字符串对象不具有 then 方法），返回 Promise 实例的状态从一生成就是resolved，所以回调函数会立即执行。Promise.resolve()方法的参数，会同时传给回调函数。
        ```

    - 不带有任何参数

      - `Promise.resolve()`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。

        所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用`Promise.resolve()`方法。

        ```js
        const p = Promise.resolve();
        
        p.then(function () {
          // ...
        });
        
        //上面代码中的p就是一个Promise对象
        ```

      - 需要注意的是，立即`resolve()`的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时。

        ```js
        setTimeout(function () {
          console.log('three');
        }, 0);
        
        Promise.resolve().then(function () {
          console.log('two');
        });
        
        console.log('one');
        
        // one
        // two
        // three
        
        
        //上面代码中，setTimeout(fn, 0)在下一轮“事件循环”开始时执行，Promise.resolve()在本轮“事件循环”结束时执行，console.log('one')则是立即执行，因此最先输出。
        ```





- Promise.reject()

  - `Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

    ```js
    const p = Promise.reject('出错了');
    // 等同于
    const p = new Promise((resolve, reject) => reject('出错了'))
    
    p.then(null, function (s) {
      console.log(s)
    });
    // 出错了
    
    //上面代码生成一个 Promise 对象的实例p，状态为rejected，回调函数会立即执行。
    ```

  - Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。

    ```js
    Promise.reject('出错了')
    .catch(e => {
      console.log(e === '出错了')
    })
    // true
    
    //上面代码中，Promise.reject()方法的参数是一个字符串，后面catch()方法的参数e就是这个字符串。
    ```

    

  

- Promise.try()

  - 实际开发中，经常遇到一种情况：不知道或者不想区分，函数`f`是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管`f`是否包含异步操作，都用`then`方法指定下一步流程，用`catch`方法处理`f`抛出的错误。一般就会采用下面的写法。

    ```js
    Promise.resolve().then(f)
    //上面的写法有一个缺点，就是如果f是同步函数，那么它会在本轮事件循环的末尾执行。
    ```

  - 可以统一用`promise.catch()`捕获所有同步和异步的错误。

    ```js
    Promise.try(() => database.users.get({id: userId}))
      .then(...)
      .catch(...)
    
    //让同步函数同步执行，异步函数异步执行，并且让它们具有统一的 API 
    ```

    

- 应用

  - 加载图片

    我们可以将图片的加载写成一个`Promise`，一旦加载完成，`Promise`的状态就发生变化。

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

  - 

  - 