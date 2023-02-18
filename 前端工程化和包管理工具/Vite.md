## Vite

- 什么是构建工具

  - 企业级项目里都可能会具备哪些功能

    - typescript：如果遇到ts文件我们需要使用tsc将typescript代码转换为js代码
    - React/Vue：安装react-compiler/vue-complier，将我们写的jsx文件或者.vue文件转换为render函数（因为浏览器只认识html、css、js）
    - less/sass/postcss/component-style；需要安装less-loader，sass-loader等一系列编译工具
    - 语法降级：babel --->  将es的新语法转化为旧版浏览器可以接受的语法
    - 体积优化：uglifyjs ---> 将我们的代码进行压缩变成体积更小性能更高的文件

  - 有一个东西能够把上面的东西全部集成到一起，我们只需要关心我们自己的代码就好了。这个东西就叫做**构建工具**

  - **打包：将我们写的浏览器不认识的代码，交给构建工具进行编译处理的过程就叫做打包，打包完成以后会给我们一个浏览器可以认识的文件**

  - 构建工具承担了什么：

    - 模块化开发支持：支持直接从node_modules里引入代码 + 多种模块化支持
    - 处理代码兼容性：比如babel语法降级，less，ts语法转换（**不是构建工具做的，而是构建工具将这些语法对应的处理工具集成进来自动化处理**）
    - 提高项目性能：压缩文件，**代码分割**
    - 优化开发体验：
      - 构建工具会帮你自动监听文件的变化，当文件变化以后自动帮你调用对应的集成工具进行重新打包，然后再在浏览器重新运行（整个过程叫做热更新）
      - 开发服务器：跨域的问题，用react-cli、create-react-element、vue-cli解决跨域的问题

  - 总结：构建工具让我们可以不用每次都关心我们的代码在浏览器如何运行，我们只需要首次给构建工具提供一个配置文件（这个配置文件也不是必须的，如果你不给它，它也会默认的去帮你处理），有        了这个继承的配置文件以后，我们就可以在下次需要更新的时候调用一次对应的命令就好了，如果我们再结合热更新，我们就更加不需要管任何东西了，这就是构建工具去做的东西，**他让我们不用关心生成的代码也不用关心代码如何再浏览器运行，只需要关系我们的开发指明写的好就行了**

    

- vite相对于webpack的优势

  - 当我们开始构建越来越大型的应用时，需要处理的JavaScript也呈指数级增长，使**启动开发服务器时间变长**；使用HMR（热更新）时，文件修改后的效果也需要**几秒后才能在浏览器中反应出来**

  - webpack的编译原理：AST抽象语法分析的工具，分析出你写的这个js文件有哪些导入和导出操作

  - 因为webpack支持多种模块化，它一开始必须要统一模块化代码，这就意味着它需要把所有的依赖全部读一遍，然后再启动服务器

  - 而vite是一开始就启动服务器，然后再运行到需要的地方再去加载

  - vite是基于es modules的，与webpack侧重点不一样

    - webpack更多的关注兼容性
    - 而vite关注浏览器端的开发体验

    

- vite脚手架（create-vite）和vite的区别

  - vite中文网：https://vitejs.cn/guide/
  - 比如我们做了`yarn create vite`时，会发生：
    - 帮我们全局安装一个东西：`create-vite`（vite的脚手架）
    - 直接运行这个`create-vite`的bin目录下的一个执行配置
  - `create-vite`和`vite`的关系：
    - create-vite（vite脚手架）内置了vite
    - 就像使用vue-cli会内置webpack
  - 比如：
    - 预设：买房子 --> 毛胚房 （我们的工程） -->  买家居，装修，埋线 -->  精装修的房（搭建好了）
    - 我们自己搭建一个项目：下载vite，vue，post-css，less，babel
    - vue-cli（开发商）给我们提供已经精装修的模板：帮你把vue下好了，同时还帮你把配置跳到了最佳实践
    - create-vite（开发商）给你一套精装修模板（给你一套预设）：下载vite，vue，post-css，less，babel好了，并且给你做好了最佳实践的配置





- vite启动项目初体验

  - 开箱即用（out of box）：你不需要做任何额外的配置就可以使用vite来帮你处理构建工作

  - 在默认情况下，我们的esmodule（import）去导入资源的时候，要么是绝对路径，要么是相对路径

    - 既然我们现在的最佳实践就是node_modules，那么为什么es官方再外面导入非绝对路径和非相对路径的资源的时候不默认帮我们搜寻node_modules呢？
      - 因为直接导入一个包，会牵连出那个包所另外依赖的很多很多个包

  - 步骤：

    - 先`npm init -y`

    - 然后再`npm i vite -D`

    - 然后再package.json文件中的scripts结点下配置：

      ```js
      "scripts": {
          "dev": "vite"
      }
      ```

    - 然后`npm run dev`启动项目

      

- vite的依赖预购建

  - 需要依赖预购建的原因

    - **CommonJS 和 UMD 兼容性:** 开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。
    - **性能：** Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

  - 在处理的过程中如果看到了有非绝对路径或者非相对路径的引用，他会尝试开启路径补全

    找寻依赖的过程是自当前目录依次向上查找的过程，知道搜寻到根目录或者搜寻到对应依赖为止

  - **依赖预购建**：首先vite会找到对应的依赖，然后调用esbuild（对js语法进行处理的一个库），将其他规范的代码转换成esmodule规范，然后放到档期那目录下的node_modules/.vite/deps，同时对esmodule规范的各个模块进行统一集成

    - 它解决了3个问题：
      - 不同的第三方包会有不同的导出格式（这是vite无法约束人家的事）
      - 对路径的处理上可以直接使用.vite/deps，方便路径重写
      - 网络多包传输的性能问题（在处理一个依赖时发现这个依赖需要更多依赖）（也是原生esmodule规范不敢支持node_modules的原因之一），有了依赖预购建之后无论他有多少的额外export和import，vite都会尽可能的将他们进行集成最后只生成一个或者几个模块

  - 注意：vite.config.js相当于webpack.config.js

    

- vite配置文件语法提示以及开发环境和生产环境区分

  - vite配置文件的语法提示

    - 如果你使用的是webstorm，那你可以得到很好的语法补全
    - 如果你使用的是vscode或者其他的编译器，则需要做一些特殊处理

    vscode的配置：

    - 在vite.config.js文件下配置：

      ```js
      import { defineConfig } from 'vite'
      
      export default defineConfig({
          optimizeDeps: {
              exclude: [],                //将指定数组中的依赖不进行依赖预购建
          },
      })
      ```

      或者可以写成：

      ```js
      /** @type import("vite").UserConfig */
      const viteConfig = {
          optimizeDeps: {
              exclude: [],                //将指定数组中的依赖不进行依赖预购建
          },
      }
      
      export default viteConfig;
      ```

  - 关于环境的处理**（注：以下例子中的配置optimizeDeps是例子，非必须）**

    - 过去外面使用webpack的时候，外面要区分配置文件的一个环境

      - webpack.dev.config
      - webpack.prod.config
      - webpack.base.config
      - webpackmerge

    - 处理步骤：

      - 创建vite.dev.config.js，配置：

        ```js
        import { defineConfig } from 'vite'
        
        export default defineConfig({})
        ```

      - 创建vite.prod.config.js

        ```js
        import { defineConfig } from 'vite'
        
        export default defineConfig({})
        ```

      - 创建vite.base.config.js，配置：

        ```js
        import { defineConfig } from 'vite'
        
        export default defineConfig({
            optimizeDeps: {
                exclude: [],                //将指定数组中的依赖不进行依赖预购建
            },
        })
        ```

      - 在vite.config.js中配置：

        ```js
        import { defineConfig } from "vite"
        import viteBaseConfig from "./vite.base.config"
        import viteDevConfig from "./vite.dev.config"
        import viteProdConfig from "./vite.prod.config"
        
        //策略模式
        const envResolver = {
            "build": () => Object.assign({...viteBaseConfig,...viteProdConfig}),
            "serve": () => Object.assign({...viteBaseconfig,...viteDevConfig})
        }
        
        export default defineConfig(config:({ command:"build"|"serve"}) => {
            //是build还是serve主要取决于外面敲的命令是开启开发环境还是生产环境
            return envResolver[command]();
        })
        
        /*
        export default defineConfig( config: ({ command:"build"|"serve"}) => {
            if(command === "build") {
                //代表生产环境的配置
            } else {
                //代表开发环境的配置
            }
        })
        */
        
        
        ```

  - 补充：为什么vite.config.js可以数学成esmodule的形式，这是因为vite它在读取这个vite.config.js的时候会率先用node去解析文件语法，如果发现你是esmodule规范会直接将你的esmodule规范进行替换变成commonjs规范

    

- vite环境变量的配置

  - 环境变量：会根据当前的代码环境产生值而变化的变量就叫做环境变量

    - Vite 在一个特殊的 **`import.meta.env`** 对象上暴露环境变量。这里有一些在所有情况下都可以使用的内建变量：

      - **`import.meta.env.MODE`**: {string} 应用运行的模式。
      - **`import.meta.env.BASE_URL`**: {string} 部署应用时的基本 URL。他由`base` 配置项决定。
      - **`import.meta.env.PROD`**: {boolean} 应用是否运行在生产环境。
      - **`import.meta.env.DEV`**: {boolean} 应用是否运行在开发环境 (永远与 `import.meta.env.PROD`相反)。

      

  - 代码环境：

    - 开发环境
    - 测试环境
    - 预发布环境
    - 灰度环境
    - 生产环境

  - 比如小程序的sdk中有一个`APP_KEY`（外面请求第三方sdk接口的时候需要带上的一个身份信息），它在测试环境和生产还有开发环境是不一样的key

    - 开发环境：110
    - 生产环境：111
    - 测试环境：112

  - 又比如在与后端对接的时候，前端在开发环境中请求的后端API地址和生产环境的后端API地址不是同一个

    - 开发和测试环境下，根URL：http://test.api/
    - 生产环境下，根URL：http://api/

    

  - 在vite中的环境变量处理：

    - dotenv这个第三方库

    - dotenv会自动读取.env文件，并解析这个文件中的对应环境变量,并将其注入到process对象下（但是vite考虑到和其他配置的一些冲突问题，它不会直接注入到process对象下）（因为涉及到vite.config.js中的一些配置）

      - root
      - envDir：用来配置当前环境变量的文件地址

    - vite给我们提供了一些补偿措施：我们可以**用vite的loadEnv来手动确认env文件**

      process.cwd方法：返回当前node进程的工作目录

      ```js
      	//这里的第二个参数是根据我们的命令来的。如果是用了npm run dev，那么mode就是development；如果用来npm run build，那么mode就是production
      export default defineConfig(config:({ command:"build"|"serve",mode:string}) => {
          //是build还是serve主要取决于外面敲的命令是开启开发环境还是生产环境
          console.log("porcess",process.cwd())
          //档期env文件所在的目录
          //第二个参数不是必须要使用process.cwd()，也可以直接写一个目录
          const env = loadEnv(mode,process.cwd(),prefixes:".env")
          console.log("env///",env)
          return envResolver[command]();
      })
      ```

      - 当我们调用loadEnv的时候，它会做以下几件事：

        - 直接找到.env文件不解释，并解析其中的环境变量，并放进一个对象里

        - 会将传进来的mode这个变量的值进行拼接(如拼接为`.env.development`，并根据我提供的目录（第二个参数）去取对应的配置文件并进行解析，并放进一个对象里

        - 我们可以理解为：

          ```js
          const baseEnvConfig = 读取.env的配置
          const modeEnvConfig = 读取env相关配置
          const lastEnvConfig = {...baseEnvConfig,...modeEnvConfig}
          ```

      - 如果是客户端，vite会把对应的环境变量注入到`import.meta.env`中去

        - vite做了一个拦截，它为了防止我们将隐私性的变量直接送进`import.meta.env`中，所以它做了一层拦截，如果你的环境变量不是以`VITE`开头的，它就不会帮你注入到客户端中去，如果我们想要更改这个前缀，可以去使用envPrefix配置

        - 在vite.base.config.js中：

          ```js
          import { defineConfig } from 'vite'
          
          export default defineConfig({
              envPrefix: "ENV_"  //配置vite注入客户端环境变量校验的env前缀
          })
          ```

          

  - `.env`文件

    - Vite 使用 dotenv 从你的 环境目目录中的下列文件加载额外的环境变量：

      ```bash
      .env                # 所有情况下都会加载
      .env.local          # 所有情况下都会加载，但会被 git 忽略
      .env.[mode]         # 只在指定模式下加载
      .env.[mode].local   # 只在指定模式下加载，但会被 git 忽略
      ```

    - 加载的环境变量也会通过 `import.meta.env` 暴露给客户端源码。

    - 为了防止意外地将一些环境变量泄漏到客户端，只有以 `VITE_` 为前缀的变量才会暴露给经过 vite 处理的代码。例如下面这个文件中：

      ```js
      DB_PASSWORD=foobar
      VITE_SOME_KEY=123
      ```

      只有 `VITE_SOME_KEY` 会被暴露为 `import.meta.env.VITE_SOME_KEY` 提供给客户端源码，而 `DB_PASSWORD` 则不会。

    - 注：

      - `.env`：所有环境都需要用到的环境变量
      - `.env.development`：开发环境下需要用到的环境变量（默认情况下vite将我们的开发环境取名为development）
      - `.env.production`：生产环境下需要用到的环境变量（默认情况下vite将我们的生产环境取名为production）

  - 步骤：

    - 创建不同环境下的.env配置文件，如：

      - .env
      - .env.development
      - .env.production
      - .env.test

    - 配置上面说的loadEnv

    - 在package.json中配置：

      ```js
      "script": {
          "dev":"vite",
          "build":"vite build",
          "test":"vite --mode test"
      }
      ```

    - 之后通过

      - `npm run dev`在生产环境中运行
      - `npm run test`在测试环境中运行
      - `npm run build`在生产环境中运行

    - d

  - 

- 

- 



