## 迭代器（Iterator）和生成器（Generator）

#### 迭代器（Iterator）

- **迭代器是一种特殊对象，每一个迭代器对象都有一个next()，该方法返回一个对象，包括value和done属性。**

- ES5实现迭代器：

  ```js
      //实现一个返回迭代器对象的函数，注意该函数不是迭代器，返回结果才叫做迭代器。
      function createIterator(items) {
        var i = 0;
        return {
          next() {
            var done = (i >= items.length); // 判断i是否小于遍历的对象长度。
            var value = !done ? items[i++] : undefined; //如果done为false，设置value为当前遍历的值。
            return {
              done,
              value
            }
          }
        }
      }
      const a = createIterator([1, 2, 3]);
      
      //该方法返回的最终是一个对象，包含value、done属性。
      console.log(a.next()); //{value: 1, done: false}
      console.log(a.next()); //{value: 2, done: false}
      console.log(a.next()); //{value: 3, done: false}
      console.log(a.next()); //{value: undefined, done: true}
  ```



#### 生成器（Generator）

- **生成器是函数：用来返回迭代器。**

  - 这个概念有2个关键点，一个是函数、一个是**返回迭代器**。这个函数不是上面ES5中创建迭代器的函数，而是ES6中特有的，**一个带有*（星号）的函数，同时也需要使用到yield。**

    ```js
     //生成器函数，ES6内部实现了迭代器功能，你要做的只是使用yield来迭代输出。
        function *createIterator() {
          yield 1;
          yield 2;
          yield 3;
        }
        const a = createIterator();
        console.log(a.next()); //{value: 1, done: false}
        console.log(a.next()); //{value: 2, done: false}
        console.log(a.next()); //{value: 3, done: false}
        console.log(a.next()); //{value: undefined, done: true}
    ```

  - 生成器的yield关键字有个功能，就是当你执行一次next()，那么只会执行一个yield后面的内容，然后语句终止运行。

- 在for循环中使用迭代器

  - 即使你是在for循环中使用yield关键字，也会暂停循环。

    ```js
      function *createIterator(items) {
          for(let i = 0; i < items.length;  i++) {
            yield items[i]
          }
        }
        const a = createIterator([1, 2, 3]);
        console.log(a.next()); //{value: 1, done: false}
    ```

- yield使用限制

  - yield只可以在生成器函数内部使用，如果在非生成器函数内部使用，则会报错。

- 生成器函数表达式

  - ```js
       const createIterator = function *() {
            yield 1;
            yield 2;
        }
        const a = createIterator();
        console.log(a.next());
    ```

- 在对象中添加生成器函数

  - 我们可以在obj中添加一个生成器，也就是添加一个带星号的方法：

    ```js
        const obj = {
          a: 1,
          *createIterator() {
            yield this.a
          }
        }
        const a = obj.createIterator();
        console.log(a.next());  //{value: 1, done: false}
    ```

  

  

- 可迭代对象和for of循环

  - **迭代器是对象，生成器是返回迭代器的函数。**

    凡是通过生成器生成的迭代器，都是可以迭代的对象(可迭代对象具有Symbol.iterator属性)，也就是可以通过for of将value遍历出来。

  - ```js
    function *createIterator() {
          yield 1;
          yield 2;
          yield 3;
        }
        const a = createIterator();
        for(let value of a) {
          console.log(value)
        }
        // 1 2 3
    
    //上面的例子告诉我们生成器函数返回的迭代器是一个可以迭代的对象
    ```

  - Sumbol.iterator的用法

    - ```js
       function *createIterator() {
            yield 1;
            yield 2;
            yield 3;
          }
          const a = createIterator(); //a是一个迭代器
          const s = a[Symbol.iterator]();//使用Symbol.iterator访问迭代器
          console.log(s.next()) //{value: 1, done: false}
      ```

    - Symbol.iterator还可以用来检测一个对象是否可迭代

      ```js
      typeof obj[Symbol.iterator] === "function"
      ```

- 创建可迭代对象

  - **在ES6中，数组、Set、Map、字符串都是可迭代对象。**

  - **默认情况下定义的对象（object）是不可迭代的，但是可以通过Symbol.iterator创建迭代器。**

    ```js
       const obj = {
          items: []
        }
        obj.items.push(1);//这样子虽然向数组添加了新元素，但是obj不可迭代
        for (let x of obj) {
          console.log(x) // _iterator[Symbol.iterator] is not a function
        }
    
        //接下来给obj添加一个生成器，使obj成为一个可以迭代的对象。
        const obj = {
          items: [],
          *[Symbol.iterator]() {
            for (let item of this.items) {
              yield item;
            }
          }
        }
        obj.items.push(1)
        //现在可以通过for of迭代obj了。
        for (let x of obj) {
          console.log(x)
        }
    ```

    

- **<u>内置迭代器</u>**

  - 数组、Set、Map都是可迭代对象，即它们内部实现了迭代器，并且提供了3种迭代器函数调用。

  - **entries() 返回迭代器**：<u>返回键值对</u>

    - ```js
      //数组
          const arr = ['a', 'b', 'c'];
          for(let v of arr.entries()) {
            console.log(v)
          }
          // [0, 'a'] [1, 'b'] [2, 'c']
          
      //Set
          const arr = new Set(['a', 'b', 'c']);
          for(let v of arr.entries()) {
            console.log(v)
          }
          // ['a', 'a'] ['b', 'b'] ['c', 'c']
      
      //Map
          const arr = new Map();
          arr.set('a', 'a');
          arr.set('b', 'b');
          for(let v of arr.entries()) {
            console.log(v)
          }
          // ['a', 'a'] ['b', 'b']
      ```

  - **values() 返回迭代器**：<u>返回</u>键值对的<u>value</u>

    - ```js
          //数组
          const arr = ['a', 'b', 'c'];
          for(let v of arr.values()) {
            console.log(v)
          }
          //'a' 'b' 'c'
      
          //Set
          const arr = new Set(['a', 'b', 'c']);
          for(let v of arr.values()) {
            console.log(v)
          }
          // 'a' 'b' 'c'
      
          //Map
          const arr = new Map();
          arr.set('a', 'a');
          arr.set('b', 'b');
          for(let v of arr.values()) {
            console.log(v)
          }
          // 'a' 'b'
      ```

  - **keys() 返回迭代器**：<u>返回</u>键值对的<u>key</u>

    - 

      ```js
       //数组
          const arr = ['a', 'b', 'c'];
          for(let v of arr.keys()) {
            console.log(v)
          }
          // 0 1 2
          
          //Set
          const arr = new Set(['a', 'b', 'c']);
          for(let v of arr.keys()) {
            console.log(v)
          }
          // 'a' 'b' 'c'
      
          //Map
          const arr = new Map();
          arr.set('a', 'a');
          arr.set('b', 'b');
          for(let v of arr.keys()) {
            console.log(v)
          }
          // 'a' 'b'
      ```

  - 虽然上面列举了3种内置的迭代器方法，但是不同集合的类型还有自己默认的迭代器，**在for of中，数组和Set的默认迭代器是values()，Map的默认迭代器是entries()。**

    

- 对象for of循环解构

  - 对象本身不支持迭代，但是我们可以自己添加一个生成器，返回一个key，value的迭代器，然后使用for of循环解构key和value。

  - ```js
     const obj = {
          a: 1,
          b: 2,
          *[Symbol.iterator]() {
            for(let i in obj) {
              yield [i, obj[i]]
            }
          }
        }
        for(let [key, value] of obj) {
          console.log(key, value)
        }
        // 'a' 1, 'b' 2
    ```

- 字符串迭代器

  - ```js
    const str = 'abc';
        for(let v of str) {
          console.log(v)
        }
        // 'a' 'b' 'c'
    ```

- 展开运算符和迭代器

  - ```js
     const a = [1, 2, 3];
        const b = [4, 5, 6];
        const c = [...a, ...b]
        console.log(c) // [1, 2, 3, 4, 5, 6]
    ```

  

- 高级迭代器功能

  - 传参

    - 生成器里面有2个yield，当执行第一个next()的时候，返回value为1，然后给第二个next()传入参数10，**传递的参数会替代掉<u>上一个next()</u>的yield返回值**。在下面的例子中就是first。

    - ```js
         function *createIterator() {
            let first = yield 1;
            yield first + 2;
          }
          let i = createIterator();
          console.log(i.next()); // {value: 1, done: false}
          console.log(i.next(10)); // {value: 12, done: false}
      ```

  - 在迭代器中抛出错误

    - ```js
       function *createIterator() {
            let first = yield 1;
            yield first + 2;
          }
          let i = createIterator();
          console.log(i.next()); // {value: 1, done: false}
          console.log(i.throw(new Error('error'))); // error
          console.log(i.next()); //不再执行
      ```

  - 生成器返回语句

    - 生成器中添加return表示退出操作。

    - ```js
          function *createIterator() {
            let first = yield 1;
            return;                                      //return会结束函数，return后面的yield不会执行
            yield first + 2;
          }
          let i = createIterator();
          console.log(i.next()); // {value: 1, done: false}
          console.log(i.next()); // {value: undefined, done: true}
      ```

  - 委托生成器（即生成器嵌套生成器）

    - ```js
       function *aIterator() {
            yield 1;
          }
          function *bIterator() {
            yield 2;
          }
          function *cIterator() {
            yield *aIterator()
            yield *bIterator()
          }
          
          let i = cIterator();
          console.log(i.next()); // {value: 1, done: false}
          console.log(i.next()); // {value: 2, done: false}
      ```

      

- 异步任务执行器

  - 任务执行器：**任务执行器是一个函数，用来循环执行生成器，因为我们知道生成器需要执行N次next()方法，才能运行完，所以我们需要一个自动任务执行器帮我们做这些事情，这就是任务执行器的作用。** 

  - 下面编写一个异步任务执行器。

    ```js
        //taskDef是一个生成器函数，run是异步任务执行器
        function run(taskDef) {
          let task = taskDef(); //调用生成器
          let result = task.next(); //执行生成器的第一个next()，返回result
          function step() {
            if(!result.done) {
            //如果done为false，则继续执行next()，并且循环step，直到done为true退出。
              result = task.next(result.value);
              step();
            }
          }
          step(); //开始执行step()
        }
    ```

  - 测试：

    ```js
     run(function *() {
          let value = yield 1;
          value = yield value + 20;
          console.log(value) // 21
        })
    ```

    