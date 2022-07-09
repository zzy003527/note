

## TypeScript数据类型

- TypeScript介绍

  - **TypeScript**（简称TS）是JavaScript的**超集**（JS有的TS都有）

  - TypeScript = **Type** + JavaScript（在JS基础之上，为JS添加了**类型支持**）

  - TypeScript是微软开发的开源编程语言，开源在任何运行JavaScript的地方运行

  - TypeScript为什么要为JS添加类型支持？

    - 背景: JS的类型系统存在“先天缺陷”，JS代码中绝大部分错误都是**类型**错误(Uncaught **Type**Error)。问题:增加了找Bug、改Bug的时间，严重影响开发效率

    - 从编程语言的动静来区分, TypeScript属于静态类型的编程语言, JS属于动态类型的编程语言。

      静态类型:编译期做类型检查;   动态类型:执行期做类型检查。

      代码编译和代码执行的顺序: 先编译再执行

    - 对于JS来说:需要等到代码真正去**执行**的时候才能**发现错误**(晚)

      对于TS来说:在代码**编译**的时候(代码执行前)就可以**发现错误**(早)

      并且，配合VSCode等开发工具, TS可以**提前到在编写代码的同时**就发现代码中的错误,**减少找Bug、改Bug时间**

  - TypeScript相比JS的优势

    - 更早(写代码的同时)发现错误，**减少找Bug、改Bug时间**，提升开发效率
    - 程序中任何位置的代码都有**代码提示**，随时随地的安全感,增强了开发体验
    - 强大的**类型系统**提升了代码的可维护性,使得**重构代码更加容易**
    - 支持**最新的ECMAScript语法**，优先体验最新的语法，让你走在前端技术的最前沿
    - TS **类型推断**机制，**不需要**在代码中的**每个地方都显示标注类型**,让你在享受优势的同时,尽量降低了成本
    - 除此之外，Vue 3源码使用TS重写、Angular 默认支持TS、React与 TS完美配合, TypeScript 已成为大中型前端项目的首先编程语言

- TypeScript体验

  - 安装编译TS的工具包：`npm i -g typescript`
    - 为什么要安装编译TS的工具包？
      - 因为Node.js/浏览器只认识JS代码，不认识TS代码。需要先将TS代码转化为JS代码，然后才能执行。
    - typescript包：用来编译TS代码的包，提供了**tsc**命令，实现了TS->JS的转化
    - 验证是否安装成功： tsc -v （查看typescript的版本）
  - 编译并运行TS代码
    - 创建hello.ts文件（注意：TS文件的后缀名为**.ts**）
    - 将TS编译为JS：在终端中输入命令，`tsc hello.ts`（此时，在统计目录中会出现一个同名的JS文件）
    - 执行JS代码：在终端中输入命令，`node hello.js`
    - 说明：所有合法是JS代码都是TS代码
    - 注意：由TS编译生成的**JS文件**，代码中就**没有类型信息**了
  - 简化运行TS的步骤
    - 问题描述：每次修改代码后，都要重复执行两个命令，才能运行TS代码，太繁琐
    - 简化方式：使用**ts-node**包，直接在Node.js中执行TS代码
    - 安装命令：`npm i -g ts-node`（ts-node包提供了ts-node命令）
    - 使用方式：`ts-node hello.ts`
    - 解释：ts-node命令在内部偷偷的将TS->JS，然后再运行JS代码





- TypeScript常用类型

  - 概述：TypeScript是JS的超级，TS提供了JS的所有功能，并且额外的增加了：**类型系统**

    - 所有的JS代码都是TS代码
    - JS由类型（比如，number/string等），但是**JS不会检查变量的类型是否发生变化**。而**TS会检查**。
    - TypeScript类型系统的主要优势：可以**显示标记出代码中的意外行为**，从而降低了发生错误的可能性

  - 类型注解

    - 示例代码：

      ```typescript
      let age: number = 18
      ```

    - 说明：代码中的`: number`就是类型注解

    - 作用：为变量**添加类型约束**。比如，上述代码中，约定变量age的类型为number（数值类型）

    - 解释：**约定了什么类型，就只能给该变量赋值该类型的值**，否则就会报错,如：

      ```typescript
      let age: number = '18'          //报错，把string类型分配给了number类型
      ```

  - 常用基础类型概述

    可以将TS中的常用基础类型细分为两类：JS已有类型   和    TS新增类型

    - JS已有类型
      - 原始类型：number/string/bollean/null/undefined/symbol
      - 对象类型: object(包括：数组、对象、函数等)
    - TS新增类型
      - 联合类型、自定义类型（类型别名）、接口、元组、字面量类型、枚举、void、any等

  - 原始类型

    - 包含：number/string/bollean/null/undefined/symbol

    - 特点：简单。这些类型完全按照JS中类型的名称来书写

    - ```typescript
      let age: number = 18
      let myName: string = '刘老师'
      let isLoading: boolean = false
      let a: null = null
      let b: undefined = undefined
      let s: symbol = Symbol()
      ```

  - 数组类型

    - 属于对象类型：object(包括：数组、对象、函数等)

    - 特点：对象类型。在TS中更加细化，**每个具体的对象都有自己的类型语法**

    - 数组类型的两种写法：（推荐使用`number[]`写法）

      ```typescript
      let numbers: number[] = [1,3,5]
      let strings: Arrag<string> = ['a','b','c']
      ```

    - 需求：数组中既要有number类型，又要有string类型，应该怎么写？

      - ```typescript
        let arr: (number | string)[] = [1,'a',3,'b']
        ```

      - 解释：**|** （竖线）在TS中叫做**联合类型**（由两个或多个其他类型组成的类型，表示可以是这些类型中的任意一种）

      - 注意：这是TS中联合类型的语法，只有一根竖线，不要与JS中的或（||）混淆了

  - 类型别名

    - **类型别名**（自定义类型）：为任意类型起别名

    - 使用场景：当同一类型（复杂）被多次使用时，可以通过类型别名，**简化该类型的使用**

    - ```typescript
      type CustomArray = (number | string)[]
      let arr1: CustomArray = [1,'a',3,'b']
      let arr2: CustomArray = ['x','y',6,7]
      ```

    - 解释：

      - 使用`type`关键字来创建类型别名
      - 类型别名（比如此处的CustomArray），可以是任意合法的变量名称
      - 创建类型别名后，直接**使用该类型别名作为变量的类型注解**即可

  - 函数类型

    - 函数的类型实际上指的是：函数**参数和返回值**的类型

    - 为函数指定类型的两种方式：

      - 单独指定参数、返回值的类型
      - 同时指定参数、返回值的类型

    - 单独指定参数、返回值的类型：

      ```typescript
      function add(num1: number,num2: number): number {
          return num1 + num2
      }
      
      const add = (num1: number,num2: number): number => {
          return num1 + num2
      }
      //括号后的类型注解是返回值的类型注解
      ```

    - 同时指定参数、返回值的类型：

      - ```typescript
        const add:(num1: number,num2: number) => number = (num1,num2) => {
          return num1 + num2
        }
        
        //:(num1: number,num2: number) => number    这一部分是类型注解
        ```

      - 解释：当函数作为表达式时，可以通过**类似箭头函数的语法**来为函数添加类型

      - 注意：这种形式只适用于函数表达式

    - 如果函数没有返回值，那么，函数返回值类型为：void

      - ```typescript
        function greet(name: string): void {
            console.log('Hello', name)
        }
        ```

    - 可选参数

      - 使用函数实现某个功能时，参数可以传也可以不传。这种情况下，再给函数参数指定类型时，就用到**可选参数**了

      - 比如，数组的slice方法，可以slice()也可以slice(1)还可以slice(1,3)

      - ```typescript
        function mySlice(start?: number, end?: number): void {
            console.log('起始索引：',start,'结束索引：',end)
        }
        ```

      - 可选参数：在可传可不传的参数名称后面添加`?`（问号）

      - 注意：**可选参数只能出现在参数列表的最后**，也就是说可选参数后面不能再出现必选参数

  - 对象类型

    - JS中的对象是由属性和方法构成的，而TS中**对象的类型**就是在**描述对象的结构**（有什么类型的属性和方法）

    - 对象类型的写法：

      ```typescript
      let person: { name: string; age: number; sayHi(): void } = {
          name: 'jack',
          age: 19,
          sayHi() {}
      }
      ```

    - 解释：

      - 直接使用{}来描述对象结构。属性采用**属性名：类型**的形式；方法采用**方法名()：返回值类型**的形式

      - 如果方法有参数，就在方法名后面的小括号中指定参数类型(比如： `greet(name: string):void`)

      - 在一行代码中指定对象的多个属性类型时，**使用`;`（分号）来分隔**

        - 如果一行代码只指定一个属性类型（通过换行来分隔多个属性类型），可以去掉;（分号）

          ```typescript
          //如：
          let person: {
              name: string
              age: number
              sayHi: () => void
              greet(name: string): void
          } = {
              name: '刘老师',
              age: 18,
              sayHi() {},
              greet(name) {}
          }
          ```

        - 方法的类型也可以使用箭头函数形式（比如： {sayHi:()=>void}）

    - 对象的可选属性

      - 对象的属性或方法，也可以是可选的，此时就用到**可选属性**了

      - 比如，在使用axios({...})时，如果发送GET请求，method属性就可以省略了

        ```typescript
        function myAxios(config: { url: string; method?: string}) {
            console.log(config)
        }
        ```

      - **可选属性**的语法与函数可选参数的语法一致，都使用**？**（问号）来表示

    - 接口

      - 当一个**对象类型**被多次使用时，一般会使用**接口（interface）**来描述对象的类型，达到**复用**的目的

      - ```typescript
        interface IPerson {
            name: string
            age:  number
            sayHi(): void
        } 
        
        let person: IPerson = {
            name: 'jack',
            age: 19,
            sayHi() {}
        }
        ```

      - 解释：

        - 使用`interface`关键字来声明接口
        - 接口名称（比如，此处的IPerson），可以是任意合法的变量名称
        - 声明接口后，直接**使用接口名称作为变量的类型**
        - 因为每一行只有一个属性类型，因此，属性类型后面没有;（分号）

      - **interface（接口）和type（类型别名）的对比** ：

        - 相同点：都可以给对象指定类型

        - 不同点：

          - 接口只能为对象指定类型
          - 类型别名不仅可以为对象指定类型，也可以为任意类型指定别名

        - ```typescript
          interface IPerson {
              name: string
              age: number
              sayHi(): void
          }
          type IPerson = {
              name: string
              age: number
              sayHi(): void
          }
          type NumStr = number | string
          ```

      - 接口继承

        - 如果两个接口之间有相同的属性或方法，可以**将公共的属性或方法抽离出来，通过继承来实现复用**。

        - 比如，这两个接口都有x、y属性，重复写两次会很繁琐

          ```typescript
          interface Point2D { x: number; y: number}
          interface Point3D { x: number; y: number; z: number}
          ```

        - 更好的方式：

          ```typescript
          interface Point2D { x: number; y: number}
          interface Point3D extends Point2D { z: number }
          ```

        - 解释：

          - 使用**`extend`（继承）**关键字实现了接口Point3D继承Point2D

          - 继承后，Point3D就有了Point2D的所有属性和方法（此时，Point3D同时有x、y、z三个属性）

            

  - 元组

    - 场景：在地图中，使用经纬度坐标来标记位置信息。可以使用数组来记录坐标，那么，该数组中只有两个元素，并且这两个元素都是数组类型。

      - ```typescript
        let position: number[] = [39.5427,116.2317]
        ```

      - 使用number[]的缺点：不严谨，因为该类型的数组中可以出现任意多个数字

      - 更好的方法：**元组（Tuple）**

    - 元组是另一种类型的数组，它**确切地知道包含多少个元素，以及特定索引对应的类型**

      ```typescript
      let position: [number,number] = [39.5427,116.2317]
      ```

    - 解释：

      - 元组类型可以确切地标记处有多少个元素，以及每个元素的类型
      - 该示例中，元组有两个元素，每个元素的类型都是number

      

  - 类型推论

    - 在TS中，某些没有明确指出类型的地方，TS的**类型推论机制会帮助提供类型**

    - 换句话说：由于类型推论的存在，这两种情况下，类型注解可以**省略**不写：

      - **声明变量并初始化时**(如果声明变量但没有立即初始化值，就需要手动添加类型注解)

      - **决定函数返回值时**

      - ```typescript
        let age: 18   // TS自动推断出变量age为number类型
        
        function add(num1: number, num2: number) { return num1 + num2 }     
        // 相当于 function add(num1: number, num2: number):number { return num1 + num2 }
        ```

    - 推荐：**能省略类型注解的地方就省略**

    - 技巧：如果不知道类型，可以通过把鼠标放在变量名称上，利用VSCode的提示来查看类型

  - 类型断言

    - 有时候我们会比TS更加明确一个值的类型，此时，可以使用**类型断言**来指定更具体的类型

    - 比如：

      ```typescript
      <a href="http://www.baidu.com/" id="link">百度</a>
      
      const aLink = document.getElementById('link')
      ```

    - 注意：getElementById方法返回值的类型是HTMLElement，该类型只包含所有标签公共的属性或方法，不包含a标签特有的href等属性。因此，这个**类型太宽泛（不具体）**，无法操作href等a标签特有的属性或方法

    - 解决方式：这种情况下就需要**使用类型断言指定更加具体的类型**

    - 使用类型断言

      - ```typescript
        const aLink = document.getElementById('link') as HTMLAnchorElement
        ```

      - 解释：

        - 使用**`as`关键字**实现类型断言
        - 关键字`as`后面的类型是一个更加具体的类型（HTMLAnchorElement是HTMLElement的子类型）
        - 通过类型断言，aLink的类型变得更加具体，这样就可以访问a标签特有的属性或方法了

      - 另一种语法，使用**<>语法**，这种语法不常用

        - ```typescript
          const aLink = <HTMLAnchorElement>document.getElementById('link')
          ```

        - 技巧：在浏览器控制台，通过console.dir()打印DOM元素，在属性列表的最后面，即可看到该元素的类型

        

  - 字面量类型

    - ```typescript
      let str1 = 'Hello TS'         //为string类型
      const str2 = 'Hello TS'        //为'Hello TS'类型
      ```

    - 解释：

      - str1是一个变量(let)，它的值可以是任意字符串，所以类型为：string
      - str2是一个常量(const)，它的值不能变化只能是'Hello TS'，所以，它的类型也为：'Hello TS'

    - 注意：此处的**'Hello TS'**，就是一个**字面量类型**。也就是说**某个特定的字符串也可以作为TS中的类型**

      除字符串外，任意的JS字面量（比如：对象、数字等）都可以作为类型使用

    - 使用模式：**字面量类型配合联合类型一起使用**

    - 使用场景：用来**表示一组明确的可选值列表**

    - 比如，在贪吃蛇中，游戏的可选值只能说上、下、左、右中的任意一个

      ```typescript
      function changeDirection(direction: 'up' | 'down' |　'left' | 'right') {
          console.log(direction)
      }
      //解释：参数direction的值只能是up/down/left/right中的任意一个
      ```

    - 优势：相比于string类型，使用字面量类型更加精确、严谨

  

  

  - 枚举

    - 枚举的功能类似于字面量类型+联合类型组合的功能，也可以**表示一组明确的可选值**

    - **枚举：定义一组命名常量**。它描述一个值，该值可以是这些命名常量中的一个

    - ```typescript
      enum Direction { Up, Down, Left, Right }
      function changeDirection(direction: Direction) {
          console.log(direction)
      }
      ```

    - 解释：

      - 使用**`enum`关键字**定义枚举
      - 约定枚举名称、枚举中的值以大写字母开头
      - 枚举中的多个值之间通过**`，`（逗号）**分隔
      - 定义好枚举后，直接使用枚举名称作为类型注解

    - 注意：形参direction的**类型为枚举Direction**，那么实参的**值就应该是枚举Direction成员的任意一个**

    - **访问枚举成员：**

      - ```typescript
        enum Direction { Up, Down, Left, Right}
        function changeDirection(direction: Direction) {
            console.log(direction)
        }
        changeDirection(Direction.Up)
        ```

      - 解释：类似于JS中的对象，直接通过**点(.)语法**访问枚举的成员

    - 问题：我们把枚举成员作为了函数的实参，它的值是什么

      - ```typescript
        changeDirection(Direction.Up)    //其中Direction.Up的值是0
        ```

      - 解释：通过将鼠标移入Direction.Up，可以看到枚举成员Up的值为0

      - 注意：**枚举成员是有值的**，默认为：**从0开始自增的数值**

      - 我们把枚举成员的值为数字的枚举成为：**数字枚举**

      - 我们也可以给枚举中的成员初始化值

        ```typescript
        enum Direction { Up = 10, Down, Left, Right }      //Down=11,Left=12,Right=13
        
        enum Direction { Up = 2, Down = 4, Left = 8, Right = 16}
        ```

    - 字符串枚举：枚举成员的值是字符串

      - ```typescript
        enum Direction {
            Up = 'UP',
            Down = 'DOWN',
            Left = 'LEFT',
            Right = 'RIGHT'
        }
        ```

      - 注意：字符串枚举没有自增长行为，因此，**字符串枚举的每个成员必须有初始值**

    - 枚举的特点及原理

      - 枚举是TS为数不多的非JavaScript类型级扩展（不仅仅是类型）的特性之一。

      - 因为：其他类型仅仅被当作类型，而**枚举不仅用作类型，还提供值**（枚举成员都是有值的）

      - 也就是说，其他的类型会在编译为JS代码时自动移除。但是**枚举类型会被编译为JS代码**

      - ```typescript
        enum Direction {
            Up = 'UP',
            Down = 'DOWN',
            Left = 'LEFT',
            Right = 'RIGHT'
        }
        //变为
        var Direction;
        (function (Direction) {
            Direction["Up"] = 'UP',
            Direction["Down"] = 'DOWN',
            Direction["Left"] = 'LEFT',
            Direction["Right"] = 'RIGHT'
        })(Direction || (Direction = {}));
        ```

      - 说明：枚举与前面的字面量类型+联合类型组合的功能类似，都用来表示一组明确的可选值列表。

      - 一般情况下，**推荐使用字面量类型+联合类型组合的方式**，因为相比枚举，这种方式更加直观、简洁、高效

        

  - any类型

    - **原则：不推荐使用any**。这会让TypeScript变为"AnyScript"（失去TS类型保护的优势）

    - 因为当值的类型为any时，可以对该值进行任意操作，并且不会有代码提示

    ```typescript
    let obj: any = { x:0 }
    
    obj.bar = 100
    obj()
    const n: number = obj
    ```

    - 解释：以上操作都不会有任何类型错误提示，即使可能存在错误

    - 尽可能地避免使用any类型，除非**临时使用any**来"避免"书写很长、很复杂的类型

    - 其他隐式具有any类型的情况：
      - 声明变量不提供类型也不提供默认值
      - 函数参数不加类型
    - 注意：因为不推荐使用any，所以上述两种情况都要提供类型

  - typeof

    - JS中提供了typeof操作符，用来在JS中获取数据的类型

      ```js
      console.log(typeof "Hello world")          //打印出string
      ```

    - 实际上，**TS也提供了typeof操作符**：可以在**类型上下文**中引用变量或属性的类型（类型查询）

    - 使用场景：根据已有变量的值，获取该值的类型，来简化类型书写

      ```typescript
      let p = { x: 1, y: 2}
      function formatPoint(point: { x: number; y: number }) {}
      formatPoint(p)
      
      function formatPoint(point: typeof p) {}
      let num: typeof p.x                 //num是number类型的
      
      //以下无法使用typeof
      function add(num1: number, num2: number) {
          return num1 + num2
      }
      let ret: typeof add(1,2)
      ```

    - 解释：

      - 使用`typeof`操作符来获取变量p的类型，结果与第一种（对象字面量形式的类型）相同
      - typeof出现在**类型注解的位置**（参数名称的冒号后面）**所处的环境就在类型上下文**（区别于JS代码）
      - 注意：typeof只能用来查询变量或属性的类型，无法查询其他形式的类型（比如：函数调用后返回的类型）









- TypeScript高级类型

  - 概述

    - TS中的高级类型有很多，有：
      - class类
      - 类型兼容性
      - 交叉类型
      - 反省和keyof
      - 索引签名类型和索引查询类型
      - 映射类型

    

  - class类

    - TypeScript全面支持ES2015中引入的`class`关键字，并为其添加了类型注解和其他语法（比如：可见性修饰符等）。

    - class基本使用如下：

      - ```typescript
        class Person {}
        const p = new Person()            //const p: Person，p的类型是Person
        ```

      - 解释：

        - 根据TS中的类型推论，可以知道Person类的实例对象p的类型是Person
        - TS中的**class，不仅提供了class的语法功能，也作为一种类型存在**

    - 实例属性初始化：

      - ```typescript
        class Person {
            age: number
            gender = '男'
            //gender: string = '男'
        }
        ```

      - 解释：

        - 声明成员age，类型为number（没有初始值）
        - 声明成员gender，并设置初始值，此时可省略类型注解（TS类型推论为string类型）

    - 构造函数

      - ```typescript
        class Person {
            age: number
            gender: string
            
            constructor(age: number,gender: string) {
                this.age = age
                this.gender = gender
            }
        }
        ```

      - 解释：

        - 成员初始化（比如：age：number）后，才可以通过this.age来访问实例成员
        - 需要**为构造函数指定类型注解**，否则会被隐式推断为any；构造函数不需要返回值类型

    - 实例方法：

      - ```typescript
        class Point {
            x = 10
            y = 10
            
            scale(n: number): void {
                this.x *= n
                this.y *= n
            }
        }
        ```

      - 解释：方法的类型注解（参数和返回值）与函数用法相同

    - 继承

      - 类继承的两种方式：

        - **extends**（继承父类）
        - implements（实现接口）

      - 说明：JS种只有extends，而implements是TS提供的

      - ```typescript
        //extends(继承父类)
        class Animal {
            move() { console.log('Moving along!')}
        }
        class Dog extends Animal {
            bark() { console.log('汪!')}
        }
        const dog = new Dog()
        ```

      - 解释：

        - 通过**extends**关键字实现**继承**
        - 子类Dog继承父类Animal，则Dog的实例对象dog就同时具有了父类Animal和子类Dog的所有属性和方法

      - ```typescript
        //implements(实现接口)
        interface Singable {
            sing(): void
        }
        class Person implements Singable {
            sing() {
                console.log('啦啦啦啦啦')
            }
        }
        ```

      - 解释：

        - 通过**`implements`**关键字让class实现接口
        - Person类实现接口Singable意味着Person类种必须提供Singable接口种指定的所有方法和属性

      

    - 类成员可见性

      - 类成员可见性：可以使用TS来**控制calss的方法或属性对于class外的代码是否可见**

      - <u>可见性修饰符</u>包括：

        - **public（公有的）**
        - protected（受保护的）
        - private（私有的）

      - public：表示公有的、公开的，共有成员可以被任何地方访问，默认可见性

        - ```typescript
          class Animal {
              public move() {
                  console.log('Moving along!')
              }
          }
          ```

        - 解释：

          - 在类属性或方法前面添加**`public`关键字**，来修饰该属性或方法是共有的
          - 因为**public**是默认可见性，所以，**可以直接省略**

      - **protected**：表示**受保护的**，仅对**其声明所在类和子类中**（非实例对象）可见

        - ```typescript
          class Animal {
              protected move() { console.log('Moving along!')}
          }
          class Dog extends Animal {
              bark() {
                  console.log('汪!')
                  this.move()
              }
          }
          ```

        - 解释：

          - 在类属性或方法前面添加**`protected`关键字**，来修饰该属性或方法是受保护的
          - 在子类的方法内部可以通过this来访问父类中受保护的成员，但是**对实例不可见**

      - **private**：表示**私有的，只在当前类中可见**，对实例对象以及子类也是不可见的

        - ```typescript
          class Animal {
              private move() { console.log('Moving along!')}
              walk() {
                  this.move()
              }
          }
          ```

        - 解释：

          - 在类属性或方法前面添加**`private`关键字**，来修饰该属性或方法是私有的
          - 私有的属性和方法只在当前类中可见，对子类和实例对象也都是不可见的

    - **readonly（只读修饰符）**

      - **readonly**：表示**只读，用来防止在构造函数之外对属性进行赋值**

      - ```typescript
        class Person {
            readonly age: number = 18
            constructor(age: number) {
                this.age = age
            }
            
            setAge() {
                this.age = 20             //此处会报错，因为age是只读属性
            }
        }
        ```

      - 解释：

        - 使用**`readonly`关键字**修饰该属性是只读的，注意**只能修饰属性不能修饰方法**

        - 注意：属性age后面的类型注解（比如此处的number）如果不加，则age的类型为18（字面量类型）

        - **接口或者{}表示的对象类型，也可以使用readonly**

          ```typescript
          interface IPerson {
              readonly name: string
          }
          let obj: IPerson = {
              name: 'jack'                  //报错
          }
          //或者
          let obj: { readonly name: string } = {
              name: 'jack'
          }
          obj.name = 'rose'         //报错
          ```

      - **注意：只要是readonly来修饰的属性，必须手动提供明确的类型**

        

  - 类型兼容性

    - 两种类型系统：
      - Structural Type System(结构化类型系统)
      - Nominal Type System(标明类型系统)

    - **TS采用的是结构化类型系统**，也叫做duck typing（鸭子类型），**类型检查关注的是值所具有的形状**

    - 也就是说，在结构类型系统中，如果两个对象具有相同的性质，则认为他们属于同一类型

      - ```typescript
        class Point { x: number; y: number }
        class Point2D{ x: number; y: number}
        
        const p: Point = new Point2D()
        ```

      - 解释：

        - Point和Point2D是两个名称不同的类
        - 变量p的类型被显示标注为Point类型，但是它的值确实Point2D的实例，并且没有类型错误
        - 因为TS是结构化类型系统，只检查Point和Point2D的结构是否相同（相同，都具有x和y两个属性，属性类型也相同）
        - 但是，如果在Nominal Type System中（比如：C#、Java等），它们是不同的类，类型无法兼容

    - 对象之间的类型兼容性

      - 注意：在结构化类型系统中，如果两个对象具有相同的性质，则认为它们属于同一类型，这是不准确的说法

      - **更准确的说法是：对于对象类型来说，y的成员至少与x相同，则x兼容y<u>（成员多的可以赋值给少的）</u>**

      - ```typescript
        class Point { x: number; y: number }
        class Point3D { x: number; y: number; z: number }
        const p: Point = new Point3D()
        ```

      - 解释：

        - Point3D的成员**至少**与Point相同，则**Point兼容Point3D**
        - 所以成员多的Point3D可以赋值给成员少的Point

    - 接口之间的类型兼容性

      - 除了class之外，TS中的其他类型也存在相互兼容的情况，包括：

        - 接口兼容性
        - 函数兼容性等

      - 接口之间的兼容性，类似于class。并且，class和interface之间也可以兼容

      - ```typescript
        //接口之间可以兼容
        interface Point { x: number; y: number }
        interface Point2D { x: number; y :number }
        let p1: Point
        let p2: Point2D = p1
        
        interface Point3D { x: number; y: number; z: number }
        let p3: Point3D
        p2 = p3
        
        //class和interface之间也可以兼容
        class Point4D { x: number; y: number; z: number }
        let p3: Point2D = new Point4D()
        ```

    - 函数之间的类型兼容性

      - **函数之间兼容性比较复杂**，需要考虑：

        - 参数个数
        - 参数类型
        - 返回值类型

      - **参数个数**，参数多的兼容参数少的（或者说，**参数少的可以赋值给多的**）

        - ```typescript
          type F1 = (a: number) => void
          type F2 = (a: number, b: number) => void
          let f1: F1
          let f2: F2 = f1
          
          const arr = ['a','b','c']
          arr.forEach(() => {})
          arr.forEach((item) => {})
          ```

        - 解释：

          - 参数少的可以赋值给参数多的，所以，f1可以赋值给f2
          - 数组forEach方法的第一个参数是回调函数，该实例中类型为：(value:string,index:number,array:string[])=>void
          - **在JS中省略用不到的函数参数实际上是很常见的，这样的使用方式，促成了TS中函数类型之间的兼容性**
          - 并且因为回调函数是有类型的，所以，TS会自动推导出参数item、index、array的类型

      - **参数类型**，相同位置的参数类型要相同（原始类型）或兼容（对象类型）

        - ```typescript
          type F1 = (a: number) => string
          type F2 = (a: number) => string
          let f1: F1
          let f2: F2 = f1
          ```

        - 解释：函数类型F2兼容函数类型F1，因为F1和F2的第一个参数类型相同

        - ```typescript
          interface Point2D { x: number; y: number }
          interface Point3D { x: number; y: number; z: number }
          type F2 = (p: Point2D) => void
          type F3 = (p: Point3D) => void
          let f2: F2
          let f3: F3 = f2
          f2 = f3    //报错，因为把参数多的赋给参数少的
          ```

        - 解释：

          - 注意：此处与前面讲到的接口兼容性冲突（接口角度是多给少，函数角度是少给多）
          - 技巧：**将对象拆开，把每个属性看作一个个参数**，则**参数少的(f2)可以赋值给参数多的(f3)**

      - **返回值类型**，只关注返回值类型本身即可

        - ```typescript
          type F5 = () => string
          type F6 = () => string
          let f5: F5
          let f6: F6
          
          type F7 = () => { name: string }
          type F8 = () => { name: string; age: number }
          let f7: F7
          let f8: F8
          f7 = f8
          ```

        - 解释：

          - 如果**返回值类型是原始类型，此时两个类型要相同**。比如：上面的类型F5和F6
          - 如果**返回值类型是对象类型**，此时**成员多的可以赋值给成员少的**。比如：下面的类型F7和F8

          

  - 交叉类型

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





























