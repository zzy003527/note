## 前端工程化与webpack

- 实际前端开发

  - 模块化（js的模块化、css的模块化、资源的模块化）
  - 组件化（复用现有的UI解构、样式、行为）
  - 规范化（目录结构的划分、编码规范化、接口规范化、文档规范化、Git分支管理）
  - 自动化（自动化构建、自动部署、自动化测试）

- 什么是前端工程化

  - 前端工程化指的是：在**企业级的前端开发项目**中，把前端开发所需的**工具、技术、流程、经验**等进行规范化、标准化
  - 企业中的Vue项目和React项目，都是基于**工程化的方式**进行开发的
  - 好处：前端开发**自成体系**，有一套**标准的开发方案和流程**

- 前端工程化的解决方案

  - 早期：
    - grunt(https:///www.gruntjs.net/)
    - gulp(https://zh.gulpjs.com.cn/)
  - 目前主流：
    - webpack(https://www/webpackjs.com/)
    - parcel(https://zh.parceljs.org/)

  

- webpack的基本使用

  - 什么是webpack

    - 概念：webpack是前端工程化的具体解决方案
    - 主要功能：它提供了友好的**前端模块化开发**支持，以及**代码压缩混淆、处理浏览器端JavaScript的兼容性、性能优化**等强大功能
    - 目前Vue，React等前端项目，基本上都是基于webpack进行工程化开发的

  - 创建项目

    - 新建项目空白目录，并运行npm init -y命令，初始化包管理配置文件package.json
    - 新建src源代码目录
    - 新建src -> index.html首页和src ->index.js脚本文件
    - 初始化首页基本的结构
    - 运行npm install jquery -S命令，安装jQuery
    - 通过ES6模块化的方式导入jQuery，实现效果（import $ from 'jquery'）

  - 在项目中安装webpack

    - 在终端运行如下的命令，安装webpack相关的两个包：

      ```js
      npm install webpack@5.42.1 webpack-cil@4.7.2 -D
      
      -S 是 --save 的简写（开发和上线后都要用）
      -D 是 --save--dev 的简写（只在开发用）
      ```

  - 在项目中配置webpack

    - 在**项目根目录**中，创建名为**webpack.config.js**的webpack配置文件，并初始化如下的基本配置：

      ```js
      module.exports = {
          mode:'development'       //mode 用来指定构建模式，可选值有 development 和production
      }
      ```

    - 在package.json的script节点下，新增**dev脚本**如下：

      ```js
      "script": {
          "dev":"webpack"    //script 节点下的脚本，可以通过npm run执行，例如 npm run dev
      }
      ```

    - **在终端中运行npm run dev命令**，启动webpack进行项目的打包构建（之后导入自动生成的dist下的main.js）

  - mode的可选值

    ​       **mode节点**的可选值有两个，分别是：

    - development
      - **开发环境**
      - **不会**对打包生成的文件进行**代码压缩和性能优化**
      - 打包**速度快**，适合在**开发阶段**使用
    - production
      - **生产环境**
      - **会**对打包生成的文件进行**代码压缩和性能优化**
      - 打包**速度很慢**，仅适合在项目**发布阶段**使用

  - webpack中的默认约定

    ​     在webpack 4.x和5.x的版本中，有如下的默认约定：

    - 默认的打包入口文件为**src -> index.js**
    - 默认的输出文件路径为**dist->main.js**
    - 注意：可以在**webpack.config.js**中修改打包的默认约定

  - 自定义打包的入口与出口

    - 在**<u>webpack.config.js</u>**配置文件中，通过**entry**节点**指定打包的入口**。通过**output**节点**指定打包的出口**

    - ```js
      const path = require('path');           //导入node.js中专门操作路径的模块
       
      module.exprots = {
          entry:path.join(__dirname,'./src/index.js'),   //打包入口文件的路径
          output: {
              path:path.join(__dirname,'./dist'), //输出文件的存放路径
              filename:'bundle.js'    // 输出文件的名称
          }
      }
      ```

    - 注意：每次修改后都需要重新运行npm run dev才有效果（下面解决此问题）

  

- webpack中的插件

  - 通过安装和配置第三方的插件，可以**拓展webpack的能力**，从而让webpack用起来更方便。最常用的webpack插件有如下两个：

    - **webpack-dev-server**
      - 类似于node.js中用到的nodemon
      - 每当修改了源代码，webpack会自动进行项目的打包和构建
    - **html-webpack-plugin**
      - webpack中的HTML插件（类似于一个模板引擎插件）
      - 可以通过此插件自定制index.html页面的内容

  - 安装webpack-dev-server

    - 运行如下命令：

      ```js
      npm install webpack-dev-server@3.11.2 -D
      ```

    - 然后修改**package.json -> seripts**中的**dev**命令，如下：

      ```js
      "scripts": {
          "dev":"webpack serve",         //script节点下的脚本，可以通过npm run执行
      }
      ```

    - 再次运行**npm run dev**命令，重新进行项目的打包

    - 在浏览器中访问http://localhost:8080地址，查看自动打包效果

    - 注意：webpack-dev-server会启动一个**实时打包的http服务器**

    - 注意：要用http协议打开页面

    - **注意：修改后的代码是保存在<u>根目录/</u> 里面的index.js的，所以html页面引用的是<u>/index.js</u>而不是src中的index.js**

  - 安装html-webpack-plugin

    - 运行如下命令：

      ```js
      npm install html-webpack-plugin@5.3.2 -D
      ```

    - 配置html-webpack-plugin**（在webpack.config.js中）**

      ```js
      //1.导入HTML插件，得到一个构造函数
      const HtmlPlugin = require('html-webpack-plugin')
      
      //2.创建HTML插件的实例对象
      const htmlPlugin = new HtmlPlugin({
          template:'./src/index.html',       //指定源文件的存放路径
          filename:'./index.html',       // 指定生成的文件的存放路径
      })
      
      module.exports = {
          mode:"development",
          plugins:[htmlPlugin],    //3.通过pulgins节点，使htmlPlugin插件生效
      }
      ```

    - 注意：

      - 通过HTML插件复制到项目根目录中的index.html页面，**也被放到了内存中**
      - HTML插件在生成的index.html**页面，自动注入**了打包的bundle.js文件

  - devServer节点

    - **在webpack.config.js**配置文件中，可以通过**devServer**节点对webpack-dev-server插件进行更多的配置

    - ```js
      devServer: {
          open:true,       //初次打包完成后，自动打开浏览器
          host:'127.0.0.1',      //实施打包所使用的主机地址
          port:80,                //实时打包所使用的端口号
      }
      ```

    - 注意：凡是修改了webpack.config.js配置文件，或修改了package.json配置文件，**必须重启实时打包的服务器**，否则最新的配置无法生效

      

- webpack中的loader

  - loader概述

    - 在实际开发过程中，webpack默认只能打包处理以.js后缀名结尾的模块。其他**非.js后缀名结尾的模块**，webpack默认处理不了，**需要调用loader加载器才可以正常打包**，否则会报错

    - loader加载器的作用：**协助webpack打包处理特定的文件模块。**比如：
      - css-loader可以打包处理.css相关的文件
      - less-loader可以打包处理.less相关的文件
      - babel-loader可以打包处理webpack无法处理的高级JS语法

    

  - 打包处理css文件

    - 运行**npm i style-loader@3.0.0 css-loader@5.2.6 -D**命令，安装处理css文件的loader

    - 在**webpack.config.js**的**module->rules**数组中，添加loader规则，如下：

      ```js
      module:{    //所有第三方文件模块的匹配机制
          rules: [   //文件后缀名的匹配机制
              {test:/\.css$/,use:['style-loader','css-loader']}
          ]
      }
      ```

    - 其中，**test**表示匹配的**文件类型**，**use**表示对应**要调用的loader**

    - 注意：

      - use数组中指定的loader**顺序是固定的**
      - 多个loader的调用顺序是：**从后往前调用**

    - webpack默认只能打包.js结尾的文件，处理不了其他后缀的文件。因此当webpack发现某个文件处理不了的时候，会查找**webpack.config.js**这个配置文件，看**module.rules数组**中，是否配置了对应的loader加载器；webpack把index.css这个文件，**先**转交给**最后一个loader**进行处理（先转交给css-loader），当css-loader处理完毕之后，会**把处理的结果转交给下一个loader**（转交给style-loader），当style-loader处理完毕之后，发现**没有下一个loader了**，于是把处理的结果转交**给webpack**，webpack把style-loader处理的结果，**合并到/dist/index.js中**，最终生成打包好的文件

    

  - 打包处理less文件

    - 运行**npm i less-loader@10.0.1 less@4.1.1 -D** 命令

    - 在webpack.config.js的module->rules数组中，添加loader规则如下：

      ```js
      module: {    //所有第三方文件模块的匹配机制
          rules:[      //文件后缀名的匹配机制
              {test:/\.less$/,use: ['style-loader','css-loader','less-loader']}
          ]
      }
      ```

    

  - 打包处理样式表中与**url路径相关**的文件

    - 运行**npm i url-loader@4.1.1 file-loader@6.2.0 -D**命令

    - 在**webpack.config.js**的**module->rules数组**中，添加loader规则如下：

      ```js
      module: {            //所有第三方文件模块的匹配机制
          rules:[              //文件后缀名的匹配机制
              //在配置url-loader的时候，多个参数之间，用&符号进行分隔
              {test:/\.jpg|png|gif$/,use: 'url-loader?limit=22229&outputPath=images'},
          ]
      }
      ```

    - 其中**？**之后的是**loader的参数项**：

      - limit用来指定**图片的大小**，单位是字节（byte）

      - 只有**≤**limit大小的图片，才会被转为base64格式的图片

        

  - 打包处理js文件中的高级语法

    - webpack只能打包处理一部分高级的JavaScript语法。对于那些webpack无法处理的高级js语法，需要借助于babel-loader进行打包处理。

    - 安装babel-loader相关的包

      - 运行如下命令安装对应的依赖包：

        ```js
        npm i babel-loader@8.2.2 @babel/core@7.14.6 @babel/plugin-proposal-decorators@7.14.5 -D
        ```

      - 在**webpack.config.js**的**module->rules数组**中，添加loader规则如下：

        ```js
        //注意：必须使用exclude指定排除项：因为node_modules 目录下的第三方包不需要被打包
        {test:/\.js$/,use:'babel-loader',exclude:'/node_modules/'}
        ```

    - 配置babel-loader

      - **<u>在项目根目录下</u>**，创建名为**bable.config.js**的配置文件，定义**Babel的配置项**如下：

        ```js
        modules.exports = {
            //声明babel可用的插件
            plugins:[['@babel/plugin-proposal-decorators',{legacy:true}]]
        }
        ```

      - 在webpack调用babel-loader的时候，会先加载plugins插件来使用

        

- 打包发布**<u>（npm run build）</u>**

  - 配置webpack的打包发布

    - 在**package.json文件的scripts节点**下，新增build命令如下：

      ```js
      "scripts": {
          "dev": "webpack serve",     //开发环境中，运行dev命令
          "build": "webpack --mode production"     //项目发布时，运行build命令
      }
      ```

    - **--mode**是一个参数项，用来指定webpack的**运行模式**。production代表生产环境，会对打包生成的文件进行**代码压缩和性能优化**

    - 注意：通过 --mode 指定的参数项，会**覆盖**webpack.config.js中的mode选项

    

  - 把JavaScript文件同一生成到js目录中

    - 在**webpack.config.js**配置文件的**output节点**中，进行如下的配置：

      ```js
      output: {
          path:path.join(__dirname,'dist'),
              //明确告诉webpack把生成的index.js文件存放到dist目录下的js子目录中
          filename:'js/index.js',
      }
      ```

  - 把图片文件同一生成到image目录中

    - 修改**webpack.config.js中的url-loader**配置项，新增**outputPath**选项即可指定图片文件的输出路径：

      ```js
      {
          test:/\.jpg|png|gifs/,
          use: {
              loader:'url-loader',
              options: {
                  limit:22228,
                  //明确指定把打包生成的图片文件，存储到dist目录下的image文件夹中
                  outputPath:'images',
              }
          }
      }
      ```

      

  - 自动清理dist目录下的旧文件

    - 为了在每次打包发布的时候**自动清理掉dist目录中的旧文件**，可用安装并配置**clean-webpack-plugin**插件：

      ```js
      //1.安装清理dist目录的webpack插件
      npm install clean-webpack-plugin@3.0.0 -D
      
      //2.按需导入插件、得到插件的构造函数之后，创建插件的实例对象（在webpack.config.js中）
      const {CleanWebpackPlugin} = require('clean-webpack-plugin')
      const cleanPlugin = new CleanWebpackPlugin()
      
      //3.把创建的cleanPlugin插件实例对象，挂载到plugins节点中
      plugins:[htmlPlugin,cleanPlugin],    //挂载插件
      ```

      

- Source Map

  - 什么是Source Map

    - **Source Map就是一个信息文件，里面储存着位置信息**。也就是说，Source Map文件中储存着压缩混淆后的代码，所对应的**转换前的位置**
    - 有了它，在出错的时候，除错工具将**直接显示原始代码，而不是转换后的代码**，能极大的方便后期的调试

  - 解决默认Source Map的问题

    - 开发环境下，推荐在**webpack.config.js**中添加如下的配置，即可保证运行时**报错的行数与源代码的行数保持一致**

    - ```js
      module.exports = {
          mode:'development',
          //eval-source-map 仅限在开发模式下使用，不建议在生产模式下使用
          //此选项生成的Source Map 能够保证 运行时报错的行数 与 源代码的行数 保持一致
          devtool:'eval-source-map',
      }
      ```

  - webpack生产环境下的Source Map

    - 在**生产环境**下，如果**省略了devtool选项**，则最终生成的文件中**不包含Source Map**。这能够**防止原始代码**通过Source Map的形式**暴露**给别人

  - 只定位行数不暴露源码

    - 在生产环境下，如果**只想定位报错的具体行数**，且**不想暴露源码**。此时可用将**devtool**的值设置为**nosources-source-map。**
    - 采用此选项后，应该将服务器配置为：**不允许普通用户访问source map文件**
    - 在实际发布时，建议直接关闭Source Map或将devtool的值设置为nosources-source-map

  - Source Map的最佳实践

    - 开发环境下：
      - 建议把devtool的值设置为**eval-source-map**
      - 好处：可用精确定位到具体的错误行
    - 生产环境下：
      - 建议**关闭Source Map**或将devtool的值设置为**nosources-source-map**
      - 好处：防止源码泄露，提高网站的安全性

    

- webpack中@的原理和好处

  - 建议使用@表示src源代码目录，从外往里查找；不要使用../从里往外查找

  - 在webpack.config.js中，添加

    ```js
    resolve: {
        alias:{
            //告诉webpack，程序员的源代码中，@符号表示src这一层目录
            '@': path.join(__dirname,'/src/')
        }
    }
    ```

    