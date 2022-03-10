## 跨域与JSONP

- 了解同源策略和跨域

  - 什么是**同源**

    - 如果两个页面的**协议，域名**和**端口**都相同，则两个页面具有**相同的源**

    - 例如，下表给出了相对于http://www.test.com/index.html页面的同源检测：

      | URL                                | 是否同源 | 原因                                    |
      | ---------------------------------- | -------- | --------------------------------------- |
      | http://www.test.com/other.html     | 是       | 同源（协议、域名、端口相同）            |
      | https://www.test.com/about.html    | 否       | 协议不同（http和https）                 |
      | http://blog.test.com/movie.html    | 否       | 域名不同（www.test.com与blog.test.com） |
      | http://www.test.com:7001/home.html | 否       | 端口不同（默认的80端口和7001端口）      |
      | http://www.test.com:80/main.html   | 是       | 同源（协议、域名、端口相同）            |

  - 什么是**同源策略**

    - **同源策略**（英文全称Same origin policy)是**浏览器**提供的一个**安全功能**
    - MDN官方给定的概念：同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制
    - 通俗的理解：浏览器规定，A网站的JavaScript，不允许和**非同源的**网站C之间，进行资源的交互，例如：
      - 无法读取非同源网页的Cookie、LocalStorage和IndexedDB
      - 无法接触非同源网页的DOM
      - 无法向非同源地址发送Ajax请求

  - 跨域

    - 什么是**跨域**
      - **同源**指的是两个URL的协议、域名、端口一致，反之则是**跨域**
      - 出现跨域的根本原因：**浏览器的同源策略**不允许非同源的URL之间进行资源的交互
    - 浏览器对跨域请求的拦截
      - ![QQ图片20220124203541](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20220124203541.png)
      - 注意：浏览器允许发起跨域请求，但是，跨域请求回来的数据，会被浏览器拦截，无法被页面获取到
    - 如何实现跨域数据请求
      - 现如今，实现跨域数据请求，最主要的两种解决方案，分别是**JSONP**和**CORS**
      - JSONP：出现的早，兼容性好（兼容低版本IE）。是前端程序员为了解决跨域问题，被迫想出来的一种**临时解决方案**。缺点是**只支持GET请求**，不支持POST请求
      - CORS：出现的较晚，它是W3C标准，属于跨域Ajax请求的根本解决方案。支持GET和POST请求。缺点是不兼容某些低版本的浏览器

  - JSONP

    - JSONP（JSON with Padding）是JSON的一种”使用模式“，可用于解决主流浏览器的跨服数据访问的问题

    - JSONP的实现原理

      - 由于**浏览器同源策略**的限制，网页中**无法通过Ajax请求非同源的接口数据**。但是<script>标签不受浏览器同源策略的影响，跨域通过src属性，请求非同源的js脚本
      - 因此，JSONP的实现原理，就是通过<script>标签的src属性，请求跨域的数据接口，并通过**函数调用**的形式，接收跨域接口响应回来的数据

    - 自己实现一个简单的JSONP

      - 定义一个success回调函数：

        ```js
        <script>
         function success(data) {
           console.log('获取到了data数据')
           console.log(data)
        }    
        </script>
        ```

      - 通过<script>标签，请求接口数据：

        ```js
        <script src="httpL//ajax.frontend.itheima.net:3006/api/jsonp?callback=success&name=zs&age=20"></script>
        
        //通过响应字符串的形式在script标签填入数据
        ```

    - JSONP的缺点

      - 由于JSONP是通过<script>标签的src属性来实现跨域数据获取的，所以，JSONP只支持GET数据请求，不支持POST请求
      - 注意：**JSONP和Ajax之间没有任何关系**，不能把JSONP请求数据的方式叫做Ajax，因为JSONP没有用到XMLHttpRequest这个对象

    - jQuery中的JSONP

      - jQuery提供的$.ajax()函数，除了可以发起真正的Ajax数据请求之外，还能够发起JSONP数据请求，例如：

        ```js
        $.ajax({
            url:'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
            //如果要使用$.ajax()发起JSONP请求，必须指定datatype为jsonp
            dataType:'jsonp',
            success:function(res) {
                console.log(res)
            }
        })
        ```

      - 默认情况下，使用jQuery发起JSONP请求，会自动携带一个**callback=jQueryxxx**的函数，**jQueryxxx**是随机生成的一个回调函数名称

    - 自定义参数及回调函数名称

      - 在使用jQuery发起JSONP请求时，如果想要自定义JSONP的**参数**以及**回调函数名称**，可以通过如下两个参数来指定：

      - ```js
        $.ajax({
              url:'http://ajax.frontend.itheima.net:3006/api/jsonp?name=zs&age=20',
            //如果要使用$.ajax()发起JSONP请求，必须指定datatype为jsonp
            dataType:'jsonp',
            //发送到服务端的参数名称，默认值为callback
            jsonp:'callback',
            //自定义的回调函数名称，默认值为jQueryxxx格式
            jsonpCallback:'abc',
            success:function(res) {
                console.log(res)
            }
        })
        ```

    - jQuery中JSONP的实现过程

      - jQuery中的JSONP，也是通过<script>标签的src属性实现跨域数据访问的，只不过，jQuery采用的是**动态创建和移除<script> 标签**的方式，来发起JSONP数据请求
      - 在**发起JSONP请求**的时候，动态向<header>中append一个<script>标签
      - 在**JSONP请求成功**以后，动态从<header>中移除刚才append进去的<script>标签

    

- 防抖和节流

  - 什么是防抖

    - 防抖策略（debounce）是当事件被触发后，**延迟n秒**后再**执行回调**，如果在这**n秒内事件又被触发**，则**重新计时**

  - 防抖的应用场景

    - 用户在输入框中连续输入一串字符时，可以通过防抖策略，只在输入完后，才执行查询的请求，这样可以有效减少请求次数，节约请求资源；

  - 实现输入框的防抖

    - ```js
      var timer = null            //1.防抖动的timer
      
      function debounceSearch(keywords) {     //2.定义防抖的函数
          timer = setTimeout(function() {
              //发起JSONP请求
              getSuggestList(keywords)    //此行为一个请求函数
          },500)
      }
      
      $('#ipt').on('keyup',function() {   //3.在触发keyup事件时，立即清空timer
          clearTimer(timer)
          //……省略其他代码
          debounceSearch(keywords)
      })
      ```

      

    

  - 什么是节流

    - 节流策略（throttle），顾名思义，可以减少一段时间内事件的触发频率

  - 节流的应用场景

    - 鼠标连续不断地触发某事件（如点击），只在单位时间内只触发一次
    - 懒加载时要监听计算滚动条的位置，但不必每次滑动都触发，可以降低计算的频率，而不必去浪费CPU资源
    
  - 
  
  - **节流阀**的概念
  
    - 高铁卫生间是否被占用，由红绿灯控制**,红灯表示被占用，绿灯表示可使用**。
  
      假设每个人上卫生间都需要花费**5分钟**，则**五分钟之内**，被占用的卫生间无法被其他人使用。上一个人使用完毕后，需要将红灯**重置**为绿灯,表示下一个人可以使用卫生间。
  
      下一个人在上卫生间之前，需要**先判断控制灯**是否为绿色,来知晓能否上卫生间。
  
    - 节流阀为**空**，表示**可以执行下次操作**;**不为空**,表示**不能执行下次操作**。
  
    - 当前操作执行完，必须将节流阀**重置**为空，表示可以执行下次操作了
  
    - 每次执行操作前，必须**先判断节流阀是否为空**
  
    - 使用节流优化鼠标跟随效果：
  
      ```js
      $(function() {
      var angel = $('#angel')
      var timer = null        //1.预定义一个timer节流阀
      $(document).on('mousemove',function(e) {
          if(timer) {        //判断节流阀是否为空，如果不为空，则证明距离上次执行间隔不足16毫秒
               return
          }
          timer = setTimeout(function() {
              $(angel).css('left',e.pageX + 'px').css('top',e.pageY + 'px')
              timer = null          //2.当设置了鼠标跟随效果后，请清空timer节流阀，方便下次开启延时器
          }，16)
      })
      })
      ```
  
  - 总结防抖和节流的区别
  
    - 防抖：如果事件被频繁触发，防抖能保证**只有最后一次触发生效**。前面N多次的触发都会被忽略。
    - 节流：如果事件被频繁触发，节流能够**减少事件被触发的频率**，因此，节流是**有选择地执行**一部分事件