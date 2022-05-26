## ES6

- ES6简介

  - ES6的全称是ECMAScript，它是由ECMA国际标准化组织制定的**一项脚本语言的标准化规范**

  - 为什么使用ES6？

    每一次标准的诞生都意味着语言的完善，功能的加强。JavaScript语言本身也有一些令人不满意的地方

    - 变量提升特性增加了程序运行时的不可预测性
    - 语法过于松散，实现相同的功能，不同的人可能会写出不同的代码



- ES6中的新增语法

  - let

    - ES6中新增的用于声明变量的关键字

    - **let不存在变量提升**（只能先声明再使用）

      ```js
      console.log(a);          //a is not defined
      let a = 20;
      ```

    - let声明的变量只在所处于的块级有效(let关键字声明的变量具有块级作用域)

      ```js
      if(true) {
          let a =10;
      }
      console.log(a);          //a is not defined
      ```

    - 暂时性死区

      ```js
      var tmp = 123;
      if(true) {
          console.log(tmp);           //因为let中定义了tmp，所以tmp和if这个大括号绑定了，所有在大括号内对tmp的操作均是对于大括号中的tmp的操作，与大括号外的tmp无关，此处为tmp is not defined
          let tmp;
      }
      ```

    - 注意：使用let关键字声明的变量才具有块级作用域（在当前大括号中），使用var声明的变量不具备块级作用域特性

    - 目的：防止循环变量变成全局变量

    - 经典面试题

      ```js
      题目一：
      var arr = [];
      for(var i = 0;i < 2;i++) {
          arr[i] = function() {
              console.log(i);
          }
      }
      arr[0]();                     //输出2，因为var出来的i是全局变量，最后console.log出来的i是全局变量中的i=2
      arr[1]();                     //同上
      
      题目二：
      let arr = [];
      for(let i = 0;i < 2;i++) {
          arr[i] = function() {
              console.log(i);
          }
      }
      arr[0]();                //输出0，因为每次循环都会产生一个块级作用域，每个块级作用域中的i都是不同的，在arr[0]()函数执行时，输出的是自己function（）的上一级（即for循环的大括号）作用域下的i的值，即为0   
      arr[1]();                //输出1，同上
      ```

    

  - const

    - 作用：声明常量，常量就是**值（内存地址）**不能变化的量

    - 普通类型不能改值，引用类型不能改地址

    - 具有块级作用域

      ```js
      if(true) {
          const a = 10;
      }
      console.log(a);        //a is not defined
      ```

    - 声明常量时必须赋值

      ```js
      const PI;        //Missing initializer in const declaration
      ```

    - 常量赋值后，值不能更改

      ```js
      const PI = 3.14;
      PI = 100;       //Assignment to constant variable
      
      const ary = [100,200];
      ary[0] = 'a';
      ary[1] = 'b';
      console.log(ary);        //['a','b'],更改成功，因为此操作没有更改ary的内存地址
      ary = ['a','b'];       //Assignment to constant variable，此操作更改了ary的内存地址（因为用一个新数组给其赋值了）
      ```

  - let、const、var的区别

    - 使用**var**声明的变量，其作用域为**该语句所在的函数内，且存在变量提升现象**

    - 使用**let**声明的变量，其作用域为**该语句所在的代码块内，不存在变量提升**

    - 使用**const**声明的是常量，在后面出现的代码中**不能再修改该常量的值**

    - | var          | let            | const          |
      | ------------ | -------------- | -------------- |
      | 函数级作用域 | 块级作用域     | 块级作用域     |
      | 变量提升     | 不存在变量提升 | 不存在变量提升 |
      | 值可更改     | 值可更改       | 值不可更改     |

      

  - 解构赋值

    - ES6中允许从数组中提取值，按照对应位置，对变量赋值。对象也可以实现解构

    - 解构赋值：按照一定模式，从数组中或对象中提取值，将提取出来的值赋值给另外的变量

    - 数组解构

      ```js
      let [a,b,c] = [1,2,3];
      console.log(a);
      console.log(b);
      console.log(c);
      
      如果解构不成功，变量的值为undefined
      let [foo] = [];
      let [bar,foo] = [1];         //foo的值均为undefined
      ```

    - 对象解构

      ```js
      第一种写法：
      let person = {name:'zhangsan',age:20};
      let {name,age} = person;
      console.log(name);     //'zhangsan'
      console.log(age);        //20
      
      第二种写法：
      let {name:myName,age:myAge} = person;      //myName,myAge属于别名
      console.log(myName);     //'zhangsan'
      console.log(myAge);      //20
      ```

      

  - 箭头函数

    - ES6中新增的定义函数的方式

      ```js
      () => {}
      const fn = () => {}
      ```

    - 如果函数体中只有一句代码，且代码的执行结果就是返回值，可以省略大括号

      ```js
      function sum(num1,num2) {
          return num1 + num2;
      }
      const sum = (num1,num2) => num1 + num2;
      ```

    - 如果形参只有一个，可以省略小括号

      ```js
      function fn(v) {
          return v;
      }
      cosnt fn = v => v;
      ```

    - 箭头函数不绑定this关键字，箭头函数中的this，指向的是**函数定义位置的上下文this**

      ```js
      const obj = {name:'张三'};
      function fn() {
          console.log(this);
          return () => {
              console.log(this);      //箭头函数中的this指向箭头函数定义的位置，即fn，因此此this为fn的this
          }
      }
      const resFn = fn.call(obj);         //此处用call函数将fn的this指向obj对象，因此箭头函数中的this也指向obj对象
      resFn();                                //因此此两个输出的this均为obj对象
      ```

    - 箭头函数面试题

      ```js
      问：弹出的值是什么？
      var obj = {
          age:20,
          say:() => {
              alert(this.age)           //当前箭头函数定义在obj对象中，因为obj对象不能产生作用域，因此箭头函数定义在全局作用域下，因此this指向的是window，而window对象中没有age属性，因此弹出的是undefined
          }
      }
      obj.say();
      ```

  - 剩余参数

    - 剩余参数语法允许我们将一个不定数量的参数表示为一个数组(...args是一个数组)

      ```js
    function sum (first, ...args) {
          console.log(first);    //10
          console.log(args);        //[20,30]
      }
      sum(10,20,30)
      
      //定义一个能计算n个值的函数
      const sum = (...args) => {
          let total = 0;
          args.forEach((item) => {
              total += item;
          })
          return total;
      };
      sum(10,20);
      sum(10,20,30);
      ```
  
    - 剩余参数和解构配合使用

      ```js
    let students = ['wangwu','zhangsan','lisi'];
      let [s1,...s2] = students;
      console.log(s1);     //'wangwu'
      console.log(s2);     //['zhangsan','lisi']
      ```
  
      

- ES6的内置对象扩展

  - Array的扩展方法

    - 扩展运算符（展开语法）

      - 扩展运算符可以将数组或者对象转为用逗号分隔的参数序列

        ```js
        let ary = [1,2,3];
        ...ary  //1,2,3
        console.log(...ary);          //1 2 3(因为逗号被console.log当作自身的参数分隔符，被省略了)
        ```

      - 扩展运算符可以应用于**合并数组**

        ```js
        //方法一
        let ary1 = [1,2,3];
        let ary2 = [3,4,5];
        let ary3 = [...ary1,...ary2];
        //方法二
        ary1.push(...ary2);
        ```

      - 扩展运算符可以将类数组或可遍历对象转换为真正的数组

        ```js
        let oDivs = document.getElementsByTagName('div');
        oDivs = {...oDivs};
        ```

    - 构造函数方法：Array.from()

      - 将类数组或可遍历对象转换为真正的数组

        ```js
        let arrayLike = {
            '0':'a',
            '1':'b',
            '2':'c',
            length:3              //此处的length是必需的，若没有，则返回空数组
        };
        let arr2 = Array.from(arrayLike);      //['a','b','c']
        ```

      - 方法还可以接收第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组(类似于map()方法)

        ```js
        let arrayLike = {
            "0":1,
            "1":2,
            "length":2
        }
        let newAry = Array.from(aryLike,(item) => {
            return item * 2;
        });
        ```

    - 实例方法：find()

      - 用于找出**第一个符合条件的数组成员**，如果**没有找到，返回undefined**

      - ```js
        ary.find((item,index) => {});
        find中参数为一个函数，item为当前成员，index为当前索引号
        ```

      - ```js
        let ary = [{
            id:1,
            name:'张三'
        },{
            id:2,
            name:'李四'
        }];
        let target = ary.find((item,index) => {
            item.id == 2
        });                    //用target接收返回的对象
        ```

    - 实例方法：findIndex()

      - 用于找出**第一个符合条件的数组成员的位置**，如果**没有找到返回-1**

      - ```js
        ary.findIndex((value,index) => {});
        findIndex中为一个函数，item为当前成员，index为当前索引号
        ```

      - ```js
        let ary = [1,5,10,15];
        let index = ary.findIndex((value,index) => {
            value > 9;
        });
        console.log(index);       //2
        ```

      

    - 实例方法：entries()，keys()和values()

      - 们都返回一个遍历器对象，可以用`for...of`循环进行遍历
  
      - 它们都是用于遍历数组，区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。
      
      - ```js
        for (let index of ['a', 'b'].keys()) {
          console.log(index);
        }
        // 0
        // 1
        
        for (let elem of ['a', 'b'].values()) {
          console.log(elem);
        }
        // 'a'
        // 'b'
        
        for (let [index, elem] of ['a', 'b'].entries()) {
          console.log(index, elem);
        }
        // 0 "a"
        // 1 "b"
        ```
      
    - 实例方法：flat()

      - falt()方法用来"拉平"数组，即将多维数组转为低维数组，参数是要拉平的层数
  
      - ```js
        [1, 2, [3, [4, 5]]].flat()         // 默认拉平一层
        // [1, 2, 3, [4, 5]]
        
        [1, 2, [3, [4, 5]]].flat(2)
        // [1, 2, 3, 4, 5]
        ```
      
      - 如果不管有多少层嵌套，都要转成一维数组，可以用`Infinity`关键字作为参数。
      
        ```js
        [1, [2, [3]]].flat(Infinity)
        // [1, 2, 3]
        ```
      
      - 如果原数组有空位，`flat()`方法会跳过空位。
      
    - 实例方法：includes()
  
      - 表示某个数组是否包含给定的值，返回布尔值

      - ```js
        [1,2,3].includes[2]      //true
        [1,2,3].includes[4]      //false
        ```
  
        
  
  - 数值的扩展
  
    - Number.isFinite() 和 Number.isNaN()
  
      - `Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是`Infinity`。

      - `Number.isNaN()`用来检查一个值是否为`NaN`。

      - 与`isFinite()`和`isNaN()`的区别：`isFinite()`和`isNaN()`会先把非数值的值转化为数值，再判断

        ```js
        isFinite(25) // true
        isFinite("25") // true
        Number.isFinite(25) // true
        Number.isFinite("25") // false
        
        isNaN(NaN) // true
        isNaN("NaN") // true
        Number.isNaN(NaN) // true
        Number.isNaN("NaN") // false
        Number.isNaN(1) // false
        ```
  
    - Number.EPSILON
  
      - 它表示 1 与大于 1 的最小浮点数之间的差。
  
      - `Number.EPSILON`可以用来设置“能够接受的误差范围”。比如，误差范围设为 2 的-50 次方（即`Number.EPSILON * Math.pow(2, 2)`），即如果两个浮点数的差小于这个值，我们就认为这两个浮点数相等。
  
      - ```js
        function withinErrorMargin (left, right) {
          return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
        }
        
        0.1 + 0.2 === 0.3 // false
        withinErrorMargin(0.1 + 0.2, 0.3) // true
        
        1.1 + 1.3 === 2.4 // false
        withinErrorMargin(1.1 + 1.3, 2.4) // true
        
        // 上面的代码为浮点数运算，部署了一个误差检查函数。
        ```
  
      
  
  - 对象的扩展
  
    - 对象属性的遍历
  
      - `for...in`
        - `for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。
      - `Object.keys`
        - `Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。
      - `Object.getOwnPropertyNames`
        - `Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。
      - `Object.getOwnPropertySymbols`
        - `Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名。
      - `Reflect.ownKeys`
        - `Reflect.ownKeys`返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
      - 以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。
        - 首先遍历所有数值键，按照数值升序排列。
        - 其次遍历所有字符串键，按照加入时间升序排列。
        - 最后遍历所有 Symbol 键，按照加入时间升序排列。
  
    - `Object.assign`
  
      - `Object.assign()`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
  
        ```js
        const target = { a: 1 };
        
        const source1 = { b: 2 };
        const source2 = { c: 3 };
        
        Object.assign(target, source1, source2);
        target // {a:1, b:2, c:3}
        
        // 第一个参数是目标对象，后面的参数都是源对象。
        ```
  
      - 如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
  
        ```js
        const target = { a: 1, b: 1 };
        
        const source1 = { b: 2, c: 2 };
        const source2 = { c: 3 };
        
        Object.assign(target, source1, source2);
        target // {a:1, b:2, c:3}
        ```
  
      - 如果只有一个参数，`Object.assign()`会直接返回该参数。
  
      - 如果该参数不是对象，则会先转成对象，然后返回。
  
      - 由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。
  
      - 注意：
  
        - `Object.assign()`方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。
        - `Object.assign()`对于同名属性的处理方法是替换，而不是添加。
  
    - `Object.getOwnPropertyDescriptors()`
  
      - `Object.getOwnPropertyDescriptors()`方法，返回指定对象所有自身属性（非继承属性）的描述对象。
  
        ```js
        const obj = {
          foo: 123,
          get bar() { return 'abc' }
        };
        
        Object.getOwnPropertyDescriptors(obj)
        // { foo:
        //    { value: 123,
        //      writable: true,
        //      enumerable: true,
        //      configurable: true },
        //   bar:
        //    { get: [Function: get bar],
        //      set: undefined,
        //      enumerable: true,
        //      configurable: true } }
        ```
  
    - `Object.setPrototypeOf`
  
      - 用来设置一个对象的原型对象（prototype），返回参数对象本身
  
        ```js
        // 格式
        Object.setPrototypeOf(object, prototype)
        
        // 用法
        const o = Object.setPrototypeOf({}, null);
        
        // 例子
        let proto = {};
        let obj = { x: 10 };
        Object.setPrototypeOf(obj, proto);
        
        proto.y = 20;
        proto.z = 40;
        
        obj.x // 10
        obj.y // 20
        obj.z // 40
        ```
  
      - 如果第一个参数不是对象，会自动转为对象。但是由于返回的还是第一个参数，所以这个操作不会产生任何效果。
  
      - 由于`undefined`和`null`无法转为对象，所以如果第一个参数是`undefined`或`null`，就会报错。
  
    - `Object.getPrototypeOf`
  
      - ，用于读取一个对象的原型对象。
  
        ```js
        // 例子
        function Rectangle() {
          // ...
        }
        
        const rec = new Rectangle();
        
        Object.getPrototypeOf(rec) === Rectangle.prototype
        // true
        
        Object.setPrototypeOf(rec, Object.prototype);
        Object.getPrototypeOf(rec) === Rectangle.prototype
        // false
        ```
  
      - 如果参数不是对象，会被自动转为对象。
  
      - 如果参数是`undefined`或`null`，它们无法转为对象，所以会报错。
  
      - 
  
      - d
  
      - 
  
    - `Object.fromEntries()`
  
      - 方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象。
  
        ```js
        Object.fromEntries([
          ['foo', 'bar'],
          ['baz', 42]
        ])
        // { foo: "bar", baz: 42 }
        ```
  
    
  
  - 运算符的扩展
  
    - 指数运算符（`**`）
  
      - 这个运算符的一个特点是右结合，而不是常见的左结合。多个指数运算符连用时，是从最右边开始计算的。
  
      - ```js
        2 ** 2 // 4
        2 ** 3 // 8
        
        // 相当于 2 ** (3 ** 2)
        2 ** 3 ** 2
        // 512
        ```
  
      - 指数运算符可以与等号结合，形成一个新的赋值运算符（`**=`）。
  
        ```js
        let a = 1.5;
        a **= 2;
        // 等同于 a = a * a;
        
        let b = 4;
        b **= 3;
        // 等同于 b = b * b * b;
        ```
  
    - 链运算判断符(`?.`)
  
      - 直接在链式调用的时候判断，左侧的对象是否为`null`或`undefined`。如果是的，就不再往下运算，而是返回`undefined`。
  
        ```js
        const firstName = message?.body?.user?.firstName || 'default';
        
        const fooValue = myForm.querySelector('input[name=foo]')?.value
        ```
  
      - 链判断运算符`?.`有三种写法。
  
        - `obj?.prop` // 对象属性是否存在
        - `obj?.[expr]` // 同上
        - `func?.(...args)` // 函数或对象方法是否存在
  
    - Null 判断运算符`??`
  
      - 它的行为类似`||`，但是只有运算符左侧的值为`null`或`undefined`时，才会返回右侧的值。
  
      - ```js
        const headerText = response.settings.headerText ?? 'Hello, world!';
        const animationDuration = response.settings.animationDuration ?? 300;
        const showSplashScreen = response.settings.showSplashScreen ?? true;
        //上面代码中，默认值只有在左侧属性值为null或undefined时，才会生效。
        ```
  
    - 逻辑赋值运算符`||=`、`&&=`、`??=`
  
      - ```js
        // 或赋值运算符
        x ||= y
        // 等同于
        x || (x = y)
        
        // 与赋值运算符
        x &&= y
        // 等同于
        x && (x = y)
        
        // Null 赋值运算符
        x ??= y
        // 等同于
        x ?? (x = y)
        ```
  
      - 它们的一个用途是，为变量或属性设置默认值。
  
        ```js
        // 老的写法
        user.id = user.id || 1;
        
        // 新的写法
        user.id ||= 1;
        
        // 上面示例中，user.id属性如果不存在，则设为1，新的写法比老的写法更紧凑一些
        ```
  
        
  
  - string的扩展方法
  
    - 模板字符串
  
      - ES6新增的创建字符串的方式，使用反引号定义
  
      - ```js
        let name = `zhangsan`;
        ```
  
      - 模板字符串可以解析变量
  
        ```js
        let name = 'zhangsan';
        let sayHello = `hello,my name is ${name}`;         //hello,my name is zhangsan
        ```
  
      - 模板字符串中**可以换行**
  
        ```js
        let result = {
            name:'zhangsan',
            age:20,
            sex:"男"
        }
        let html = `<div>
            <span>${result.name}</span>
        <span>${result.age}</span>
        <span>${result.sex}</span>
        </div>`;
        ```
  
      - 模板字符串中可以**调用函数**(调用函数处会显示函数的返回值)
  
        ```js
        const sayHello = function() {
            return 'lloollo';
        }
        let greet = '${sayHello()}哈哈哈';
        console.log(greet);           //lloollo哈哈哈
        ```
  
    - 实例方法：stratsWith()和endsWith()
  
      - startsWith()：表示参数字符串是否在原字符串的头部，返回布尔值
  
      - endsWith()：表示参数字符串是否在原字符串的尾部，返回布尔值
  
      - ```js
        let str = 'Hello World!';
        str.startsWith('Hello')    //true
        str.endsWith('!');          //true
        //注意此两方法区分大小写
        ```
  
      
  
    - 实例方法：fromCodepiont() 和 codePiontAt()
  
      - 均能够正确处理 4 个字节储存的字符，返回一个字符的码点。
  
      - fromCodepiont() 可以将码点转化为字符，而codePiontAt()可以将字符转化为码点
  
      - ```js
                let s = '🌹'
                console.log(s.codePointAt(0));              // 127801
                console.log(String.fromCodePoint(127801));    //  🌹
        ```
  
      
  
    - 实例方法： padStart() 和 padEnd()
  
      - 如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。
  
      - ```js
        'x'.padStart(5, 'ab') // 'ababx'
        'x'.padStart(4, 'ab') // 'abax'
        
        'x'.padEnd(5, 'ab') // 'xabab'
        'x'.padEnd(4, 'ab') // 'xaba'
        ```
  
      - `padStart()`和`padEnd()`一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。
  
      - 如果省略第二个参数，默认使用空格补全长度。
  
        
  
    - 实例方法： trimStart() 和 trimEnd()
  
      - `trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。
  
      - ```js
        const s = '  abc  ';
        
        s.trim() // "abc"
        s.trimStart() // "abc  "
        s.trimEnd() // "  abc"
        ```
  
      - `trimLeft()`是`trimStart()`的别名，`trimRight()`是`trimEnd()`的别名。
  
        
  
    - 实例方法：repeat()
  
      - repeat方法表示将原字符串重复n次，返回一个新字符串
  
      - ```js
        'x'.repeat(3);        //"xxx"
        'hello'.repeat(2);        //"hellohello"
        ```
  
      - 注意：
  
        - 参数如果是小数，会被取整。**（向下取整）**
        - 如果`repeat`的参数是负数或者`Infinity`，会报错。
        - 参数`NaN`等同于 0。
        - 如果`repeat`的参数是字符串，则会先转换成数字。
  
        
  
  - Symbol
  
    - 概念：ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。
  
    - 特点
    
      - Symbol属性对应的值是唯一的，解决**命名冲突问题**
      - Symbol值不能与其他数据进行计算，包括同字符串拼串
      - for in、for of 遍历时不会遍历Symbol属性。
    
    - 创建Symbol属性值
    
      - Symbol是函数，但并不是构造函数。创建一个Symbol数据类型：
    
        ```js
         let mySymbol = Symbol();
         console.log(typeof mySymbol);  //打印结果：symbol
         console.log(mySymbol);         //打印结果：Symbol()
        ```
    
    - Symbol的使用
    
      - 将Symbol作为对象的属性值
    
        ```js
        let mySymbol = Symbol();
            let obj = {
                name: 'smyhvae',
                age: 26
            };
            //obj.mySymbol = 'male'; //错误：不能用 . 这个符号给对象添加 Symbol 属性。
            obj[mySymbol] = 'hello';    //正确：通过属性选择器给对象添加 Symbol 属性。后面的属性值随便写。
            console.log(obj);
        ```
  
      - 创建Symbol属性值时，传参作为标识
    
        ```js
        //如果通过 Symbol()函数创建了两个值，这两个值是不一样的：
            let mySymbol1 = Symbol();
            let mySymbol2 = Symbol();
            console.log(mySymbol1 == mySymbol2); //打印结果：false
            console.log(mySymbol1);         //打印结果：Symbol()
            console.log(mySymbol2);         //打印结果：Symbol()
        ```
    
      - 最后两行的打印结果却发现，二者的打印输出，肉眼看到的却相同。那该怎么区分它们呢？
    
        既然Symbol()是函数，函数就可以传入参数，我们可以通过参数的不同来作为**标识**。比如：
    
        ```js
          //在括号里加入参数，来标识不同的Symbol
            let mySymbol1 = Symbol('one');
            let mySymbol2 = Symbol('two');
        
            console.log(mySymbol1 == mySymbol2); //打印结果：false
            console.log(mySymbol1);         //打印结果：Symbol(one)
            console.log(mySymbol2);         //打印结果：Symbol(two)。颜色为红色。
            console.log(mySymbol2.toString());//打印结果：Symbol(two)。颜色为黑色。
        ```
    
      - Symbol.prototype.description
    
        - 可以直接返回 Symbol 的描述。
    
        - ```js
          const sym = Symbol('foo');
          
          sym.description // "foo"
          ```
        
      - `Object.getOwnPropertySymbols()`
      
        - 可以获取指定对象的所有 Symbol 属性名。该方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
      
          ```js
          const obj = {};
          let a = Symbol('a');
          let b = Symbol('b');
          
          obj[a] = 'Hello';
          obj[b] = 'World';
          
          const objectSymbols = Object.getOwnPropertySymbols(obj);
          
          objectSymbols
          // [Symbol(a), Symbol(b)]
          ```
        
      - Symbol.for() 和 Symbol.keyFor()
      
        - 如果我们希望重新使用同一个 Symbol 值，`Symbol.for()`方法可以做到这一点.它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。
      
          ```js
          let s1 = Symbol.for('foo');
          let s2 = Symbol.for('foo');
          
          s1 === s2 // true
          
          //s1和s2都是 Symbol 值，但是它们都是由同样参数的Symbol.for方法生成的，所以实际上是同一个值。
          ```
        
        - `Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`key`。(即Symbol.for声明的要用keyFor来询问)
        
          ```js
          let s1 = Symbol.for("foo");
          Symbol.keyFor(s1) // "foo"
          
          let s2 = Symbol("foo");
          Symbol.keyFor(s2) // undefined
          
          // 上面代码中，变量s2属于未登记的 Symbol 值，所以返回undefined。
          ```
        
      - 定义常量
      
        - Symbol 可以用来定义常量：
      
          ```js
             const MY_NAME = Symbol('my_name');
          ```
      
  
  
  
  
  
  
  
  - Map数据结构
  
    - Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。
  
    - ```js
      const m = new Map();
      const o = {p: 'Hello World'};
      
      m.set(o, 'content')
      m.get(o) // "content"
      
      m.has(o) // true
      m.delete(o) // true
      m.has(o) // false
      
      //上面代码使用 Map 结构的set方法，将对象o当作m的一个键，然后又使用get方法读取这个键，接着使用delete方法删除了这个键。
      ```
  
    - 作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。
  
      ```js
      const map = new Map([
        ['name', '张三'],
        ['title', 'Author']
      ]);
      
      map.size // 2
      map.has('name') // true
      map.get('name') // "张三"
      map.has('title') // true
      map.get('title') // "Author"
      ```
  
    - 如果对同一个键多次赋值，**后面的值将覆盖前面的值。**
  
    - 如果读取一个未知的键，则返回`undefined`。
  
    - Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题
  
    - 实例方法
  
      - `size`属性返回 Map 结构的成员总数。
      - `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。
      - `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。
      - `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中
      - `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。
      - `clear`方法清除所有成员，没有返回值。
  
    - 遍历方法
  
      - `Map.prototype.keys()`：返回键名的遍历器。
      - `Map.prototype.values()`：返回键值的遍历器。
      - `Map.prototype.entries()`：返回所有成员的遍历器。
      - `Map.prototype.forEach()`：遍历 Map 的所有成员。
      - **需要特别注意的是，Map 的遍历顺序就是插入顺序。**
  
    ​	
  
  - `Weakmap	`数据结构
  
    - `WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。
  
      ```js
      // WeakMap 可以使用 set 方法添加成员
      const wm1 = new WeakMap();
      const key = {foo: 1};
      wm1.set(key, 2);
      wm1.get(key) // 2
      
      // WeakMap 也可以接受一个数组，
      // 作为构造函数的参数
      const k1 = [1, 2, 3];
      const k2 = [4, 5, 6];
      const wm2 = new WeakMap([[k1, 'foo'], [k2, 'bar']]);
      wm2.get(k2) // "bar"
      ```
  
    - `WeakMap`与`Map`的区别
  
      - 首先，`WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。
  
        ```js
        const map = new WeakMap();
        map.set(1, 2)
        // TypeError: 1 is not an object!
        map.set(Symbol(), 2)
        // TypeError: Invalid value used as weak map key
        map.set(null, 2)
        // TypeError: Invalid value used as weak map key
        ```
  
      - 其次，`WeakMap`的键名所指向的对象，不计入垃圾回收机制。
  
        - 一个典型应用场景是，在网页的 DOM 元素上添加数据，就可以使用`WeakMap`结构。当该 DOM 元素被清除，其所对应的`WeakMap`记录就会自动被移除。
  
          ```js
          const wm = new WeakMap();
          
          const element = document.getElementById('example');
          
          wm.set(element, 'some information');
          wm.get(element) // "some information"
          
          //上面代码中，先新建一个 WeakMap 实例。然后，将一个 DOM 节点作为键名存入该实例，并将一些附加信息作为键值，一起存放在 WeakMap 里面。这时，WeakMap 里面对element的引用就是弱引用，不会被计入垃圾回收机制。
          
          //也就是说，上面的 DOM 节点对象除了 WeakMap 的弱引用外，其他位置对该对象的引用一旦消除，该对象占用的内存就会被垃圾回收机制释放。WeakMap 保存的这个键值对，也会自动消失。
          ```
  
    - 总结：`WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏。
  
    - `WeakMap`有四个方法可用：`get()`、`set()`、`has()`、`delete()`。
  
    
  
  - `Set`数据结构
  
    - ES6提供了新的数据结构Set，它类似于数组，但是成员的值都是唯一的，没有重复的值
  
    - Set本身是一个构造函数， 用来生成Set数据结构
  
      ```js
      const s = new Set();
      s.size表示s的长度
      ```
  
    - Set函数可以接收一个数组作为参数，用来初始化。
  
      ```js
      const set = new Set([1,2,3,4,4]);
      ```
  
    - 利用Set实现数组去重
  
      ```js
      var arr = [1,2,3,4,4];
      const s = new Set(arr);
      const arr1 = [...s];           //[1,2,3,4]
      ```
  
    - 实例方法
  
      - add(value):添加某个值，返回Set结构本身
  
      - delete(value)：删除某个值，返回一个布尔值，表示删除是否成功
  
      - has(value)：返回一个布尔值，表示该值是否为Set的成员
  
      - clear()：清除所有成员，没有返回值
  
      - ```js
        const s = new Set();
        s.add(1).add(2).add(3);         //向set结构中添加值
        s.delete(2);              //删除set结构中的2值
        s.has(1);                 //表示set结构中是否有1这个值，返回布尔值
        s.clear();                //清除set结构中的所有值
        ```
  
    - 遍历set
  
      - `Set`的遍历顺序就是插入顺序。这个特性有时非常有用，比如使用 Set 保存一个回调函数列表，调用时就能保证按照添加顺序调用。
  
      - Set 结构的实例有四个遍历方法，可以用于遍历成员。
      
        - `Set.prototype.keys()`：返回键名的遍历器
      
        - `Set.prototype.values()`：返回键值的遍历器
      
        - `Set.prototype.entries()`：返回键值对的遍历器
      
        - `Set.prototype.forEach()`：使用回调函数遍历每个成员
      
          ```js
          s.forEach(value => console.log(value));
          ```
  
    
  
  - `Weakset`数据结构
  
    - WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。
  
      - 首先，WeakSet 的成员只能是对象，而不能是其他类型的值。
      - 其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
  
    - ES6 规定 WeakSet 不可遍历。
  
    - WeakSet 是一个构造函数，可以使用`new`命令，创建 WeakSet 数据结构。
  
      ```js
      const ws = new WeakSet();
      ```
  
    - WeakSet 结构有以下三个方法。
  
      - **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员。
  
      - **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员。
  
      - **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。
  
      - ```js
        const ws = new WeakSet();
        const obj = {};
        const foo = {};
        
        ws.add(window);
        ws.add(obj);
        
        ws.has(window); // true
        ws.has(foo);    // false
        
        ws.delete(window);
        ws.has(window);    // false
        ```
  
    - WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。
  
      
  
  - `WeakRef`数据结构
  
    - WeakRef 对象，用于直接创建对象的弱引用。
  
      ```javascript
      let target = {};
      let wr = new WeakRef(target);
      
      // target是原始对象，构造函数WeakRef()创建了一个基于target的新对象wr。这里，wr就是一个 WeakRef 的实例，属于对target的弱引用，垃圾回收机制不会计入这个引用，也就是说，wr的引用不会妨碍原始对象target被垃圾回收机制清除。
      ```
  
    - WeakRef 实例对象有一个`deref()`方法，如果原始对象存在，该方法返回原始对象；如果原始对象已经被垃圾回收机制清除，该方法返回`undefined`。
  
      ```js
      let target = {};
      let wr = new WeakRef(target);
      
      let obj = wr.deref();
      if (obj) { // target 未被垃圾回收机制清除
        // ...
      }
      
      // 上面示例中，deref()方法可以判断原始对象是否已被清除。
      ```
  
    - 弱引用对象的一大用处，就是作为缓存，未被清除时可以从缓存取值，一旦清除缓存就自动失效。
  
  - 
  
  - k