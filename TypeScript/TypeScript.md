

## TypeScript

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
      let strings: Array<string> = ['a','b','c']
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
        let age = 18   // TS自动推断出变量age为number类型
        
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
      - 注意：typeof只能用来查询变量或属性的类型，**无法查询其他形式的类型（比如：函数调用后返回的类型）**









- TypeScript高级类型

  - 概述

    - TS中的高级类型有很多，有：
      - class类
      - 类型兼容性
      - 交叉类型
      - 泛型和keyof
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
        - Person类实现接口Singable意味着Person类中必须提供Singable接口种指定的所有方法和属性

      

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

        - **当readonly与其他访问修饰符同时存在时，readonly需要写在后面**

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
  
          - <u>**注意：此处与前面讲到的接口兼容性冲突（接口角度是多给少，函数角度是少给多）**</u>
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

    - **交叉类型（&）**：功能类似于接口继承（extends），**<u>用于组合多个类型为一个类型（</u>常用于对象类型）**。

    - 比如：
  
      - ```typescript
        interface Person { name: string }
        interface Contact { phone: string }
        type PersonDetail = Person & Contact
        let obj: PersonDetail = {
            name: 'jack',
            phone: '133...'
        }
        ```

      - 解释：使用交叉类型后，新的类型PersonDetail就**同时具备了**Person和Contact的所有属性类型。

      - 相当于：
  
        ```typescript
        type Person Detail = { name: string; phone: string }
        ```

    - 交叉类型（&）和接口继承（extends）的对比：

      - 相同点：都可以实现对象类型的组合

      - 不同点：两种方式实现类型组合时，对于**同名属性之间，处理类型冲突的方式不同**
  
      - ```typescript
        interface A {
            fn: (value: number) => string
        }
        interface B extends A {
            fn: (value: string) =>  string
        }                  //接口继承会报错
        
        //----------------------------------------
        interface A {
            fn: (value: number) => string
        }
        interface B {
            fn: (value: string) =>  string
        }         
        type C = A & B                  //交叉类型不报错
        
        //以上代码，接口继承会报错（因为类型不兼容）；交叉类型没有错误，可以理解为
        fn: (value: string | number) => string
        ```

        

  - 泛型

    - **泛型**是可以在**保证类型安全**前提下，让函数等**与多种类型一起工作**，从而**实现复用**，常用于：**函数、接口、class**中

    - 需求：创建一个id函数，传入什么数据就返回该数据本身（也就是说，参数和返回值类型相同）
  
      ```typescript
      function id(value: number): number { return value }
      //比如，id(10)调用以上函数就会直接返回10本身。但是，该函数只接受数值类型，无法用于其他类型
      //为了能让函数能够接受任意类型，可以将参数类型修改为any。但是，这样子就失去了TS的类型保护，类型不安全
      function id(value: any): any { return value }
      
      //泛型在保证类型安全（不丢失类型信息）的同时，可以让函数等与多种不同的类型一起工作，灵活可复用
      ```

    - 创建泛型函数
  
      - ```typescript
        function id<Type>(value: Type): Type { return value }
        ```

      - 解释：
  
        - 语法：在函数名称的后面加**`<>`（尖括号）**，**尖括号中添加类型变量**，比如此处的Type
        - **类型变量**Type，**是一种特殊类型的变量，它处理类型**而不是值
        - 该类型变量相当于一个类型容器，能够捕获用户提供的类型（具体是什么由用户调用该函数时指定）
        - 因为Type是类型，因此可以将其作为函数参数和返回值的类型，表示参数和返回值具有相同的类型
        - 类型变量Type，可以是任意合法的变量名称

    - 调用泛型函数：
  
      - ```typescript
        function id<Type>(value: Type):Type { return value }
        const num = id<number>(10)
        const str = id<string>('a')
        ```

      - 解释：
  
        - 语法：在函数名称的后面添加**`<>`（尖括号）**，**尖括号中间指定具体的类型**，比如number
        - 当传入类型number后，这个类型就会被函数声明时指定的类型变量Type捕获到
        - 此时，Type的类型就是number，所以，函数id参数和返回值的类型也都是number

      - 同样，如果传入类型string，函数id参数和返回值的类型就都是string

      - 这样，通过泛型就做到了让id函数与多种不同的类型一起工作，**实现了复用的同时保证了类型安全**

    - 简化调用泛型函数
  
      - ```typescript
        funciton id<Type>(value: Type): Type { return value }
        
        let num = id<number>(10)
        //可写为
        let num = id(10)
        ```

      - 解释：
  
        - 在调用泛型函数时，**可以省略<类型>来简化泛型函数的调用**
        - 此时，TS内部会采用以一种叫做**类型参数推断**的机制，来根据传入的实参自动推断出类型变量Type的类型
        - 比如，传入实参10，TS会自动推断出变量num的类型number，并作为Type的类型

      - 推荐：使用这种简化的方式调用泛型函数，使代码更易于阅读

      - 说明：当编译器无法推断类型或者推断的类型不准确使，就需要显式地传入类型参数

    - **泛型约束**

      - 泛型约束：默认情况下，泛型函数的类型变量Type可以代表多个类型，这导致无法访问任何属性

      - 比如，id('a')调用函数时获取参数的长度：
  
        - ```typescript
          function id<Type>(value: Type): Type {
              console.log(value.length)
              return value
          }
          ```

        - 解释：Type可以代表任意类型，无法保证一定存在length属性，比如number类型就没有length。此时，就需要为泛型**添加约束**来**收缩类型**（缩窄类型取值范围）

      - 添加泛型约束收缩类型，主要有两种方式：
  
        - 指定更加具体的类型
        - 添加约束

      - **指定更加具体的类型**
  
        - ```typescript
          function id<Type>(value: Type[]): Type[] {
              console.log(value.length)
              return value
          }
          ```

        - 比如，将类型修改为Type[]（Type类型的数组），因为只要是数组就一定存在length属性，因此就可以访问了

      - 添加约束
  
        - ```typescript
          interface ILength { length: number }
          function id<Type extends ILength>(value: Type): Type {
              console.log(value.length)
              return value
          }
          ```

        - 解释：
  
          - 创建描述约束的接口ILength，该接口要求提供length属性
          - 通过**`extends`**关键字使用该接口，为泛型（类型变量）添加约束
          - 该约束表示：**传入的类型必须具有length属性**

        - 注意：传入的实参（比如数组）只要有length属性即可，这也符合前面的接口的类型兼容性

      - 泛型的类型变量可以有多个，并且**类型变量之间还可以约束**（比如，第二个类型变量受第一个类型变量约束）

        比如，创建一个函数来获取对象中属性的值：
  
        - ```typescript
          function getProp<Type,Key extends keyof Type>(obj: Type,key: Key) {
              return obj[key]
          }
          let person = { name: 'jack',age: 18 }
          getProp(person, 'name')
          getProp('abcd',1)   //若第一个参数是字符串，则第二个参数填写数字表示索引
          ```

        - 解释：
  
          - 添加了第二个类型变量Key，两个类型变量之间使用**`,`(逗号)**分隔
          - **keyof**关键字**接收一个对象类型，生成其键名称（可能是字符串或数字）的联合类型**
          - 本示例中keyof Type实际上获取的是person对象所有键的联合类型，也就是：'name' | 'age'
          - 类型变量Key受Type约束，可以理解为：Key只能是Type所有键中的任意一个，或者是只能访问对象中存在的属性

    - **泛型接口**

      - 泛型接口：接口也可以配合泛型来使用，以增加其灵活性，增强其复用性
  
      - ```typescript
        interface IdFunc<Type> {
            id: (value: Type) => Type
            ids: () => Type[]
        }
        
        let obj: IdFunc<number> = {
            id(value) { return value }，
            ids() { return [1,3,5] }
        }
        ```

      - 解释：
  
        - 在接口名称的后面添加**<类型变量>**，那么，这个接口就变成了泛型接口
        - 接口的类型变量，对接口中所有其他成员可见，也就是**接口中所有成员都可以使用类型变量**
        - 使用泛型接口时，**<u>需要显式指定</u>**具体的**类型**（比如，此处的IdFunc<number>）
        - 此时，id方法的参数和返回值类型都是number；ids方法的返回值类型是number[]

      - 数组是泛型接口

        - 实际上，JS的数组在TS中就是一个**泛型接口**
  
        - ```typescript
          const strs = ['a','b','c']
          strs.forEach
          const nums = [1,3,5]
          nums.forEach
          ```

        - 解释：当我们在使用数组是，TS会根据数组的不同类型，来自动将类型变量设置为相应的类型

        - 技巧：可以通过Ctrl+鼠标左键（Mac：option+鼠标左键）来查看具体的类型信息

    - **泛型类**

      - 泛型类：class也可以配合泛型来使用

      - 比如，React的class组件的基类Component就是泛型类，不同的组件有不同的props和state
  
        - ```typescript
          interface IState { count: number }
          interface IProps { maxLength: number }
          class Input Count extends React.Component<IProps,IState> {
              state: IState = {
                  count: 0
              }
              render() {
                  return <div>{this.props.maxLength}</div>
              }
          }
          ```

        - 解释：React.Component泛型类两个类型变量，分别指定props和state类型

      - 创建泛型类
  
        - ```typescript
          class GenericNumber<NumType> {
              defaultValue: NumType
              add: (x: NumType,y: NumType) => NumType
               constructor(value: NumType) {
                  this.defaultValue = value
              }
          }
          ```

        - 解释：
  
          - 类似于泛型接口，在class名称后面添加**<类型变量>**，这个类就变成了泛型类
          - 此处的add方法，采用的是箭头函数形式的类型书写方式
  
        - ```typescript
          const myNum = new GenericNumber<number>() 
          myNum.defaultValue = 10
          
          const myNum1 = new GenericNumber(100)
          //当类中有constructor时，可以在传入时省略类型变量，TS会自动推导类型
          ```

        - 类似于泛型接口，在创建class实例时，在类名后面通过<类型>来指定明确的类型

        

    - **泛型工具类型**

      - 泛型工具类型：TS内置了一些常用的工具类型，来简化TS中的一些常见操作

      - 说明：它们都是**基于泛型实现**的（泛型适用于多种类型，更加通用），并且是内置的，可以直接在代码中使用

      - 主要有以下几个：
  
        - Parial<Type>
        - Readonly<Type>
        - Pick<Type,Keys>
        - Record<Keys,Type>

      - Parial<Type>

        - **Parial<Type>用来构造（创建）一个类型，将Type的所有属性设置为可选**
  
        - ```typescript
          interface Props {
              id: string
              children: number[]
          }
          type PartialProps = Partial<Props>
          ```

        - 解释：构造出来的新类型PartialProps结构和Props相同，但所有属性都变为可选的

      - Readonly<Type>

        - **Readonly<Type>用来构造一个类型，将Type的所有属性都设置为readonly（只读）**
  
        - ```typescript
          interface Props {
              id: string
              children: number[]
          }
          type ReadonlyProps = Readonly<Props>
          
          let props: Readonly Props = { id: '1', children: [] }
          props.id = '2'        //报错，因为此时的props只读
          ```

        - 解释：构造出来的新类型ReadonlyProps结构和Props相同，但所有属性都变为只读的

        - 当我们想重新给id属性赋值时，就会报错：无法分配到"id"，因为它是只读属性

      - Pick<Type,Keys>

        - **Pick<Type,Keys>从Type中选择一组属性来构造新类型**
  
        - ```typescript
          interface Props {
              id: string
              title: string
              children: number[]
          }
          type PickProps = Pick<Props,'id' | 'title'>
          ```

        - 解释：
  
          - Pick工具类型有两个类型变量
            - 第一个表示选择谁的属性
            - 第二个表示选择哪几个属性
          - 其中第二个类型变量，如果只选择一个则只传入该属性名即可
          - **第二个类型变量传入的属性只能是第一个类型变量中存在的属性**
          - 构造出来的新类型PickProps ，只有id和title两个属性类型

      - Record<Keys,Type>

        - **Record<Keys,Type>构造一个对象类型，属性键为Keys，属性类型为Type**
  
        - ```typescript
          type RecordObj = Record<'a' | 'b' | 'c',string[]>
          let obj: RecordObj = {
              a: ['1'],
              b: ['2'],
              c: ['3']
          }
          ```

        - 解释：
  
          - Record工具类型有两个类型变量
            - **表示对象有哪些属性**
            - **表示对象属性的类型**
          - 构造的新对象类型RecordObj表示：这个对象有三个属性分别为a/b/c，属性值的类型都是string[]

          

  - 索引签名类型

    - 绝大多数情况下，我们都可以在使用对象前就确定对象的结构，并为对象添加准确的类型

    - 使用场景：**当无法确定对象中有哪些属性**（或者是对象中可以出现任意多个属性），此时，**就用到索引签名类型了**
  
    - ```typescript
      interface AnyObject {
          [key: string]: number
      }
      let obj: AnyObject = {
          a: 1,
          b: 2
      }
      ```

    - 解释：
  
      - 使用**`[key：string]`**来约束该接口中允许出现的属性名称。表示只要是string类型的属性名称，都可以出现在对象中
      - 这样对象obj中就可以出现任意多个属性（比如a、b等）
      - **key只是一个占位符**，可以换成任意合法的变量名称
      - 注：**JS中对象（{}）的键是string类型的**

    - 在JS中数组是一类特殊的对象，特殊在**数组的键（索引）是数值类型**

      - 并且，数组也可以出现任意多个元素。所以，在数组对应的泛型接口中，也用到了索引签名类型
  
      - ```typescript
        interface MyArray<T> {
            [n: number]: T
        } 
        let arr: MyArray<number> = [1,3,5]
        ```

      - 解释：

        - MyArray接口模拟原生的数组接口，并使用`[n: number]`来作为索引签名类型

        - 该索引签名类型表示：是要是number类型的键（索引）都可以出现在数组中，或者说数组中可以有任意多个元素

        - 同时也符合数组索引是number类型这一前提

          

  - 映射类型

    - **映射类型：基于旧类型创建新类型（对象类型）**，减少重复、提高开发效率

    - 比如，类型PropKeys有x/y/z，另一个类型Type1中也有x/y/z，并且Type1中x/y/z的类型相同：
  
      - ```typescript
        type PropKeys = 'x' | 'y' | 'z'
        type Type1 = { x: number; y: number; z: number}
        ```

      - 这样书写没错，但是x/y/z 重复写了两次，这种情况，就可以用映射类型来简化
  
      - ```typescript
        type PropKeys = 'x' | 'y' | 'z'
        type Type2 = { [Key in PropKeys]: number }
        ```

      - 解释：
  
        - 映射类型是基于索引签名类型的，所以，该语法类似于索引签名类型，也使用了**`[]`**
        - **Key in PropKeys**表示Key可以是PropKeys联合类型中的任意一个，类似于for in（let k in obj）
        - 使用映射类型创建的新对象类型Type2和类型Type1结构完全相同
        - 注意：**<u>映射类型只能在类型别名（type）中使用，不能在接口（interface）中使用</u>**

    - 映射类型除了根据联合类型创建新类型外，还可以根据对象类型来创建：
  
      - ```typescript
        type Props = { a: number;b: string; c:boolean }
        type Type3 = { [Key in keyof Props]: number }
        ```

      - 解释：
  
        - 首先，先执行**`keyof Props`**获取到对象类型中所有键的联合类型，即：'a' | 'b' | 'c'
        - 然后，**`Key in...`**就表示Key可以是Props中所有的键名称中的任意一个

    - 上面的**泛型工具类型**（比如：Partial<Type>）都是**基于映射类型实现**的

      - 比如：Partial<Type>的实现
  
      - ```typescript
        type Partial<T> = {
            [P in keyof T]?: T[P]
        }
        type Props = { a: number; b: string; c: boolean }
        type PartialProps = Partial<Props>
        ```

      - 解释：
  
        - **keyof T**即keyof Props，表示获取Props的所有键，也就是：'a' | 'b' | 'c'
        - 在[]后面添加`?`(问号)，表示将这些属性变为**可选**的，以此来实现Partial的功能
        - 冒号后面的**T[P]表示获取T中每个键对应的类型**。比如，如果是'a'则类型是number；如果是'b'则类型是string
        - 最终，新类型PartialProps和旧类型Props结构完全相同，只是让所有类型都变为可选了

    - 索引查询类型

      - 上面的**T[P]**语法，在TS中叫做**索引查询（访问）类型**

      - 作用：**用来查询属性的类型**
  
      - ```typescript
        type Props = { a: number; b: string; c: boolean }
        type TypeA = Props['a']
        ```

      - 解释：**Props['a']**表示查询类型Props中属性'a'对应的类型number。所以，TypeA的类型为number

      - 注意**：[]中的属性必须存在于被查询类型中**，否则就会报错

      - **索引查询类型**的其他使用方式：**同时查询多个索引的类型**
  
        - ```typescript
          type Props = { a: number; b: string; c: boolean }
          type TypeA = Props['a' | 'b']         //string | number
          //相当于          
          type TypeB = Props[keyof Props]
          ```

        - 解释：
  
          - 使用字符串字面量的联合类型，获取属性a和b对应的类型，结果为：string | number
          - 使用keyof操作符获取Props中所有键对应的类型，结果为：string | number | boolean
    
  - 重载
  
    - 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。
  
    - 比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`。
  
      利用联合类型，我们可以这么实现：
  
      ```typescript
      function reverse(x: number | string): number | string | void {
          if (typeof x === 'number') {
              return Number(x.toString().split('').reverse().join(''));
          } else if (typeof x === 'string') {
              return x.split('').reverse().join('');
          }
      }
      ```
  
    - **然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。**
  
    - 这时，我们可以使用重载定义多个 `reverse` 的函数类型：
  
      ```typescript
      function reverse(x: number): number;
      function reverse(x: string): string;
      function reverse(x: number | string): number | string | void {
          if (typeof x === 'number') {
              return Number(x.toString().split('').reverse().join(''));
          } else if (typeof x === 'string') {
              return x.split('').reverse().join('');
          }
      }
      ```
  
    - 上例中，我们重复定义了多次函数 `reverse`，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。
  
    - 注意:**TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要<u>优先把精确的定义写在前面。</u>**
  
    
  
  - 声明合并
  
    - 如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型
  
    - 函数的合并：就是上面的重载
  
    - 接口的合并
  
      - 接口合并的属性应该是唯一的
  
      - ```typescript
        interface Alarm {
            price: number;
        }
        interface Alarm {
            price: number;  // 虽然重复了，但是类型都是 `number`，所以不会报错
            weight: number;
        }
        
        //---------------------------------------------------------------
        interface Alarm {
            price: number;
        }
        interface Alarm {
            price: string;  // 类型不一致，会报错
            weight: number;
        }
        
        // index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.
        ```
  
      - 接口中方法的合并与函数一样
  
        ```typescript
        interface Alarm {
            price: number;
            alert(s: string): string;
        }
        interface Alarm {
            weight: number;
            alert(s: string, n: number): string;
        }
        //相当于
        interface Alarm {
            price: number;
            weight: number;
            alert(s: string): string;
            alert(s: string, n: number): string;
        }
        ```
  
    - 类的合并与接口的合并一样





- TypeScript类型声明文件
  - 概述
    - 有些第三方库使用TS编写的，但是都要编译成JS代码才能给开发者使用，此时就失去了TS的类型保护，因此需要加上类型声明文件
    - **类型声明文件：用来为已存在的JS库提供类型信息**
    
  - TS中的两种文件类型
  
    - TS中有两种文件类型：
      1. **.ts文件**
      2. **.d.ts文件**
  
    - .ts文件
      - **既包含类型信息又可执行代码**
      - 可以被编译为.js文件，然后执行代码
      - 用途：编写程序代码的地方
    - .d.ts文件
      - **只包含类型信息**的类型声明文件（不可包含可执行代码）
      - **捕获生成.js文件**，仅用于**提供类型信息**
      - 用途：为JS提供类型信息
    - 总结：.ts是implementation（代码实现文件）；**.d.ts是declaration（类型声明文件）**
    - 如果要为JS库提供类型信息，要使用.d.ts文件
  
  - 类型声明文件的使用说明
  
    - 在使用TS开发项目时，**类型声明文件的使用**有以下两种方式：
  
      1. 使用已有的类型声明文件
      2. 创建自己的类型声明文件
  
    - 学习顺序：**先会用（**别人的）**再会写**（自己的）
  
    - **使用已有的类型声明文件**
  
      - 有两种，分别为：
  
        1. 内置类型声明文件
        2. 第三方库的类型声明文件
  
      - **内置类型声明文件**：TS为JS运行时可用的所有标准化内置API都提供了声明文件
  
        - ```typescript
          (method)Array<number>.forEach(callbackfn: (value: number, index: number, array: number[]) => void,thisArg?: any):void
          ```
  
        - 实际上这都是TS提供的内置类型声明文件
  
        - 可用通过Ctrl + 鼠标左键（Mac：option＋　鼠标左键）来查看内置类型声明文件内容
  
        - 比如，查看forEach方法的类型声明，再VSCode中会自动调整到lib.es5.**d.ts**类型声明文件中
  
        - 当然，像window、document等BOM、DOM   API也都有相应的类型声明（lib.dom**.d.ts**）
  
      - **第三方库的类型声明文件**：目前，几乎所有常用的第三方库都有相应的类型声明文件
  
        - 第三方库的类型声明文件有两种存在形式：
  
          1. **库自带类型声明文件**
          2. **由DefinitelyTyped提供**
  
        - **库自带类型声明文件**：比如axios
  
          - 解释：这种情况下，正常导入该库，**TS就会自动加载库自己的类型声明文件**，以提供该库的类型声明
  
        - **由DefinitelyTyped提供**
  
          - DefinitelyTyped是一个github仓库，**用来提供高质量TypeScript类型声明**
  
          - 可以通过npm/yarn来下载该仓库提供的TS类型声明包，这些包的名称格式为： **`@types/*`**
  
          - 比如，@types/react、@types/lodash等
  
          - 说明：在实际项目开发是，如果你使用的第三方库没有自带的声明文件，VSCode会给出明确的提示
  
            ```typescript
            import _ from 'lodash'
            //报错提示：尝试使用`npm i --save-dev @types/lodash`（如果存在），或者添加一个包含`declare module 'lodash';`的新声明（.d.ts）文件
            ```
  
          - 解释：当按照@types/*类型声明包后，**TS也会自动加载该类声明包**，以提供该库的类型声明
  
          - 补充：TS官方文档提供了一个页面，可以来查询@types/*库
  
    - 创建自己的类型声明文件
  
      - 有两种，分别为：
  
        1. **项目内共享类型**
        2. **为已有JS文件提供类型声明**
  
      - **项目内共享类型：如果多个.ts文件中都用到同一个类型，此时可以创建.d.ts文件提供该类型，**实现类型共享
  
        - 操作步骤：
          - 创建index**.d.ts**类型声明文件
          - 创建需要共享的类型，**并使用export导出**（TS中的类型也可以使用import/export实现模块化功能）
          - 在需要使用共享类型的.ts文件中，通过import导入即可（.d.ts后缀导入时，直接省略）
  
      - **为已有JS文件提供类型声明：**
  
        - 有两种情况：
  
          1. 在将**JS项目<u>迁移</u>到TS项目时**，为了让已有的.js文件有类型声明
          2. 成为库作者，创建库给其他人使用
  
        - 注意：**类型声明文件的编写于模块化方式相关**，不同的模块化方式有不同的写法。但是由于历史原因，JS模块化的发展经历过多种变化（AMD、CommonJS、UMD、ESModule等），而TS支持各种模块化形式的类型声明。这就导致类型声明文件相关内容（https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html）又多又杂
  
        - 演示：基于**最新的ESModule**（import/export）来为已有的.js文件，创建类型声明文件
  
        - 开发环境准备：使用webpack搭建，通过**ts-loader**处理.ts文件
  
        - 演示过程：
  
          - 说明：
  
            - TS项目中也可以使用.js文件
            - 在导入.js文件时，**TS会自动加载于.js同名的.d.ts文件**，以提供类型声明
  
          - `declare`关键字：**用于类型声明，为其他地方**（比如：.js文件）**已存在的变量声明类型，而不是创建一个新的变量（在let、const、function等前面加）**
  
            1. 对于type、interface等这些明确就是TS类型的（只能在TS中使用的），可以省略declare关键字
            2. 对于let、function等具有双重含义（在JS、TS中都能用），应该使用declare关键字，明确指定此处用于类型声明
  
          - 步骤：
  
            - 在js文件的**同一个目录下**创建.d.ts文件
            - 在.d.ts文件对应js文件声明每一个变量（在声明前面加declare，如： `declare let a: number`）
            - 声明完变量之后，在.d.ts中用export导出每一个要用到的变量
            - 然后在要用的地方导入js文件，js文件就可以用了
  
            
