## JavaScript面向对象

- 面向对象编程介绍

  - 两大编程思想

    - 面向过程
    - 面向对象

  - 面向过程编程POP（Process-oriented programming）

    - **面向过程**就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了
    - **面向过程，就是按照我们分析好了的步骤，按照步骤解决问题**

  - 面向对象编程OOP（Object Orinented Programming）

    - 面向对象是把事务分解成为一个个对象，然后由对象之间分工与合作
    - 面向对象是以对象功能来划分问题，而不是步骤
    - 在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工
    - 面向对象编程具有灵活、代码可复用、容易维护和开发的优点，更适合多人合作的大型软件项目
    - 面向对象的特性：
      - 封装性
      - 继承性
      - 多态性

  - 面向过程和面向对象的对比

    - 面向过程：

      - 优点：性能比面向对象高，适合跟硬件联系很紧密的东西，例如单片机就采用的面向过程编程
      - 缺点：没有面向对象易维护、易复用、易扩展

    - 面向对象：

      - 优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

      - 缺点：性能比面向过程低

        

- ES6中的类和对象

  - 面向对象

    - 面向对象更贴近我们的实际生活，可以使用面向对象描述显示世界事物，但是事物分为具体的事物和抽象的事物
    - 面向对象的思维特点：
      - 抽取（抽象）对象共用的属性和行为阻止（封装）成一个**类**（模板）
      - 对类进行实例化，获取类的**对象**
    - 面向对象编程我们考虑的使有哪些对象，按照面向对象的思维特点，不断地创建对象，使用对象，指挥对象做事情

  - 对象

    - 现实生活中：万物皆对象，对象是**一个具体的事物**，看得见摸得着的实物。例如一本书、一个人、一辆汽车可以是”对象“，一个数据库，一张网页，一个与远程服务器的连接也可以是”对象“
    - **在JavaScript中，对象是一组无序的相关属性的方法的集合，所有的事物都是对象**，例如字符串、数值、数组、函数等
    - 对象是由**属性**和**方法**组成的：
      - 属性：事物的**特征**，在对象中用**属性**来表示（常用名词）
      - 方法：事物的**行为**，在对象中用**方法**来表示（常用动词）

  - 类class

    - 在ES6中新增加了类的概念，可以使用**class**关键字声明一个类，之后以这个类来实例化对象
    - **类**抽象了对象的公共部分，它**泛指**某一大类（class）
    - **对象特指**某一个，通过类实例化一个具体的对象

  - 创建类

    - 语法：

      ```js
      class name{
            //class body
      }
      
      创建实例：
      var xx = new name();
      ```

    - **注意：必须使用new实例化对象**

    - 通过class关键字创建类，类名我们还是习惯性定义首字母大写

    - 类里面有个constructor函数，可以接受传递过来的参数，同时返回实例对象

    - constructor函数只要new生成实例时，就会自动调用这个函数，如果我们不写这个函数，类也会自动生成这个函数

    - 生成实例new不能省略

    - 最后注意语法规范，创建类 类名后面不要加小括号，生成实例 类名后面加小括号，构造函数不需要加function（）

    - d

  - 类constructor构造函数

    - **constructor()**方法是类的构造函数（默认方法），**用于传递参数，返回实例对象**，通过new命令生成对象实例时，自动调用该方法。如果没有显示定义，类内部会自动给我们创建一个**constructor（）**

    - ```js
      class Star {
          constructor(uname,age) {
              this.uname = uname;
              this.age = age;
          }
      }
      
      var ldh = new Star('刘德华'，18)
      ```

  - 类添加方法

    - 语法：

      ```js
      class Person {
          constructor(name,age) {         //constructor构造器或者构造函数
              this.name = name;
              this.age = age;
          }
          say() {
              console.log(this.name + '你好');
          }
      }
      ```

    - 类中的函数不需要写function

    - 多个函数方法之间不需要添加逗号分隔

      

- 类的继承

  - 继承

    - 现实中的继承：子承父业，比如我们都继承了父亲的姓

    - 程序中的继承：子类可以继承父类的一些属性和方法

    - 注意：

      - 继承中，如果实例化子类输出一个方法，先看子类有没有这个方法，如果有就先执行子类的
      - 继承中，如果子类里面没有，就去查找父类有没有这个方法，如果有，就执行父类的这个方法**<u>（就近原则）</u>**

    - 语法：

      ```js
      class Father { //父类
      }
      class Son extends Father {    //子类继承父类
      }
      
      例子1：
      class Father {
          constructor() {}
          money() {
              console.log('100';)
          }
      }
      class Son extends Father {}
      var son = new Son();
      son.money();                   //console.log出100
      
      例子2：
      Class Father {
          constructor(x,y) {
              this.x = x;
              this.y = y;
          }
          sum() {
              console.log(this.x + this.y);
          }
      }
      
      Class Son extends Father {
          constructor(x,y) {
              super(x,y);          //调用了父类中的构造函数
          }
      }
      var son = new Son(1,2);
      son.sum();                     //输出3
      ```

  - super关键字

    - **super关键字**用于访问和调用对象父类上的函数。可以调用父类的构造函数，**也可以调用父类的普通函数**

    - ```js
      例子：
      class Father {
          say() {
              return '我是爸爸';
          }
      }
      class Son extends Father {
          say() {
              //console.log('我是儿子')                             //输出'我是儿子'
              console.log(super.say() + '的儿子')                   //输出'我是爸爸的儿子'
          }
      }
      var son = new Son();
      son.say()                                  
      ```

    - super.say()就是调用父类中的普通函数say()

    - 注意：子类在构造函数中使用super，必须在子类this之前调用（必须先调用父类的构造方法，再使用子类的构造方法)

      ```js
      Class Father {
          constructor(x,y) {
              this.x = x;
              this.y = y;
          }
          sum() {
              console.log(this.x + this.y);
          }
      }
      
      class Son extends Father {
          constructor(x,y) {
              //利用super调用父类中的构造函数
              //super必须在子类this之前调用
              super(x,y);
              this.x = x;
              this.y = y;
          }
          subtract() {
              console.log(this.x - this.y);                //此方法调用的是Son中的x和y
          }
      }
      var son = new Son(5,3);
      son.subtract();                  //输出2
      son.sum();                       //输出8
      ```

  - 使用类的3个注意点

    - 在ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象

    - 类里面的**共有的**属性和方法一定要加this使用

    - ```js
      <button>点击</button>
      <script>
          class Star {
          constructor(uname,age) {
              this.uname = uname;
              this.age = age;
              this,sing();      //需要通过对象自己（this）才能调用自己的方法
              this.btn = document.querySelector('button');     //此处给btn添加点击事件
              this.btn.onclick = this.sing;      //注意此处事件不需要加括号
          }
          sing() {
              console.log(this.uname);                 //类里面的共有的属性和方法一定要加this使用
          }
      }
      
      var ldh = new Star('刘德华')
      lds.sing();
      </script>
      ```

    - 类里面的this指向问题

      - constructor中的this指向的是  **创建的实例对象**

      - 方法中的this指向的是    **调用了这个函数的实例对象**

      - ```js
        <button>点击</button>
        
        <script>
            class Star {
            constructor(uname,age) {
                console.log(this);                   //指向ldh等创建出来的实例对象
                this.uname = uname;                   
                this.age = age;
                this,sing();      
                this.btn = document.querySelector('button');     
                this.btn.onclick = this.sing;      
            }
            sing() {
                console.log(this.uname);                 
                console.log(this);           //输出btn（这个sing中的this指向btn这个按钮，因为这个按钮调用了这个函数）
            }
            dance() {
                console.log(this);        //输出ldh（dance中的this指向ldh，因为ldh调用了this）
            }
        }
        
        var ldh = new Star('刘德华')
        lds.dance();
        </script>
        ```

      - 

      - d

    - 

    - d

  - 

  - 

  - 

