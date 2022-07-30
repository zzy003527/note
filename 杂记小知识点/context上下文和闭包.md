# 前端暑假第二周分享会



## JS的上下文

> 执行上下文是评估和执行 JavaScript 代码的环境的抽象概念。每当 Javascript 代码在运行的时候，它都是在执行上下文中运行

- **全局执行上下文** — 这是默认或者说基础的上下文，任何不在函数内部的代码都在全局上下文中。它会执行两件事：创建一个全局的 window 对象（浏览器的情况下），并且设置 `this` 的值等于这个全局对象。一个程序中只会有一个全局执行上下文。
- **函数执行上下文** — 每当一个函数被调用时, 都会为该函数创建一个新的上下文。每个函数都有它自己的执行上下文，不过是在函数被调用时创建的。函数上下文可以有任意多个。每当一个新的执行上下文被创建，它会按定义的顺序（将在后文讨论）执行一系列步骤。

### 执行栈

> 也称“调用栈”，是一种拥有 LIFO（后进先出）数据结构的栈，被用来存储代码运行时创建的所有执行上下文
>
> 当 JavaScript 引擎第一次遇到你的脚本时，它会创建一个全局的执行上下文并且压入当前执行栈。每当引擎遇到一个函数调用，它会为该函数创建一个新的执行上下文并压入栈的顶部。当一个函数执行完毕，它的执行上下文会从当前栈弹出，控制流程到达下一个执行上下文

```js
let a = 'Hello World!';

function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}

function second() {
  console.log('Inside second function');
}

first();
console.log('Inside Global Execution Context');
```

### 创建执行上下文

>  This 绑定、创建词法环境组件、创建变量环境组件。

- **This绑定**：全局执行上下文中，this 的值指向全局对象，在浏览器中指向 Window 对象
- **词法环境**

>**词法环境**是一种规范类型，基于 ECMAScript 代码的词法嵌套结构来定义**标识符**和具体**变量**和函数的关联。一个词法环境由环境记录器和一个可能的引用外部词法环境的空值组成。

​	词法环境的**内部**有两个组件：(1) **环境记录器**和 (2) 一个**外部环境的引用**

>1. 环境记录器是存储变量和函数声明的实际位置。
>2. 外部环境的引用意味着它可以访问其父级词法环境（作用域）。

​	词法环境有两种类型

> 1.**全局环境**：外部环境引用是 null。它拥有内建的 Object/Array/等、在环境记录器内的原型函数（关联全局对象，比如 window 对象）还有任何用户定义的全局变量
>
> 2.**函数环境**：函数内部用户定义的变量存储在环境记录器中。并且引用的外部环境可能是全局环境，或者任何包含此内部函数的外部函数。

​	以上两种环境，分别对应两种环境记录器

> 1. **对象环境记录器**：存储变量、函数和参数
> 2. **声明式环境记录器**：用来定义出现在全局上下文中的变量和函数的关系

​	关系如下

​	全局环境=>词法环境=>环境记录器=>对象

​									  =>外部环境（null）

​	变量环境=>词法环境=>环境记录器=>声明

​									  =>外部环境（全局环境或外部函数）

- **变量环境**

> 同样是一个词法环境，有着上面定义的词法环境的所有属性
>
> 其环境记录器持有变量声明语句在执行上下文中创建的绑定关系

```js
let a = 20;
const b = 30;
var c;
function multiply(e, f) {
	var g = 20;
	return e * f * g;
}
c = multiply(20, 30);
```
- **上下文的创建阶段**

```js
GlobalExectionContext = {
//全局执行上下文
  ThisBinding: <Global Object>,
  LexicalEnvironment: {
   //词法环境
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
      a: < uninitialized >,
      b: < uninitialized >,
      multiply: < func >
    }
    outer: <null>
  },

  VariableEnvironment: {
   //变量环境
    EnvironmentRecord: {
      Type: "Object",
      // 在这里绑定标识符
      c: undefined,
    }
    outer: <null>
  }
}

FunctionExectionContext = {
// 函数执行上下文
  ThisBinding: <Global Object>,

  LexicalEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      Arguments: {0: 20, 1: 30, length: 2},
    },
    outer: <GlobalLexicalEnvironment>
  },

VariableEnvironment: {
    EnvironmentRecord: {
      Type: "Declarative",
      // 在这里绑定标识符
      g: undefined
    },
    outer: <GlobalLexicalEnvironment>
  }
}
     
// 只有遇到调用函数 multiply 时，函数执行上下文才会被创建
// 创建阶段时,变量最初设置为 undefined（var 情况下），或者未初始化（let 和 const 情况下）（变量提升）
```

- **执行阶段**

  完成对所有这些变量的分配，最后执行代码

  如果 JavaScript 引擎不能在源码中声明的实际位置找到 `let` 变量的值，它会被赋值为 `undefined`





## 闭包

- 基础例子

```js
1: let a = 3
2: function addTwo(x) {
3:   let ret = x + 2
4:   return ret
5: }
6: let b = addTwo(a)
7: console.log(b)
```

- 词法作用域
  - 词法作用域在函数声明的时候就确定了

```js
1: let val = 2
2: function multiplyThis(n) {
3:   let ret = n * val
4:   return ret
5: }
6: let multiplied = multiplyThis(6)
7: console.log('example of scope:', multiplied)

//一个函数可以访问在它的调用上下文中定义的变量，这个就是词法作用域（Lexical scope）
```

- 返回函数的函数

```js
 1: let val = 7
 2: function createAdder() {
 3:   function addNumbers(a, b) {
 4:     let ret = a + b
 5:     return ret
 6:   }
 7:   return addNumbers
 8: }
 9: let adder = createAdder()
10: let sum = adder(val, 8)
11: console.log('example of function returning a function: ', sum)
```

- 闭包

```js
 1: function createCounter() {
 2:   let counter = 0
 3:   const myFunction = function() {
 4:     counter = counter + 1
 5:     return counter
 6:   }
 7:   return myFunction
 8: }
 9: const increment = createCounter()
10: const c1 = increment()
11: const c2 = increment()
12: const c3 = increment()
13: console.log('example increment', c1, c2, c3)       //1,2,3
```

> **无论何时声明新函数并将其赋值给变量，都要存储函数定义和闭包。闭包包含 在函数创建时作用域中的所有变量，它类似于背包。函数定义附带一个小背包，它的包中存储了函数定义创建时作用域中的所有变量。**

> 函数包含函数定义，还有一个带有变量的闭包
>
> 当函数返回函数时，返回的函数可以访问不属于全局作用域的变量，但它们仅存在于其闭包中

最后一个案例

```js
let c = 4
function addX(x) {
  return function(n) {
     return n + x
  }
}
const addThree = addX(3)
let d = addThree(c)
console.log('example partial application', d)            //7
```

