# mustache模板引擎

- ### 什么是模板引擎

  - 模板引擎是将数据要变为视图最优雅的解决方案

  - 比如：

    - 数据：

      ```js
      [
          {"name":"小米","age":12,"sex":"男"}，
          {"name":"小秒","age":15,"sex":"女"}，
          {"name":"小点","age":22,"sex":"男"}
      ]
      ```

    - 视图：

      ```html
      <ul>
          <li>
              <div class="hd">小米的基本信息</div>
              <div class="bd">
                  <p>姓名：小米</p>
              </div>
          </li>
      </ul>
      ```

    - Vue的解决方法：

      ```html
      <li v-for="item in arr"></li>
      ```

      - 这实际上就是一种模板引擎

  - 历史上曾经出现的数据变为视图的方法

    - 纯DOM法

      - ```html
        <body>
            <ul id="list"></ul>
        
            <script>
                var arr = [
                {"name":"小米","age":12,"sex":"男"},
                {"name":"小秒","age":15,"sex":"女"},
                {"name":"小点","age":22,"sex":"男"}
                ]
        
                let ul = document.getElementById('list')
                for(var i = 0;i < arr.length;i++) {
                    let oLi = document.createElement('li');
                    oLi.innerHTML = arr[i].name;
                    list.appendChild(oLi)
                }
            </script>
        </body>
        ```

    - 数组join法

      - ```html
        <body>
            <ul id="list"></ul>
            
            <script>
                var arr = [
                {"name":"小米","age":12,"sex":"男"},
                {"name":"小秒","age":15,"sex":"女"},
                {"name":"小点","age":22,"sex":"男"}
                ]
        
                var list = document.getElementById('list')
        
                var str = [
                '<li>',
                '    <div class="hd"></div>',
                '    <div class="bd">',
                '        <p>姓名:</p>',
                '        <p>年龄:</p>',
                '        <p>性别:</p>',
                '    </div>',
                '</li>',
            ].join('')
        
        
            for(var i = 0;i < arr.length;i++) {
                    list.innerHTML += [
                '<li>',
                '    <div class="hd">'+arr[i].name+'的信息</div>',
                '    <div class="bd">',
                '        <p>姓名:'+arr[i].name+'</p>',
                '        <p>年龄:'+arr[i].age+'</p>',
                '        <p>性别:'+arr[i].sex+'</p>',
                '    </div>',
                '</li>',
            ].join('')
                }
            </script>
        </body>
        ```

    - ES6的反引号法

      - ```html
        <body>
            <ul id="list"></ul>
        
            <script>
                var arr = [
                {"name":"小米","age":12,"sex":"男"},
                {"name":"小秒","age":15,"sex":"女"},
                {"name":"小点","age":22,"sex":"男"}
                ]
        
                var list = document.getElementById('list')
        
                var str = [
                '<li>',
                '    <div class="hd"></div>',
                '    <div class="bd">',
                '        <p>姓名:</p>',
                '        <p>年龄:</p>',
                '        <p>性别:</p>',
                '    </div>',
                '</li>',
            ].join('')
        
        
            for(var i = 0;i < arr.length;i++) {
                    list.innerHTML += `
                    <li>
                        <div class="hd">${arr[i].name}的基本信息</div>
                        <div class="bd">
                            <p>姓名：${arr[i].name}</p>
                            <p>年龄:${arr[i].age}</p>
                            <p>性别:${arr[i].sex}</p>
                        </div>
                    </li>
                    `;
                }
            </script>
        </body>
        ```

    - 模板引擎

    

- ### mustache库

  - 简介

    - mustache官方git：https://github.com/janl/mustache.js
    - 它的嵌入标记是`{{}}`
    - mustache是**最早的模板引擎库**

  - #### mustache库基本使用

    - 必须引入mustache库，可以在bootcdn.com上找到他

    - 循环对象数组

      - ```js
        Mustache.render(templateStr,data)
        ```

      - 第一个参数是模板字符串，如：

        ```html
        var templateStr = `
                    <ul>
                        {{#arr}}
                            <li>
                                <div class="hd">{{name}}的基本信息</div>
                                <div class="bd">
                                    <p>姓名：{{name}}</p>
                                    <p>年龄:{{age}}</p>
                                    <p>性别:{{sex}}</p>
                               </div>
                            </li>
                        {{/arr}}
                    </ul>
                `
        ```

      - 第二个参数是数据，如：

        ```js
        var data = {
                   arr:  [
                        {"name":"小米","age":12,"sex":"男"},
                        {"name":"小秒","age":15,"sex":"女"},
                        {"name":"小点","age":22,"sex":"男"}
                    ]
                }
        ```

      - 运行后**返回被填充好的模板（DOM结构）**

        - ```html
                          <ul>
                              <li>
                                  <div class="hd">小米的基本信息</div>
                                  <div class="bd">
                                      <p>姓名：小米</p>
                                      <p>年龄:12</p>
                                      <p>性别:男</p>
                                 </div>
                              </li>
                              <li>
                                  <div class="hd">小秒的基本信息</div>
                                  <div class="bd">
                                      <p>姓名：小秒</p>
                                      <p>年龄:15</p>
                                      <p>性别:女</p>
                                 </div>
                              </li>
                              <li>
                                  <div class="hd">小点的基本信息</div>
                                  <div class="bd">
                                      <p>姓名：小点</p>
                                      <p>年龄:22</p>
                                      <p>性别:男</p>
                                 </div>
                              </li>
                      </ul>
          ```

    - 不循环

      ```js
       var templateStr = `
                  <h1>我买了一个{{thing}},好{{mood}}啊</h1>
              `;
      
              var data = {
                  thing: '华为手机',
                  mood: "开心"
              }
      ```

    - 循环简单数组

      ```js
         var templateStr = `
                  <ul>
                      {{#arr}}
                          <li>{{.}}</li>
                      {{/arr}}    
                  </ul>
              `;
      
              var data = {
                  arr: ['A','B','C','D']
              }
      ```

    - 数组的嵌套情况

      ```js
         var templateStr = `
                  <ul>
                      {{#arr}}
                          <li>{{name}}的爱好是：</li>
                          <ol>
                              {{#hobbies}}  
                                  <li>{{.}}</li>
                              {{/hobbies}}  
                          </ol>
                      {{/arr}}    
                  </ul>
              `;
              let data = {
                  arr:  [
                      {"name":"小米","age":12,"sex":"男","hobbies":['游泳','羽毛球']},
                      {"name":"小秒","age":15,"sex":"女","hobbies":['抽烟','烫头','打球']},
                      {"name":"小点","age":22,"sex":"男","hobbies":['喝酒']}
                  ]
              }
      
      ```

    - 布尔值

      ```js
      var templateStr = `
                  {{#m}}
                      <h1>你好</h1>
                  {{/m}}
              `
      
              var data = {
                  m: true
              }
      ```

      

- #### mustache的底层核心机理

  - 什么是tokens

    - tokens是一个JS的嵌套数组，实际上就是模板字符串的JS表示

    - 它是"抽象语法树"，"虚拟节点"的开山鼻祖

    - 比如：

      - 模板字符串：

      ```html
      <h1>
          我买了一个{{thing}},好{{mood}}啊
      </h1>
      ```

      - tokens：

      ```js
      [
          ["text","<h1>我买了一个"],
          ["name","thing"],
          ["text","好"],
          ["name","mood"],
          ["text","啊</h1>"]
      ]
      ```

    - 循环情况下的tokens

      - 当模板字符串中有循环存在时，它将被编译为**嵌套更深**的tokens

      - ```html
        <div>
            <ul>
                {{#arr}}
                <li>{{.}}</li>
                {{/arr}}
            </ul>
        </div>
        ```

        ```js
        [
            ["text","<div><ul>"],
            ["#","arr",[
                ["text","<li>"],
                ["namr","."],
                ["text","</li>"],
            ]],
            ["text","</ul></div>"],
        ]
        ```

  - mustache库底层重点要做的两个事情

    - **将模板字符串编译为tokens形式**
    - **将tokens结合数据，解析为dom字符串**

    

- ### 手写mustache库

  - 首先需要有一个Scanner类

    - ```js
      //Scanner.js
      // Scanner扫描器类
      export default class Scanner {
          constructor(templateStr) {
              // 将模板字符串写到实例身上
              this.templateStr = templateStr
              // 指针
              this.pos = 0;
              // 尾巴,一开始就是模板字符串原文
              this.tail = templateStr
          }
      
          // 功能弱，就是走过指定内容
          scan(tag) {
              if (this.tail.indexOf(tag) == 0) {
                  //tag有多长，比如{{长度是2，就让指针后移多少位
                  this.pos += tag.length
                  // 尾巴也要变,改变尾巴为从当前指针这个字符开始，到最后的全部字符
                  this.tail = this.templateStr.substring(this.pos)
              }
          }
      
          // 让指针进行扫描，直到遇见指定内容结束，并且能够返回结束之前路过的文字
          scanUtil(stopTag) {
              // 记录一下执行本方法时，pos的值
              const pos_backup = this.pos
              // 当尾巴的开头不是stopTag时，说明还没有扫描到stopTag，指针后移;移动到templateStr的最后一位停止
              while (!this.eos() && this.tail.indexOf(stopTag) != 0) {
                  this.pos++;
                  // 尾巴是包括指针以及指针后面的内容
                  this.tail = this.templateStr.substr(this.pos)
              }
      
              return this.templateStr.substring(pos_backup, this.pos);
          }
      
          // 判断指针是否以及到头，返回布尔值
          eos() {
              return this.pos >= this.templateStr.length
          }
      }
      ```

  - 然后需要有一个将HTML的templateStr变为tokens的函数

    - ```js
      //parseTemplateToTokens.js
      
      import Scanner from "./Scanner.js";
      import nestTokens from "./nestTokens.js";
      
      // 将templateStr转为Tokens数组
      export default function parseTemplateToTokens(templateStr) {
          let tokens = [];
          // 创建扫描器
          let scanner = new Scanner(templateStr)
          let words;
          while (!scanner.eos()) {
              // 收集开始标记出现之前的文字
              words = scanner.scanUtil('{{')
              if (words != '') {
                  // 尝试一下去掉空格,智能判断是普通文字的空格，还是标签中的空格
                  // 空格是不是在标签内
                  let isIn = false;
                  // 空白字符串
                  let _words = ''
                  for (let i = 0; i < words.length; i++) {
                      // 如果不是空格，那就拼接上去
                      if (words[i] != ' ') {
                          _words += words[i]
                      } else {
                          // 如果这项是空格，只有当它在标签内的时候才拼接上
                          if (isIn) {
                              _words += words[i]
                          }
                      }
                      // 重新设置isIn（是否在标签内）的值
                      if (words[i] == '<') {
                          isIn = true;
                      } else if (words[i] == '>') {
                          isIn = false
                      }
                  }
                  tokens.push(["text", _words])
              }
              // 过{{
              scanner.scan("{{")
              words = scanner.scanUtil('}}')
              if (words != '') {
                  // words就是{{}}中间的东西，判断一下首字符是否为#
                  if (words[0] == '#') {
                      // 从下标为1的项开始存，因为0是#
                      tokens.push(["#", words.substring(1)])
                  } else if (words[0] == '/') {
                      // 从下标为1的项开始存，因为0是/
                      tokens.push(["/", words.substring(1)])
                  } else {
                      tokens.push(["name", words])
                  }
              }
              // 过}}
              scanner.scan("}}")
          }
      
          // 返回折叠收集的tokens
          return nestTokens(tokens);
      }
      ```

  - 之后需要将收集到的tokens折叠起来

    - 即将`#`后的元素放入自己的子元素中

    - ```js
      //nestTokens.js
      // 折叠tokens的函数,将#和/之间的tokens能够整合起来，作为它的下标为2的项
      export default function nestTokens(tokens) {
          // 结果数组
          let nestedTokens = []
          // 收集器collector开始时指向nestedTokens这个数组
          // 收集器的指向会变化，当遇见#的时候，收集器会指向这个token的下标为2的新数组
          let collector = nestedTokens
          // 栈结构
          let sections = []
      
          for (let i = 0; i < tokens.length; i++) {
              let token = tokens[i];
      
              switch (token[0]) {
                  case '#':
                      // 收集器中放入token
                      collector.push(token)
                      // 入栈
                      sections.push(token)
                      // 收集器要换人
                      // 创造一个子对象项,让收集器指向子对象数组（这样可以让下面要收集的都进入子对象数组）
                      collector = token[2] = []
                      break;
                  case '/':
                      // 出栈
                      sections.pop()
                      // 改变收集器为栈结构的上一层（队尾是栈顶）那项的下标为2的数组;如果栈中没有东西了，就指向最终结果数组
                      collector = sections.length > 0 ? sections[sections.length - 1][2] : nestedTokens
                      break;
                  default:
                      // 普通情况下将数据推入收集器
                      // 当前collector可能是nestedTokens，也可能是某个token的下标为2的数组
                      collector.push(token)
              }
          }
      
      
          return nestedTokens;
      }
      ```

  - 再之后需要一个将tokens注入数据变回HTML的函数

    - ```js
      //renderTemplate.js
      
      import lookup from "./lookup.js";
      import parseArray from "./parseArray.js";
      
      // 函数的功能是让tokens数组变为带数据的dom字符串
      export default function renderTemplate(tokens, data) {
          // 结果字符串
          let resultStr = ''
          // 遍历tokens
          for (let i = 0; i < tokens.length; i++) {
              let token = tokens[i]
              // 看类型
              if (token[0] == 'text') {
                  // 拼起来
                  resultStr += token[1];
              } else if (token[0] == 'name') {
                  // 如果是name类型，那么就直接使用它的值（用lookup函数查询A.B.C之类的数据）
                  resultStr += lookup(data, token[1])
              } else if (token[0] == '#') {
                  resultStr += parseArray(token, data)
              }
          }
      
          return resultStr
      }
      ```

    - 此时需要由一个函数来获取数据对象中的值

      ```js
      //lookup.js
      // 功能是可以在dataObj对象中，寻找用连续点符号的KeyName属性
      
      export default function lookup(dataObj, KeyName) {
          // 看看KeyName中有没有点符号，但不能是.本身
          if (KeyName.indexOf('.') != -1 && KeyName != '.') {
              // 如果有点符号，那么拆开
              let keys = KeyName.split('.')
              // 设置一个临时变量temp，让它一层一层找下去
              let temp = dataObj;
              for (let i = 0; i < keys.length; i++) {
                  temp = temp[keys[i]]
              }
              return temp;
          }
          return dataObj[KeyName]
      }
      ```

    - 然后还需要对数据数组进行处理（决定循环次数）

      ```js
      //parseArray.js
      import lookup from "./lookup.js";
      import renderTemplate from "./renderTemplate.js";
      
      /*处理数组，结合renderTemplate实现递归
       注意这个函数接收的参数是token，不是tokens
       token是tokens里面的一个单独元素
      
       这个函数要递归调用renderTemplate函数，调用的次数由data决定
       比如data的形式是这样的：
              {
                  students: [
                      { 'name': '小明', 'hobbies': ['游泳','健身']},
                      { 'name': '小红', 'hobbies': ['足球','篮球','羽毛球']},
                      { 'name': '小强', 'hobbies': ['吃饭','睡觉']}
                  ]
              }
        那么parseArray()函数就要递归调用renderTemplate函数3次，因为数组长度是3
      
      */
      export default function parseArray(token, data) {
          // 得到整体数据data中这个数组要使用的部分
          let v = lookup(data, token[1])
          // 结果字符串
          let resultStr = ''
          // 遍历v数组
          // 把token[2]当成一个tokens
          // 它是遍历数据V，而不是遍历tokens，数组中数据有多少就遍历多少次
          for (let i = 0; i < v.length; i++) {
              // 这里由多一个"."属性
              resultStr += renderTemplate(token[2], {
                  // 现在这个数据就是v[i]的展开
                  ...v[i],
                  // 加上一个.属性
                  '.': v[i]
              })
          }
      
          return resultStr
      }
      ```

      

  - 最后在index函数中有一个render方法

    - ```js
      //index.js
      import parseTemplateToTokens from "./parseTemplateToTokens.js";
      import renderTemplate from './renderTemplate.js'
      
      export default class Mymustache {
          render(templateStr, data) {
              // 让模板字符串变为tokens数组
              let tokens = parseTemplateToTokens(templateStr)
              // 调用renderTemplate函数，让tokens数组变为dom字符串
              let domStr = renderTemplate(tokens, data)
      
              return domStr
          }
      }
      ```

  - 在index.html中使用

    - ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
      </head>
      <body>
          <div id="container"></div>
          <!-- <script src="./index.js" type="module"></script> -->
          <script type="module">
              // 挂载在window
              import Mymustache1 from './src/index.js'
              let mymustache = new Mymustache1()
              window.Mymustache = mymustache
      
              var templateStr = `
                  <div class="aaa">
                      <ul>
                          {{#students}}
                          <li>
                              学生{{name}}的爱好是
                              <ul>
                                  {{#hobbies}}
                                  <li>{{.}}</li>
                                  {{/hobbies}}    
                              </ul>
                          </li>
                          {{/students}}    
                      </ul>    
                  </div>
              `;
      
              var data = {
                  students: [
                      { 'name': '小明', 'hobbies': ['游泳','健身']},
                      { 'name': '小红', 'hobbies': ['足球','篮球','羽毛球']},
                      { 'name': '小强', 'hobbies': ['吃饭','睡觉']}
                  ]
              }
              let domStr = Mymustache.render(templateStr,data)
              console.log(domStr);
              let container = document.querySelector("#container")
              container.innerHTML = domStr
          </script>
      </body>
      </html>
      ```

  - 

- 