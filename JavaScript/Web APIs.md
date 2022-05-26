#  Web APIs

### 基本概念

- JS的组成

  - 1.ECMAScript：JavaScript语法

    2.Web APIs：DOM（页面文档对象模型）、BOM（浏览器对象模型）

- API和Web API

  - API（应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源代码，或理解内部工作机制的细节
  - 即：API是给程序员提供的一种工具，以便能更轻松的实现想完成的功能
  - **Web API是浏览器**提供的一套操作**浏览器功能**和**页面元素**的API(BOM和DOM)

### DOM

- 什么是DOM

  - 文档对象模型（Document Object Model,简称**DOM**），是W3C组织推荐的处理可扩展标记语言（HTML或者XML）的**标准编程接口**

- DOM树

  - ![QQ图片20211215121307](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211215121307.png)
  - 文档：一个页面就是一个文档，DOM中使用document表示
  - 元素：页面中的所有标签都是元素，DOM中使用element表示
  -   节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM中使用node表示
  - **DOM把以上内容都看作是对象**

- 如何获取页面元素

  - 根据ID获取

    - 使用getElementById（）方法可以获取带有ID 的元素

    - ```
      <div id='time'>2019-9-9<div>
      var timer = document.getElementById('time');
      console.log(timer);
      ```

    - 参数id是大小写敏感的字符串

    - 返回的是一个元素对象

    - **console.dir 打印我们返回的元素对象，更好的查看里面的属性和方法**

  - 根据标签名获取

    - 使用getElementsByTagName()方法可以返回带有指定标签名的**对象的集合**,以伪数组的形式存储

    - ```
      document.getElementsByTagName('标签名');
      ```

    - 还可以获取某个父元素内部所有指定标签名的子元素

      ```
      element.getElementsByTagName('标签名')；
      
      var ol = document.getElementsById('ol');
      console.log(ol.getElementsByTagName('li'));
      ```

      注意：父元素必须是单个对象（必须指名是哪一个元素对象，获取时不包括父元素自己）

  - 通过HTML5新增的方法获取

    - document.getElementsByClassName('类名')； //根据类名返回元素对象集合
    - document.querySelector('选择器')； //根据指定选择器返回第一个元素对象
      - **切记里面的选择器需要加符号， .box、#id**
    - document.querySelectorAll('选择器')； //根据指定选择器返回

  - 获取特殊元素

    - 获取body标签

      document .body

    - 获取html元素

      document.documentElement

- 事件基础

  - JavaScript使我们有能力创建动态页面，而事件是可以被JavaScript侦测到的行为。简单理解为：触发——响应机制

  - 网页中每个元素都可以触发JavaScript的事件，如：我们可以在用户点击某按钮时产生一个事件，然后去执行某些操作

  - 事件三要素：事件源，事件类型，事件处理程序

    - 事件源：事件被触发的对象（谁）
    - 事件类型：如何触发，什么事件，比如鼠标点击（onclick），还是鼠标经过，还是鼠标按下
    - 事件处理程序：通过一个函数赋值的方式完成

  - 执行事件的步骤

    - 获取事件源
    - 注册事件（绑定事件）
    - 添加事件处理程序（采取函数赋值形式）

  - 常见的鼠标事件

    | 鼠标事件    | 触发条件         |
    | ----------- | ---------------- |
    | onclick     | 鼠标点击左键触发 |
    | onmouseover | 鼠标经过触发     |
    | onmouseout  | 鼠标离开触发     |
    | onfocus     | 获得鼠标焦点触发 |
    | onblur      | 失去鼠标焦点触发 |
    | onmousemove | 鼠标移动触发     |
    | onmouseup   | 鼠标弹起触发     |
    | onmousedown | 鼠标按下触发     |

- 操作元素

  - JavaScript的DOM操作可以改变网页内容、结构和样式，我们可以利用DOM操作元素来改变元素里面的内容、属性等。

  - 改变元素内容

    - element.innerText

      从起始位置到终止位置的内容，但它去除html标签，同时**空格和换行也会去掉**

    - elememt.innerHTML

      起始位置到终止位置的全部内容，包括html标签，同时保留空格和换行

    - innerText和innerHTML的区别

      - innerText不识别html标签，去除空格和换行

      - innerHTML识别html标签,保留换行和空格

      - 这两个属性是可读写的，可以获取元素里面的内容

      - ```
        var p = document.querySeletor('p');
        console.log(p.innerText);
        ```

  - 修改元素属性

    - 1.获取元素 2.注册事件

    - ```
       <button id="yin">鄞灿</button>
          <button id="me">我</button>
          <img src="./zhu.jfif" alt="">
          <script>
              //获取元素
              var yin = document.getElementById('yin');
              var me = document.getElementById('me');
              var img = document.querySelector('img');
              //注册事件
              me.onclick = function() 
              {
                  img.src = './peng.jfif';
              }
              yin.onclick = function() 
              {
                  img.src = './zhu.jfif';
              }
      ```

  - 表单元素的属性操作

    - 利用DOM可以操作以下表单元素属性：type、value、checked、selected、disabled

    - ```
        <button>按钮</button>
          <input type="text" value="输入内容">
          <script>
              var btn = document.querySelector('button');
              var input = document.querySelector('input');
              btn.onclick = function() {
                  input.value = '被点击了';
                  // btn.disabled = true;
                  this.disabled = true;
                  // this指向的是事件函数的调用者
              }
      ```

    - **this指向事件函数的调用者**

  - 样式属性操作

    - element.style 行内样式操作

      ```
       <div></div>
          <script>
              var div = document.querySelector('div');
              div.onclick = function() {
                  this.style.backgroundColor = 'purple';
              }
          </script>
      元素.style.属性
      ```

      - 注意：1.JS里面的样式采用驼峰命名法，如：fontSize, backgroundColor

         2.JS修改style样式操作，产生的是行内样式，**css权重比较高**

    - element.className 类名样式操作

      ```
      <style>
              div {
                  width: 100px;
                  height: 100px;
                  background-color: pink;
              }
              .change {
                  background-color: purple;
                  color: #fff;
                  font-size: 25px;
                  margin-top: 100px;
              }
          </style>
      </head>
      <body>
          <div>文本</div>
          <script>
              var test = document.querySelector('div');
              test.onclick = function() {
                  this.className = 'change';
              }
          </script>
      </body>
      ```

      - 注意：1.如果样式修改较多，可以采取操作类名方式更改元素样式

         2.class因为是个保留字，因此使用className来操作元素类名属性

         3.**className会直接更改元素的类名，会覆盖原先的类名，若要保留，则写**

         **元素.className = '原先类名 更改后类名'**

  - 排他思想

    - 如果有同一组元素，我们想要某一个元素实现某种样式，需要用到循环的排他思想

    - 步骤：1.所有元素全部清除样式（干掉其他人） 2.给当前元素设置样式（留下我自己） 3.注意顺序不能颠倒

    - ```
      var btns = document.getElementsByTagName('button');
              for(var i = 0; i < btns.length; i++)
              {
                  btns[i].onclick = function()
                  {
                      for(var i = 0;i < btns.length; i++)
                      {
                          btns[i].style.backgroundColor = '';
                      }
                      this.style.backgroundColor = 'pink';
                  }
              }
      ```

  - 自定义属性的操作

    - 获取属性**值**

      - element.属性 获取属性值

      - element.getAttribute('属性')

      - 区别：element.属性：获取内置属性值（元素本身自带的属性）

         **element.getAttribute('属性')； ：主要获得自定义的属性**

    - 设置属性值

      - element.属性 = ‘值’ 设置内置属性值

      - element.setAttribute(‘属性’，‘值’)；

      - 区别：element.属性 ：设置内置属性值

         element.setAttribute('属性')； ：主要设置自定义的属性

    - 移除属性

      - removeAttribute（属性）

  - H5自定义属性

    - 自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中

    - 自定义属性获取是通过getAttribute（‘属性’）获取的

    - 但是有些自定义属性很容易引起歧义，不容易判断是元素的内置属性还是自定义属性

    - 设置自定义属性

      - H5规定自定义属性data-开头做为属性名并且赋值

        如：

        

        或者用JS设置：element.setAttribute('data-index',2)

    - 获取H5自定义属性

      - 兼容性获取 element.getAttribute('data-index');
      - H5新增element.dataset.index 或者 element.dataset['index'] （ie11才开始支持）
        - 如果自定义属性里面有多个-连接的单词，获取时采取驼峰命名法
        - 只能获取data-开头的

- 节点操作

  - 为什么学节点操作

    - 利用父子兄节点关系获取元素
    - 逻辑性强，但兼容性稍差

  - 节点概述

    - 网页中所有内容都是节点（标签、属性、文本、注释等），在DOM中，节点用node来表示

    - HTML DOM树中的所有节点均可通过JavaScript进行访问，所有HTML元素（节点）均可被修改，也可以创建和删除

    - 一般的，

      节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性

      - **元素节点nodeType为1**
      - **属性节点nodeType为2**
      - **文本节点nodeType为3（文本节点包括文字、空格、换行等）**

  - 节点层级

    - 利用DOM树可以把节点划分为不同的层级关系，常见的是父子兄层级关系

    - 父级节点

      - node.parentNode
      - parentNode属性可返回某节点的父节点，注意是**最近的一个父节点**
      - 如果指定的节点没有父节点则返回NULL

    - 子节点

      - 1.parentNode.childNodes (标准)

        - parentNode.childNodes返回包含指定节点的子节点的集合，该集合为即时更新的集合
        - **注意：返回值里面的子节点包含文本节点、元素节点等**
        - 如果只想要获得里面的元素节点，则需要专门处理
        - 

        ```
        var ul = document.querySeletor('ul');
        for(var i = 0,i < ul.length;i++)
            {
                if(ul.childNodes[i].nodeType == 1)
                    {
                        console.log(ul.childNodes[i]);
                    }
            }
        ```

      - 2.**parentNode.children** (非标准)

        - parentNode.children是一个只读属性，返回**所有的子元素节点**。它只返回子元素节点，其余节点不返回

      - 3.parentNode.firstChild

        - firstChild返回第一个子节点，找不到则返回null。同样也是包含所有的节点

      - 4.parentNode.lastChild

      - 5.parentNode.firstElementChild（ie9以上支持）

        - firstElementChild返回第一个子元素节点，找不到则返回null

      - 6.parentNode.lastElementChild（ie9以上支持）

        - lastElementChild返回最后一个子元素节点，找不到则返回null

    - 兄弟节点

      - node.nextSibling

        - nextSibling返回当前元素的下一个兄弟节点，找不到则返回null。同样，也是包含所有的节点

      - node.previousSibling

        - previousSibling返回当前元素的上一个兄弟节点，找不到则返回null。同样，也是包含所有的节点

      - node.nextElementSibling（ie9以上支持）

        - nextElementSibling返回当前元素下一个**兄弟元素节点**，找不到则返回null

      - node.previousElementSibling（ie9以上支持）

        - previousElementSibling返回当前元素上一个**兄弟元素节点**，找不到则返回null

      - 自己封装一个兼容性的函数：

        ```
        function getNextElementSibling(element) {
        var el = element;
            while(el = el.nextSibling) {
                  if(el.nodeType === 1) {
                      return el;
                  }
            }
            return null;
        }
        ```

    - 创建节点

      - document.createElement('tagName')
      - document.createElement()方法创建由tagName指定的HTML元素。因为这些元素原先不存在，是根据我们的需求动态生成的，所有我们也称为动态创建元素节点

    - 添加节点

      - node.appendChild(child)

        - node.appendChild()方法将一个节点**添加到指定父节点的子节点列表末尾**。类似于css里面的after伪元素

        - node为父元素，child为子元素

        - ```
          <ul></ul>
          <script>
                  //创建节点元素节点
                  var li = document.createElement('li');
                  //添加节点node.appendChild(child)    node 父级  child是子级后面追加元素
                  var ul = document.querySelector('ul');
                  ul.appendChild(li);
          </script>
          ```

      - node.insertBefore(child,指定元素)

        - nodeinsertBefore()方法将一个节点添加到父节点的指定子节点**前面**。类似于css里面的before伪元素

        - ```
          //添加节点node.insertBefore(child,指定元素)
                  var lili = document.createElement('li');
                  ul.insertBefore(lili,ul.children[0]);
          ```

    - 删除节点

      - node.removeChild(child)

        - node.removeChild()方法从DOM中删除一个子节点，返回删除的节点

        - ```
           <button>按钮</button>
              <ul>
                  <li>熊大</li>
                  <li>熊二</li>
                  <li>光头强</li>
              </ul>
              <script>
                  var ul = document.querySelector('ul');
                  var btn = document.querySelector('button');
                  btn.onclick = function() {
                      if(ul.children.length == 0) {
                          this.disabled = true;
                      } else {
                          ul.removeChild(ul.children[0]);
                      }
                  }
              </script>
          ```

    - 复制节点（克隆节点）

      - node.cloneNode()
      - node.cloneNode()方法返回调用该方法的节点的一个副本。也成为克隆节点/拷贝节点
      - 注意：
        - 如果括号参数**为空或者为false**，则是**浅拷贝**，即只克隆复制节点本身，不克隆里面的子节点
        - 如果括号参数为**true**，则是**深度拷贝**，会复制节点本身以及里面所有的子节点。

  - 三种动态创建元素区别

    - document.write()
    - document.innerHTML
    - document.createElement()
    - 区别：
      - document.write()是直接将内容写入页面的内容流，**但是文档流执行完毕，则它会导致页面全部重绘**（重新创造一个只有write出来的元素的页面）
      - innerHTML是将内容写入某个DOM节点，不会导致页面全部重绘
      - innerHTML创建多个元素效率更高（不要拼接字符串，采取数组形式拼接），结构稍微复杂
      - createElement（）创建多个元素效率稍低一点点，但是结构更清晰

- DOM重点核心

  - 文档对象模型，是W3C组织推荐的处理可扩展标记语言的标准**编程接口**
  - W3C已经定义了一系列的DOM接口，通过这些DOM接口可以改变网页的内容、结构和样式
  - 对于JavaScript，为了能够使JavaScript操作HTML，JavaScript就有了一套自己的DOM编程接口
  - 对于HTML，DOM使得HTML形成一棵DOM树，包含文档、元素、节点

  #### 事件高级

- 注册事件概述

  - 给元素添加事件，称为注册事件或者绑定事件

  - 注册事件有两种方式：传统方式和方法监听注册方式

  - 传统注册方式

    - 利用on开头的事件，如：onclick
    - btn.onclick = function (){}
    - 特点：注册事件的唯一性
    - 同一个元素同一个事件只能设置一个处理函数，**最后注册的处理函数将会覆盖前面注册的处理函数**

  - 方法监听注册方式

    - W3C标准、推荐方式
    - addEvenListener()它是一个方法
    - IE9之前的IE不支持此方法，可用attachEvent（）代替
    - 特点：同一个元素同一个事件可用注册多个监听器
    - 按注册顺序依次执行

  - **addEventListener 事件监听方式**

    - eventTarget.addEvenListener(type,listener[, useCapture])
    - eventTarget.addEvenListener()方法将指定的监听器注册到evenTarget是（目标对象）上，当该对象触发指定的事件时，就会执行事件处理函数
    - 该方法接收三个参数：
      - type：事件类型字符串，比如click、mouseover，注意这里不要带on
      - listener：事件处理函数，事件发生时，会调用该监听函数
      - useCapture：可选参数，是一个布尔值，默认是false。**若为true，则处于捕获阶段；为false或不写，则处于冒泡阶段。**

  - attachEvent事件监听方式

    - eventTarget.attachEvent(eventNameWithOn,callback)

    - eventTarget.attachEvent()方法将指定的监听器注册到eventTarget（目标对象）上，当该对象触发指定的事件时，指定的回调函数就会被执行

    - 该方法接收两个参数：

      - eventNameWithOn：事件类型字符串，如：onclick、onmouseover，这里要带on
      - callback：事件处理函数，当目标触发事件时回调函数被调用

    - 注册事件兼容性解决方案：

      - ```
        function addEventListener(element,eventName,fn) {
        //判断当前浏览器是否支持addEventListener方法
            if (element.addEventListener) {
                element.addEventListener(eventName,fn);  //第三个参数默认是false
            }  else if (element.attachEvent) {
                element.attachEvent('on'+eventName,fn);
            } else {
                //相当于element.onclick = fn;
                element['on' + eventName] = fn;
            }
        }
        ```

- 删除事件（解绑事件）

  - 删除事件的方式

    - 传统注册方式：eventTarget.onclick = null;

    - 方法监听注册方式

      - eventTarget.removeEventListener(type,listner[, useCapture]);
      - eventTarget.detachEvent(eventNameWithOn,callback); ——与eventTarget.attachEvent()配套

    - 删除事件代码：

      ```
        <div>1</div>
          <div>2</div>
          <script>
              var divs = document.querySelectorAll('div');
              divs[0].onclick = function() {
                  alert(11);
                  //传统方式删除事件
                  divs[0].onclick = null;
              }
              //removeEventListener删除事件
              divs[1].addEventListener('click',fn);
              function fn() {
                  alert(22);
                  divs[1].removeEventListener('click',fn);
              }
          </script>
      ```

    - 删除事件兼容性函数解决方案

      - ```
        function removeEventListener(element,eventName,fn) {
            //判断当前浏览器是否支持removeEventListener方法
            if(element.removeEventListener) {
                element.removeEventListener(eventName,fn);   //第三个参数，默认是false
            }  else if (element.detachEvent) {
                      element.detachEvent('on' + eventName,fn);
            }  else {
                      element['on' + eventName] = null;
            }
        }
        ```

- DOM事件流

  - **事件流**描述的是从页面中接收事件的顺序
  - **事件**发生时会在元素节点之间按照特定的顺序传播，这个**传播过程即DOM事件流**
  - DOM事件流分为三个阶段
    - 捕获阶段：由DOM最顶层节点开始，然后逐级向下传播到最具体的元素接收的过程
    - 当前目标阶段
    - 冒泡阶段：事件开始时由最具体的元素接收，然后逐级向上传播到DOM最顶层节点的过程
  - 实际开发更关注事件冒泡
  - 有些事件是没有冒泡的，如：onblur、onfocus、onmouseenter、onmouseleave

- 事件对象

  - 什么是事件对象

    - ```js
      eventTarget.onclick = function(event) {}
      eventTarget.addEventListener('click',function(event) {})
      //这个event就是事件对象，我们通常也写成e或者evt
       //event就是一个事件对象，写到我们侦听函数的小括号里面，当形参来看
              //事件对象只有有了事件才会存在，它是系统给我们自动创建的，不需要我们传递参数
              //事件对象 是 我们事件的一系列相关数据的集合，比如鼠标点击里面就包含了鼠标的相关信息，如鼠标坐标；如果是键盘事件里面就包含了键盘事件的信息，比如判断用户按下了哪个键
              //这个事件对象我们可用自己命名，如event、evt、e
             
      ```

    - 官方解释：event对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态

    - 简单理解：事件发生后，**跟事件相关的一系列信息数据的集合**都放到这个对象里面，这个对象就**是事件对象event**，它有很多属性和方法

    - 事件对象也有兼容性问题（ie678），通过window.event（兼容性的写法）—— **e = e || window.event**

  - 事件对象的常见属性和方法

    - | 事件对象属性方法    | 说明                                                         |
      | ------------------- | ------------------------------------------------------------ |
      | e.target            | 返回触发事件的对象 标准                                      |
      | e.srcElement        | 返回触发事件的对象 非标准（ie678使用）                       |
      | e.type              | 返回事件的类型 比如click mouseover 不带on                    |
      | e.cancelBubble      | 该属性阻止冒泡 非标准（ie678使用）                           |
      | e.returnValue       | 该属性阻止默认事件（默认行为）， 非标准（ie678使用），比如不让链接跳转 |
      | e.preventDefault()  | 该方法阻止默认事件（默认行为），标准，比如不让链接跳转       |
      | e.stopPropagation() | 阻止冒泡，标准                                               |

    - e.target与this的区别

      - e.target返回的是触发事件的对象（元素）；this返回的是绑定事件的对象（元素）
      - 即e.target点击了哪个元素，就返回哪个元素；this哪个元素绑定了这个点击事件，就返回谁

    - 阻止默认事件

      ```
      <script>
              var a = document.querySelector('a');
              a.addEventListener('click',function(e) {
                  e.preventDefault();     //dom标准写法
              })
      //传统的注册方式
              a.onclick = function(e) {
                  //普通浏览器  e.preventDefault();  方法
                  e.preventDefault();
                  //低版本浏览器  （ie678） returnValue属性
                  e.returnValue;
              }
      //我们可用利用return false也能阻止默认行为，没有兼容性问题   特点：return后面的代码不执行，而且只限于传统的注册方式
              return false;
          </script>
      ```

- 阻止事件冒泡

  - 阻止事件冒泡的两种方式

    - 标准写法：利用事件对象里面的stopPropagation()方法**（写在需要出现的事件中）**

    - 非标准写法：IE678利用事件对象cancelBubble属性

    - ```
        <div class="father">
              <div class="son">son</div>
          </div>
          <script>
              var son = document.querySelector('.son');
              son.addEventListener('click',function(e) {
                  alert('son');
                  e.stopPropagation();
                  e.cancelBubble = true;
              },false);
              var father = document.querySelector('.father');
              father.addEventListener('click',function() {
                  alert('father');
              },false);
              document.addEventListener('click',function() {
                  alert('document');
              });
          </script>
      ```

  - 阻止事件冒泡的兼容性解决方案

    ```
    if(e && e.stopPropagation) {
       e.stopPropagation();
    } else {
        window.event.cancelBubble = true;
    }
    ```

- 事件委托

  - 事件委托也叫事件代理，在jQuery里面称为事件委派

  - 事件委托的原理

    - 不是每个子节点单独设置事件监听器，而是事件监听器设置再其父节点上，然后利用冒泡原理影响设置每个子节点。
    - 如：给ul注册点击事件，然后利用事件对象的target来找到当前点击的li，因为点击li，事件会冒泡到ul上，ul有注册事件，就会触发事件监听器

  - 事件委托的作用：只操作了一次DOM，就提高了程序的性能

  - ```
     <ul>
            <li>弹框弹框</li>
            <li>弹框弹框</li>
            <li>弹框弹框</li>
            <li>弹框弹框</li>
            <li>弹框弹框</li>
        </ul>
        <script>
            //事件委托原理：给父节点添加侦听器，利用事件冒泡影响每一个子节点
            var ul = document.querySelector('ul');
            ul.addEventListener('click',function(e) {
                alert('弹框');
                //e.target可以得到我们点击的对象
                //所以e.target.style.backgroundColor = 'pink';  可以改变点击li的背景颜色
            })
        </script>
    ```

- 常用的鼠标事件

  - | 鼠标事件    | 触发条件         |
    | ----------- | ---------------- |
    | onclick     | 鼠标点击左键触发 |
    | onmouseover | 鼠标经过触发     |
    | onmouseout  | 鼠标离开触发     |
    | onfocus     | 获得鼠标焦点触发 |
    | onblur      | 失去鼠标焦点触发 |
    | onmousemove | 鼠标移动触发     |
    | onmouseup   | 鼠标弹起触发     |
    | onmousedown | 鼠标按下触发     |

  - 禁止鼠标右键菜单

    - contextmenu只要控制应该合适显示上下文菜单，主要用于程序员取消默认的上下文菜单

    - ```
      document.addEventListener('contextmenu',function(e) {
      e.preventDefault();
      })
      ```

  - 禁止鼠标选中（selectstart开始选中）

    - ```
      document.addEventListener('selectstart',function(e) {
       e.preventDefault();
      })
      ```

     例子：

    ```
    我是一段不愿意分享的文字
        <script>
            //contextmenu可以禁用右键菜单
            document.addEventListener('contextmenu',function(e) {
                e.preventDefault();
            })
            //禁止选中文字selectstart
            document.addEventListener('selectstart',function(e) {
                e.preventDefault();
            })
        </script>
    ```

  - 鼠标事件对象

    - event对象代表事件的状态，跟事件相关的一系列信息的集合。现阶段我们主要是用鼠标事件对象MouseEvent和键盘事件对象keyboardEvent

    - | 鼠标事件对象 | 说明                                      |
      | ------------ | ----------------------------------------- |
      | e.clientX    | 返回鼠标相对于浏览器窗口可视区的X坐标     |
      | e.clientY    | 返回鼠标相对于浏览器窗口可视区的Y坐标     |
      | e.pageX      | 返回鼠标相对于文档页面的X坐标（IE9+支持） |
      | e.pageY      | 返回鼠标相对于文档页面的Y坐标（IE9+支持） |
      | e.screenX    | 返回鼠标相对于电脑屏幕的X坐标             |
      | e.screenY    | 返回鼠标相对于电脑屏幕的Y坐标             |

- 常用的键盘事件

  - | 键盘事件   | 触发条件                                                     |
    | ---------- | ------------------------------------------------------------ |
    | onkeyup    | 某个键盘按键被松开时触发                                     |
    | onkeydown  | 某个键盘按键被按下时触发                                     |
    | onkeypress | 某个键盘按键被按下时 触发 **（但它不识别功能键，如ctrl、shift、箭头等）** |

    - 注意：
      - 如果使用addEventListener 不需要加on
      - onkeypress和前面两个的区别是：它不识别功能键，比如左右箭头、shift、ctrl等
      - 三个事件的执行顺序是：keydown——keypress——keyup

  - 键盘事件对象

    - | 键盘事件对象 属性 | 说明                  |
      | ----------------- | --------------------- |
      | keyCode           | 返回**该键**的ASCII值 |

    - 注意：**onkeydown和onkeyup不区分字母大小写，onkeypress区分字母大小写**

       我们在实际开发中，更多的使用keydown和keyup，它能识别所有的键（包括功能键）

       keypress不识别功能键，但是keyCode属性能区分大小写，返回不同的ASCII值

### BOM

- 什么是BOM

  - BOM（Browser Object Model）即**浏览器对象模型**，它提供了独立于内容而**与浏览器窗口进行交互的对象**，其核心对象是windows
  - BOM由一系列相关的对象构成，并且每个对象都提供了很多方法和属性
  - BOM缺乏标准，JavaScript语法的标准化组织是ECMA，DOM的标准组织是W3C，BOM最初是Netscape浏览器标准的一部分

- BOM的构成

  - BOM比DOM更大，它包含DOM

  - **window对象是浏览器的顶级对象**，它具有双重角色

    - 它是JS访问浏览器窗口的一个接口

    - 它是一个全局对象。定义在全局作用域中的变量、函数都会变成window对象的属性和方法

      在调用时可以省略window，前面学习的对话框都属于window对象方法，如alert（）、prompt（）等。

    - 注意：**window下的一个特殊属性：window.name**

- window对象的常见事件

  - 窗口加载事件

    - ```
      window.onload = function() {}
      或者
      window.addEventListener("load",function() {});
      ```

    - window.onload是窗口（页面）加载事件，当文档内容完全加载完成会触发该事件（包括图像、脚本文件、CSS文件等），就调用的处理函数

    - 注意：

      - 有了window.onload就可以把JS代码写到页面元素的上方，因为onload是等页面内容全部加载完毕，再去执行处理函数
      - window.onload传统注册方式只能写一次，如果有多个，会以最后一个window.onload为准
      - 如果使用addEventListener则没有限制

    - ```
      document.addEventListener('DOMContentLoaded',function(){})
      DOMContentLoaded事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等（IE9以上支持）
      ```

      - 如果页面的图片很多的话，从用户访问到onload触发可能需要较长的时间，交互效果就不能实现，必然影响用户的体验，此时用DOMContentLoaded事件比较合适

  - 调整窗口大小事件

    - ```
      window.onresize = function() {}
      或者
      window.addEventListener("resize",function() {});
      ```

    - window.onresize是调整窗口大小加载事件，当触发时就调用的处理函数

    - 注意：

      - 只要窗口大小发生像素变化，就会触发这个事件
      - 我们经常利用这个事件完成响应式布局。**window.innerWidth为当前屏幕的宽度**

- 定时器

  - window对象给我们提供了2个非常好用的方法——定时器

    - setTimeout（）
    - setInterval（）

  - setTimeout（）定时器

    - ```
      window.setTimeout(调用函数，[延迟的毫秒数])；
      ```

    - setTimeout（）方法用于设置一个定时器，该定时器在定时器到期后执行调用函数

    - 注意：

      - window可以省略

      - 这个调用函数可以直接写函数，或者写函数名 或者采取字符串‘函数名（）’三种形式。第三种不推荐

        ```js
        直接写函数：setTimeout(function() {
                     console.log('时间到了');
                 },2000)
        写函数名：setTimeout(calkback,3000);
        采取字符串‘函数名（）’：setTimeout('calkback()',5000);
        ```

      - 延迟的毫秒数省略默认是0，如果写，必须是毫秒

      - 因为定时器可能有很多，所有我们经常给定时器赋值一个标识符

        ```js
         var time1 = setTimeout(calkback,3000);
         var time2 = setTimeout('calkback()',5000); 
        ```

    - setTimeout（）这个调用函数我们也称为**回调函数callback**

      - 普通函数是按照代码顺序直接调用
      - 而这个函数，**需要等待**时间，时间到了才去调用这个函数，因此称为回调函数
      - 简单理解：回调，就是回头调用的意思。上一件事干完，再**回**头再**调**用这个**函数**。
      - 之前的element.onclick = function() {} 或者 element.addEventListener('click',fn);里面的函数也是回调函数

  - 停止setTimeout（）定时器

    - ```
      window.clearTimeout(timeoutID)
      ```

    - clearTimeout()方法取消了先前通过调用setTimeout()建立的定时器

    - 注意：

      - window可以省略
      - **里面的参数就是定时器的标识符**

    - ```
      <button>点击停止定时器</button>
          <script>
              var btn = document.querySelector('button');
              var timer = setTimeout(function() {
                  console.log('爆炸了');
              },5000)
              btn.addEventListener('click',function() {
                  clearTimeout(timer);
              })
          </script>
      ```

  - setInterval()定时器

    - ```
      window.setInterval(回调函数，[间隔的毫秒数]);
      ```

    - setInterval()方法重复调用一个函数，每隔这个时间，就去调用一次回调函数

    - 注意：

      - window可以省略
      - 这个调用函数可以**直接写函数**，**或者写函数名** 或者 **采取字符串‘函数名（）’**三种形式
      - 间隔的毫秒数省略默认是0，如果写，必须是毫秒，表示每隔多少毫秒就自动调用这个函数
      - 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符

  - 停止setInterval（）定时器

    - ```
      window.clearInterval(intervalID);
      ```

    - clearInterval（）方法取消了先前通过调用setInterval（）建立的定时器

    - 注意：

      - window可以省略
      - **里面的参数就是定时器的标识符**

  - this指向问题

    - this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，一般情况下this的最终指向的是那个调用它的对象

    - 全局作用域或者普通函数中this指向全局对象window（**注意定时器里面的this指向window**）

    - ```
      console.log('this');
      
      function fn() {
      console.log('this');
      }
      window.fn();
      
      window.setTimeout(function() {
       console.log('this');
      },1000);
      ```

    - 方法调用中 谁调用 this指向谁

      ```
      var o = {
          sayHi: function() {
              console.log('this');             //this指向的是这个o对象
          }
      }
      var btn = document.querySelector('button');
      btn.onclick = function() {
        console.log('this');                       //this指向的是btn这个按钮对象
      }
      ```

    - 构造函数中this指向构造函数的实例

      ```
      function Fun() {
      console.log('this');       //this指向的是fun实例对象
      }
      var fun = new Fun();
      ```

- JS执行队列

  - JS是单线程

    - JavaScript语言的一大特点就是**单线程**，也就是说，**同一时间只能做一件事**。这是因为javas这门脚本语言但是的使命所致——JavaScript是为处理页面中用户的交互，以及操作DOM而诞生的。比如我们对某个DOM元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除。
    - 单线程就意味着，所有任务都需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是：如果JS执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

  - 同步和异步

    - HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程。于是，JS中出现了**同步**和**异步**
    - 同步
      - 前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的。比如做饭的同步做法：我们要烧水煮饭，等水开了（10分钟之后），再去切菜，炒菜
      - 同步任务都在主线程上执行，形成一个**执行栈**
    - 异步
      - 你在做一件事情时，因为这件事情会花费很长时间，在做这件事的同时，你还可以去处理其他事情。比如做饭的异步做法，我们在烧水的同时，利用这十分钟去切菜，炒菜
      - 异步任务
        - JS的异步是通过回调函数实现的，一般而言，异步任务有以下三种类型：
          - 普通事件，如click、resize等
          - 资源加载，如load、error等
          - 定时器，包括setInterval、setTimeout等

  - JS执行机制

    - 先执行**执行栈中的同步任务**

    - 异步任务（回调函数）放入任务队列中

    - 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取**任务队列**中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

      ![QQ图片20211225195234](C:\Users\ZZY\Desktop\study\markdown插图\QQ图片20211225195234.png)

    - 由于主线程不断地重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为**事件循环（event loop）**

- location对象

  - 什么是location对象

    - window对象给我们提供了一个**location属性**用于**获取或设置窗体的URL**，并且可以用于**解析URL**。因为这个属性返回的时一个对象，所以我们将这个属性也称为**location对象**。

  - URL

    - **统一资源定位符（Uniform Resource Locator，URL）**是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

    - URL的一般语法格式为：

      - protocol://host[:port]/path/[?query]#fragment
      - [http://www.itcast.cn/index.html?name=andy&age=18#link](https://gitee.com/link?target=http%3A%2F%2Fwww.itcast.cn%2Findex.html%3Fname%3Dandy%26age%3D18%23link)

    - | 组成     | 说明                                                         |
      | -------- | ------------------------------------------------------------ |
      | protocol | 通信协议，常用的http，ftp，maito等                           |
      | host     | 主机（域名） [www.itheima.com](https://gitee.com/link?target=http%3A%2F%2Fwww.itheima.com) |
      | port     | 端口号 ，可选，省略时使用方案的默认端口，如http的默认端口为80 |
      | path     | 路径，由零或多个'/'符号隔开的字符串，一般用来表示主机上的一个目录或文件地址 |
      | query    | 参数，以键值对的形式，通过&符号分隔开来                      |
      | fragment | 片段，#后面内容，常见于链接、锚点                            |

  - location对象的属性

    - | location对象属性  | 返回值                                                       |
      | ----------------- | ------------------------------------------------------------ |
      | location.href     | 获取或者设置 整个URL                                         |
      | location.host     | 返回主机（域名） [www.itheima.com](https://gitee.com/link?target=http%3A%2F%2Fwww.itheima.com) |
      | location.port     | 返回端口号 ，如果未写返回空字符串                            |
      | location.pathname | 返回路径                                                     |
      | location.search   | 返回参数                                                     |
      | location.hash     | 返回片段 #后面内容，常见于链接、锚点                         |

      **重点记住：href和search**

  - location对象的方法

    - | location对象方法   | 返回值                                                       |
      | ------------------ | ------------------------------------------------------------ |
      | location.assign()  | 跟href一样，可以跳转页面（也称为重定向页面）                 |
      | location.replace() | 替换当前页面，因为不记录历史，所以不能后退页面               |
      | location.reload()  | 重新加载页面，相当于刷新按钮或者f5.如果参数为true，强制刷新ctrl+f5 |

    - ```
        <button>点击</button>
          <script>
              var btn = document.querySelector('button');
              btn.addEventListener('click',function() {
                  //记录浏览历史，所以可以实现后退功能
                  location.assign('http://www.itcast.cn');
                  //不记录浏览历史，不可以后退
                  location.replace('http://www.itcast.cn');
                  location.reload();
              })
          </script>
      ```

- navigator对象

  - navigator对象包含有关浏览器的信息，它有很多属性，我们最常用的是userAgent，该属性可以返回由客户机发送服务器的user-agent头部的值

  - 下面前端代码可以判断用户那个终端打开页面，实现跳转

    ```
    if((navigator.userAgent.match(/(phone|pad|pod|iPhone|ios|ipad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
        window.location.href = '';       //手机
    } else {
        window.location.href = '';        //电脑
    }
    ```

- history对象

  - window对象给我们提供了一个history对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的URL

  - | history对象方法 | 作用                                                         |
    | --------------- | ------------------------------------------------------------ |
    | back()          | 可以后退功能                                                 |
    | forward()       | 前进功能                                                     |
    | go(参数)        | 前进后退功能，参数如果是1，前进1个页面；如果是-1，后退一个页面 |

  - history对象一般在实际开发中比较少用，但是会在一些OA办公系统中见到