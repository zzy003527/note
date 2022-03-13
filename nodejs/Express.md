## Express

- Express简介

  - 官方给出的概念：Express是**基于Node.js平台**，快速、开放、极简的**Web开发框架**

  - 通俗的理解：Express的作用和Node.js内置的http模块类似，**是专门用来创建Web服务器的**

  - **Express的本质**：就是一个npm上的第三方包，提供了快速创建Web服务器的便捷方法

  - Express的中文官网：http://www.expressjs.com.cn/

  - 进一步理解Express

    - 不使用Express能否创建Web服务器？

      能，使用Node.js提供的原生http模块即可

    - 有了http内置模块，为什么还要用Express？

      http内置模块用起来很复杂，开发效率低；EXpress是基于内置的http模块进一步封装出来的，能够极大的提高开发效率

  - Express能做什么

    对于前端程序员来说，最常见的两种服务器，分别是：

    - **Web网站服务器**：专门对外提供Web网页资源的服务器
    - **API接口服务器**：专门对外提供API接口的服务器
    - 使用Express，我们可以很方便、快速的创建Web网站的服务器或API接口的服务器

- Express的基本使用

  - 安装

    - 在项目所处的目录中，运行如下的终端命令，可将express安装到项目中使用

    - ```js
      npm i express@4.17.1
      ```

  - 创建基本的Web服务器-                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 

    - ```js
      //1.导入express
      const express = require('express')
      //2.创建web服务器
      const app = express()
      
      //3.调用app.listen（端口号，启动成功后的回调函数），启动服务器
      app.listen(80,() => {
          console.log('express server running at http://127.0.0.1')
      })
      ```

  - 监听GET请求

    - 通过app.get()方法，可以监听客户端的GET请求，具体的语法格式如下：

    - ```js
      //参数1：客户端请求的URL地址
      //参数2：请求对应的处理函数
      //      req:请求对象（包含了与请求相关的属性与方法）
      //      res:响应对象（包含了与响应相关的属性与方法）
      app.get('请求URL'，function(req,res) {/*处理函数*/})
      ```

      

  - 监听POST请求

    - 通过app.post()方法，可以监听客户端的POST请求，具体的语法格式如下：

    - ```js
      //参数1：客户端请求的URL地址
      //参数2：请求对应的处理函数
      //      req:请求对象（包含了与请求相关的属性与方法）
      //      res:响应对象（包含了与响应相关的属性与方法）
      app.post('请求URL',function(req,res) {/*处理函数*/})
      ```

      

  - 把内容**响应**给客户端

    - 通过res.send()方法，可以把处理好的内容，发送给客户端：

    - ```js
      app.get('/user',(req,res) => {
          //向客户端发送JSON对象
          res.send({name:'zs',age:20,gender:' 男'})
      })
       
      app.post('/user',(req,res) => {
          //向客户端发送文本内容
          res.send('请求成功')
      })
      ```

      

  - 获取URL中携带的查询参数

    - 通过**req.query**对象，可以访问到客户端通过**查询字符串**的形式，发送到服务器的参数：

    - 默认情况下，req.query对象是一个空对象

    - ```js
      app.get('/',(req,res) => {
          //req.query是一个空对象
          //客户端使用？name=zs&age=20 这种查询字符串方式，发送到服务器的参数可以通过req.query对象访问到，例如：
          //req.query.name   req.query.age
          console.log(req.query)
      })
      ```

  - 获取URL中的**动态参数**

    - 通过**req.params**对象，可以访问到URL中，通过**：**匹配到的**动态参数**：

    - ```js
      //URL地址中，可以通过：参数名 的形式，匹配动态参数值
      app.get('/user/:id',(req,res) => {
           //res.params 默认是一个空对象
          //里面存放着通过 ： 动态匹配到的参数值
          console.log(req.params)
      })
      ```

      

- 托管静态资源

  - express.static()

    - express提供了一个非常好用的函数，叫做**express.static()**，通过它，我们可以**非常方便地创建一个静态资源服务器**，例如，通过如下代码就可以将public目录下的图片、CSS文件、JavaScript文件对外开发访问了：

    - ```js
      app.use(express.static('public'))
      ```

    - 现在就可以访问public目录中的所有文件了

    - 注意：Express在**指定的**静态目录中查找文件，并对外提供资源的访问路径。因此，**存放静态文件的目录名不会出现在URL中**

  - 托管多个静态资源目录

    - 如果要托管多个静态资源目录，请多次调用express.static()函数：

    - ```js
      app.use(express.static('public'))
      app.use(express.static('files'))
      ```

    - 访问静态资源文件是，express.static()函数会**根据目录的添加顺序**查找所需的文件

  - 挂载路径前缀

    - 如果希望在托管的**静态资源访问路径**之前，**挂载路径前缀**，则可以使用如下的方式：

    - ```js
      app.use('/public',express.static('public'))
      ```

    - 现在，就可以通过带有/public前缀地址来访问public目录中的文件了

      

- nodemon

  - 为什么要使用nodemon

    - 在编写调试Node.js项目的时候，如果修改了项目的代码，则需要频繁的手动close掉，然后再重新启动，非常繁琐
    - 现在，可以使用nodemon(https://www.npmjs.com/package/nodemon)这个工具，它能够**监听项目文件的变动**，当代码被修改后，nodemon会**自动帮我们重启项目**，极大方便了开发和调试

  - 安装nodemon

    - 在终端中，运行如下命令，即可将nodemon安装为全局可用的工具：

    - ```js
      npm install -g nodemon
      ```

  - 使用nodemon

    - 当基于Node.js编写了一个网站应用的时候，传统的方式，是运行**node app.js**命令,来启动项目。这样做的坏处是:代码被修改之后,需要手动重启项目。

    - 现在，我们可以将node命令替换为nodemon命令,使用nodemon app.js来启动项目。这样做的好处是:代码被修改之后，会被nodemon监听到，从而实现自动重启项目的效果。

    - ```cmd
      node app.js
      # 将上面的终端命令，替换为下面的终端命令，即可事先自动重启项目的效果
      nodemon app.js
      ```

      

- Express路由

  - 在Express中，路由指的是**客户端的请求**与**服务器处理函数**之间的**映射关系**

  - Express中的路由分3部分组成，分别是**请求的类型、请求的URL地址、处理函数**，格式如下：

    ```js
    app.METhod(PATH,HANDLER)
    ```

  - Express中的路由的例子：

    ```js
    //匹配GET请求，且请求URL为 /
    app.get('/',function(req,res) => {
            res.send('Hello World!')
     })
     
     //匹配POST请求，且请求URL为/
     app.post('/',function(req,res) {
         res.send('Got a POST request')
     })
    ```

  - 路由的匹配过程

    - 每当一个请求到达服务器之后，**需要先经过路由的匹配**，只有匹配成功之后，才会调用对应的处理函数
    - 在匹配时，会按照路由的顺序进行匹配，如**请求类型**和**请求URL**同时匹配成功，则Express会将这次请求，转交给对应的function函数进行处理
    - ![QQ图片20220120233020](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220120233020.png)
    - 路由匹配的注意点：
      - 按照定义的**先后顺序**进行匹配
      - **请求类型**和**请求的URL**同时匹配成功，才会调用对应的处理函数

  - 路由的使用

    - 最简单的用法

      - 在Express中使用路由最简单的方式，就是把路由挂载到app上，实例代码如下：

        ```js
        const express = require('express')
        const app = express()
        //挂载路由
        app.get('/',(req,res) => {res.send('Hello World.')})
        app.post('/',(req,res) => {res.send('Post Request')})
        
        //启动web服务器
        app.listen(80,() => {console.log('server running at http://127.0.0.1')})
        ```

    - **模块化**路由

      - 为了**方便对路由进行模块化的管理**，Express**不建议**将路由直接挂载到app上，而是**推荐将路由抽离为单独的模块**
      - 将路由抽离为单独模块的步骤如下：
        - 创建路由模块对应的.js文件
        - 调用**express.Router()**函数创建路由对象
        - 向路由对象上挂载具体的路由
        - 使用**module.exports**向外共享路由
        - 使用**app.use()**函数注册路由模块

    - 创建路由模块

      - ```js
        var express = require('express')        //1.导入express
        var router = express.Router()           //2.创建路由对象
        
        router.get('/user/list',function(req,res) {     //3.挂载获取用户列表的路由
            res.send('Get user list.')                    
        })
        router.post('/user/add',function(req,res) {     //4.挂载添加用户的路由
            res.send('Add new user')
        })
        
        module.exports = router                        //5.向外导出路由对象
        ```

    - 注册路由模块

      - ```js
        //1.导入路由模块
        const userRouter = require('./router/user.js')
        
        //2.使用app.use()方法注册路由模块
         app.use(userRouter)
        
        //app.use()的作用就是来注册全局中间件的
        ```

    - 为路由模块**添加前缀**

      - 类似于托管静态资源时，为静态资源统一挂载访问前缀意义，路由模块添加前缀的方式也非常简单：

        ```js
        //1.导入路由模块
        const userRouter = require('./router/user.js')
        //2.使用app.use()注册路由模块，并添加统一的访问前缀 /api
        app.use('/api',userRouter)
        ```

        

- Express中间件

  - 中间件的概念

    - 什么是中间件

      中间件（Middleware），特指**业务流程**中的**中间处理环节**

    - Express中间件的**调用流程**

      - 当一个请求到达Express的服务器之后，可能连续调用多个中间件，从而对这次请求进行**预处理**
      - ![QQ图片20220121000201](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220121000201.png)

    - Express中间件的**格式**

      - Express的中间件，**本质**上就是一个**function处理函数**，Express中间件的格式如下：
      - ![QQ图片20220121000636](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220121000636.png)
      - 注意：中间件函数的形参列表中，**必须包含next参数**。而路由处理函数中只包含req和res

    - next函数的作用

      - **next函数**是实现**多个中间件连续调用**的关键，它标识把流转关系**转交**给下一个**中间件或路由**

  - Express中间件的初体验

    - 定义中间件函数

      - 可以通过如下方式，定义一个最简单的中间件函数’

      - ```js
        //常量mw所指向的就是一个中间件函数
        const mw = function(req,res,next)  {
            console.log('这是一个最简单的中间件函数 ')
            //注意：在当前中间件的业务处理完毕后，必须调用next（）函数
            //表示把流转关系转交个下一个中间件或路由
            next()
        }
        ```

    - **全局生效**的中间件

      - 客户端发起的**任何请求**，到达服务器后，**都会触发的中间件**，叫做全局生效的中间件

      - 通过调用**app.use（中间件函数）**，即可定义一个**全局生效**的中间件，实例代码如下：

      - ```js
        //常量mw所指向的就是一个中间件函数
        const mw = function(req,res,next)  {
        console.log('这是一个最简单的中间件函数 ')
            //注意：在当前中间件的业务处理完毕后，必须调用next（）函数
            //表示把流转关系转交个下一个中间件或路由
            next()
        }
        //全局生效的中间件
        app.use(mw)
        ```

    - 定义**全局中间件**的**简化形式**

      - ```js
        //全局生效的中间件
        app.use(function(req.res,next) {
            console.log('这是一个最简单的中间件函数')
            next()
        })
        ```

    - 中间件的**作用**

      - 多个中间件之间，**共享一份req和res**。基于这样的特性，我们可以在**上游**的中间件中，统一为req或res对象添加**自定义**的**属性或方法**，供**下游**的中间件或路由使用

    - 定义**多个**全局中间件

      - 可以使用app.use()**连续定义多个**全局中间件。客户端请求到达服务器之后，会按照中间件**定义的先后顺序**依次进行调用，实例代码如下：

      - ```js
        app.use(function(res,req,next) {
           console.log('调用了第一个全局中间件')
            next()
        })
        app.use(function(res,req,next) {
            console.log('调用了第二个全局中间件')
            next()
        })    
        app.get('/user',(req,res) => {                   //请求这个路由，会依次触发上述两个全局中间件
            res.send('Home page.')
        })
        ```

    - 局部生效的中间件

      - 不使用app.use()定义的中间件，叫做局部生效的中间件，实例代码如下：

      - ```js
        //定义中间件函数mw1
        const mw1 = function(req,res,next) {
            console.log('这是中间件函数')
            next()
        }
        //mw1这个中间件只在“当前路由中生效”，这种用法属于“局部生效的中间件”
        app.get('/',mw1,function(req,res) {
        res.send('Home page.')
        })
        //mw1这个中间件不会影响下面这个路由
        app.get('/user',function(req,res) {
            res.send('User page.')
        })
        ```

    - 定义**多个**局部中间件

      - 可以在路由中，通过如下两种**等价**的方式，**使用多个局部中间件**：

      - ```js
        //以下两种写法是完全等价的，可根据自己的喜好，选择任意一种方式进行使用
        app.get('/',mw1,mw2,(req,res) => {res.send('Home page.')})
        app.get('/',[mw1,mw2],(req,res) => {res.send)'Home page.'})
        ```

    - 了解中间件的**五个使用注意事项**

      - 一定要**在路由之前**注册中间件
      - 客户端发送过来的请求，**可以连续调用多个**中间件进行处理
      - 执行完中间件的业务代码之后，**不要忘记调用next（）函数**
      - 为了**防止代码逻辑混乱**，调用next（）函数后不要再写额外的代码
      - 连续调用多个中间件时，多个中间件之间，**共享**req和res对象

  - 中间件的分类

    - **注意：除了错误级别的中间件，其他的中间件，必须在路由之前进行配置**

    Express官方把**常见的中间件用法**，分成了**5大类**，分别是：

    - **应用级别**的中间件

      - 通过**app.use()或app.get()或app.post()，绑定到app实例上的中间件**，叫做应用级别的中间件，代码示例如下：

      - ```js
        //应用级别的中间件（全局中间件）
        app.use(req,res,next) => {
            next()
        }
        //应用级别的中间件（局部中间件）
        app.get('/',mw1,function(req,res) {
            rs.send('Home page.')
        })
        ```

    - **路由级别**的中间件

      - 绑定到**express.Router()**实例上的中间件，叫做路由级别的中间件。它的用法和应用级别中间件没有任何区别，只不过，**应用级别中间件是绑定到app实例上，路由级别中间件绑定到router实例上**，代码示例如下：

      - ```js
        const app = express()
        const router = express.Router()
        
        //路由级别的中间件
        router.use(function(req,res,next) {
            console.log('Time',Date,now())
            next()
        })
        app.use('/',router)
        ```

    - **错误级别**的中间件

      - 错误级别中间件的作用：专门用来捕获整个项目中发生的异常错误，从而防止项目异常崩溃的问题

      - 格式：错误级别中间件的function处理函数中，必须由四个形参，形参顺序从前到后，分别是（err,req,res,next)

      - ```js
        app.get('/',function(req,res) {
            throw new Error('服务器内部发生了情况')
            res.send('Home page.')
        })
        app.use(function(err,req,res,next) {
            console.log('发生了错误',err,message)
            res.send('Error' + err.message)
        })
        ```

    - **Express内置**的中间件

      - 自Express 4.16.0版本开始，Express便内置了**3个**常用的中间件，极大的提高了Express项目的开发效率和体验：

        - **Express.static**快速托管静态资源的内置中间件，例如HTML文件，图片，css（**无兼容性**）

        - **Express.json**解析Json格式的请求体数据（**有兼容性**，仅在4.16.0+版本中可用）

          - 在服务器，可用使用req.body这个属性，来接收客户端发送过来的请求体数据
          - 默认情况下，如果不配置解析表单数据的中间件，则req.body默认等于undefined

        - **express.unlencoded**解析URL-encoded格式的请求体数据（**有兼容性**，仅在4.16.0+版本可用）

        - ```js
          //配置解析application/json格式数据的内置中间件
          app.use(express.json())
          //配置解析application/x-www-form-urlencoded格式数据的内置中间件
          app.use(express.urlencoded({extended:false}))
          ```

    - **第三方**的中间件

      - 非Express官方内置的，而是由第三方开发出来的中间件，叫做第三方中间件。在项目中，大家可以**按需下载并配置**第三方中间件，从而提高项目的开发效率

      - 例如：在express@4.16.0之前的版本中，经常使用body-parser这个第三方中间件，来解析请求体数据。使用步骤

        ：

        - 运行npm install body-parser安装中间件
        - 使用require导入中间件
        - 调用app.use()注册并使用中间件
        - 注意：Express内置的express.urlencoded中间件，就是基于body-parser这个第三方中间件进一步封装出来的

  - 自定义中间件

    - **需求描述**与**实现步骤**

      - 自己**手动模拟**一个类似于express.urlencoded这样的中间件，来 **解析POST提交到服务器的表单数据**
      - 实现步骤：
        - 定义中间件
        - 监听req的data事件
        - 监听req的end事件
        - 使用querystring模块解析请求体数据
        - 将解析出来的数据对象挂载为req.body
        - 将自定义中间件封装为模块

    - 定义中间件

      - 使用app.use()来定义全局生效的中间件，代码如下：

      - ```js
        app.use(function(req,res,next) {
             //中间件的业务逻辑
        })
        ```

    - 监听req的data事件

      - 在中间件中，需要监听req对象的data事件，来获取客户端发送到服务器的数据

      - 如果数据量比较大，无法一次性发送完毕，则客户端会**把数据切割后，分批发送到服务器**。所有data事件可能会触发多次，每一次触发data事件时，**获取到数据只是完整数据的一部分**，需要手动对接到的数据进行拼接

      - ```js
        //定义变量，用来存储客户端发送过来的请求体数据
        let str = ''
        //监听req对象的data事件（客户端发送过来的新的请求体数据）
        req.on('data',(chunk) => {
            //拼接请求体数据，隐式转换为字符串
            str += chunk
        })
        ```

    - 监听req的end事件

      - 当请求体数据**接收完毕**之后，会自动触发req的end事件

      - 因此，我们可以在req的end事件中**，拿到并处理完整的请求体数据**。示例代码如下：

      - ```js
        //监听req对象的end事件（请求发送完毕后自动触发）
        req.on('end',() => {
            //打印完整的请求体数据
            console.log(str)
            //TODO:把字符串格式的请求体数据，解析成对象格式
        })
        ```

    - 使用querystring模块解析请求体数据

      - Node.js内置了一个**querystring**模块，**专门用来处理查询字符串**。通过这个模块提供的**parse()**函数，可以轻松把查询字符串，解析成对象的格式。示例代码如下：

      - ```js
        //导入处理querystring的Node.js内置模块
        const qs = require('querystring')
        //调用qs.parse()方法，把查询字符串解析为对象
        const body = qs.parse(str)
        ```

    - 将解析出来的数据对象挂载为**req.body**

      - **上游的中间件和下游的中间件及路由**之间，**共享同一份req和res**。因此，我们可以将解析出来的数据，挂载为req的自定义属性，命名为**req.body**，供下游使用，示例代码如下：

      - ```js
        req.on('end',() => {
            const body = qs.parse(str)            //调用qs.parse()方法，把查询字符串解析为对象
            req.body = body                   //将解析出来的请求体对象，挂载为req.body属性
            next()                          //最后一定要调用next（）执行后续的业务逻辑
        })
        ```

    - 将自定义中间件封装为模块

      - 为了优化代码的结构，我们可以把自定义的中间件函数，封装为独立的模块，示例代码如下：

      - ```js
        //custom-body-parser.js模块中的代码
        const qs = require('querystring')
        function bodyParser(req,res,next) {}
        module.exports = bodyParser    //向外导出解析请求体数据的中间件函数
        
        //导入自定义的中间件模块
        const myBodyParser = require('custom-body-parser')
        app.use(myBodyParser)
        ```

        

- 使用Express写接口

  - 创建基本的服务器

    - ```js
      //1.导入express
      const express = require('express')
      //2.创建web服务器
      const app = express()
      //3.调用app.listen（端口号，启动成功后的回调函数），启动服务器
      app.listen(80,() => {
          console.log('express server running at http://127.0.0.1')
      })
      ```

  - 创建API路由模块

    - ```js
      //apiRouter.js【路由模块】
      const express = require('express')
      const apiRouter = express.Router()
      
      //bind your router here...
      
      module.exports = apiRouter
      
      //----------------------
      
      //app.js    【导入并注册路由模块】
      const apiRouter = require('./apiRouter.js')
      app.use('/api',apiRouter)
      ```

  - 编写GET接口

    - ```js
      apiRouter.get('/get',(req,res) => {
          //1.获取到客户端通过查询字符串，发送到服务器的数据
          const query = req.query
          //2.调用res.send()方法，把数据响应给客户端
          res.send({
              status:0,                   //状态，0表示成功，1表示失败
              msg:'GET请求成功',            //状态描述
              data:query                   //需要响应给客户端的具体数据
          })
      })
      ```

  - 编写POST接口

    - ```js
      apiRouter.post('/post',(req,res) => {
          //1.获取客户端通过请求体，发送到服务器的URL-encoded数据
          const body = req.body
          //2.调用res.send()方法，把数据响应给客户端
          res.send({
              status:0,                   //状态，0表示成功，1表示失败
              msg:'POST请求成功',            //状态描述
              data:body                 //需要响应给客户端的具体数据
          })
      })
      ```

    - **注意：如果要获取URL-encoded格式的请求体数据，必须配置中间件app.use(express.urlencoded({extended:false}))**

  - CORS跨域资源共享

    - 接口的**跨域问题**
      - 刚才编写的GET和POST接口，存在一个很严重的问题：**不支持跨域请求**
      - **协议，域名，端口号任何一项不同就存在跨域问题**
      - 解决接口跨域问题的方案主要有两种：
        - **CORS**（主流的解决方案：**推荐使用**）
        - **JSONP**（有缺陷的解决方案：只支持GET请求）

    - 使用CORS中间件解决跨域问题

      - cors是Express的一个第三方中间件。通过安装和配置cors中间件，跨域很方便地解决跨域问题
      - 使用步骤：
        - 运行npm install cors**安装中间件**
        - 使用const cors = require('cors')**导入中间件**
        - 在路由之前调用app.use(cors())**配置中间件**

    - 什么是CORS

      - CORS（Cross-Origin Resource Sharing，跨域资源共享）由一系列**HTTP响应头**组成，**这些HTTP响应头绝对浏览器是否阻止前端JS代码跨域获取资源**
      - 浏览器的**同源安全策略**默认会阻止网页“跨域”获取资源。但如果接口服务器**配置了CORS相关的HTTP响应头**，就可以**解除浏览器端的跨域访问机制**
      - ![QQ图片20220121171727](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220121171727.png)

    - CORS的注意事项

      - CORS主要在**服务器端**进行配置。客户端浏览器**无需做任何额外的配置**，即可请求开启了CORS的接口
      - CORS在浏览器中**有兼容性**，只有支持XMLHttp-Request Level2的浏览器，才能正常访问开启了CORS的服务端就扣（例如：IE10+、Chrome4+、FireFox3.5+）

    - Cors响应头部 - Access-Control-Allow-**Origin**

      - 响应头中可以携带一个**Access-Control-Allow-Origin**字段，其语法如下：

      - ```js
        Access-Control-Allow-Origin:<origin> | *
        ```

      - 其中，origin参数的值指定了**允许访问该资源的外域URL**

      - 例如，下面的字段值将**只允许**来自http://itcast.cn的请求：

        ```js
        res.setHeader('Access-Control-Allow-Origin','http://itcast.cn')
        ```

      - 如果指定了Access-Control-Allow-Origin字段的值为通配符*，表示允许来自任何域的请求，示例代码如下：

        ```js
        res.setHeader('Access-Control-Allow-Origin','*')
        ```

    - CORS响应头部 - Access-Control-Allow-**Headers**

      - 默认情况下，CORS**仅**支持**客户端向服务器**发送如下的9个**请求头**：

        - Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width、Content-Type（值仅限于text/plain、multipart/form-data、application/x-www-form-urlencoded三者之一）

      - 如果客户端向服务器**发送了额外的请求头信息**，则需要在**服务器端**，通过Access-Control-Allow-Headers对**额外的请求体进行声明**，否则这次请求将会失败

      - ```js
        //允许客户端额外向服务器发送Content-Type请求头和X-Custom-Header请求头
        //注意：多个请求头之间使用英文的逗号进行分割
        res.setHeader('Access-Control-Allow-Headers','Content-Type,X-Custom-Header')
        ```

    - CORS响应头部 - Access-Control-Allow-**Methods**

      - 默认情况下，CORS仅支持客户端发起GET、POST、HEAD请求

      - 如果客户端希望通过**PUT、DELETE**等方式请求服务器的资源，则需要在服务器端，通过Access-Control-Allow-Methods来**指明实际请求所允许使用的HTTP方法**

      - 示例代码如下：

        ```js
        //只允许POST、GET、DELETE、HEAD请求方法
        res.setHeader('Access-Control-Allow-Methods','POST,GET,DELETE,HEAD')
        //允许所有的HTTP请求方法
        res.setHeader('Access-Control-Allow-Methods','*')
        ```

      

    - CORS请求的分类

      客户端在请求CORS接口时，根据**请求方式**和**请求头**的不同，可以将CORS的请求分为两大类，分别为：

      - 简单请求
      - 预检请求

    - 简单请求

      - 同时满足以下两大条件的请求，就属于简单请求：
        - **请求方式**：GET、POST、HEAD三者之一
        - HTTP头部信息不超过以下几种字段：无自定义头部字段、Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width、Content-Type（值仅限于text/plain、multipart/form-data、application/x-www-form-urlencoded三者之一）

    - 预检请求

      - 只要符合以下任何一个条件的请求，都需要进行预检请求：
        - 请求方式为**GET、POST、HEAD<u>之外</u>的请求Method类型**
        - 请求头中**包含自定义头部字段**
        - 向服务器发送了**application/json格式的数据**
      - 在浏览器与服务器正式通信之前，浏览器会**先发送OPTION请求进行预检以获知服务器是否允许该实际请求**，所以这一次的OPTION请求称为“预检请求”。**服务器成功响应预检请求后，才会发送真正的请求，并且携带真实数据**

    - **简单请求**和**预检请求**的区别

      - **简单请求的特点**：客户端与服务器之间**只会发生一次请求**
      - 预检请求：客户端与服务端之间会发生两次请求，**OPTION预检请求成功之后，才会发起正则的请求**

  - JSONP接口

    - 回顾JSONP的**概念**和**特点**

      - 概念：浏览器端通过<script>标签的src属性，请求服务器上的数据，同时，服务器返回一个函数的调用。这种请求数据的方式叫做JSONP
      - 特点：
        - JSONP不属于真正的Ajax请求，因为它没有使用XMLHttpRequest这个对象
        - JSONP仅支持GET请求，不支持POST、PUT、DELETE等请求

    - 创建JSONP接口的注意事项

      - 如果项目中**已经配置了CORS**跨域资源共享，为了**防止冲突**，**必须在配置CORS中间件之前声明JSONP的接口**，否则JSONP接口会被处理成开启了CORS的接口。示例代码如下：

      - ``` js
        // 优先创建JSONP接口【这个接口不会被处理成CORS接口】
        app.get('/api/jsonp',(req,res) => {})
        
        // 再配置CORS中间件【后续的所有接口，都会被处理成CORS接口】
        app.use(cors())
        //这是一个开启了CORS的接口
        app.get('/api/get',(req,res) => {})
        ```

    - 实现JSONP接口的步骤

      - **获取**客户端发送过来的**回调函数的名字**
      - **得到要**通过JSONP形式**发送给客户端的数据**
      - 根据前两步得到的数据，**拼接出一个函数调用的字符串**
      - 把上一步拼接得到的字符串，响应给客户端的<script>标签进行解析执行

    - 实现JSONP接口的具体代码

      - ```js
        app.get('/api/jsonp',(req,res) => {
            //1.获取客户端发送过来的回调函数的名字
            const funcName = req.query.callback
            //2.得到要通过JSONP形式发送给客户端的数据
            const data = {name:'zs',age:22}
            //3.根据前两步得到的数据，拼接出一个函数调用的字符串
            const scriptStr = '${funcName}(${JSON.stringify(data)})'
            //4.把上一步拼接得到的字符串，响应给客户端的<script>标签进行解析执行
            res.send(scriptStr)
        })
        ```

    - 在网页中使用jQuery发起JSONP请求

      - 调用$.ajax() 函数，**提供JSONP的配置选项**，从而发起JSONP请求，示例代码如下：

      - ```js
        ${'#btnJSONP'}.on('click',function() {
            $.ajax({
                method:'GET',
                url:'http://127.0.0.1/api/jsonp',
                dataType:'jsonp',           //表示要发起JSNOP请求
                success:function(res) {
                    console.log(res)
                }
            })
        })
        ```

        
