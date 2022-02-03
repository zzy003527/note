## ajax

- URL地址

  - URL全称是（UniformResourceLocator）中文叫**统一资源定位符**，用于标识互联网上每个资源的唯一存放位置。

  - 浏览器只有通过URL地址，才能正确定位资源的存放位置，从而成功访问到对应的资源

  - URL地址的组成部分

    URL地址一般由三部分组成：

    - 客户端与服务器之间的**通信协议**
    - 存有该资源的**服务器名称**
    - 资源在服务器上**具体的存放位置**
    - ![QQ图片20220122171145](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220122171145.png)

- 客户端与服务器的通信过程

  - ![QQ图片20220122171437](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220122171437.png)
  - 注意：
    - 客户端与服务器之间的通信过程，分为**请求-处理-响应**三个步骤
    - 网页中的每一个资源，都是通过**请求-处理-响应**的方式从服务器获取回来的
  - 基于浏览器的开发中工具分析通信过程
    - 打开Chrome浏览器
    - Ctrl+Shift+I打开Chorome的开发者面板
    - 切换到Network面板
    - 选中Doc页签
    - 刷新页面，分析客户端与服务器的通信过程

- 服务器对外提供了哪些资源

  - 网页中常见资源
    - 文字内容
    - Image图片
    - Audio音频
    - Video视频

  - **网页中的数据，也是服务器对外提供的一种资源。**例如股票数据，各行业排行榜等

  - 网页中如何请求数据

    - 数据也是服务器对外提供的一种资源。只要是资源，必然要通过**请求-处理-响应**的方式进行获取

    - 如果要在网页中请求服务器上的数据资源，则需要用到**XMLHttpRequest对象**

    - XMLHttpRequest（简称xhr）是浏览器提供的js成员，通过它，可以请求服务器上的数据资源

    - 最简单的用法：

      ```js
      var xhrObj = new XMLHttpRequest()
      ```

  - 资源的请求方式

    - 客户端请求服务器时，请求的方式有很多种，最常见的两种请求方式分别为get和post请求
      - **get请求**通常用于**获取服务端资源**（向服务端要资源）
        - 例如：根据URL地址，从服务器获取HTML文件，css文件，js文件，图片文件，数据资源等
      - **post请求**通常用于**向服务器提交数据**（往服务器发送资源）
        - 例如：登录时向服务器**提交的登录信息**、注册时向服务器**提交的注册信息**、添加用户时向服务器**提交的用户信息**等各种**数据提交操作**

- 了解AJAX

  - AJAX简介

    - AJAX全称为Asynchronous JavaScript And XML，就是异步的JS 和XML
    - 通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**
    - AJAX不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式
    - AJAX可以让我们轻松实现网页与服务器之间的**数据交互**

  - XML简介

    - XML：可扩展标记语言

    - XML被设计用来传输和存储数据

    - XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来表示一些数据。

    - ```xml
      比如有一个学生数据：
          name = '孙悟空';age = 18；gendr = "男"；
      用XML表示：
      <student>
              <name>孙悟空</name>
              <age>18</age>
              <gender>男</gender>
      </student>
      用JSON表示：
          {"name"："孙悟空"，"age":18,"gender":"男"}
      ```

  - AJAX的特点

    - AJAX的优点
      - 可以无需刷新页面而与服务器进行通讯
      - 允许你根据用户时间来更新部分页面内容
    - AJAX的缺点
      - 没有浏览历史，不能回退
      - 存在跨域问题（同源）
      - SEO不友好

- jQuery中的Ajax

  - 浏览器中提供的XMLHttpRequest用法比较复杂，所有jQuery对XMLHttopRequest进行了封装，提供了一系列Ajax相关的函数，极大地降低了Ajax的使用难度

  - jQuery中发起Ajax请求最常用的三个方法如下：

    - $.get()
    - $.post()
    - $.ajax()

  - $.get()函数的语法

    - jQuery中$.get()函数的功能单一，专门用来发起get请求，从而将服务器上的资源请求到客户端来进行使用

    - $.get()函数的语法如下：

      ```js
      $.get(url,[data],[callback])
      ```

    - 其中，三个参数各自代表的含义如下：

      | 参数名   | 参数类型   | 是否必选 | 说明                         |
      | -------- | ---------- | -------- | ---------------------------- |
      | **url**  | **string** | **是**   | 要请求的**资源地址**         |
      | data     | object     | 否       | 请求资源期间要**携带的参数** |
      | callback | function   | 否       | 请求成功时的**回调函数**     |

  - $.get()发起**不带参数**的请求

    - 使用$.get()函数发起不带参数的请求时，直接提供**请求的URL地址**和**请求成功之后的回调函数**即可，示例代码如下：

    - ```js
      $.get('http://www.liulongbin.top:3006/api/getbooks',function(res) {
          console.log(res)         //这里的res是服务器返回的数据
      })
      ```

  - $.get()发起**带参数**的请求

    - 使用$.get()函数发起带参数的请求是，示例代码如下：

    - ```js
      $.get('http://www.liulongbin.top:3006/api/getbooks',{id:1},function(res) {
          console.log(res)
      })
      ```

      

  - $.post()函数的语法

    - jQuery中$.post()函数的功能单一，专门用来发起post请求，从而向服务器提交数据

    - $.post()函数的语法如下：

      ```js
      $.post(url,[data],[callback])
      ```

    - 其中，三个参数各自代表的含义如下：

      | 参数名   | 参数类型   | 是否必选 | 说明                       |
      | -------- | ---------- | -------- | -------------------------- |
      | **url**  | **string** | **是**   | **提交数据的地址**         |
      | data     | object     | 否       | **要提交的数据**           |
      | callback | function   | 否       | 数据提交成功的**回调函数** |

  - $.post()向服务器提交数据

    - 示例代码如下：

      ```js
      $.post(
         'http://www.liulongbin.top:3006/api/getbooks'，       //请求的URL地址
          {bookname:'水浒传',author:'施耐庵',publisher:'上海图书出版社'},      //要提交给服务器的数据
          function(res) {
              console.log(res)        //回调函数
          }
      )
      ```

      

  - $.ajax()函数的语法

    - 相比于<u>$.get()</u>和<u>$.post()</u>函数，jQuery中提供的$.ajax()函数，是一个功能比较综合的函数，它允许我们对Ajax请求进行更详细的配置

    - $.ajax()函数的基本语法如下：

      ```js
      $.ajax({
          type:'',     //请求的方式，例如GET或POST
          url:'',       //请求的URL地址
          data:{},      //这次请求要携带的数据
          success:function(res) {}       //请求成功之后的回调函数
      })
      ```

    - 使用$.ajax()发起GET请求

      - 使用$.ajax()发起GET请求时，只需要将**type属性**的值设置为‘**GET**’即可

      - ```js
        $.ajax({
            type:'GET',           //请求的方式
            url:'http://www.liulongbin.top:3006/api/getbooks',    //请求的URL地址
            data:{id:1},           //这次请求要携带的数据
            success:function(res) {               //请求成功之后的回调函数
                console.log(res)
            }
        })
        ```

    - 使用$.ajax()发起POST请求

      - 使用$.ajax()发起POST请求时，只需要将type的值设置为‘POST’即可：

      - ```js
        $.ajax(
            type:'POST',                 //请求的方式
            url:'http://www.liulongbin.top:3006/api/getbooks'，       //请求的URL地址
            data:{bookname:'水浒传',author:'施耐庵',publisher:'上海图书出版社'},      //要提交给服务器的数据
            success:function(res) {
                console.log(res)        //请求成功之后的回调函数
            }
        )
        ```

- 接口

  - 接口的概念

    - 使用Ajax请求数据时，**被请求的URL地址**，就叫做**数据接口**（简称**接口**）。同时，每个接口必须有**请求方式**
    - 例如：
      - http://www.liulongbin.top:3006/api/getbooks           获取图书列表的接口（GET请求）
      - http://www.liulongbin.top:3006/api/addbook            添加图书的接口（POST请求）

  - 分析接口的请求过程

    - 通过GET方式请求接口的过程
      - ![QQ图片20220122192412](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220122192412.png)
    - 通过POST方式请求接口的过程
      - ![QQ图片20220122192428](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220122192428.png)

  - 接口测试工具

    - 什么是接口测试工具
      - 为了验证接口能否被正常访问，我们常常需要使用接口测试工具，来对数据接口进行检测
      - **好处**：接口测试工具能让我们在**不写任何代码**的情况下，对接口进行**调用和测试**
    - 下载并安装PostMan
      - 访问PostMan的官方下载网址https://www.getpostman.com/downloads/,下载所需的安装程序后，直接安装即可
    - 使用PostMan测试GET接口
      - 选择请求的方式
      - 填写请求的URL地址
      - 填写请求的参数
      - 点击Send按钮发起GET请求
      - 查看服务器响应的结果
    - 使用PostMan测试POST接口
      - 选择请求的方式
      - 填写请求的URL地址
      - 选择Body面板并勾选数据格式
      - 填写要发送到服务器的数据
      - 点击Send按钮发起POST请求
      - 擦好看服务器响应的结果

  - 接口文档

    - 什么是接口文档

      - 接口文档，顾名思义就是**接口的说明文档，它是我们调用接口的依据**。好的接口文档包含了对**接口URL**，**参数**以及**输出内容**的说明，我们参照接口文档就能方便的知道接口的作用，以及接口如何进行调用

    - 接口文档的组成部分

      - 接口文档可以包含很多信息，也可以按需进行精简，不过，一个合格的接口文档，应该包含以下6向内容，从而为接口的调用提供依据：
        - **接口名称**：用来标识各个接口的简单说明，如**登录接口，获取图书列表接口**等
        - **接口URL**：接口的调用地址
        - **调用方式**：接口的调用方式，如GET或POST
        - **参数格式**：接口需要传递的参数，每个参数必须包含**参数名称、参数类型、是否必选、参数说明**这4项内容
        - **响应格式**：接口的返回值的详细描述，一般包含**数据名称、数据类型、说明**3项内容
        - 返回示例（可选）：通过对象的形式，例举服务器返回数据的结构

    - 接口文档示例：

      - ![QQ图片20220123101854](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220123101854.png)

      - ![QQ图片20220123102004](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220123102004.png)

        
