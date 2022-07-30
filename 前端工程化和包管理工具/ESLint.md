## ESLint

- 创建项目时,要选择ESLint + Standard config,接下来选择Lint on save,然后In dedicated config files

- .eslintrc.js的配置文件中的rules规则

  - no-console表示不能使用console对象的方法
  - no-debugger代表禁用debugger

- 初步了解常见的ESLint的语法规则

  - ESLint不允许出现连续两个的空行

  - 要求或禁止文件末尾必须有空行

  - | 规则名称                    | 规则约束/默认约束                          |
    | --------------------------- | ------------------------------------------ |
    | quotes                      | 默认:字符串需要使用单引号包裹              |
    | key-spacing                 | 默认:对象的属性和值之间,需要有一个空格分割 |
    | comma-dangle                | 默认:对象或数组的末尾,不允许出现多余的逗号 |
    | no-multiple-empty-lines     | 不允许出现多个空行                         |
    | eol-last                    | 默认:文件的末尾必须保留一个空行            |
    | spaced-comment              | 在注释中的//或者/*后强制使用一致的间距     |
    | indent                      | 强制一致的缩进                             |
    | import/first                | import导入模块的语句必须声明在文件的顶部   |
    | space-before-function-paren | 方法的形参之前是否需要保留一个空格         |
    | no-trailing-spaces          | 不允许在行尾出现多余的空格                 |

  

- 介绍

  - 为什么要用ESLint
    - 统一团队编码规范
    - 统一语法
    - 减少git不必要的提交
    - 避免低级错误
    - 在编译时检查语法，而不是等到JS引擎运行时才检查
  - eslint用法
    - 可以手动下载配置
    - 可以通过vue脚手架创建项目时自动下载配置

- 认识ESLint

  - ESLint包

    - 安装方式：
      - 通过npm 或yarn直接全局或项目安装 `npm i eslint -D`
      - 通过vue脚手架创建项目时选择安装eslint模块包`vue create 创建过程中选择 lint`

  - VSCode中ESLint扩展工具

    - 安装方式：通过vscode搜索安装

      

- 手动下载配置（原理）

  - 创建项目

    - 创建一个测试文件夹：`eslint_test`
    - 初始化项目：`npm init -y`（创建package.json）

  - ESLint安装

    - 直接在项目中安装eslint包`npm i eslint -D`
    - 注意安装结构：`node_modules`中下载了很多包
      - `.bin/eslint.cmd`脚本文件，内部通过nodejs执行eslint运行包的代码
      - `@eslint包`用来规范eslint配置文件
      - `eslint开头的包`是eslint运行包，包含eslint代码

  - 生成ESLint配置文件

    - **注意：ESLint可以创建独立的配置文件.eslintrc.js，也可以直接在package.json中配置**
    - 执行`node_modules/.bin`文件夹里的**eslint脚本**来**创建配置文件**
      - 包含完整脚本路径的命令：`./node_modules/.bin/eslint --init`
      - 也可以用npx来执行**（推荐）**：`npx eslint --init`（npx是随node一起安装的，能去**.bin目录**里找模板脚本文件，简化执行脚本的语法）
    - 创建配置文件过程中 ，需要选择配置
      - 第一个问题一般选择第三个（To check syntax, find problems,and enforce code style)
      - 第二个问题暂时选择第二个（CommonJS(require/exports)
      - 第三个问题选择你用的框架
      - 第四个问题选择是否用到ts
      - 第五个问题是问你要在哪里运行当前的项目代码（要在Browser（浏览器）中运行，还是在node环境下运行）
      - 第六个问题是问你需不需要定义一套规则（可以选第一个——Use a popular style guide)
      - 第七个问题选择Standrad这一套
      - 第八个问题问的是配置文件用哪种格式的（选JavaScript）
      - 第九个问题是否需要安装检查到的依赖包（选择yes）
    - 执行结果
      - 创建了配置文件`.eslintrc.js`
      - 下载了相关规范包（主要是下载了standard语法规范包，而它需要用到import/node/promise包）

  - 生成配置文件报错

    - 如果在生成配置文件最后面报了一个错：

      ```js
      Error:An error occurred while generating your JavaScript config file./t follow your liniting rules.
      ```

    - 如果不理它，那么后面检查代码规范时，会导致检查失败`Invalid ecmaVersion`

    - 原因：创建配置文件选项过程中，选择了强制代码风格规范而这个选择，会降低当前项目eslint版本（8.3=>7.32），从而不支持es13的语法

  - 解决方案

    - 修改生成的配置文件里的配置项目：`ecmaVersion`，**从13（latest)=>12**
    - 本质原因：这些 新下载的包 用到还是eslint的旧版本，而旧版本的eslint还不支持es13

  - ESLint执行

    - 命令：**`npx eslint ./需要检查语法的文件路径`**
    - 如果违反规范，会将错误提示到终端，说明eslint工作正常

    

- ESLint配置文件入门

  - 官网：https://eslint.bootcss.com/docs/user-guide/configuring

  - 配置文件格式

    - 我们通过`npx eslint --init`创建配置文件时，有提供配置文件的格式给我们选择：
      - .js/.yaml/.json
    - eslint加载的优先级是：js > yaml > json，所以我们最好选择**js格式**

  - JS格式使用模式

    - 我们注意到配置文件（.eslintrc.js）内部使用的module.exports导出配置数据
    - 这是因为我们在前面添加配置文件时，选择了`CommonJS`
    - 如果选择了`JavaScript modules`，那么就会使使用`export`导出配置数据
    - 推荐：`CommonJS`
      - 由于我们开发时，一般使用的时vue脚手架，内部webpack打包默认用到是CommonJS
      - 所以ESLint配置文件 应尽可能与它保持一致

  - env节点

    - ```js
      "env": {
          "browser":true,
          "commonjs":true,
          "es2021":true
      }
      ```

    - 由于ESLint的各种规范中，一般都不允许使用**未在页面内声明的成员**（比如没有let，const等声明的变量）

    - 而开发中经常会用到一些运行环境自带的api，如：

      - 浏览器中的window/document等
      - nodejs中的__dirname等
      - es2021的WeakRef等

    - 所以要告诉eslint，当前代码是运行在哪些环境中，这样检查时就不会报错了

  - globals节点

    - ```js
      "globals": {
          "_":true,   //可以读取，可以修改
          "$":false    //可以读取，不能修改
      }
      ```

    - 也可以使用`globals`节点来手动配置全局成员

    - 注意：这个节点需要手动添加，默认是没有的

  - extends节点

    - ```js
      "extends": {
          "standard"             //"eslint-config-standard"
      }
      ```

    - ESLint检查时用哪些规范？通过这个节点可以配置使用**内置规范**还是**第三方规范**

      - 如：`"eslint-config-standard": "^16.0.3"`
      - 配置的是 使用第三方下载的`eslint-config-standard`规范
      - 注意：使用extends时，可以省略`eslint-config-`，直接写成`standard`

  - parserOptions节点

    - ```js
      "parserOptions": {
          "ecmaVersion": 12
      }
      ```

    - ESLint解析器解析代码时，可以指定用**哪个js的版本**

    - 注意：这里是指定检查时安装哪个js语法检查，但是这里不包含全局变量

    - 全局变量需要通过前面的`env`节点配置

  - rules节点

    - ```js
      "rules": {}
      ```

    - 两个用法：

      - 不使用`extends`节点配置 **整套的规范** ，而是在`rules`节点中直接配置
        - 即在上面第六个问题时选择`Inspect your JavaScript files`
      - 使用`extends`节点配置**整套的规范**，在`rules`节点中修改部分规范的配置
      - d

      

- ESLint检查语法的规则

  - ESLint语法规范本质

    - 就是**【函数】**
    - 我们可以通过看ESLint源码查看：
      - eslint内置285个规则，每条规则都是一个**create函数**
      - 在进行语法检查时，会将代码传入这些函数内做检查

  - ESLint内置语法规范

    - ESLint安装时，就已经准备了285个规范了
    - 具体分为七类

  - 自定义语法规范

    - 除了内置的规范外，如果想加入自己的新规范怎么办

    - 我们就可以按照ESLint的机制来添加自己的规范了

    - 比如我们下载的第三方规则包里有些就用了自己的规则

      

- 语法规范包类型

  - 前面说到，ESLint按照时自带285个规范，而开发时，未必都要使用

  - 所以就有了不同的选择组合：

    - **ESLint内置规范包**：eslint-all/eslint-recommended
    - **标准规范包**:eslint-config-standard
    - **第三方规范包**:(google/airbnb/facebook...)

  - 内置规范包

    - 已经随eslint一起下载：
      - `eslint-all`：使用全部285个规则
      - `eslint-recommended`：只使用推荐的60个规则

  - 标准规范包（需要下载）

    - 包名：`eslint-config-standard`也使用了200多个规则

    - 下载方式：

      - 可以在前面创建eslint配置文件时选择下载（standard）

      - 手动下载：官方git地址：https://github.com/standard/standard

        - 下载包：`npm i eslint-config-standard -D`

        - 降低eslint版本：`npm i eslint@7.32.0`（standard依赖低版本的eslint）

        - 修改eslint配置文件（.eslintrc.js）中的es版本：

          ```js
          "parserOptions": {
              "ecmaVersion": 12 
          }
          ```

  - 第三方规范包

    - 包名：`eslint-config-airbnb-base`

    - 官方npm地址：https://www.npmjs.com/package/eslint-config-airbnb-base

    - 手动下载：

      - `npm info "eslint-config-airbnb-base@latest" peerDependencies`

      - 下载相关包：`npx install-peerdeps --dev eslint-config-airbnb-base`

        

- 配置规范包

  - 修改eslint配置文件

    ```js
    module.export = {
        "env": {
        "browser":true,
        "commonjs":true,
        "es2021":true
        },
        // "extends": "eslint:all",               //内置，所有规则
        // "extends": "eslint:recommended",        //内置，推荐规则
        "extends": "eslint-config-standard",       //第三方，标准规则
        // "extends": "eslint-config-aribnb-base",     //第三方，airbnb公司规则
        "parserOptions": {
            "ecmaVersion": 12
        },
        "rules": {
        }
    }
    ```

  - 运行ESLint

    - 使用eslint检查  **目标文件或文件夹**

    - 注意：ESLint默认只检查js文件代码，如果想检查vue或react文件，需要装其他插件包

      ```js
      npx eslint ./index.js          //检查目标文件
      npx eslint ./src         //检查吗  目标文件夹  中所有js文件
      ```

    - 测试不同包检查相同代码

      - 严格程度：all > airbnb-base > standard > recommended

        