## 初识Node.js

- 什么是Node.js

  - **Node.js是**一个基于Chrome V8引擎的**JavaScript运行环境**
  - Node.js的官网地址：https://nodejs.rog/zh-cn/
  - **浏览器**是JavaScript的**前端运行环境**，而**Node.js**是Ja-vaScript的**后端运行环境**
  - Node.js中**无法调用**DOM和BOM等**浏览器内置API**

- Node.js可以做什么

  - Node.js作为一个JavaScript的运行环境，仅仅提供了基础的功能和API。然而，基于Node.js提供的这些基础功能，很多强大的工具和框架如雨后春笋，层出不穷，所以学会了Node.js，可以让前端程序员胜任更多的工作和岗位
  - 基于Express框架（http://www.expressjs.com.cn/），可以快速构建Web应用
  - 基于Electron框架（https://electronjs.org/），可以构建跨平台的桌面应用
  - 基于restify框架(http://restify.com/)，可以快速构建API接口项目
  - 读写和操作数据库、创建实用的命令行工具辅助前端开发……

- 什么是终端

  - 终端（英文：Terminal）是专门为开发人员设计的，**用于实现人机交互**的一种方式

  - 终端中的快捷键

    在Windows的powershell或cmd终端中，我们可以通过如下快捷键，来提高终端的操作效率：

    - 使用**↑**键，可以快速定位到上一次执行的命令
    - 使用**tab**键，能够快速补全路径
    - 使用**esc**键，能够快速清空当前已输入的命令
    - 输入**cls**命令，可以清空终端

- 在Node.js环境中执行JavaScript代码

  - 打开终端（win+R输入cmd打开cmd终端       或者         在代码当前页面shift+鼠标右键打开PowerShell终端）
  - 输入node要执行的js文件的路径（cd 路径 可以改变当前路径）

- fs文件系统模块

  - **fs模块**是Node.js官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。

  - fs.readFile()方法，用来**读取**指定文件中的内容

    - 语法格式：

      ```js
      fs.readFile(path[,options],callback);
      ```

    - 参数解读：

      - 参数1(path)：**必选**参数，字符串，表示文件的路径
      - 参数2(options)：可选参数，表示以什么编码格式来读取文件
      - 参数3(callback)：**必选**参数，文件读取完成后，通过回调函数拿到读取的结果

    - 实例代码：

      ```js
      //以utf8的编码格式，读取指定文件的内容，并打印err和dataStr的值
      const fs = require('fs');
      fs.readFile('./files/11.txt','utf8',function(err,dataStr) {
          console.log(err);                   //读取失败的结果
          console.log('-------');
          console.log('dataStr');             //读取成功的结果
      });
      ```

    - 如果读取成功，则err的值为null，dataStr的值为读取成功的结果

    - 如果读取失败，则err的值为错误对象，dataStr的值为undefined

    - 判断文件是否读取成功

      - 可以判断err对象是否为null，从而知晓文件读取的结果

        ```js
        const fs = require('fs');
        fs.readFile('./files/1.txt','utf8',function(err,result) {
            if(err) {
                return console.log('文件读取失败' + err.message)
            }
            console.log('文件读取成功' + result);
        })
        ```

  - fs.writeFile()方法，用来向指定的文件中**写入**内容

    - 语法格式：

    ```js
    fa.write(file,data[,option],callback);
    ```

    - 参数解读：
      - 参数1(file)：**必选**参数，需要指定一个**文件 路径的字符串**，表示文件的存放路径
      - 参数2（data）：**必选**参数，表示要写入的内容
      - 参数3（option）：可选参数，表示以什么格式写入文件内容，默认值是utf8
      - 参数4（callback）：**必选**参数，文件写入完成后的回调函数

    - 实例代码：

      ```js
      const fs = require('fs');
      fs.writefile('./files/2.txt','Hello Node.js',function(err) {
          console.log(err);
      })
      ```

    - 如果文件写入成功，则err的值为null

    - 如果文件写入失败，则err的值为一个错误对象

    - 判断文件是否写入成功

      ```js
      const fs = require('fs');
      fs.writeFile('F://files/2.txt','Hello Node.js! ',function(err) {
           if(err) {
                return console.log('文件写入失败！ ' + err.message);
           }
          console.log('文件写入成功！ ');
      })
      ```

  - 如果要在JavaScript代码中使用fs模块来操作文件，则需要使用如下的方式先导入它：

    ```js
    const fs = require('fs');
    ```

  - fs模块-路径动态拼接的问题

    - 在使用fs模块操作文件时，如果提供的操作路径是以./或../开头的**相对路径**时，很容易出现路径动态拼接错误的问题

    - 原因：代码在运行的时候，**会以执行node命令时所处的目录**，动态拼接出被操作文件的完整路径

    - 解决方案1：在使用 fs模块操作文件时，**直接提供完整的路径**，不要提供./或../开头的相对路径，从而防止路径动态拼接的问题

    - 解决方案2：**__dirname**表示当前文件所处的目录

      ```js
      fs.readFile(__dirname + '/files/1.txt','utf8',function(err,dataStr) {
          if(err) {
               return console.log('读取文件失败！ ' + err.message);
          }
          console.log('读取文件成功！ ' + dataStr);
      })
      ```

      

- path路径模块

  - **path模块**是Node.js官方提供的、用来**处理路径**的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求

  - 例如：

    - path.join()方法，用来**将多个路径片段拼接成一个完整的路径字符串**

      - 语法格式：

        ```js
        path.join([...paths]);
        ```

      - 参数解读：

        - ...path<string>路径片段的序列
        - 返回值：<string>

      - 代码示例：

        ```js
        const pathStr = path.join('/a','/b/c','../','/d','e');                 //  ../会抵消前一个路径
        console.log('pathStr');         //输出\a\b\d\e
        
        const pathStr2 = path.join(__dirname,'./files/1.txt');
        console.log('pathStr2');      //输出 当前文件所处目录\files\1.txt
        ```

      - 注意：**今后凡是涉及到路径拼接的操作，都要使用path.join()方法进行处理。**不要直接使用 + 字符进行字符串的拼接

    - path.extname()方法，用来获取路径中扩展名部分

      - 语法格式：

        ```js
        path.extname(path);
        ```

      - 参数解读：

        - path<string>必选参数，表示一个路径的字符串
        - 返回：<string>返回得到的扩展名字符串

      - 代码示例：

        ```js
        const fpath = '/a/b/c/index.html';            //路径字符串
        const fext = path.extname(fpath);
        console.log(fext);         //输出 .html
        ```

        

    - path.basename()方法，用来从路径字符串中，将文件名解析出来

      - 使用path.basename()方法，可以获取路径中的最后一部分，经常通过这个方法获取路径中的文件名

      - 语法格式：

        ```js
        path.basename(path[,ext]);
        ```

      - 参数解读：

        - path<string>必选参数，表示一个路径的字符串
        - ext<string>可选参数，表示文件扩展名
        - 返回：<string>表示路径中的最后一部分

      - 代码示例：

        ```js
        const fpath = '/a/b/c/index.html';            //文件的存放路径
        var fullName = path.basename(fpath);
        console.log(fullName);      //输出index.html
        var nameWithoutExt = path.basename(fpath,'.html');
        console.log(nameWithoutExt)        //输出index
        ```

        

  - 如果要在JavaScript的代码中，使用path模块来处理路径，则需要使用如下的方式先导入它：

    ```js
    const path = require('path');
    ```

  - 案例格式

    - 步骤1：导入需要的模块并创建正则表达式

      ```js
      // 1.1  导入fs文件系统模块
      const fs = require('fs');
      // 1.2 导入path路径处理模块
      const path = require('path');
      
      // 1.3 匹配<style></style>标签的正则
      //其中\s表示空白字符；\S表示非空白字符；*表示匹配任意次
      const regStyle = /<style>[\s\S]*<\/style>/
      // 1.4 匹配<script></script>标签的正则
      const regScript = /<script>[\s\S]*<\/script>/
      ```

    - 步骤2：使用fs模块读取需要被处理的HTML文件

      ```js
      // 2.1 读取需要被处理的HTML文件
      fs.readFile(path.join(__dirname,'../素材/index.html'),'utf8',(err,dataStr) => {
          // 2.2 读取HTML文件失败
          if(err) {
              return console.log('读取HTML文件失败 ' + err.message);
          }
          // 2.3 读取HTML文件成功后，调用对应的方法，拆解出 CSS、js和html文件
          resolveCSS(dataStr);
          resolveJS(dataStr);
          resolveHTML(dataStr);
      })
      ```

    - 步骤3：自定义resolveCSS方法

      ```js
      // 3.1 处理CSS样式
      function resolveCSS(htmlStr) {
          // 3.2 使用正则提取页面中的<style></style?标签
          const r1 = regStyle.exec(htmlStr);
          // 3.3 将提取出来的样式字符串，做进一步的处理
          const newCSS = r1[0].replace('<style>','').replace('</style>','');
          // 3.4 将提取出来的CSS样式，写入到index.css 文件中
          fs.writeFile(path.join(__dirname,'./clock/index.css'),newCSS,err => {
              if(err) {
                  return console.log('写入CSS样式失败 ' + err.message);
              }
              consloe.log('写入CSS样式成功 ')；
          })
      }
      ```

    - exec()方法用于检索字符串中的正则表达式的匹配

      - 返回一个数组，其中存放匹配的结果，如果未找到匹配，则返回值为null

    - 步骤4：自定义resolveJS方法

      ```js
      // 4.1 处理JS脚本
      function resolveJS(htmlStr) {
          // 4.2 使用正则提取页面中的<script></script>标签
          const r2 = regScript.exec(htmlStr);
          // 4.3 将提取出来的脚本字符串，做进一步的处理
          const newJS = r2[0].replace('<script>','').replace('</script>','');
          // 4.4 将提取出来的JS脚本，写入到index.js文件中
          fs.writeFile(path.join(__dirname,'./clock/index.js'),newJS,err => {
              if(err) {
                  return console.log('写入JavaScript脚本失败 ' + err.message);
              }
              console.log('写入js脚本成功 ');
          })
      }
      ```

    - 步骤5：自定义resolveHTML方法

      ```js
      // 5. 处理html文件
      function resolveHTML(htmlStr) {
          // 5.1 使用字符串中的replace方法，将内嵌的<style>和<script>标签，替换为外联的<link>和<script>标签
          const newHTML = htmlStr.replace(regStyle,'<link rel="stylesheet" herf="./index.css"/>')
                                 .replace(regScript,'<script src="./index.js"></script>');
          // 5.2 将替换完成之后的html代码，写入到index.html文件中
          fs.writeFile(path.join(__dirname,'./clock/index.html'),newHTML,err => {
              if(err) {
                  return console.log('写入HTML文件失败 ' + err.message);
              }
              console.log('写入HTML页面成功 ')；
          })
      }
      ```

    - 案例的两个注意点

      - fs.writeFile()方法只能用来创建文件，不能用来创建路径

      - 重复调用fs.writeFile()写入同一个文件，新写入的内容会覆盖之前的旧内容

        

- http模块

  - 什么是客户端，什么是服务器？

    在网络节点中，负责消费电脑资源的电脑，叫做客户端；**负责对外提供网络资源**的电脑，叫做服务器。

  - **http模块**是Node.js官方提供的、用来**创建web服务器**的模块。通过http模块提供的<u>http.createServer()</u>方法，就能方便的把一台普通的电脑，变成一台Web服务器，从而对外提供Web资源服务

  - 如果要使用http模块创建Web服务器，需要先导入：

    ```js
    const http = require('http');
    ```

  - 服务器和普通电脑的**区别**在于，服务器上安装了**web服务器软件**，例如：IIS、Apache等。通过安装这些服务器软件，就能把一台普通的电脑变成一台web服务器。

    在Node.js中，我们不需要使用IIS、Apache等这些第三方web服务器软件，因为我们可以基于Node.js提供的http模块，**通过几行简单的代码，就能轻松的手写一个服务器软件**，从而对外提供web服务

  - 服务器相关的概念

    - IP地址
      - **IP地址**就是互联网上**每台计算机的唯一地址**，因此IP地址具有唯一性。如果把“个人电脑”比作“一台电话”，那么“IP地址”就相当于“电话号码”，只有在知道对方IP地址的前提下，才能与对应的电脑之间进行数据通信
      - IP地址的格式：通常用“**点分十进制**”表示成（a.b.c.d）的形式，其中，a，b，c，d都是0--255之间的十进制数。例如：用点分十进制表示的IP地址（192.168.1.1)
      - 注意：
        - **互联网中每台Web服务器，都有自己的IP地址**，例如：大家可以在Windows的终端中运行ping www.baidu.com命令，即可查看到百度服务器的IP地址
        - 在开发期间，自己的电脑既是一台服务器，也是一个客户端，为了方便测试，可以在自己的浏览器中输入127.0.0.1这个IP地址，就能把自己的电脑当作一台服务器进行访问了
    - **域名**和**域名服务器**
      - 尽管IP地址能够唯一地标记网络上的计算机，但IP地址是一长串数字，不直观，而且不便于记忆，于是人们又发明了另一套**字符型**的**地址方案**，即所谓的**域名（Domain Name）地址**
      - **IP地址**和**域名**是**一一对应的关系**，这份对应关系存放在一种叫域名服务器（DNS）的电脑中。使用者只需通过好记的域名访问对应的服务器即可，对应的转换工作由域名服务器实现。因此，**域名服务器就是提供IP地址和域名之间的转换服务的服务器。**
      - 注意：
        - 单纯使用IP地址，互联网中的电脑也能够正常工作，但是有了域名的加持，能让互联网的世界变得更加方便
        - 在开发测试期间，**127.0.0.1** 对应的域名是 **localhost**，它们都代表我们使用的这台电脑，在使用效果上没有任何区别
    - 端口号
      - 计算机中的端口号，就好像是现实生活中的门牌号一样。通过门牌号，外卖小哥可以在整栋大楼众多的房间中，准确的把外卖送到你的手中
      - 同样的道理，在一台电脑中，可以运行成百上千个web服务。每个web服务都对应一个唯一的端口号。客户端发送过来的网络请求，通过端口号，可以被准确地交付给**对应的web服务**进行处理
      - 注意：
        - 每个端口号不能同时被多个web服务占用
        - 在实际应用中，URL中的**80端口可以被省略**

  - 创建最基本的web服务器

    - 基本步骤：

      - 导入http模块
      - 创建web服务器实例
      - 为服务器实例绑定**request**事件，**监听客户端的请求**
      - 启动服务器

    - 步骤1：导入http模块

      - ```js
        const http = require('http');
        ```

    - 步骤2：创建web服务器实例

      - 调用 http.createServer()方法，即可以快速创建一个web服务器实例：

      - ```js
        const server = http.createServer();
        ```

    - 步骤3：为服务器实例绑定request事件

      - 即可监听服务端发送过来的网络请求

      - ```js
        // 使用服务器实例的 .on()方法，为服务器绑定一个request事件
        server.on('request',(req,res) => {
            // 只要有客户端来请求我们自己的服务器，就会触发request事件，从而调用这个事件处理函数
            console.log('Someone visit our web server.');
        })
        ```

    - 步骤4：启动服务器

      - 调用服务器实例的 .listen() 方法，即可启动当前的web服务器实例

      - ```js
        // 调用server.listen(端口号，cb回调)方法，即可启动当前的web服务器实例
        server.listen(80,() => {
            console.log('http server running at http://127.0.0.1');
        })
        ```

        

    - **req**请求对象

      - 只要服务器接收到了客户端的请求，就会调用通过**server.on()** 为服务器绑定的**request事件处理函数**

      - 如果想在事件处理函数中，访问与客户端相关的**数据**或**属性**，可以使用如下方式：

        - ```js
          server.on('request',(req) => {
              //req是请求对象，它包含了与客户端相关的数据和属性，例如：
              //req.url是客户端请求的URL地址
              //req.method是客户端的method请求类型
              const st = 'Your request url is ${req.url}, and request method is ${req.method}';
              console.log(str);
          })
          ```

    - **res**响应对象

      - 在服务器的request事件处理函数中，如果项访问与服务器相关的数据或属性，可以采用如下方式：

        - ```js
          server.on('request', (req,res) => {
              //res是响应对象，它包含了与服务器相关的数据和属性，如：
              // 要发送到客户端的字符串
              const str = 'Your request url is ${req.url}, and request method is ${req.method}'
              //res.end()方法的作用：向客户端发送指定的内容，并结束这次请求的处理过程
              res.end(str);
          })
          ```

    - **<u>解决中文乱码问题</u>**

      - 当调用res.end()方法，向客户端发送中文内容的时候，会出现乱码问题，此时，需要**手动设置内容的编码格式**：

      - ```js
        server.on('request', (req,res) => {
            //发送的内容包含中文
            const str = '您请求的url地址是 ${req.url}，请求的method类型是 ${req.method}';
            //为了防止中文显示乱码的问题，需调用rex.setHeader()方法，设置响应头 Content-Type 的值为 text/html:charset=utf-8
            res.setHeader('Content-Type','text/html;charset=utf-8');
            //把包含中文的内容，响应给客户端
            res.end(str);
        })
        ```

        

  - 根据不同的url响应不同的html内容

    - 核心实现步骤：

      - 获取**请求的url地址**
      - 设置**默认的响应内容**为404 Not found
      - 判断用户请求的是否为**/**或**/index.html**首页
      - 判断用户请求的是否为**/about.html**关于页面
      - 设置**Content-Type响应头**，防止中文乱码
      - 使用**res.end()**方法把内容响应给客户端

    - 动态响应内容：

      - ```js
        const http = require('http')
        const server = http.createServer()
        server.on('request', function(req,res) {
            const url = req.url;                    //1.获取请求的url地址
            let content = '<h1>404 Not found!</h1>';            //2.设置默认的内容为404 Not found
            if(url === '/' || '/index.html') {
                content = '<h1>首页</h1>'                    //3.用户请求的是首页
            } else if(url === '/about.html') {
                content = '<h1>关于页面</h1>'                 //4.用户请求的是关于页面
            }
            res.setHeader('Content-Type','text/html;charset=utf-8')       //5.设置Content-Type响应头，防止中文乱码
            res.end(content)                         //把内容发送给客户端
        })
        server.listen(80, () => {
            console.log('server running at http://127.0.0.1')
        })
        ```

        

  - 案例格式：

    - 核心思路：

      - 把文件的实际存放路径，作为每个资源请求的url地址
    
- 实现步骤：
    
  - 步骤1：导入需要的模块
    
    - ```js
          // 1.1导入http模块
          const http = require('http')
          // 1.2导入fs系统模块
          const fs = require('fs')
          // 1.3导入path路径处理模块
          const path = require('path')
          ```
    
  - 步骤2创建基本的web服务器
    
    - ```js
          // 2.1 创建web服务器
          const server = http.createServer();
          // 2.2 监听web服务器的request事件
          server.on('request',function (req,res) {})
          // 2.3 启动当前的web服务器
          server.listen(80,function() {
              console.log('server listen at http://127.0.0.1');
          })
          ```
    
  - 步骤3：将资源的请求url地址映射为文件的存放路径
    
    - ```js
          // 3.1 获取到客户端请求的url地址
          const url = req.url
          // 3.2 把请求的url地址，映射为本地文件的存放路径
          const fpath = path.join(__dirname,url)
          ```
    
  - 步骤4：读取文件内容并响应给客户端
    
    - ```js
          // 4.1 根据“映射”过来的文件路径读取文件
          fs.readFile(fpath,'utf8',(err,dataStr) => {
              // 4.2 读取文件失败后，向客户端响应固定的错误信息
              if(err) {
                  return res.end('404 Not found')
              }
              // 4.3 读取文件成功后，将读取成功的内容响应给客户端
              res.end(dataStr)
          })
          ```
    
  - 步骤5：优化资源的请求路径
    
    - ```js
          // ***将3.2的实现方式，改为如下代码：***
          // 5.1 预定义空白的文件存放路径
          let fpath = ''
          if(url === '/') {
              // 5.2 如果请求的路径是否为/，则手动指定文件的存放路径
              fpath = path.join(__dirname,'./clock/index.html')
          } else {
              // 5.3 如果请求的路径不为 / ,则动态拼接文件的存放路径
              fpath = path.join(__dirname,'./clock',url)
          }
          ```
    
    