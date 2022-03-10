## Ajax加强

- XMLHttpRequest的基本使用

  - 什么是XMLHttpRequest

    - XMLHttpRequest（简称xhr）是浏览器提供的JavaScript对象，通过它，可以请求服务器上的数据资源。之前学的jQuery中的Ajax函数，就是基于xhr对象封装出来的

  - 使用xhr发起GET请求

    - 步骤：

      - 创建xhr对象
      - 调用xhr.open()函数
      - 调用xhr.send()函数
      - 监听xhr.onreadystatechange事件

    - ```js
      //1.创建xhr对象
      var xhr = new XMLHttpRequest()
      //2.调用open函数，指定 请求方式 与 URL地址
      xhr.open('GET','http://www.liulongin.top:3006/api/getbooks')
      //3.调用send函数，发起Ajax请求
      xhr.send()
      //4.监听xhr.onreadystatechange事件
      xhr.onreadystatechange = function() {
          //4.1监听xhr对象的请求状态 readyState；与服务器响应的状态status
          if(xhr.readyState === 4 && xhr.status === 200) {
               //4.2打印服务器响应回来的数据
              console.log(xhr.responseText)
          }
      }
      ```

  - 了解xhr对象的readyState属性

    - XMLHttpRequest对象的readyState属性，用来表示当前**Ajax请求所处的状态**。每个Ajax请求必然处于以下状态的一个：

      | 值    | 状态             | 描述                                                     |
      | ----- | ---------------- | -------------------------------------------------------- |
      | 0     | UNSENT           | XMLHttpRequest对象已被创建，但尚未调用open方法           |
      | 1     | OPENED           | open（）方法已被调用                                     |
      | 2     | HEADERS_RECEIVED | send（）方法已被调用，响应头也已经被接收                 |
      | 3     | LOADING          | 数据接收中，此时response属性中已经包含部分数据           |
      | **4** | **DONE**         | **Ajax请求完成**，这意味着数据传输已经彻底**完成或失败** |

  - 使用xhr发起带参数的GET请求

    - 使用xhr对象发起带参数的GET请求时，只需在调用xhr.open期间，为URL地址指定参数即可：

      ```js
      xhr.open('GET','http://www.liulongin.top:3006/api/getbooks?id=1')
      ```

    - ?xx=xx这种在URL地址后面拼接的参数，叫做**查询字符串**

  - 查询字符串

    - 什么是查询字符串

      - 定义：查询字符串（URL参数）是指在URL的末尾加上用于向服务器发送信息的字符串（变量）

      - 格式：将英文的**?**放在URL末尾，然后再加上**参数=值**，想加上多个参数的话，使用**&**符号进行分隔。以这个形式，可以将想要发送给服务器的数据添加到URL中

      - ```js
        //不带参数的URL地址
        'http://www.liulongin.top:3006/api/getbooks'
        //带一个参数的URL地址
        'http://www.liulongin.top:3006/api/getbooks?id=1'
        //带两个参数的URL地址
        'http://www.liulongin.top:3006/api/getbooks?id=1&bookname=西游记'
        ```

    - GET请求携带参数的本质

      - 无论使用$$$.ajax(),还是使用$$$.get(),又或者直接使用xhr对象发起GET请求，当需要携带参数的时候，本质上，都是直接将参数以查询字符串的形式，追加到URL地址的后面，发送到服务器的

      - ```js
        $.get('url',{name:'zs',age:20},function(){})
        //等价于
        $.get('url?name=zs&age=20',function() {})
        
        $.ajax({method:'GET',url:'url',data:{name:'zs',age:20},success:function() {}})
        //等价于
        $.ajax({method:'GET',url:'url?name=zs&age=20',success:function() {}})
        ```

  - URL编码与解码

    - 什么是URL编码

      - URL地址中，只允许出现英文相关的字母、标点符号、数字，因此，在URL地址中不允许出现中文字符
      - 如果URL中需要包含中文这样的字符，则必须对中文字符进行**编码**（转义）
      - **URL编码的原则**：使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符
      - URL编码原则的通俗理解：使用**英文字符**去表示**非英文字符**

    - 如何对URL进行编码与解码

      浏览器提供了URL编码与解码的API，分别是：

      - encodeURI（）编码的函数

      - decodeURI（）解码的函数

      - ```js
        encodeURI('需要编码的字符')
        decodeURI('需要解码的字符')
        ```

    - URL编码的注意事项

      - 由于浏览器会自动对URL地址进行编码操作，因此，大多数情况下，程序员不需要关心URL地址的编码与解码操作
      - 更多关于URL编码的知识，可以参考以下博客：https://blog.csdn.net/Lxd_0111/article/details/78028889

  - 使用xhr发起POST请求

    - 步骤：

      - 创建xhr对象
      - 调用xhr.open()函数
      - **设置Content-Type属性**（固定写法）
      - 调用xhr.send(),**同时指定要发送的数据（以查询字符串的方式）**
      - 监听xhr.onreadystatechange事件

    - ```js
      //1.创建xhr对象
      var xhr = new XMLHttpRequest()
      //2.调用xhr.open()函数
      xhr.open('POST','http://www.liulongbin.top:3006/api/addbook')
      //3.设置Content-Type属性（固定写法）
      xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
      //4.调用xhr.send(),同时指定要发送的数据
      xhr.send('bookname=水浒传&author=施耐庵&publisher=天津图书出版社')
      //5.监听xhr.onreadystatechange事件
      xhr.onreadystatechange = function(){
          if(xhr.readyState === 4 & xhr.status === 200) {
              console.log(xhr.responseText)
          }
      }
      ```
      
      
      

- 数据交换格式

  - 数据交互格式，就是**服务器端**与**客户端**之间进行**数据传输与交换的格式**

  - 前端领域，经常提及的两种数据交换格式分别是XML和JSON，其中XML用到非常少，所以我们重点学习JSON

  - XML

    - 什么是XML
      - XML的英文全称是EXtensible Markup Language，即可扩展标记语言。因此，XML和HTML类似，也是一种标记语言
    - XML和HTML的区别
      - XML和HTML虽然都是标记语言，但是它们两者之间没有任何的关系
      - HTML被设计用来描述网页上的**内容**，是网页内容的载体
      - XML被设计用来**传输和存储数据**，是数据的载体
    - XML的缺点
      - XML格式臃肿，和数据无关的代码多，体积大，传输效率低
      - 在JavaScript中解析XML比较麻烦

  - JSON

    - 什么是JSON

      - 概念：JSON的英文全称是JavaScript Object Notation，即“JavaScript对象表示法“。简单来说，**JSON就是JavaScript对象和数组的字符串表示法**。它使用文本表示一个JS对象或数组的信息，因此，**<u>JSON的本质就是字符串</u>**
      - 作用：JSON是一种**轻量级的文本数据交换格式**，在作用上类似于XML，专门用于存储和传输数据，但是JSON比XML**更小、更快、更易解析**
      - 现状：JSON是在2001年开始被推广和使用的数据格式，到现在为止，**JSON已经成为了主流的数据交换格式**

    - JSON的两种结构

      - JSON就是用字符串来表示JavaScript的对象和数组。所以，JSON中包含**对象和数组**两种结构，通过这两种结构的**相互嵌套**，可以表示各种复杂的数据结构

      - **对象结构**：对象结构在JSON中表示为{}括起来的内容。数据结构为{key：value；key：value，…}的键值对结构。其中，key必须是使用**英文的双引号包裹的字符串**，value的数据类型可以是**数字、字符串、布尔值、null、数组、对象**6种类型

        ```json
        {
            name:"zs",                //name没有"包裹
            'age':20,                //age没有"包裹
            "gender":'男',                //男没有"包裹
            "address":undefined,                  //undefined不为json的类型，需改为null
            "hobby":["吃饭","睡觉",'打豆豆'],            //打豆豆要用"包裹       
            say:function() {}                    //say没有用"包裹，且function不为json的属性
        }
        ```

      - **数组结构**：数组结构在JSON种表示为[]括起来的内容。数据结构为[“Java“，”JavaScript”，30，true…]。数组中数据的类型可以是**数字、字符串、布尔值、null、数组、对象**6种类型

    - JSON语法注意事项

      - **<u>属性名必须使用双引号包裹</u>**
      - 字符串类型的值必须使用双引号包裹
      - JSON中不允许使用单引号表示字符串
      - JSON中不能写注释
      - JSON的最外层必须是对象或数组格式
      - 不能使用undefined或函数作为JSON的值
      - **JSON的作用**：在计算机与网络之间存储和传输数据
      - **JSON的本质**：用字符串来表示JavaScript对象数据或数组数据

    - JSON和JS对象的关系

      - JSON是JS对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串。例如：

        ```json
        //这是一个对象
        var obj = {a:'Hello',b:'World'}
        
        //这是一个JSON字符串，本质是一个字符串
        var json = '{"a":"Hello","b":"World"}'
        ```

    - JSON和JS对象的互转

      - 要实现从JSON字符串转换为JS对象，使用JSON.parse()方法

        ```js
        var obj = JSON.parse('{"a":"Hello","b":"World"}')
        //结果是 {a:'Hello',b:'World'}
        ```

      - 要实现从JS对象转换为JSON字符串，使用JSON.stringify()方法

        ```js
        var json = JSON.stringify({a:'Hello',b:'World'})
        //结果是'{"a":"Hello","b":"World"}'
        ```

    - 序列化和反序列化

      - 把**数据对象转换为字符串**的过程，叫做**序列化**，例如：调用JSON .**stringify**()函数的操作，叫做JSON序列化
      - 把**字符串转换为数据对象**的过程，叫做**反序列化**，例如：调用JSON.**parse**()函数的操作，叫做JSON反序列化

- 封装自己的Ajax函数

  - 定义options参数选项

    - itheima（）函数是我们自定义的Ajax函数，它接收一个 配置对象作为参数，配置对象中可以配置如下属性：
      - method 请求的类型
      - url  请求的URL地址
      - data 请求携带的数据
      - success   请求成功之后的回调函数

  - 处理data函数

    - 需要把data对象，转化成查询字符串的格式，从而提交给服务器，因此提前定义resolveData函数如下：

      ```js
      /*
      处理data函数
      @param{data}需要发送到服务器的数据
      @returns{string}返回拼接好的查询字符串 name=zs&age=10
      */
      function resolveData(data) {
          var arr = []
          for(let k in data) {
              arr.push(k + '=' + data[k])
          }
          return arr.join('&')
      }
      ```

  - 定义itheima函数

    - 在itheima（）函数中，需要创建xhr对象，并监听onreadystatechange事件：

      ```js
      function itheima(options) {
          var xhr = new XHLHttpRequest()
          //拼接查询字符串
          var qs = resolveData(options.data)
          
          //监听请求状态改变的事件
          xhr.onreadystatechange = function() {
              if(xhr.readyState === 4 && xhr.status === 200) {
                  var result = JSON.parse(xhr.responseText)
                  options.success(result)
              }
          }
      }
      ```

  - 判断请求的类型

    - 不同的请求类型，对应xhr对象的不同操作，因此需要对请求类型进行if…else…的判断：

      ```js
      if(opotions.method.toUpperCase() === 'GET') {
          xhr.open(options.method,options.url + '?' + qs)
          xhr.send()
      }else if (options.method.toUpperCase() === 'POST') {
          //发起POST请求
          xhr.open(options.method,options.url)
          xhr.setRequestHeader('Contect-Type','application/x-www-form-urlencoded')
          xhr.send(qs)
      }
      ```

  

  

- XMLHttpRequest Level2的新特性

  - 认识XMLHttpRequest Level2

    - 旧版XMLHttpRequest的缺点
      - 只支持文本数据的传输，无法用来读取和上传文件
      - 传送和接收数据时，没有进度消息，只能提示有没有完成
    - XMLHttpRequest level2的新功能
      - 可以设置HTTP请求的时限
      - 可以使用FormData对象管理表单数据
      - 可以上传文件
      - 可以获得数据传输的进度信息

  - 设置HTTP请求时限

    - 有时，Ajax操作很耗时，而且无法预知要花多少时间。如果网速很慢，用户可能要等很久。新版本的XMLHttpRequest对象，增加了timeout属性，可以设置HTTP请求的时限：

    - ```js
      xhr.timeout= 3000
      ```

    - 上面的语句，将最长等待时间设置为3000毫秒。过了这个时限，就自动停止HTTP请求。与之配套的还有一个timeout事件，用来指定回调函数：

      ```js
      xhr.ontimeout = function(event) {
          alert('请求超时')
      }
      ```

    - 注意：**设置在open函数之前**

  - FormData对象管理表单数据

    - Ajax操作往往用来提交表单数据。为了方便表单处理，HTML5新增了一个FormData对象，可以模拟表单操作：

    - ```js
      //1.新建FormData对象
      var fd = new FormData()
      //2.为FormData添加表单项
      fd.append('uname','zs')
      fd.append('upwd','123456')
      //3.创建XHR对象
      var xhr = new XMLHttpRequest()
      //4.指定请求类型与URL地址
      xhr.open('POST','http://www.liulongbin.top:3006/api/formdata')
      //5.直接提交FormData对象，这与提交网页表单的效果完全一样
      xhr.sned(fd)
      ```

    - FormData对象也可以用来获取网页表单的值，示例代码如下：

      ```js
      //获取表单元素
      var form = document.querySelector('#form1')
      //监听表单元素的submit事件
      form.addEventListener('submit',function(e) {
      e.preventDefault()
      //根据form表单创建FormData对象，会自动将表单数据填充到FormData对象中
          var fd = new FormData(form)
          var xhr = new XMLHttpRequest()
          xhr.open('POST','http://www.liulongbin.top:3006/api/fromdata')
          xhr.send(fd)
          xhr.onreadytechange = function() {}
      })
      ```

  - 上传文件

    - 新版 XMLHttpRequest对象，不仅可以发送文本信息，还可以上传文件

    - 实现步骤：

      - 定义UI结构

        ```html
        <!-- 1.文件选择框-->
        <input type="file" id="file1"/>
        <!-- 2.上传按钮-->
        <button id="btnUpload">
            上传文件
        </button>
        <!-- 3.用来显示上传成功以后的图片-->
        <img src="" alt="" id="img" width="800px" />
        ```

      - 验证是否选择了文件

        ```js
        //1.获取上传文件的按钮
        var btnUplaod = document.querySelector('#btnUpload')
        //2.为按钮添加click事件监听
        btnUpload.addEventListener('click',function() {
            //3.获取到选择的文件列表
            var files = document.querySelector('#file1').files      //files为一个数组
            if(files.length <= 0) {
                return alert('请选择要上传的文件！')
            }
            //后续业务逻辑
        })
        ```

      - 向FormData中追加文件

        ```js
        //1.创建FormData对象
        var fd = new FormData() 
        //2.向FormData中追加文件
        fd.append('avatar',files[0])
        ```

      - 使用xhr发起上传文件的请求

        ```js
        //1.创建xhr对象
        var xhr = new XMLHttpRequest()
        //2.调用open函数，指定请求类型与URL地址。其中，请求类型必须为POST
        xhr.open('POST','http://www.liulongbin.top:3006/api/upload/avatar')
        //3.发起请求
        xhr.send(fd)
        ```

      - 监听onreadystatechange结构

        ```js
        xhr.onreadystatechange = function() {
                if(xhr.readyState === 4 && xhr.status === 200) {
                    var data = JSON.parse(xhr.responseText)
                    if(data.status === 200) { //上传文件成功
                        //将服务器返回的图片地址，设置为<img>标签的src属性
                        document.querySelector('#img').src = 'http://www.liulongbin.top:3006' + data.url
                    }  else {//上传文件失败
                        console.log(data.message);
                    }
                 }
            }
        ```

  - 显示文件上传进度

    - 新版本的XMLHttpRequest对象中，可以通过监听xhr.upload.onprogress事件，来获取到文件的上传进度。语法格式如下：

    - ```js
      //创建XHR对象
      var xhr = new XMLHttpRequest()
      //监听xhr.upload.onprogress事件
      xhr.upload.onprogress = function(e) {
          //e.lengthComputable是一个布尔值，表示当前上传的资源是否具有可计算的长度
          if(e.lengthComputable) {
              //e.loaded 已传输的字节
              //e.total 需传输的总字节
              var percentComplete = Math.ceil((e.loaded / e.total) * 100)
          }
      }
      ```

    - 导入bootstrap库，选择进度条样式进入

    - 监听上传进度的事件

      ```js
      xhr.upload.onprogress = function(e) {
         if(e.lengthComputable) {
             //1.计算出当前上传进度的百分比
             var percentComplete = Math.ceil((e.loaded / e.total) * 100)
             $('#percent')
             //2.设置进度条的高度
             .attr('style','width:' + percentComplete + '%')
             //3.显示当前的上传进度百分比
             .html(percentComplete + '%')
         }
      }
      ```

    - 监听上传完成的事件

      ```js
      xhr.upload.onload = function() {
          $('$percent')
          //移除上传中的类样式
          .removeClass()
          //添加上传完成的类样式
          .addClass('progress-bar progress-bar-success')
      }
      ```

      

  

  

- jQuery高级用法

  - jQuery实现文件上传

    - 定义UI结构

      ```html
      <!--导入jQuery-->
      <script src="./lib/jquery.js"></script>
      <!--文件选择框-->
      <input type="file" id="file1"/>
      <button id="btnUpload">
          上传
      </button>
      ```

    - 验证是否选择了文件

      ```js
      $('#btnUpload').on('click',function() {
          //1.将jQuery对象转化为DOM对象，并获取选中的文件列表
          var files = $('file1')[0].files
          //2.判断是否选择了文件
          if(files.length <= 0) {
              return alert('请选择图片后再上传')
          }
      })
      ```

    - 向FormData中追加文件

      ```js
      //向FormData中追加文件
      var fd = new FormData()
      fd.append('avatar',files[0])
      ```

    - 使用jQuery发起上传文件的请求

      ```js
          $.ajax({
              method:'POST',
              url:'http://www.liulongbin.top:3006/api/upload/avatar',
              data:fd,
              //不对FormData中的数据进行url编码，而是将FormData数据原样发送到服务器
              processData:false,
              //不修改Content-Type属性，使用FormData默认的Content-Type值
              contentType:false,
              success:function(res) {
                  console.log(res);
              }
          })
      ```

  - 使用jQuery实现loading效果

    - ajaxStart(callback)

      - Ajax请求**开始**时，指向ajaxStrat函数。可以在ajaxStart的callback中显示loading效果，示例代码如下：

      - ```js
        //自jQuery版本1.8起，该方法只能被附加到文档
        $(document).ajaxStrat(function() {
           $('#loading').show() 
        })
        ```

      - 注意：$(document).ajaxStart()函数**会监听当前文档内所有的Ajax请求**

    - ajaxStop(callback)

      - Ajax请求结束时，执行ajaxStop函数，可以在ajaxStop的callback中因此loading效果，示例代码如下：

      - ```js
        //自jQuery版本1.8起，该方法只能被附加到文档
        $(document).ajaxStop(function() {
            $('#loading').hide()
        })
        ```

        

- axios

  - 什么是axios

    - Axios是专注于**网络数据请求**的库
    - 相比于原生的XMLHttpRequest对象，axios**简单易用**
    - 相比于jQuery，axios更加**轻量化**，只专注于网络数据请求

  - 引入axios：

    ```js
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    ```
    
  - axios发起GET请求

    - axios发起请求的语法：

      ```js
      axios.get('url',{params:{/*参数*/}}).then(callback)
      
      示例：
       document.querySelector('#btn1').addEventListener('click',function() {
           //请求的URL地址
                  var url = 'http://www.liulongbin.top:3006/api/get'
                  //请求的参数对象
                  var paramsObj = {name:'zs',age:20}
                  //调用axios.get()发起GET请求
                  axios.get(url,{params:paramsObj}).then(function(res) {
                      //res.data是服务器返回的数据
                      var result = res.data
                      console.log(res);
                  })
              })
      ```

  - axios发起POST请求

    - axios发起post请求的语法：

      ```js
      axios.post('url',{/*参数*/}).then(callback)
      
      示例：
      document.querySelector('#btn2').addEventListener('click',function() {
          //请求的URL地址
                  var url = 'http://www.liulongbin.top:3006/api/post'
                   //请求的参数对象
                  var dataObj = {address:'北京',location:'顺义区'}
                  //调用axios.post()发起POST请求
                  axios.post(url,dataObj).then(function(res) {
                      //res.data是服务器返回的数据
                      console.log(res.data);
                  })
              })
      ```

  - 直接使用axios发起请求

    - axios也提供了类似于jQuery中$.ajax()的函数，语法如下：

      ```js
      axios({
          method:'请求类型',
          url:'请求的url地址',
          data:{/*POST数据*/},
          params:{/*GET参数*/}
      }).then(callback)
      ```

    - 直接使用axios发起GET请求

      ```js
      axios({
          method:'GET',
          url:'http://www.liulongbin.top:3006/api/get',
          params:{//GET参数要通过params属性提供
             name:'zs',
              age:20
          }
      }).then(function(res) {
          console.log(res.data)
      })
      ```

    - 直接使用axios发起POST请求

      ```js
      axios({
          method:'POST',
          url:'http://www.liulongbin.top:3006/api/post',
          data:{//POST参数要通过data属性提供
          bookname:'程序员的自我修养',
              price:666
          },
      }).then(function(res) {
          console.log(res.data)
      })
      ```

      

