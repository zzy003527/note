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
    
    
    //或者
    const firstName = 'Michael';
    const lastName = 'Jackson';
    const year = 1958;
    
    export { firstName, lastName, year };
    ```

- 模块的导入（引用）

  - 在另外的js文件中，我们可以引用上面定义的模块。使用import关键字，导入分2种情况，一种是导入指定的模块，另外一种是导入全部模块。

  - **注意：import命令具有提升效果，会提升到整个模块的头部，首先执行。**

  - 导入指定的模块

    - ```js
          //导入obj数据，My类
          import {obj, My} from './xx.js'
          
          //使用
          console.log(obj, My)
      ```

  - **导入全部模块**

    - 注意：all中的属性不能修改，否则报错

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

  - 需要注意的是，写成一行以后，foo和bar实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用foo和bar。

    

- 默认模块的使用

  - 如果给模块加上default关键字，那么该js文件默认只导出该模块，还需要把大括号去掉。

  - 一个模块只能有一个默认输出，因此export default命令只能使用一次

  - 本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。

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

- 浏览器加载模块

  - 使用webpack打包了多个js文件，然后放到HTML使用script加载时，如果加载顺序不对，就会出现找不到模块的错误。

  - 这是因为模块之间是有依赖关系的，就像你使用jQuery的时候，必须先加载jQuery的代码，才能使用jQuery提供的方法。

  - **加载模块的方法，总是先加载模块1，再加载模块2，因为module类型默认使用defer属性。**

  - ```html
    <script type="module" src="module1.js"></script>
    <script type="module" src="module2.js"></script>
    ```

  - 如果浏览器报错，不能识别export模块，需要先加载babel的js插件来编译它。

  - **注意：模块在导入的时候，script标签的type属性必须改成module**

- 

- 