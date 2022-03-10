ES6

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
      const resFn = fn.call(obj);         //此处用cal函数将fn的this指向obj对象，因此箭头函数中的this也指向obj对象
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

    - 

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

    - 实例方法：includes()

      - 表示某个数组是否包含给定的值，返回布尔值

      - ```js
        [1,2,3].includes[2]      //true
        [1,2,3].includes[4]      //false
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

    - 实例方法：repeat()

      - repeat方法表示将原字符串重复n次，返回一个新字符串

      - ```js
        'x'.repeat(3);        //"xxx"
        'hello'.repeat(2);        //"hellohello"
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

    - 如果对同一个键多次赋值，后面的值将覆盖前面的值。

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
    
    
    
  - Set数据结构
  
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
  
      - Set结构的实例与数组一样，也有forEach的方法，用于对每个成员执行某种操作，没有返回值
  
      - ```js
        s.forEach(value => console.log(value));
        ```