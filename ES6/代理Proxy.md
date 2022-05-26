## 代理Proxy

- Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

- 代理的handler相当于对target设置了一层拦截，每次通过代理p访问target，都需要先进入handler进行判断和返回，如果handler为空，才会直接访问target

- 语法：

  - ```js
     let p = new Proxy(target, handler);
    
     // 在后续使用时，将p当作target使用
     ```
     
  - **target（要代理谁）**：一个目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)用Proxy来包装。

  - **handler（要做什么事情）**：一个对象，其属性是当执行一个操作时定义代理的行为的函数。

- 代理使用

  - **基础demo：**Proxy的demo有很多，这里主要看new Proxy({}, handler)的操作，**指定目标obj对象，然后handler对象执行get()操作**，get()返回值的判断是，如**果name是target目标对象的属性，则返回target[name]的值，否则返回37**，最后测试的时候，p.a是对象p的key，所以返回a的value，而p.b不存在，返回37。

  - ```js
    const obj = {
          a: 10
        }
    
    let handler = {
            get: function(target, name){
                console.log('test: ', target, name)  //get中的target是代理函数中的obj，name是obj中的属性名
                // test:  {"a":10} a
                // test:  {"a":10} b
                return name in target ? target[name] : 37
            },
            set: function(target,attr,value) {      //value是要给target[attr]设置的值
                if(value>=0&&value<150) {                //此处相当于设置拦截
                   target[attr] = value;
                } else {
                    console.warn("不能设置超出年龄范围的数字");
                }
            }
        }
    
    
        let p = new Proxy(obj, handler)
        console.log(p.a, p.b) // 10 37
        //p是代理函数，p.a指p中第一个参数obj的属性名a，将a传入第二个参数的name中，进行判断和操作
    
        p.a = 130;       //这样调用了set函数，为obj的a属性设置了值130(通过p对obj进行了修改，这就是代理)
        p.a = 200;         //会在set中生成警告 
    
    //这个例子的作用是拦截目标对象obj，当执行obj的读写操作时，进入handler函数进行判断，如果读取的key不存在，则返回默认值。
    ```

  
  
- 下面是 Proxy 支持的拦截操作一览，一共 13 种。

  - **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。

    - `get`方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（严格地说，是操作行为所针对的对象），其中最后一个参数可选。

    

  - **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。

    - `set`方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身，其中最后一个参数可选。
    - 注意，如果目标对象自身的某个属性不可写，那么`set`方法将不起作用。
    - 注意，`set`代理应当返回一个布尔值。严格模式下，`set`代理如果没有返回`true`，就会报错。

    

  - **has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值。

    - `has()`方法用来拦截`HasProperty`操作，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是`in`运算符。

    - `has()`方法可以接受两个参数，分别是目标对象、需查询的属性名。

    - ```js
      var handler = {
        has (target, key) {
          if (key[0] === '_') {
            return false;
          }
          return key in target;
        }
      };
      var target = { _prop: 'foo', prop: 'foo' };
      var proxy = new Proxy(target, handler);
      '_prop' in proxy // false
      
      //上面代码中，如果原对象的属性名的第一个字符是下划线，proxy.has()就会返回false，从而不会被in运算符发现。
      ```

      

  - **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值。

    - `deleteProperty`方法用于拦截`delete`操作，如果这个方法抛出错误或者返回`false`，当前属性就无法被`delete`命令删除。

    - ```js
      var handler = {
        deleteProperty (target, key) {
          invariant(key, 'delete');
          delete target[key];
          return true;
        }
      };
      function invariant (key, action) {
        if (key[0] === '_') {
          throw new Error(`Invalid attempt to ${action} private "${key}" property`);
        }
      }
      
      var target = { _prop: 'foo' };
      var proxy = new Proxy(target, handler);
      delete proxy._prop
      // Error: Invalid attempt to delete private "_prop" property
      
      // 上面代码中，deleteProperty方法拦截了delete操作符，删除第一个字符为下划线的属性会报错。
      ```

    

  - **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。

    - `ownKeys()`方法用来拦截对象自身属性的读取操作。具体来说，拦截以下操作。

      - `Object.getOwnPropertyNames()`
      - `Object.getOwnPropertySymbols()`
      - `Object.keys()`
      - `for...in`循环

      ```javascript
      let target = {
        a: 1,
        b: 2,
        c: 3
      };
      
      let handler = {
        ownKeys(target) {
          return ['a'];
        }
      };
      
      let proxy = new Proxy(target, handler);
      
      Object.keys(proxy)
      // [ 'a' ]
      
      // 上面代码拦截了对于target对象的Object.keys()操作，只返回a、b、c三个属性之中的a属性。
      ```

    

  - **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。

    - `getOwnPropertyDescriptor()`方法拦截`Object.getOwnPropertyDescriptor()`，返回一个属性描述对象或者`undefined`。

    - ```js
      var handler = {
        getOwnPropertyDescriptor (target, key) {
          if (key[0] === '_') {
            return;
          }
          return Object.getOwnPropertyDescriptor(target, key);
        }
      };
      var target = { _foo: 'bar', baz: 'tar' };
      var proxy = new Proxy(target, handler);
      Object.getOwnPropertyDescriptor(proxy, 'wat')
      // undefined
      Object.getOwnPropertyDescriptor(proxy, '_foo')
      // undefined
      Object.getOwnPropertyDescriptor(proxy, 'baz')
      // { value: 'tar', writable: true, enumerable: true, configurable: true }
      
      // 上面代码中，handler.getOwnPropertyDescriptor()方法对于第一个字符为下划线的属性名会返回undefined。
      ```

    

  - **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。

    - `defineProperty()`方法拦截了`Object.defineProperty()`操作。

    - ```js
      var handler = {
        defineProperty (target, key, descriptor) {
          return false;
        }
      };
      var target = {};
      var proxy = new Proxy(target, handler);
      proxy.foo = 'bar' // 不会生效
      
      // 上面代码中，defineProperty()方法内部没有任何操作，只返回false，导致添加新属性总是无效。注意，这里的false只是用来提示操作失败，本身并不能阻止添加新属性。
      ```

      

  - **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。

    - `preventExtensions()`方法拦截`Object.preventExtensions()`。该方法必须返回一个布尔值，否则会被自动转为布尔值。
    - 这个方法有一个限制，只有目标对象不可扩展时（即`Object.isExtensible(proxy)`为`false`），`proxy.preventExtensions`才能返回`true`，否则会报错。

    

  - **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。

    - `getPrototypeOf()`方法主要用来拦截获取对象原型。具体来说，拦截下面这些操作。

      - `Object.prototype.__proto__`
      - `Object.prototype.isPrototypeOf()`
      - `Object.getPrototypeOf()`
      - `Reflect.getPrototypeOf()`
      - `instanceof`

    - ```js
      var proto = {};
      var p = new Proxy({}, {
        getPrototypeOf(target) {
          return proto;
        }
      });
      Object.getPrototypeOf(p) === proto // true
      
      // 上面代码中，getPrototypeOf()方法拦截Object.getPrototypeOf()，返回proto对象。
      ```

    - 注意，`getPrototypeOf()`方法的返回值必须是对象或者`null`，否则报错。

      

  - **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值。

    - `isExtensible()`方法拦截`Object.isExtensible()`操作。

    - ```js
      var p = new Proxy({}, {
        isExtensible: function(target) {
          console.log("called");
          return true;
        }
      });
      
      Object.isExtensible(p)
      // "called"
      // true
      
      // 上面代码设置了isExtensible()方法，在调用Object.isExtensible时会输出called。
      ```

    - 注意，该方法只能返回布尔值，否则返回值会被自动转为布尔值。

      

  - **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。

    - `setPrototypeOf()`方法主要用来拦截`Object.setPrototypeOf()`方法。

    - ```js
      var handler = {
        setPrototypeOf (target, proto) {
          throw new Error('Changing the prototype is forbidden');
        }
      };
      var proto = {};
      var target = function () {};
      var proxy = new Proxy(target, handler);
      Object.setPrototypeOf(proxy, proto);
      // Error: Changing the prototype is forbidden
      
      // 上面代码中，只要修改target的原型对象，就会报错。
      ```

      

  - **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。

    - `apply`方法拦截函数的调用、`call`和`apply`操作。

    - `apply`方法可以接受三个参数，分别是目标对象、目标对象的上下文对象（`this`）和目标对象的参数数组。

    - ```js
      var target = function () { return 'I am the target'; };
      var handler = {
        apply: function () {
          return 'I am the proxy';
        }
      };
      
      var p = new Proxy(target, handler);
      
      p()
      // "I am the proxy"
      
      // 上面代码中，变量p是 Proxy 的实例，当它作为函数调用时（p()），就会被apply方法拦截，返回一个字符串。
      ```

    

  - **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。

    - `construct()`方法用于拦截`new`命令，下面是拦截对象的写法。

      ```js
      const handler = {
        construct (target, args, newTarget) {
          return new target(...args);
        }
      };
      ```

    - `construct()`方法可以接受三个参数。

      - `target`：目标对象。

      - `args`：构造函数的参数数组。

      - `newTarget`：创造实例对象时，`new`命令作用的构造函数（下面例子的`p`）。

      - ```js
        const p = new Proxy(function () {}, {
          construct: function(target, args) {
            console.log('called: ' + args.join(', '));
            return { value: args[0] * 10 };
          }
        });
        
        (new p(1)).value
        // "called: 1"
        // 10
        ```

    - `construct()`方法返回的必须是一个对象，否则会报错。

    - 另外，由于`construct()`拦截的是构造函数，所以它的目标对象必须是函数，否则就会报错。

    - 注意，`construct()`方法中的`this`指向的是`handler`，而不是实例对象。

  

- `Proxy.revocable()`

  - `Proxy.revocable()`方法返回一个可取消的 Proxy 实例。

  - ```js
    let target = {};
    let handler = {};
    
    let {proxy, revoke} = Proxy.revocable(target, handler);
    
    proxy.foo = 123;
    proxy.foo // 123
    
    revoke();
    proxy.foo // TypeError: Revoked
    ```

  - `Proxy.revocable()`方法返回一个对象，该对象的`proxy`属性是`Proxy`实例，`revoke`属性是一个函数，可以取消`Proxy`实例。上面代码中，当执行`revoke`函数之后，再访问`Proxy`实例，就会抛出一个错误。

  - `Proxy.revocable()`的一个使用场景是，目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

    

  

  

  

  #### Reflect

  - `Reflect`对象也是 ES6 新增的，意为"**映射**"，表示把其他内置对象中常用的方法映射到`Reflect`对象上。所以**只有静态方法**。`Reflect`对象的**设计目的**有这样几个：

    - **将**`Object`**对象的一些**明显属于语言内部的**方法放到**`Reflect`**对象上**。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，**未来的新方法将只部署在**`Reflect`**对象上**。也就是说，**从**`Reflect`**对象上可以拿到语言内部的方法**。
    - **修改某些**`Object`**方法的返回结果，让其变得更合理**。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。
    - **让**`Object`**操作都变成函数行为**。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为。
    - `Reflect`**对象的方法与**`Proxy`**对象的方法一一对应**，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。

  - `Reflect`对象一共有 13 个静态方法。

    - Reflect.apply(target, thisArg, args)

      - `Reflect.apply`方法等同于`Function.prototype.apply.call(func, thisArg, args)`，用于绑定`this`对象后执行给定函数。

      - 一般来说，如果要绑定一个函数的`this`对象，可以这样写`fn.apply(obj, args)`，但是如果函数定义了自己的`apply`方法，就只能写成`Function.prototype.apply.call(fn, obj, args)`，采用`Reflect`对象可以简化这种操作。

        ```js
        const ages = [11, 33, 12, 54, 18, 96];
        
        // 旧写法
        const youngest = Math.min.apply(Math, ages);
        const oldest = Math.max.apply(Math, ages);
        const type = Object.prototype.toString.call(youngest);
        
        // 新写法
        const youngest = Reflect.apply(Math.min, Math, ages);
        const oldest = Reflect.apply(Math.max, Math, ages);
        const type = Reflect.apply(Object.prototype.toString, youngest, []);
        ```

        

    - Reflect.construct(target, args)

      - `Reflect.construct`方法等同于`new target(...args)`，这提供了一种不使用`new`，来调用构造函数的方法。

        ```js
        function Greeting(name) {
          this.name = name;
        }
        
        // new 的写法
        const instance = new Greeting('张三');
        
        // Reflect.construct 的写法
        const instance = Reflect.construct(Greeting, ['张三']);
        ```

      - 如果`Reflect.construct()`方法的第一个参数不是<u>函数</u>，会报错。

        

    - Reflect.get(target, name, receiver)

      - `Reflect.get`方法查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`。

        ```js
        var myObject = {
          foo: 1,
          bar: 2,
          get baz() {
            return this.foo + this.bar;
          },
        }
        
        Reflect.get(myObject, 'foo') // 1
        Reflect.get(myObject, 'bar') // 2
        Reflect.get(myObject, 'baz') // 3
        ```

      - 如果`name`属性部署了读取函数（getter），则读取函数的`this`绑定`receiver`。

        ```js
        var myObject = {
          foo: 1,
          bar: 2,
          get baz() {
            return this.foo + this.bar;
          },
        };
        
        var myReceiverObject = {
          foo: 4,
          bar: 4,
        };
        
        Reflect.get(myObject, 'baz', myReceiverObject) // 8
        ```

      - 如果第一个参数不是<u>对象</u>，`Reflect.get`方法会报错。

        

    - Reflect.set(target, name, value, receiver)

      - `Reflect.set`方法设置`target`对象的`name`属性等于`value`。

        ```js
        var myObject = {
          foo: 1,
          set bar(value) {
            return this.foo = value;
          },
        }
        
        myObject.foo // 1
        
        Reflect.set(myObject, 'foo', 2);
        myObject.foo // 2
        
        Reflect.set(myObject, 'bar', 3)
        myObject.foo // 3
        ```

      - 如果`name`属性设置了赋值函数，则赋值函数的`this`绑定`receiver`。

        ```js
        var myObject = {
          foo: 4,
          set bar(value) {
            return this.foo = value;
          },
        };
        
        var myReceiverObject = {
          foo: 0,
        };
        
        Reflect.set(myObject, 'bar', 1, myReceiverObject);
        myObject.foo // 4
        myReceiverObject.foo // 1
        ```

      - 注意，如果 `Proxy`对象和 `Reflect`对象联合使用，前者拦截赋值操作，后者完成赋值的默认行为，而且传入了`receiver`，那么`Reflect.set`会触发`Proxy.defineProperty`拦截。

        ```js
        let p = {
          a: 'a'
        };
        
        let handler = {
          set(target, key, value, receiver) {
            console.log('set');
            Reflect.set(target, key, value, receiver)
          },
          defineProperty(target, key, attribute) {
            console.log('defineProperty');
            Reflect.defineProperty(target, key, attribute);
          }
        };
        
        let obj = new Proxy(p, handler);
        obj.a = 'A';
        // set
        // defineProperty
        
        // 上面代码中，Proxy.set拦截里面使用了Reflect.set，而且传入了receiver，导致触发Proxy.defineProperty拦截。这是因为Proxy.set的receiver参数总是指向当前的 Proxy实例（即上例的obj），而Reflect.set一旦传入receiver，就会将属性赋值到receiver上面（即obj），导致触发defineProperty拦截。如果Reflect.set没有传入receiver，那么就不会触发defineProperty拦截。
        ```

        

    - Reflect.defineProperty(target, name, desc)

      - `Reflect.defineProperty`方法基本等同于`Object.defineProperty`，用来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使用`Reflect.defineProperty`代替它。

        ```js
        function MyDate() {
          /*…*/
        }
        
        // 旧写法
        Object.defineProperty(MyDate, 'now', {
          value: () => Date.now()
        });
        
        // 新写法
        Reflect.defineProperty(MyDate, 'now', {
          value: () => Date.now()
        });
        ```

      - 如果`Reflect.defineProperty`的第一个参数不是对象，就会抛出错误

      - **这个方法可以与`Proxy.defineProperty`配合使用。**

        ```js
        const p = new Proxy({}, {
          defineProperty(target, prop, descriptor) {
            console.log(descriptor);
            return Reflect.defineProperty(target, prop, descriptor);
          }
        });
        
        p.foo = 'bar';
        // {value: "bar", writable: true, enumerable: true, configurable: true}
        
        p.foo // "bar"
        
        // 上面代码中，Proxy.defineProperty对属性赋值设置了拦截，然后使用Reflect.defineProperty完成了赋值。
        ```

        

    - Reflect.deleteProperty(target, name)

      - `Reflect.deleteProperty`方法等同于`delete obj[name]`，用于删除对象的属性。

        ```js
        const myObj = { foo: 'bar' };
        
        // 旧写法
        delete myObj.foo;
        
        // 新写法
        Reflect.deleteProperty(myObj, 'foo');
        ```

      - 该方法返回一个布尔值。如果删除成功，或者被删除的属性不存在，返回`true`；删除失败，被删除的属性依然存在，返回`false`。

      - 如果`Reflect.deleteProperty()`方法的第一个参数不是<u>对象</u>，会报错。

        

    - Reflect.has(target, name)

      - `Reflect.has`方法对应`name in obj`里面的`in`运算符。

        ```js
        var myObject = {
          foo: 1,
        };
        
        // 旧写法
        'foo' in myObject // true
        
        // 新写法
        Reflect.has(myObject, 'foo') // true
        ```

      - 如果`Reflect.has()`方法的第一个参数不是<u>对象</u>，会报错。

        

    - **Reflect.ownKeys(target)**

      - `Reflect.ownKeys`方法用于返回对象的所有属性，基本等同于`Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`之和。

        ```js
        var myObject = {
          foo: 1,
          bar: 2,
          [Symbol.for('baz')]: 3,
          [Symbol.for('bing')]: 4,
        };
        
        // 旧写法
        Object.getOwnPropertyNames(myObject)
        // ['foo', 'bar']
        
        Object.getOwnPropertySymbols(myObject)
        //[Symbol(baz), Symbol(bing)]
        
        // 新写法
        Reflect.ownKeys(myObject)
        // ['foo', 'bar', Symbol(baz), Symbol(bing)]
        ```

      - 如果`Reflect.ownKeys()`方法的第一个参数不是对象，会报错。

        

    - Reflect.isExtensible(target)

      - `Reflect.isExtensible`方法对应`Object.isExtensible`，返回一个布尔值，表示当前对象是否可扩展。

        ```js
        const myObject = {};
        
        // 旧写法
        Object.isExtensible(myObject) // true
        
        // 新写法
        Reflect.isExtensible(myObject) // true
        ```

      - 如果参数不是对象，`Object.isExtensible`会返回`false`，因为非对象本来就是不可扩展的，而`Reflect.isExtensible`会报错。

        

    - Reflect.preventExtensions(target)

      - `Reflect.preventExtensions`对应`Object.preventExtensions`方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。

        ```js
        var myObject = {};
        
        // 旧写法
        Object.preventExtensions(myObject) // Object {}
        
        // 新写法
        Reflect.preventExtensions(myObject) // true
        ```

      - 如果参数不是对象，`Object.preventExtensions`在 ES5 环境报错，在 ES6 环境返回传入的参数，而`Reflect.preventExtensions`会报错。

        

    - Reflect.getOwnPropertyDescriptor(target, name)

      - `Reflect.getOwnPropertyDescriptor`基本等同于`Object.getOwnPropertyDescriptor`，用于得到指定属性的描述对象，将来会替代掉后者。

        ```js
        var myObject = {};
        Object.defineProperty(myObject, 'hidden', {
          value: true,
          enumerable: false,
        });
        
        // 旧写法
        var theDescriptor = Object.getOwnPropertyDescriptor(myObject, 'hidden');
        
        // 新写法
        var theDescriptor = Reflect.getOwnPropertyDescriptor(myObject, 'hidden');
        ```

      - `Reflect.getOwnPropertyDescriptor`和`Object.getOwnPropertyDescriptor`的一个区别是，如果第一个参数不是对象，`Object.getOwnPropertyDescriptor(1, 'foo')`不报错，返回`undefined`，而`Reflect.getOwnPropertyDescriptor(1, 'foo')`会抛出错误，表示参数非法。

        

    - Reflect.getPrototypeOf(target)

      - `Reflect.getPrototypeOf`方法用于读取对象的`__proto__`属性，对应`Object.getPrototypeOf(obj)`。

        ```js
        const myObj = new FancyThing();
        
        // 旧写法
        Object.getPrototypeOf(myObj) === FancyThing.prototype;
        
        // 新写法
        Reflect.getPrototypeOf(myObj) === FancyThing.prototype;
        ```

      - 注意： `Reflect.getPrototypeOf`和`Object.getPrototypeOf`的一个区别是，如果参数不是对象，`Object.getPrototypeOf`会将这个参数转为对象，然后再运行，而`Reflect.getPrototypeOf`会报错。

        

    - Reflect.setPrototypeOf(target, prototype)

      - `Reflect.setPrototypeOf`方法用于设置目标对象的原型（prototype），对应`Object.setPrototypeOf(obj, newProto)`方法。它返回一个布尔值，表示是否设置成功。

        ```js
        const myObj = {};
        
        // 旧写法
        Object.setPrototypeOf(myObj, Array.prototype);
        
        // 新写法
        Reflect.setPrototypeOf(myObj, Array.prototype);
        
        myObj.length // 0
        ```

      - 如果无法设置目标对象的原型（比如，目标对象禁止扩展），`Reflect.setPrototypeOf`方法返回`false`。

      - 如果第一个参数不是对象，`Object.setPrototypeOf`会返回第一个参数本身，而`Reflect.setPrototypeOf`会报错。

      - 如果第一个参数是`undefined`或`null`，`Object.setPrototypeOf`和`Reflect.setPrototypeOf`都会报错。

    

  - 使用Proxy实现观察者模式

    - 观察者模式（Observer mode）指的是函数自动观察数据对象，一旦对象有变化，函数就会自动执行。

    - ```js
      const person = observable({
        name: '张三',
        age: 20
      });
      
      function print() {
        console.log(`${person.name}, ${person.age}`)
      }
      
      observe(print);
      person.name = '李四';
      // 输出
      // 李四, 20
      
      // 上面代码中，数据对象person是观察目标，函数print是观察者。一旦数据对象发生变化，print就会自动执行。
      ```

    - 下面，使用 Proxy 写一个观察者模式的最简单实现，即实现`observable`和`observe`这两个函数。思路是`observable`函数返回一个原始对象的 Proxy 代理，拦截赋值操作，触发充当观察者的各个函数。

      ```js
      const queuedObservers = new Set();
      
      const observe = fn => queuedObservers.add(fn);
      const observable = obj => new Proxy(obj, {set});
      
      function set(target, key, value, receiver) {
        const result = Reflect.set(target, key, value, receiver);
        queuedObservers.forEach(observer => observer());
        return result;
      }
      
      // 上面代码中，先定义了一个Set集合，所有观察者函数都放进这个集合。然后，observable函数返回原始对象的代理，拦截赋值操作。拦截函数set之中，会自动执行所有观察者。
      ```

  - 

  

  

  

  

  

  