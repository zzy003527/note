# HTTP

## Readystate和status的区别

- `onreadystatechange` 是一个事件处理程序属性，可以在XMLHttpRequest对象上设置，当XMLHttpRequest对象的`readyState`属性发生变化时，将调用该方法。

- `readyState` 属性是XMLHttpRequest对象的只读属性，它表示XMLHttpRequest对象的状态。当XMLHttpRequest对象状态发生变化时，`readyState`属性的值会发生相应的变化。

  readyState总共有5个状态值，分别为0~4，每个值代表了不同的含义，如下表所示：

  | 0    | 未初始化状态：此时，已经创建了一个XMLHttpRequest对象         |
  | ---- | ------------------------------------------------------------ |
  | 1    | 准备发送状态：此时，已经调用了XMLHttpRequest对象的open方法，并且XMLHttpRequest对象已经准备好将一个请求发送到服务器端 |
  | 2    | 已经发送状态：此时，已经通过send方法把一个请求发送到服务器端，但是还没有收到一个响应 |
  | 3    | 正在接收状态：此时，已经接收到HTTP响应头部信息，但是消息体部分还没有完全接收到 |
  | 4    | 完成响应状态：此时，已经完成了HTTP响应的接收                 |

  

- status是XMLHttpRequest对象的一个属性，表示响应的HTTP状态码。

  在HTTP1.1协议下，HTTP状态码总共可分为5大类，如下表所示：

  | 1XX  | 服务器收到请求，需要继续处理。例如101状态码，表示服务器将通知客户端使用更高版本的HTTP协议。 |
  | ---- | ------------------------------------------------------------ |
  | 2XX  | 请求成功。例如200状态码，表示请求所希望的响应头或数据体将随此响应返回。 |
  | 3XX  | 重定向。例如302状态码，表示临时重定向，请求将包含一个新的URL地址，客户端将对新的地址进行GET请求。 |
  | 4XX  | 客户端错误。例如404状态码，表示客户端请求的资源不存在。      |
  | 5XX  | 服务器错误。例如500状态码，表示服务器遇到了一个未曾预料的情况，导致了它无法完成响应，一般来说，这个问题会在程序代码出错时出现。 |

  - ```sql
    1xx：信息响应类，表示接收到请求并且继续处理
    2xx：处理成功响应类，表示动作被成功接收、理解和接受
    3xx：重定向响应类，为了完成指定的动作，必须接受进一步处理
    4xx：客户端错误，客户请求包含语法错误或者是不能正确执行
    5xx：服务端错误，服务器不能正确执行一个正确的请求
     
    100——客户必须继续发出请求
    101——客户要求服务器根据请求转换HTTP协议版本
    200——交易成功
    201——提示知道新文件的URL
    202——接受和处理、但处理未完成
    203——返回信息不确定或不完整
    204——请求收到，但返回信息为空
    205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
    206——服务器已经完成了部分用户的GET请求
    300——请求的资源可在多处得到
    301——删除请求数据
    302——在其他地址发现了请求数据
    303——建议客户访问其他URL或访问方式
    304——客户端已经执行了GET，但文件未变化
    305——请求的资源必须从服务器指定的地址得到
    306——前一版本HTTP中使用的代码，现行版本中不再使用
    307——申明请求的资源临时性删除
    400——错误请求，如语法错误
    401——请求授权失败
    402——保留有效ChargeTo头响应
    403——请求不允许
    404——没有发现文件、查询或URl
    405——用户在Request-Line字段定义的方法不允许
    406——根据用户发送的Accept拖，请求资源不可访问
    407——类似401，用户必须首先在代理服务器上得到授权
    408——客户端没有在用户指定的饿时间内完成请求
    409——对当前资源状态，请求不能完成
    410——服务器上不再有此资源且无进一步的参考地址
    411——服务器拒绝用户定义的Content-Length属性请求
    412——一个或多个请求头字段在当前请求中错误
    413——请求的资源大于服务器允许的大小
    414——请求的资源URL长于服务器允许的长度
    415——请求资源不支持请求项目格式
    416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
    417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求
    500——服务器产生内部错误
    501——服务器不支持请求的函数
    502——服务器暂时不可用，有时是为了防止发生系统过载
    503——服务器过载或暂停维修
    504——关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长
    505——服务器不支持或拒绝支请求头中指定的HTTP版本
    ```

- #### 为什么`onreadystatechange`的函数实现要同时判断`readyState`和`status`呢？

  - **第一种思考方式：只使用`readyState`判断**

    ```js
    const xhr = createXHR();
    xhr.onreadystatechange = () => {
          if (xhr.readyState === 4) {
             alert(xhr.responseText);
          } else {
             alert('Request was unsuccessful:'+xhr.status);
          }
    };
    xhr.open('get','http://120.78.164.247:9999/user/findAll', false);
    xhr.send(null);
    ```

    服务响应出错了，但还是返回了信息，这并不是我们想要的结果。如果返回不是`200`，而是`404`或者`500`，由于只使用`readystate`做判断，它不理会放回的结果是`200`、`404`还是`500`，只要响应成功返回了，就执行接下来的`javascript`代码，结果将造成各种不可预料的错误。所以只使用`readyState`判断是行不通的。

  - **第二种思考方式：只使用`status`判断**

    ```js
    const xhr = createXHR();
    xhr.onreadystatechange = () => {
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
             alert(xhr.responseText);
          }else {
             alert('Request was unsuccessful:'+xhr.status);
          }
    };
    xhr.open('get','http://120.78.164.247:9999/user/findAll', false);
    xhr.send(null);
    ```

    事实上，结果并不像预期那样。响应码确实是返回了`200`，但是总共弹出了`3`次窗口！第一次是`readyState=2`的窗口，第二次是`readyState=3`的窗口，第三次是`readyState=4`的窗口。由此，可见`onreadystatechange`函数的执行不是只在`readyState`变为`4`的时候触发的，而是`readyState(2、3、4)`的每次变化都会触发，所以就出现了前面说的那种情况。可见，单独使用`status`判断也是行不通的。

    





## AJAX原理和axios、fetch区别

- **JQuery Ajax**

  > `Ajax`前后端数据通信「同源、跨域」

  ```js
  // 用户登录 -> 登录成功 -> 获取用户信息
  /* 回调地狱 */
  $.ajax({
      url: 'http://127.0.0.1:8888/user/login',
      method: 'post',
      data: Qs.stringify({
          account: '18310612838',
          password: md5('1234567890')
      }),
      success(result) {
          if (result.code === 0) {
              // 登录成功
              $.ajax({
                  url: 'http://127.0.0.1:8888/user/list',
                  method: 'get',
                  success(result) {
                      console.log(result);
                  }
              });
          }
      }
  });
  ```

  优缺点：

  - 本身是针对`MVC`的编程,不符合现在前端`MVVM`的浪潮
  - 基于原生的`XHR`开发，`XHR`本身的架构不清晰，已经有了`fetch`的替代方案
  - `JQuery`整个项目太大，单纯使用`ajax`却要引入整个`JQuery`非常的不合理（采取个性化打包的方案又不能享受CDN服务）

- **Axios**

  > `Axios`也是对`ajax`的封装，基于`Promise`管理请求，解决回调地狱问题

  ```js
  axios({
      method: 'post',
      url: '/user/login',
      data: {
          username: 'name',
          password: 'password'
      }
  })
  .then(function (response) {
      console.log(response);
  })
  .catch(function (error) {
      console.log(error);
  });
  // 或使用 async await
  (async function () {
      let result1 = await axios.post('/user/login', {
          username: 'name',
          password: 'password'
      });
      let result2 = await axios.get('/user/list');
      console.log(result1, result2);
  })(); 
  ```

  优缺点：

  - 从浏览器中创建 `XMLHttpRequest`
  - 从 `node.js` 发出 `http` 请求
  - 支持 `Promise API`
  - 拦截请求和响应
  - 转换请求和响应数据
  - 取消请求
  - 自动转换`JSON`数据
  - 客户端支持防止`CSRF/XSRF`

- **Fetch**

  > `Fetch`是`ES6`新增的通信方法，不是`ajax`，但是他本身实现数据通信，就是基于`promise`管理的

  ```js
  try {
    let response = await fetch(url, options);
    let data = response.json();
    console.log(data);
  } catch(e) {
    console.log("Oops, error", e);
  }
  ```

  示例：

  ```js
  (async function () {
      let result = await fetch('http://127.0.0.1:8888/user/login', {
          method: 'post',
          headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
          },
          body: Qs.stringify({
              name: 'name',
              password: 'password'
          })
      })
      let data = result.json();
      console.log(data)
  
      let result2 = await fetch('http://127.0.0.1:8888/user/list').then(response => {
          return response.json();
      });
      console.log(result2);
  })(); 
  ```

  优缺点：

  - **`fetch`只对网络请求报错，对`400`，`500`都当做成功的请求，需要封装去处理**
  - **`fetch`默认不会带`cookie`，需要添加配置项**
  - **`fetch`不支持`abort`，不支持超时控制，使用`setTimeout`及`Promise.reject`的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费**
  - **`fetch`没有办法原生监测请求的进度，而XHR可以**







## 从输入url到页面展示，这中间发生了什么

- 用户输入url并回车
- 浏览器进程检查url，组装协议，构成完整的url
- 浏览器进程通过进程间通信（IPC）把url请求发送给网络进程
- 网络进程接收到url请求后检查本地缓存是否缓存了该请求资源，如果有则将该资源返回给浏览器进程
- 如果没有，网络进程向web服务器发起http请求（网络请求），请求流程如下：
  - 进行DNS解析，获取服务器ip地址，端口（端口是通过dns解析获取的吗？这里有个疑问）
  - 利用ip地址和服务器建立tcp连接
  - 构建请求头信息
  - 发送请求头信息
  - 服务器响应后，网络进程接收响应头和响应信息，并解析响应内容
- 网络进程解析响应流程
  - 检查状态码，如果是301/302，则需要重定向，从Location自动中读取地址，重新进行第4步 （301/302跳转也会读取本地缓存吗？这里有个疑问），如果是200，则继续处理请求。
  - 200响应处理：检查响应类型Content-Type，如果是字节流类型，则将该请求提交给下载管理器，该导航流程结束，不再进行后续的渲染，如果是html则通知浏览器进程准备渲染进程准备进行渲染。
- 准备渲染进程
  - 浏览器进程检查当前url是否和之前打开的渲染进程根域名是否相同，如果相同，则复用原来的进程，如果不同，则开启新的渲染进程
- 传输数据、更新状态
  - 渲染进程准备好后，浏览器向渲染进程发起“提交文档”的消息，渲染进程接收到消息和网络进程建立传输数据的“管道”
  - 渲染进程接收完数据后，向浏览器发送“确认提交”
  - 浏览器进程接收到确认消息后更新浏览器界面状态：安全、地址栏url、前进后退的历史状态、更新web页面





## cors跨域

- #### 为什么会有跨域的问题？

  为了保证用户信息的安全，所有的浏览器都遵循同源策略，那什么情况下算同源呢？同源策略又是什么呢？ **记住：协议、域名、端口号完全相同时，才是同源**

  可以参考 [Web安全 - 浏览器的同源策略](https://link.zhihu.com/?target=https%3A//developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

  在同源策略下，会有以下限制：

  - 无法获取非同源的 Cookie、LocalStorage、SessionStorage 等
  - 无法获取非同源的 dom
  - 无法向非同源的服务器发送 ajax 请求
- 跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器 让运行在一个 origin (domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。 
- 而CORS 允许浏览器向跨源服务器，发出跨域请求，从而克服了AJAX只能同源使用的限制。 
- CORS是一个W3C标准，它同时需要浏览器和服务端的支持，浏览器基本都支持，因此，想要实现CORS通信，只要服务器实现了CORS接口即可
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- 
- d
- 