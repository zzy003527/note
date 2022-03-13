## 构造函数和原型、继承及ES5中的新增方法

- 构造函数和原型

  - 概述

    - 在典型的OOP的语言中（如java），都存在类的概念，类就是对象的模板，对象就是类的实例，但在ES6之前，JS中并没有引入类的概念
    - ES6，全称ECMAScript6.0，2015.06发版。但是目前浏览器的JavaScript是ES5版本，大多数高版本的浏览器也支持ES6，不够只实现了ES6的部分特性和功能
    - 在ES6之前，对象不是基于类创建的，而是用一种称为**构造函数**的特殊函数来定义对象和它们的特征
    - 创建对象可以通过以下三种方式：
      - 对象字面量
      - new Object（）
      - 自定义构造函数

  - 构造函数

    - 构造函数是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与new一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面。

    - 在JS中，构造函数时需要注意以下两点：

      - 构造函数用于创建某一类对象，其**首字母要大写**
      - 构造函数要**和new一起使用**才有意义

    - new在执行时会做四件事情：

      - 在内存中创建一个新的空对象
      - 让this指向这个新的对象
      - 执行构造函数里面的代码，给这个新对象添加属性和方法
      - 返回这个新对象（所以构造函数里面不需要return）

    - JavaScript的构造函数中可以添加一些成员，可以在构造函数本身上添加，也可以在构造函数内部的this上添加。通过这两种方式添加的成员，就分别称为**静态成员和实例成员**。

      - 静态成员：在构造函数本身添加的成员称为**静态成员，只能由构造函数本身来访问**

      - 实例成员：在构造函数内部创建的对象成员称为**实例成员，只能由实例化的对象来访问**

      - ```js
        //构造函数中的属性和方法我们称为成员，成员可以添加
        function Star(uname,age) {
            this.uname = uname;
            this.age = age;
            this.sing = function() {
                console.log('我会唱歌')
            }
        }
        var ldh = new Star('刘德华'，18);
        
        Star.sex = '男';
        //1.实例成员就是构造函数内部通过this添加的成员,uname、age、sing就是实例成员
        //实例成员只能通过实例化的对象来访问
        //如：console.log(ldh.uname);    或   ldh.sing()
        //console.log(Star.uname)         这是错误的，不能通过构造函数来访问实例成员
        
        //2.静态成员就是在构造函数本身上添加的成员,sex就是静态成员
        //静态成员只能通过构造函数来访问
        //如：console.log(Star.sex);
        //console.log(ldh.sex);          这是错误的，不能通过实例对象来访问静态成员
        ```

  - 构造函数的问题

    - 构造函数方法很好用，但是**存在浪费内存的问题**
    - 即构造函数中的方法在创建每一个实例对象的时候都会开辟新的空间来存放方法，因为方法是复杂数据类型

  - 构造函数原型（原型对象）    prototype

    - 构造函数通过原型分配的函数是所有对象所**共享的**

    - JavaScript规定，**每一个构造函数都有一个prototype属性**，指向另一个对象。注意这个prototype就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有

    - **我们可以把那些不变的方法，直接定义在prototype对象上，这样所有对象的实例就可以共享这些方法**

    - ```js
      function Star(uname,age) {
          this.uname = uname;
          this.age = age;
      }
      
      Star.prototype.sing = function() {
          console.log('我会唱歌');
      }
      
      var ldh = new Star('刘德华'，18);
      var zxy = new Star('张学友'，19);
      ldh.sing();
      zxy.sing();     //均能输出
      ```

    - 原型是什么？

      - **原型是一个对象，我们也称为prototype为<u>原型对象</u>**

    - 原型的作用是什么？

      - **共享方法**（使每次实例对象都不用开辟新空间存放方法）

    - 一般情况下，公共的属性定义到构造函数里面（静态成员），公共的方法放到原型对象的身上

  - 对象原型     ______proto__ (双下划线proto双下划线)

    - **对象都会有一个属性   ______proto__  ** 指向构造函数的prototype原型对象，之所以我们对象可以使用构造函数prototype原型对象的属性和方法，就是因为对象有   ______proto__ 原型的存在
    - 在每一个实例对象身上系统自己自动添加一个   ______proto__ 指向我们构造函数的原型对象
    -    **__ proto__ 对象原型和prototype原型对象是等价的（即prototype是Star的，而 __ proto__ 是ldh的）**
    -    ______proto__ 对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此在实际开发中，不可以使用这个属性，他只是内部指向原型对象prototype
    - 方法的查找规则：
      - 首先看ldh对象身上是否有sing方法，如果有就执行这个对象上的sing
      - 如果没有sing这个方法，因为有   ______proto__ 的存在，就去构造函数原型对象prototype身上去查找sing这个方法
      - 如果还没有，就去构造函数原型对象prototype.______proto__ 即 Object.prototype上找
      - 再找不到就返回null

  - constructor构造函数

    - 对象原型（   ______proto__ ）和**构造函数原型对象（prototype）**里面都有一个属性**constructor**属性，constructor我们称为构造函数，因为它指向构造函数本身

    - constructor主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数

    - ```js
      function Star(uname,age) {
          this.uname = uname;
          this.age = age;
      }
      
      //很多情况下，我们需要手动的利用constructor这个属性指回  原来的构造函数
      Star.prototype = {
          //如果我们修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动的利用constructor指回原来的构造函数
          constructor:Star,
          sing:function() {
              console.log('我会唱歌');
          },
          movie:function() {
              console.log('我会演电影');
          }
      }
      
      var ldh = new Star('刘德华'，18);
      var zxy = new Star('张学友'，19);
      ```

  - 构造函数、实例、原型对象三者之间的关系

    - ![QQ图片20220221225345](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220221225345.png)

  - 原型链

    - 只要是对象就有______proto__原型，指向原型对象
    - 我们Star原型对象里面的______proto__原型指向的是Object.prototype
    - 我们Object.prototype原型对象里面的______proto__原型指向为null
    - ![QQ图片20220221230112](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220221230112.png)

  - JavaScript的成员查找机制

    - 当访问一个对象的属性（包括方法）时，首先查找这个**对象自身**有没有该属性
    - 如果没有就查找它的原型（也就是______proto__指向的**prototype原型对象**）
    - 如果还没有就查找原型对象的原型（**Object的原型对象**）
    - 以此类推一直找到Object为止（**null**）
    - ______proto__对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线

  - 原型对象this指向

    - 在构造函数中，里面this指向的时对象实例ldh
    - 原型对象函数里面的this指向的时实例对象ldh（哪个调用指向哪个）

  - 扩展内置对象

    - 可以通过原型对象，对原来的内置对象进行扩展自定义的方法，比如给数组增加自定义求偶数和的功能

      ```js
      Array.prototype.sum = function() {
          var sum = 0;
          for (var i = 0;i < this.length;i++) {
              sum += this[i];
          }
          return sum;
      }
      var arr = [1,2,3];
      console.log(arr.sum());     //输出6
      ```

    - **注意：数组和字符串内置对象不能给原型对象覆盖操作Array.prototype = {}，只能是Array.prototype.xxx = function（）{}的方式**

      

- 继承

  ES6之前并没有给我们提供extends继承。我们可以通过**构造函数+原型对象**模拟实现继承，被称为**组合继承**

  - call()

    - 调用这个函数，并且修改函数运行时的this指向

    - ```js
      fun.call(thisArg,arg1,arg2,……)
      ```

    - thisArg:当前调用函数this的指向对象

      ```js
      //call方法
      function fn(x,y) {
          console.log('llll');
          console.log(this);
          console.log(x+y)
      }
      var o = {
          name:'andy'
      }
      //call()可以调用函数
      fn.call();          //输出llll和window对象
      //call()可以改变这个函数的this指向
      fn.call(o,1,2);         //输出llll和o对象和3,1和2会传入x和y
      
      
      ```

  - 借用构造函数继承父类型属性

    - 核心原理：通过call（）把父类型的this指向子类型的this，这样就可以实现子类型继承父类型的属性

    - ```js
      //借用父构造函数继承属性
      function Father(uname,age) {
          //this指向父构造函数的对象实例
          this.uname = uname;
          this.age = age;
      }
      function Son(uname,age) {
          //this指向父构造函数的对象实例
          Father.call(this)
      }
      ```

    - ![QQ图片20220222133447](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220222133447.png)

  - 借用原型对象继承父类型方法

    - ```js
      function Father(uname,age) {
          //this指向父构造函数的对象实例
          this.uname = uname;
          this.age = age;
      }
      function Son(uname,age,score) {
          //this指向子构造函数的对象实例
          Father.call(this,uname,age);
          this.score = score;
      }
      //Son.prototype = Father.prototype;这样直接赋值会有问题，如果直接修改了子原型对象，父原型对象也会跟着一起变化
      Son.prototype = new Father();
      //如果利用对象的形式修改了原型对象，别忘了利用constructor指回原来的原型对象
      Son.prototype.constructor = Son;
      //这个是子构造函数专门的方法
      Son.prototype.exam = function() {
          console.log('考试');
      }
      var son = new Son('刘德华',18,100)
      ```

    - ![QQ图片20220222135557](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220222135557.png)

    

- 类的本质

  - class的本质还是function（也可以简单的认为，类就是构造函数的另外一种写法）
  - 类的所有方法都定义在类的prototype属性上
  - 所以ES6的类它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已
  - 构造函数拥有的特点：
    - 构造函数有原型对象prototype
    - 构造函数原型对象prototype里面有constructor指向构造函数本身
    - 构造函数可以通过原型对象添加方法
    - 构造函数创建的实例对象有______proto__原型指向构造函数的原型对象
  - 类拥有的特点：
    - 类也有原型对象prototype
    - 类的原型对象prototype中也有constructor指向类本身
    - 类也可以通过原型对象添加方法
    - 类创建的实例对象也有______proto__原型指向类的原型对象

  

- ES5中的新增方法

  - ES5新增方法概述

    - ES5中给我们新增了一些方法，可以很方便的操作数组或者字符串，这些方法主要包括：
      - 数组方法
      - 字符串方法
      - 对象方法

  - 数组方法

    - 迭代（遍历）方法：forEach()、map()、filter()、some()、every()

    - 遍历自身所有属性keys()

      - 注意keys()不会获取到构造函数的prototype中的属性
      - 而for……in会获取到prototype中的属性

    - 迭代（遍历）方法:forEach()

      - ```js
        array.forEach(function(currentValue,index,arr))
        
        例子:
        var arr = [1,2,3];
        arr.forEach(function(value,index,array) {
            console.log('每个数组元素' + value);        
            console.log('每个数组元素的索引号' + index);
            console.log('数组本身' + array);          //1,2,3
        })
        ```

      - currentValue:数组当前项的值

      - index：数组当前项的索引

      - arr：数组对象本身

    - 筛选数组方法：filter()

      - ```js
        array.filter(function(currentValue,index,arr))
        
        例子:
        var arr = [12,66,4,88];
        var newArr = arr.filter(function(value,index,arr) {
            return value >= 20;            //筛选大于20的数
            return value%2 === 0;             //筛选偶数
        })
        ```
        
      - filter()方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素，**主要用于筛选数组**

      - **注意它直接返回一个新数组**(用一个新数组接受)

      - currentValue：数组当前项的值

      - index：数组当前项的索引

      - arr：数组对象本身

    - 查找数组中是否有满足条件的元素some方法

      - ```js
        array.some(function(currentValue,index,arr))
        
        例子:
        var arr = [10,30,4];
        var flag = arr.some(function(value,index,arr) {
            return value >= 20;           //查找数组内是否有大于等于20的值
        })
        console.log(flag);
        ```

      - some()方法用于检测数组中的元素是否满足指定条件，通俗点 查找数组中是否有满足条件的元素

      - **注意它返回值是布尔值，如果查找到这个元素，就返回true，如果查找不到就返回false**

      - 如果找到第一个满足条件的元素，则终止循环，不再继续寻找

      - currentValue：数组当前项的值

      - index：数组当前项的索引

      - arr：数组对象本身

    - some和forEach的区别

      - 在forEach里面return不会终止迭代
      - 在some里面遇到return true就是终止遍历，迭代效率更高

  - 字符串方法

    - trim方法去除字符串两侧空格

      - trim()方法会从一个字符串的两端删除空白字符

      - ```js
        str.trim()
        ```

      - trim()方法并不影响原字符串本身，它返回的是一个新的字符串

      - 检测文本框输入字符串是否为空

        ```js
        if(input.value.trim() === '') {
            alert('请输入内容!')
        } else {
          div.innerHTML = input.value.trim();
        }
        ```

  - 对象方法

    - ```js
      Object.defineProperty(obj,prop,descriptor)
      
      例子：
      Object.defineProperty(obj,'num',{
          value:1000,
      })
      ```

    - Object.defineProperty()定义对象中新属性或修改原有的属性

    - obj：必须。目标对象

    - prop：必须。需定义或修改的属性的名字

    - descriptor：必须。目标属性所拥有的特性

    - Object.defineProperty()第三个参数descriptor说明：以对象形式{}书写

      - value：设置书写的值，默认为undefined
      - writable：值是否可以重写。true|false。默认为false(false代表不允许重写)
      - enumerable：目标属性是否可以被枚举。true|false。默认为false(如果为false代表遍历的时候此属性不会出现)
      - configurable：目标属性是否可以被删除或是否可以再次修改特性。true|false。默认为false(如果为false，则不允许删除这个属性,且不允许再修改第三个参数里面的特性)

