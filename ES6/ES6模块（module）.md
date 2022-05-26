## ES6模块（module）

- 模块的定义

  - 模块是自动运行在严格模式下并且没有办法退出运行的JavaScript代码。

  - 模块可以是函数、数据、类，需要指定导出的模块名，才能被其他模块访问。

  - ```js
        //数据模块
        const obj = {a: 1}
        //函数模块
        const sum = (a, b) => {
          return a + b
        }
        //类模块
        class My extends React.Components {
        
        }
    ```

- 模块的导出

  - 给数据、函数、类添加一个export，就能导出模块。一个配置型的JavaScript文件中，你可能会封装多种函数，然后给每个函数加上一个export关键字，就能在其他文件访问到。

  - `export`语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
  
     ```js
     export var foo = 'bar';
     setTimeout(() => foo = 'baz', 500);
     
     // 上面代码输出变量foo，值为bar，500 毫秒之后变成baz。
     ```
  
  - ```js
     //数据模块
        export const obj = {a: 1}
        //函数模块
        export const sum = (a, b) => {
          return a + b
        }
        //类模块
        export class My extends React.Components {
        
        }




```js
  const firstName = 'Michael';
  const lastName = 'Jackson';
  const year = 1958;

  export { firstName, lastName, year };
```

- 模块的导入（引用）

  - 在另外的js文件中，我们可以引用上面定义的模块。使用import关键字，导入分2种情况，一种是导入指定的模块，另外一种是导入全部模块。

  - **注意：import命令具有提升效果，会提升到整个模块的头部，首先执行。**(这种行为的本质是，`import`命令是编译阶段执行的，在代码运行之前。)

  - `import`命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。

    ```js
    import {a} from './xxx.js'
    
    a = {}; // Syntax Error : 'a' is read-only;
    
    // 上面代码中，脚本加载了变量a，对其重新赋值就会报错，因为a是一个只读的接口。但是，如果a是一个对象，改写a的属性是允许的
    import {a} from './xxx.js'
    
    a.foo = 'hello'; // 合法操作
    
    // 上面代码中，a的属性可以成功改写，并且其他模块也可以读到改写后的值。不过，这种写法很难查错，建议凡是输入的变量，都当作完全只读，不要轻易改变它的属性。
    ```
  
  - 导入指定的模块
  
    - ```js
          //导入obj数据，My类
          import {obj, My} from './xx.js'
          
          //使用
          console.log(obj, My)
      ```
  
  - **导入全部模块**
  
    - **注意：all中的属性不能修改，否则报错**
  
    - ```js
      //导入全部模块
          import * as all from './xx.js'
          
          //使用
          console.log(all.obj, all.sun(1, 2), all.My)
      ```



- export和import的复合写法

  - 如果在一个模块之中，先输入后输出同一个模块，import语句可以与export语句写在一起。

    ```javascript
    export { foo, bar } from 'my_module';
    
    // 可以简单理解为
    import { foo, bar } from 'my_module';
    export { foo, bar };
    ```

  - 需要注意的是，写成一行以后，**foo和bar实际上并没有被导入当前模块**，只是相当于对外转发了这两个接口，导致**当前模块不能直接使用foo和bar。**

  - 模块的接口改名和整体输出，也可以采用这种写法。

    ```js
    // 接口改名
    export { foo as myFoo } from 'my_module';
    
    // 整体输出
    export * from 'my_module';
    ```
    
  - 具名接口改为默认接口的写法如下。

    ```js
    export { es6 as default } from './someModule';
    
    // 等同于
    import { es6 } from './someModule';
    export default es6;
    ```
    
  - 同样地，默认接口也可以改名为具名接口。

    ```js
    export { default as es6 } from './someModule';
    ```
    
    

- 默认模块的使用

  - 如果给模块加上default关键字，那么该js文件默认只导出该模块，还**需要把大括号去掉。**

  - **一个模块只能有一个默认输出**，因此export default命令只能使用一次

  - 本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。

  - 因为`export default`命令其实只是输出一个叫做`default`的变量，所以它后面不能跟变量声明语句。

    ```js
    // 正确
    export var a = 1;
    
    // 正确
    var a = 1;
    export default a;
    
    // 错误
    export default var a = 1;
    
    // 上面代码中，export default a的含义是将变量a的值赋给变量default。所以，最后一种写法会报错。
    ```

  - 同样地，因为`export default`命令的本质是将后面的值，赋给`default`变量，所以可以直接将一个值写在`export default`之后。

    ```js
    // 正确
    export default 42;
    
    // 报错
    export 42;
    
    // 上面代码中，后一句报错是因为没有指定对外的接口，而前一句指定对外接口为default。
    ```

  - ```js
    //默认模块的定义
        function sum(a, b) {
            return a + b
        }
        export default sum
        
        //导入默认模块
        import sum from './xx.js'
        import add form './xx/js'               //可以任意给exprot default的变量或方法取名字
    ```

- 模块的使用限制

  - 不能在语句和函数之内使用export关键字，只能在模块顶部使用

  - **在react中，模块顶部导入其他模块。**

    ```js
        import react from 'react'
    ```

  - **在vue中，模块顶部导入其他模块。**

    ```js
     <script>
            import sum from './xx.js'
        </script>
    ```

- 修改模块导入和导出名

  - 有2种修改方式，一种是模块导出时修改，一种是导入模块时修改。

  - 导出时修改：

    ```js
    function sum(a, b) {
            return a + b
        }
        export {sum as add}
    
        import { add } from './xx.js'
        add(1, 2)
    ```

  - 导入时修改：

    ```js
        function sum(a, b) {
            return a + b
        }
        export sum
    
        import { sum as add } from './xx.js'
        add(1, 2)
    ```

- 无绑定导入

  - 当你的模块没有可导出模块，**全都是定义的全局变量的时候**，你可以使用无绑定导入。

  - 模块：

    ```js
    let a = 1
        const PI = 3.1314
    ```

  - 无绑定导入：

    ```js
    import './xx.js'
        console.log(a, PI)
    ```

  

- import()函数

  - 因为import是在编译时运行，所以没法像require一样进行动态加载，所以引入import()函数，使之可以动态加载

  - ```js
    import(specifier)
    ```

    - 上面代码中，`import`函数的参数`specifier`，指定所要加载的模块的位置。`import`命令能够接受什么参数，`import()`函数就能接受什么参数，两者区别主要是**后者为动态加载**。

  - `import()`返回一个 Promise 对象。

    ```js
    // 例子
    const main = document.querySelector('main');
    
    import(`./section-modules/${someVariable}.js`)
      .then(module => {
        module.loadPageInto(main);
      })
      .catch(err => {
        main.textContent = err.message;
      });
    ```

    - `import()`函数可以用在任何地方，不仅仅是模块，非模块的脚本也可以使用。它是运行时执行，也就是说，什么时候运行到这一句，就会加载指定的模块。
    - 另外，`import()`函数与所加载的模块没有静态连接关系，这点也是与`import`语句不相同。
    - `import()`类似于 Node 的`require`方法，区别主要是前者是异步加载，后者是同步加载。

  - 下面是`import()`的一些适用场合。

    - 按需加载。

      - `import()`可以在需要的时候，再加载某个模块。

        ```js
        button.addEventListener('click', event => {
          import('./dialogBox.js')
          .then(dialogBox => {
            dialogBox.open();
          })
          .catch(error => {
            /* Error handling */
          })
        });
        
        // 上面代码中，import()方法放在click事件的监听函数之中，只有用户点击了按钮，才会加载这个模块。
        ```

    - 条件加载

      - `import()`可以放在`if`代码块，根据不同的情况，加载不同的模块。

        ```js
        if (condition) {
          import('moduleA').then(...);
        } else {
          import('moduleB').then(...);
        }
        
        // 上面代码中，如果满足条件，就加载模块 A，否则加载模块 B。
        ```

    - 动态的模块路径

      - `import()`允许模块路径动态生成。

        ```js
        import(f())
        .then(...);
        // 上面代码中，根据函数f的返回结果，加载不同的模块。
        ```

  - 注意：

    - `import()`加载模块成功以后，这个模块会作为一个对象，当作`then`方法的参数。因此，可以使用对象解构赋值的语法，获取输出接口。

      ```js
      import('./myModule.js')
      .then(({export1, export2}) => {
        // ...·
      });
      
      // 上面代码中，export1和export2都是myModule.js的输出接口，可以解构获得。
      ```

    - 如果模块有`default`输出接口，可以用参数直接获得。

      ```js
      import('./myModule.js')
      .then(myModule => {
        console.log(myModule.default);
      });
      ```

    - 上面的代码也可以使用具名输入的形式。

      ```js
      import('./myModule.js')
      .then(({default: theDefault}) => {
        console.log(theDefault);
      });
      ```

    - 如果想同时加载多个模块，可以采用下面的写法。

      ```js
      Promise.all([
        import('./module1.js'),
        import('./module2.js'),
        import('./module3.js'),
      ])
      .then(([module1, module2, module3]) => {
         ···
      });
      ```

    - `import()`也可以用在 async 函数之中。

      ```js
      async function main() {
        const myModule = await import('./myModule.js');
        const {export1, export2} = await import('./myModule.js');
        const [module1, module2, module3] =
          await Promise.all([
            import('./module1.js'),
            import('./module2.js'),
            import('./module3.js'),
          ]);
      }
      main();
      ```

  

- 浏览器加载模块

  - 默认情况下，浏览器是同步加载 JavaScript 脚本，即渲染引擎遇到`<script>`标签就会停下来，等到执行完脚本，再继续向下渲染。如果是外部脚本，还必须加入脚本下载的时间。

    - 所以浏览器允许脚本异步加载，下面就是两种异步加载的语法。

      ```js
      <script src="path/to/myModule.js" defer></script>
      <script src="path/to/myModule.js" async></script>
      
      // 上面代码中，<script>标签打开defer或async属性，脚本就会异步加载。渲染引擎遇到这一行命令，就会开始下载外部脚本，但不会等它下载和执行，而是直接执行后面的命令。
      ```

    - `defer`与`async`的区别是：`defer`要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；`async`一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，`defer`是“渲染完再执行”，`async`是“下载完就执行”。另外，如果有多个`defer`脚本，会按照它们在页面出现的顺序加载，而多个`async`脚本是不能保证加载顺序的。

    

  - 使用webpack打包了多个js文件，然后放到HTML使用script加载时，如果加载顺序不对，就会出现找不到模块的错误。

  - 这是因为模块之间是有依赖关系的，就像你使用jQuery的时候，必须先加载jQuery的代码，才能使用jQuery提供的方法。

  - **加载模块的方法，总是先加载模块1，再加载模块2，因为module类型默认使用defer属性。**

  - ```html
    <script type="module" src="module1.js"></script>
    <script type="module" src="module2.js"></script>
    ```

  - 如果浏览器报错，不能识别export模块，需要先加载babel的js插件来编译它。

  - **注意：模块在导入的时候，script标签的type属性必须改成module**

    

  - ES6 模块也允许内嵌在网页中，语法行为与加载外部脚本完全一致。

    ```js
    <script type="module">
      import utils from "./utils.js";
    
      // other code
    </script>
    
    // 如：导入jQuery
    <script type="module">
      import $ from "./jquery/src/jquery.js";
      $('#message').text('Hi from jQuery!');
    </script>
    ```

  - 对于外部的模块脚本（上例是`foo.js`），有几点需要注意。

    - 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
    - 模块脚本自动采用严格模式，不管有没有声明`use strict`。
    - 模块之中，可以使用`import`命令加载其他模块（`.js`后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用`export`命令输出对外接口。
    - 模块之中，顶层的`this`关键字返回`undefined`，而不是指向`window`。也就是说，在模块顶层使用`this`关键字，是无意义的。
    - 同一个模块如果加载多次，将只执行一次。

  - 

  

- ES6模块与CommonJS的差异

  - 它们有三个重大差异。

    - CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

      - CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。

        ```js
        // lib.js
        var counter = 3;
        function incCounter() {
          counter++;
        }
        module.exports = {
          counter: counter,
          incCounter: incCounter,
        };
        
        // 上面代码输出内部变量counter和改写这个变量的内部方法incCounter。然后，在main.js里面加载这个模块。
        
        // main.js
        var mod = require('./lib');
        
        console.log(mod.counter);  // 3
        mod.incCounter();
        console.log(mod.counter); // 3
        
        // 上面代码说明，lib.js模块加载以后，它的内部变化就影响不到输出的mod.counter了。这是因为mod.counter是一个原始类型的值，会被缓存。除非写成一个函数，才能得到内部变动后的值。
        // lib.js
        var counter = 3;
        function incCounter() {
          counter++;
        }
        module.exports = {
          get counter() {
            return counter
          },
          incCounter: incCounter,
        };
        
        // 上面代码中，输出的counter属性实际上是一个取值器函数。现在再执行main.js，就可以正确读取内部变量counter的变动了。
        ```

      - 而ES6 的`import`的原始值变了，`import`加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。

    - CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

    - CommonJS 模块的`require()`是同步加载模块，ES6 模块的`import`命令是异步加载，有一个独立的模块依赖的解析阶段。

  - 

  

- Node.js的模块加载方法

  - JavaScript 现在有两种模块。一种是 ES6 模块，简称 ESM；另一种是 CommonJS 模块，简称 CJS。

  - CommonJS 模块是 Node.js 专用的，与 ES6 模块不兼容。语法上面，两者最明显的差异是，CommonJS 模块使用`require()`和`module.exports`，ES6 模块使用`import`和`export`。

  - Node.js 要求 ES6 模块采用`.mjs`后缀文件名。也就是说，只要脚本文件里面使用`import`或者`export`命令，那么就必须采用`.mjs`后缀名。Node.js 遇到`.mjs`文件，就认为它是 ES6 模块，默认启用严格模式，不必在每个模块文件顶部指定`"use strict"`。

  - 注意，**<u>ES6 模块与 CommonJS 模块尽量不要混用</u>**。`require`命令不能加载`.mjs`文件，会报错，只有`import`命令才可以加载`.mjs`文件。反过来，`.mjs`文件里面也不能使用`require`命令，必须使用`import`。

  - `.mjs`文件总是以 ES6 模块加载，`.cjs`文件总是以 CommonJS 模块加载，`.js`文件的加载取决于`package.json`里面`type`字段的设置。

    - 如果希望.js加载ES6模块，那么在项目的`package.json`文件中，指定`type`字段为`module`。

      ```js
      {
         "type": "module"
      }
      ```

    - 如果希望.js加载 CommonJS模块，那么在项目的`package.json`文件中，指定`type`字段为`commonjs`。

      ```js
      {
         "type": "commonjs"
      }
      ```

    

  - package.json的main字段

    - `package.json`文件有两个字段可以指定模块的入口文件：`main`和`exports`。比较简单的模块，可以只使用`main`字段，指定模块加载的入口文件。

      ```js
      // ./node_modules/es-module-package/package.json
      {
        "type": "module",
        "main": "./src/index.js"
      }
      
      // 上面代码指定项目的入口脚本为./src/index.js，它的格式为 ES6 模块。如果没有type字段，index.js就会被解释为 CommonJS 模块。
      //然后，import命令就可以加载这个模块。
      
      // ./my-app.mjs
      import { something } from 'es-module-package';
      // 实际加载的是 ./node_modules/es-module-package/src/index.js
      
      // 上面代码中，运行该脚本以后，Node.js 就会到./node_modules目录下面，寻找es-module-package模块，然后根据该模块package.json的main字段去执行入口文件。
      ```

    - 这时，如果用 CommonJS 模块的`require()`命令去加载`es-module-package`模块**会报错**，因为 CommonJS 模块不能处理`export`命令。

  - package.json的exports字段

    - 子目录别名

      - `package.json`文件的`exports`字段可以指定脚本或子目录的别名。

        ```js
        // ./node_modules/es-module-package/package.json
        {
          "exports": {
            "./submodule": "./src/submodule.js"
          }
        }
        
        // 上面的代码指定src/submodule.js别名为submodule，然后就可以从别名加载这个文件。
        
        import submodule from 'es-module-package/submodule';
        // 加载 ./node_modules/es-module-package/src/submodule.js
        ```

      - 相当于用属性名代替属性值进行导入

        

    - main 的别名

      - `exports`字段的别名如果是`.`，就代表模块的主入口，优先级高于`main`字段，并且可以直接简写成`exports`字段的值。

        ```js
        {
          "exports": {
            ".": "./main.js"
          }
        }
        
        // 等同于
        {
          "exports": "./main.js"
        }
        ```

      

    - 条件加载

      - 利用`.`这个别名，可以为 ES6 模块和 CommonJS 指定不同的入口。目前，这个功能需要在 Node.js 运行的时候，打开`--experimental-conditional-exports`标志。

        ```js
        {
          "type": "module",
          "exports": {
            ".": {
              "require": "./main.cjs",
              "default": "./main.js"
            }
          }
        }
        // 上面代码中，别名.的require条件指定require()命令的入口文件（即 CommonJS 的入口），default条件指定其他情况的入口（即 ES6 的入口）。
        
        // 上面的写法可以简写如下。
        {
          "exports": {
            "require": "./main.cjs",
            "default": "./main.js"
          }
        }
        ```

    

  - CommonJS模块加载ES6模块

    - CommonJS 的`require()`命令不能加载 ES6 模块，会报错，只能使用`import()`这个方法加载。

      ```js
      (async () => {
        await import('./my-app.mjs');
      })();
      ```

    - `require()`不支持 ES6 模块的一个原因是，它是同步加载，而 ES6 模块内部可以使用顶层`await`命令，导致无法被同步加载。

    

  - ES6模块加载CommonJS模块

    - ES6 模块的`import`命令可以加载 CommonJS 模块，但是只能整体加载，不能只加载单一的输出项。

      ```js
      // 正确
      import packageMain from 'commonjs-package';
      
      // 报错
      import { method } from 'commonjs-package';
      
      // 这是因为 ES6 模块需要支持静态代码分析，而 CommonJS 模块的输出接口是module.exports，是一个对象，无法被静态分析，所以只能整体加载。
      ```

    - 加载单一的输出项，可以写成下面这样。

      ```js
      import packageMain from 'commonjs-package';
      const { method } = packageMain;
      ```

    

  - Node.js 的内置模块

    - Node.js 的内置模块可以整体加载，也可以加载指定的输出项。

    - ```js
      // 整体加载
      import EventEmitter from 'events';
      const e = new EventEmitter();
      
      // 加载指定的输出项
      import { readFile } from 'fs';
      readFile('./foo.txt', (err, source) => {
        if (err) {
          console.error(err);
        } else {
          console.log(source);
        }
      });
      ```

      

- CommonJs模块的加载原理

  - CommonJS 的一个模块，就是一个脚本文件。`require`命令第一次加载该脚本，就会执行整个脚本，然后在内存生成一个对象。

    ```js
    {
      id: '...',
      exports: { ... },
      loaded: true,
      ...
    }
    ```

  - 上面代码就是 Node 内部加载模块后生成的一个对象。该对象的`id`属性是模块名，`exports`属性是模块输出的各个接口，`loaded`属性是一个布尔值，表示该模块的脚本是否执行完毕。

  - 以后需要用到这个模块的时候，就会到`exports`属性上面取值。即使再次执行`require`命令，也不会再次执行该模块，而是到缓存之中取值。也就是说，CommonJS 模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。

- 

- 

- 

- 

- 

- 

- 

- d





















