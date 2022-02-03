## form表单与模板引擎

- form表单的基本使用

  - 什么是表单

    - 表单在网页中主要负责**数据采集功能**。HTML中的<form>标签，就是用于采集用户输入的信息，并通过<form>标签的提交操作，把采集到的信息提交到服务器端进行处理

  - 表单的组成部分

    表单由三个基本部分组成：

    - 表单标签

    - 表单域：包含了文本框、密码框、隐藏域、多行文本框、复选框、单选框、下拉选择框和文件上传框等

    - 表单按钮

    - ```html
      <form>
          <input type="text" name="email_or_mobile"/>
          <input type='password' name='password'/>
          <input type="checkbox" name="remember_me" checked/>
          <button type="submit">
              提交
          </button>
      </form>
      ```

    

  - form标签的属性

    - form标签用来采集数据，标签的属性则是用来**规定如何把采集到的数据发送到服务器**

    - | 属性    | 值                                                           | 描述                                     |
      | ------- | ------------------------------------------------------------ | ---------------------------------------- |
      | action  | URL地址                                                      | 规定当提交表单时，向何处发送表单数据     |
      | method  | get或post                                                    | 规定以何种方式把表单数据提交到action URL |
      | enctype | application/x-www-form-urlencoded     multipart/form-data       text/plain | 规定在发送表单数据之前如何对其进行解码   |
      | target  | _blank    _self        _parent      _top         framename   | 规定在何处打开action URL                 |

    - action

      - action属性用来规定当提交表单时，**向何处发送表单数据**
      - action属性的值应该是后端提供的一个URL地址，这个URL地址专门负责接收表单提交过来的数据
      - 当<form>表单在未指定action属性值的情况下，action的默认值为当前页面的URL地址
      - 注意：当提交表单后，页面会立刻跳转到action属性指定的URL地址

    - target

      - target属性用来规定**在何处打开action URL**

      - 它的可选值有5个，默认情况下，target的值是_self，表示在相同的框架中打开action URL

      - | 值         | 描述                         |
        | ---------- | ---------------------------- |
        | **_blank** | **在新窗口中打开**           |
        | **_self**  | **默认。在相同的框架中打开** |
        | _parent    | 在父框架几种打开（很少用）   |
        | _top       | 在整个窗口中打开（很少用）   |
        | framename  | 在指定的框架中打开（很少用） |

    - method

      - method属性用来规定**以何种方式**把表单数据提交到action URL
      - 它的可选值有两个，分别是get和post
      - 默认情况下，method的值为get，表示通过URL地址的形式，把表单数据提交到action URL
      - 注意：
        - get方式适合用来提交少量的、简单的数据
        - post方式适合用来提交**大量的、复杂的**、或包含**文件上传**的数据
      - 在实际开发中，<form>表单的post提交方式用的最多，很少用get。例如登录、注册、添加数据等表单操作，都需要使用post方式来提交表单

    - enctype

      - enctype属性用来规定在**发送表单数据之前如何对数据进行编码**

      - 它的可选值有三个，默认情况下，enctype的值为application/x-www-form-urlencoded，表示在发送前编码所有字符

      - | 值                                | 描述                                                       |
        | --------------------------------- | ---------------------------------------------------------- |
        | application/x-www-form-urlencoded | 在发送前编码所有字符（默认）                               |
        | multipart/form-data               | 不对字符编码。在使用包含文件上传控件的表单时，必选使用该值 |
        | text/plain                        | 空格转换为”+“加号，但不对特殊字符编码（很少用）            |

      - 注意：

        - 在涉及到**文件上传**的操作时，**必须**将enctype的值设置为**multipart/form-data**
        - 如果表单的提交不涉及到文件上传操作，则直接将enctype的值设置为application/x-www-form-urlencoded即可

  - 表单的同步提交及缺点

    - 什么是表单的同步提交

      - 通过点击submit按钮，触发表单提交的操作，从而使页面跳转到action URL的行为，叫做表单的同步提交

    - 表单同步提交的缺点

      - form表单同步提交后，整个页面会发生跳转，**跳转到action URL所指向的地址**，用户体验很差
      - form表单同步提交后，**页面之前的状态和数据会丢失**

    - 如何解决表单同步提交的缺点

      - 解决方案：**表单只负责采集数据，Ajax负责将数据提交到服务器**

        

- 通过Ajax提交表单数据

  - 监听表单提交事件

    在jQuery中，可以使用如下两种方式，监听到表单的提交事件：

    - ```js
      $('#form1').submit(function(e) {
          alert('监听到了表单的提交事件')
      })
      ```

    - ```js
      $('#form1').on('submit',function(e) {
          alert('监听到了表单的提交事件')
      })
      ```

  - 阻止表单默认提交行为

    - 当监听到表单的提交事件之后，可以调用事件对象的event.preventDefault()函数，来阻止表单的提交和页面的跳转，示例代码如下：

    - ```js
      $('#form1').submit(function(e) {
      //阻止表单的提交和页面跳转
       e.preventDefault()
      })
      
      $('#form1').on('submit',function(e) {
      //阻止表单的提交和页面跳转
       e.preventDefault()
      })
      ```

  - 快速获取表单中的数据

    - serialize() 函数

      - 为了简化表单中数据的获取操作，jQuery提供了serialize（）函数，其语法格式如下

      - ```js
        $(selector).serialize()
        ```

    - serialize()函数示例

      ```html
      <form id="form1">
          <input type="text" name="username"/>
          <input type="password" name="password"/>
          <button type="submit">
              提交
          </button>
      </form>
      
      $('#form1').serialize()
      //调用的结果
      //username = 用户名的值 & password = 密码的值
      ```

    - 注意：在使用serialize（）函数快速获取表单数据时，**必须为每个表单元素添加name属性**

      

- 模板引擎的基本概念

  - 渲染UI结构时遇到的问题

    - ```js
       var rows = []
                  $.each(res.data,function(i,item) {
                      var str = '<li class="list-group-item"><span class="badge style="background-color:#F0AD4E;">评论时间:'+item.time+'</span><span class="badge" style="background-color:#5BC0DE;">评论人:'+item.username+'</span>'+item.content+'</li>'
                      rows.push(str)
                  })
                  $('#cmt-list').empty().append(rows.join(''))
      ```

    - 上述代码是通过**字符串拼接**的方式，来渲染UI结构

  - 什么是模板引擎

    - 模板引擎，顾名思义，它可以根据程序员指定的**模板结构和数据**，自动生成一个完整的HTML页面

  - 模板引擎的好处

    - 减少了字符串的拼接操作
    - 使代码结构更清晰
    - 使代码更易于阅读与维护

- art-template模板引擎

  - art-template简介

    - art-template是一个简约、超快的模板引擎。
    - 中文官网首页为http://aui.github.io/art-template/zh-cn/index.html

  - art-template模板引擎的基本使用

    - 使用传统方式渲染UI结构

      - ```js
        var data = {
            title:'<h3>用户信息</h3>',
            name:'zs',
            age:20,
            isVIP:true,
            regTime:new Date(),
            hobby:['吃饭','睡觉','打豆豆']
        }
        ```

    - art-template的使用步骤

      - 导入art-template

      - 定义数据

      - 定义模板**（模板的HTML结构，一定要定义到script标签中，且script有属性type="text/html”）**

      - 调用template函数**（为全局函数， template（’模板的Id‘，需要渲染的数据对象））**

      - 渲染HTML结构

      - 注意：

        - {{}}表示占位符

        - ```js
           <!-- 1.导入引擎 -->
              <script src="./template-web.js"></script>
              <script src="./jquery-3.6.0.min.js"></script>
          </head>
          <body>
              <div id="container"></div>
             <!-- 3.定义模板 -->
          <!-- 3.1模板的HTML结构，必须定义到script中 -->
          <script type="text/html" id="tpl-user">
              <h1>{{name}}   {{age}}</h1>
          </script>
          
          <script>
              //2.定义需要渲染的数据
              var data = {name:'zs',age:20}
          
              // 调用template函数
              var htmlStr = template('tpl-user',data)
              console.log(htmlStr);
              // 5.渲染HTML结构
              $('#container').html(htmlStr)
          </script>
          </body>
          </html>
          ```

  -  art-template标准语法

    - 什么是标准语法

      - art-template提供了**{{}}**这种语法格式，在{{}}内可以进行变量输出，或**循环数组**等操作，这种{{}}语法在art-template中被称为标准语法

    - 标准语法-输出

      - ```js
        {{value}}
        {{obj.key}}
        {{obj['key']}}
        {{a?b:c}}
        {{a || b}}
        {{a + b}}
        ```

      - 在{{}}语法中，可以进行**变量**的输出、**对象属性**的输出、**三元表达式**输出、**逻辑或**输出、**加减乘除等表达式**输出

    - 标准语法-原文输出

      - ```js
        {{@ value}}
         
         例子：
         {{test}}        //无法渲染
         {{@ test}}       //可以渲染
         var data = {test:'<h3>测试原文输出</h3>'}
        ```

      - 如果要输出的value值中，包含了HTML标签结构，则需要使用**原文输出**语法，才能保证HTML标签被正常渲染

    - 标准语法-条件输出

      - 如果要实现条件输出，则可以在{{}}中使用**if…else…/if**的方式，进行按需输出

      - ```js
        {{if value}}按需输出的内容{{/if}}
                             
        {{if v1}}按需输出的内容{{else if v2}} 按需输出的内容{{/if}}        
                                                
        例子：
        <script>
        <div>
            {{if flag === 0}}
             flag的值是0 
             {{else if flag === 1}}
             flag的值是1
             {{/if}}
            </div>    
        </script>
        
        
        var data = {flag:0}
        ```

    - 标准语法-循环输出

      - 如果要实现循环输出，则可以在{{}}内，通过each语法循环数组，当前循环的索引使用$$$index进行访问，当前的循环项使用$$$value进行访问

      - ```js
        {{each arr}}
             {{$index}}{{$value}}
        {{/each}}
          
          例子：
          
          <ul>
              {{each hobby}}
              <li>索引是：{{$index}},循环项是：{{$value}}</li>
              {{/each}}
          </ul>
                var data = {hobby:['吃饭'，'睡觉'，'写代码']}
          //输出结果：
                索引是：0,循环项是:吃饭
                索引是：1,循环项是:睡觉
                索引是：2,循环项是:写代码
          
        ```

    - 标准语法-过滤器

      - 过滤器的本质：一个function处理函数

      - 语法：

        ```js
        {{value | filterName}}
        ```

      - 过滤器语法类似**管道操作符**，它的上一个输出作为下一个输入

      - 定义过滤器的基本语法如下：

        ```js
        template.defaults.imports.filterName = function(value){/*return 处理的结果*/}
        ```

      - 定义一个格式化时间的过滤器dateFormat如下：

        ```js
        <div>注册时间：{{regTime | dateFormat}}</div>
        
        template.defaults.imports.dateFormat = function(date) {
            var y = date.getFullYear()
            var m = date.getMonth() + 1
            var d = date.getDate()
            
            return y + '-' + m + '-' + d  //注意，过滤器最后一定要return一个值 
        }
        
        ```

- 模板引擎的实现原理

  - 正则与字符串操作

    - 基本语法

      - exec（）函数用于**检索字符串**中的正则表达式的匹配

        ```js
        RegExpObject.exec(string)
        
        示例代码：
        var str = 'hello'
        var pattern = /o/
        //输出的结果["o",index:4,input:"hello",groups:undefined]
        console.log(pattern.exec(str))
        ```

      - 如果字符串中有匹配的值，**则返回该匹配值**，否则返回**null**

    - 分组

      - 正则表达式中（）包起来的内容表示一个分组，可以通过分组来**提取自己想要的内容**，示例代码如下：

        ```js
        var str = '<div>我是{{name}}</div>'
        var pattern = /{{([a-zA-Z]+)}}/
        
        var patternResult = pattern.exec(str)
        console.log('patternResult')
        //得到name相关的分组信息
        //["{{name}}","name",index:7,input:"<div>我是{{name}}</div>",groups:undefined]
        ```

    - 字符串的replace函数

      - replace()函数用于在字符串中用一些字符替换另一些字符，语法格式如下：

      - ```js
        var result = '123456'.replace('123','abc')    //得到的result的值为字符串'abc456'
        
        示例代码：
        var str = '<div>我是{{name}}</div>'
        var pattern = /{{([a-zA-Z]+)}}/
        
        var patternResult = pattern.exec(str)
        str = str.replace(patternResult[0],patternResult[1])      //replace函数返回值为替换后的新字符串
        //输出的内容是：<div>我是name</div>
        console.log(str)
        ```

    - 多次replace

      - ```js
        var str = '<div>{{name}}今年{{ age }}岁了</div>'
        var pattern = /{{\s*([a-zA-Z]+)\s*}}/
        
        var patternResult = pattern.exec(str)
        str = str.replace(patternResult[0],patternResult[1])
        console.log(str)          //输出<div>name今年{{ age }}岁了</div>
        
        patternResult = pattern.exec(str)
        str = str.replace(patternResult[0],patternResult[1])
        console.log(str)             //输出<div>name今年age岁了</div>
        
        patternResult = pattern.exec(str)
        console.log(patternResult)         //输出null
        ```

    - 使用while循环replace

      - ```js
        var str = '<div>{{name}}今年{{ age }}岁了</div>'
        var pattern = /{{\s*([a-zA-Z]+)\s*}}/
        
        var patternResult = null
        while(patternResult = pattern.exec(str)) {
            str = str.replace(patternResult[0],patternResult[1])
        }
        console.log(str)             //输出<div>name今年age岁了</div>
        ```

    - replace替换为真值

      - ```js
        var data = {name:'张三',age:20}
        var str = '<div>{{name}}今年{{age}}岁了</div>'
        var pattern = /{{\s*([a-zA-Z]+)\s*}}/
        
        var patternResult = null
        while((patternResult = pattern.exec(str))) {
             str = str.replace(patternResult[0],data[patternResult[1]])
        }
        console.log(str)
        ```

  - 实现简易的模板引擎

    - 实现步骤：

      - 定义模板结构
      - 预调用模板引擎
      - 封装template函数
      - 导入并使用自定义的模板引擎

    - 定义模板结构

      - ```html
        <!--定义模板结构-->
        <script type="text/html" id="tpl-user">
            <div>姓名：{{name}}</div>
             <div>年龄：{{ age }}</div>
              <div>性别：{{  gender}}</div>
               <div>住址：{{address  }}</div>
        </script>
        ```

    - 预调用模板引擎

      - ```js
        <script>
            //定义数据
            var data = {name:'zs',age:28,gender:'男',address:'北京顺义'}
            //调用模板函数
            var htmlStr = template('tpl-user',data)
            //渲染HTMLJIEG 
            document.getElementById('user-box').innerHTML = htmlStr
        </script>
        ```

    - 封装template函数

      - ```js
        function template(id,data) {
            var str = document.getElementById(id).innerHTML
            var pattern = /{{\s*([a-zA-Z]+)\s*}}/
        
            var pattResult = null
            while((pattResult = pattern.exec(str))) {
                str = str.replace(pattResult[0],data[pattResult[1]])
           }
           return str
        }
        ```

    - 导入并使用自定义的模板引擎

      - ```js
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
            <!-- 导入自定义模板引擎 -->
            <script src="./js/template.js"></script>
        </head>
        ```

      

