## vue基础入门（指令和过滤器）

- vue简介

  - 官方给出的概念：一套**用于构建用户界面**的前端**框架**
    - 构建用户界面：用vue往html页面中填充数据
    - 框架：一套现成的解决方案，程序员只能遵守框架的规范，去编写自己的业务功能
  - vue的特性
    - **数据驱动视图**
      - 在使用了vue的页面中，vue会**监听数据的变化**，从而**自动**重新渲染页面的结构
      - 好处：当页面数据发生变化时，页面会自动重新渲染
      - 注意：数据驱动视图的**单向的数据绑定**
    - **双向数据绑定**
      - 在**填写表单**时，双向数据绑定可以辅助开发者在**不操作DOM的前提下，自动**把用户填写的内容**同步到**数据源中。
      - 好处：开发者不再需要手动操作DOM元素来获取表单元素最新的值
    - 即js数据的变化，会被自动渲染到页面上；而页面上表单采集的数据发生变化时，会被vue自动获取到，并更新到js数据中
  - MVVM
    - **MVVM**是vue实现**数据驱动视图和双向数据绑定**的核心原理。MVVM指的是**M**odel、**V**iew和**V**iew**M**odel，它把每个HTML页面都拆分成了这三个部分
    - 在MVVM概念中：
      - **Model**表示当前页面渲染时所依赖的数据源
      - **View**表示当前页面所渲染的DOM结构
      - **ViewModel**表示vue的实例，它是MVVM的核心
    - MVVM的工作原理
      - **ViewModel作为MVVM的核心**，是他把当前页面的**数据源**（Model）和**页面 结构**（View）连接在了一起
      - 当**数据源发生变化**时，会被ViewModel监听到，VM会根据最新的数据与**自动更新**页面的结构
      - 当**表单元素的值发生变化**时，也会被VM监听到，VM会把变化过后的最新的值**自动同步**到Model数据源中

  - vue的版本

    - 当前，vue共有3个大版本。其中：
      - **2.x版本的vue是目前企业级项目开发中的主流版本**
      - 3.x版本的vue于2020-09-19发布，生态还不完善，尚未在企业级项目开发中普及和推广
      - 1.x版本的vue几乎被淘汰，不建议学习和使用
    - 总结：
      - **3.x版本的vue是未来企业级项目的开发趋势**
      - 2.x版本的vue在未来会被逐渐淘汰

    

- vue的基本使用

  - 基本使用步骤

    - 导入vue.js的script

    - 在页面中声明一个将要被vue所控制的DOM区域

    - 创建vm实例对象（vue实例对象）

    - ```html
      <body>
          <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
          <div id="app">{{  username  }}</div>
      
          <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
          <script src="./vue.js"></script>
          <!-- 2.创建vue的实例对象 -->
          <script>
              // 创建Vue实例对象
              const vm = new Vue({
                  //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                  el:'#app',            //此为根节点
                  // data对象就是要渲染到页面上的数据
                  data:{
                      username:'zhangsan'
                  }
              })
          </script>
      </body>
      ```

    - **el属性是固定的写法，表示当前vm示例要控制页面上的哪个区域，接受的值是<u>一个选择器</u>**

    - new Vue()构造函数得到的vm实例对象，就是ViewModel；el指向的选择器，就是View视图区域；data指向的对象，就是Model数据源

  - VUE的el和data的两种写法

    - el有两种写法

      - 创建`vue`示例对象的时候配置`el`属性
      - 先创建`vue`示例，随后通过`vm.$mount（'root')`指定`el`的值

    - data有两种写法

      - 对象式： data:{}
      - 函数式： data() { return{} }

    - 到了组件时，data必须使用函数式

    - 由vue管理的函数，一定不要写箭头函数，否则this就不再是vue实例了

    - ```js
      <!DOCTYPE html>
      <html>
        <head>
          <meta charset="UTF-8" />
          <title>el与data的两种写法</title>
          <!-- 引入Vue -->
          <script type="text/javascript" src="../js/vue.js"></script>
        </head>
        
        <body>
          <div id="root">
            <h1>你好，{{name}}</h1>
          </div>
        </body>
      
        <script type="text/javascript">
          Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
      
          // el的两种写法
          // const v = new Vue({
          // 	//el:'#root', // 第一种写法
          // 	data: {
          // 		name:'cess'
          // 	}
          // })
          // console.log(v)
          // v.$mount('#root') // 第二种写法
      
          // data的两种写法
          new Vue({
            el: '#root',
            // data的第一种写法：对象式
            // data:{
            // 	name:'cess'
            // }
      
            //data的第二种写法：函数式
            data() {
              console.log('@@@', this) // 此处的this是Vue实例对象
              return {
                name: 'cess'
              }
            }
          })
        </script>
      </html>
      ```

      

  #### vue的指令与过滤器

  ##### 指令

- 指令的概念

  - **指令（Directives）**是vue为开发者提供的**模板语法**，用于**辅助开发者渲染页面的基本结构**
  - vue中的指令**按照不同的用途**可以分为如下6大类：
    - **内容渲染**指令
    - **属性绑定**指令
    - **事件绑定**指令
    - **双向绑定**指令
    - **条件渲染**指令
    - **列表渲染**指令
  - 注意：指令是vue开发中最基础、最常用、最简单的知识点

  

- 内容渲染指令

  - **内容渲染指令**用来辅助开发者**渲染DOM元素的文本内容**。常用的内容渲染指令有如下3个：

    - v-text
    - {{}}
    - v-html

  - v-text

    - 用法示例：

      ```html
              <!-- 把username对应的值，渲染到第一个p标签中 -->
              <p v-text="username"></p>
              <!-- 把gender对应的值，渲染到第二个p标签中 -->
              <!-- 注意：第二个p标签中，默认的文本"性别："会被gender的值覆盖掉 -->
              <p v-text="gender">性别:</p>
      ```

    - `v-text`指令的缺点：会**覆盖元素内部原有的内容**

  - {{}} 语法

    - vue提供的**{{}}**语法，专门用来解决v-text会覆盖默认文本内容的问题。这种{{}}语法的专业名称是**插值表达式**（英文名为：Mustanche）

    - ```html
      <!-- 使用{{}}插值表达式，将对应的值渲染到元素的内容节点中 -->
              <!-- 同时保留元素自身的默认值 -->
              <p>姓名:{{ username }}</p>
              <p>性别:{{ gender }}</p>
          </div>
      ```

    - `{{}}`插值表达式：在实际开发中用的最多，只是内容的占位符，不会覆盖原有的内容

    - 注意：插值表达式只能用在元素的**内容节点**中，不能用在元素的属性节点中

  - v-html

    - **v-text**指令和**插值表达式**只能渲染**纯文本内容**。如果要把**包含HTML标签的字符串**渲染为页面的HTML元素，则需要用到v-html这个指令

    - ```html
              <!-- 假设data中定义了名为info的数据，数据的值为包含了HTML标签的字符串： -->
              <!-- '<h5 style="color: red;">欢迎学习vue.js</h5>' -->        
              <p v-html="info"></p>
      ```

    - `v-html`指令的作用：可以把**带有标签的字符串**，**渲染成**真正的**HTML内容**

      

- 属性绑定指令

  - 如果需要为元素的属性动态绑定属性值，则需要用到v-bind属性绑定指令。示例如下：

  - ```html
    <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
        <div id="app">
            <input type="text" v-bind:placeholder="tips">
            <hr>
            <img v-bind:src="photo" alt="" style="width:150px;">
            <!-- 或<img :src="photo" alt="" style="width:150px;"> -->    
        </div>
    
        <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
        <script src="./vue.js"></script>
        <!-- 2.创建vue的实例对象 -->
        <script>
            // 创建Vue实例对象
            const vm = new Vue({
                //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                el:'#app',
                // data对象就是要渲染到页面上的数据
                data:{
                    tips: '请输入用户名',
                    photo:'https://cn.vuejs.org/images/logo.svg'
                }
            })
        </script>
    ```

  - **vue规定v-bind：指令可以简写**为  <u>**：**</u>

  - 在使用v-bind属性绑定期间，如果绑定内容需要进行动态拼接，则字符串的外面应该包裹单引号，例如：

    ```html
    <div :title="'box' + index">这是一个div</div> 
    ```

  

- 使用JavaScript表达式

  - 在vue提供的模板渲染语法中，处理支持**绑定简单的数据值**之外，还**支持JavaScript表达式的运算**，例如：

    ```html
    {{ number + 1 }}
    
    {{ ok? 'YES':'NO' }}
    
    {{ message.split('').reverse().join('') }}
    
    <div v-bind:id='list-' + id></div>
    <div>1 + 2 的结果是: {{1+2}}</div>
    <div>{{ tips }} 反转的结果是: {{ tips.split('').reverse().join('') }}</div>
    <div :title="'box' + index">这是一个div</div>    <!-- 此处box外面要加单引号，表示它相对于：是一个字符串，否则：就会去data中寻找box，没有找到就会报错 -->
    ```

    

- 事件绑定指令

  - vue提供了**v-on事件绑定**指令，用来辅助程序员为DOM元素绑定事件监听。

  - 语法格式：**v-on：事件名称="事件处理函数的名称"**

  - 示例：

    ```html
     <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
        <div id="app">
            <p>count的值是: {{ count }}</p>
            <!-- 语法格式为：v-on：事件名称="事件处理函数的名称" -->
            <button v-on:click="addCount">+1</button>
            <button v-on:click="lessCount">-1</button>
        </div>
    
    <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
        <script src="./vue.js"></script>
        <!-- 2.创建vue的实例对象 -->
        <script>
            // 创建Vue实例对象
            const vm = new Vue({
                //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                el:'#app',
                // data对象就是要渲染到页面上的数据
                data:{
                    count: 0
                },
                //methods的作用，就是定义事件的处理函数
                methods: {
                    addCount: function() {
                        this.count++;
                    },
                    lessCount() {
                        this.count--;
                    }
                }
            })
        </script>
    ```

  - 处理事件函数的简写形式：

    ```html
    <script>
            // 创建Vue实例对象
            const vm = new Vue({
                //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                el:'#app',
                // data对象就是要渲染到页面上的数据
                data:{
                    count: 0
                },
                //methods的作用，就是定义事件的处理函数
                methods: {
                    addCount () {
                        this.count++;
                    }
                }
            })
        </script>
    
    <!-- 直接函数名（），不用:function -->
    ```

  - 绑定事件并传参

    ```html
    <!-- 在绑定事件处理函数的时候，可以使用（）传递参数 --> 
    <button v-on:click="addCount(2)">+2</button>
    
    methods: {
                    addCount: function(n) {
                        this.count += n;
                    },
                    lessCount() {
                        this.count--;
                    }
                }
    ```

  - **v-on指令的简写形式：@**

    ```js
    <button @click="addCount(2)">+2</button>
    ```

  - 注意：原生DOM对象有onclick、oninput、onkeyup等原生事件，替换为vue的事件绑定形式后，分别为：v-on:click、v-on:input、v-on:keyup

    

  - $event

    - vue提供了内置变量，名字叫做$event，他就是原生DOM的事件对象e

    - ```html
      <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
          <div id="app">
              <p>count 的值是:{{ count }}</p>
              <!-- 如果count是偶数，则按钮变成红色，否则取消背景颜色 -->
              <!-- vue提供了内置变量，名字叫做$event，他就是原生DOM的事件对象e -->
              <button @click="add(1,$event)">+N</button>
          </div>
      
          <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
          <script src="./vue.js"></script>
          <!-- 2.创建vue的实例对象 -->
          <script>
              // 创建Vue实例对象
              const vm = new Vue({
                  //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                  el:'#app',
                  // data对象就是要渲染到页面上的数据
                  data:{
                      count:0
                  },
                  methods: {
                      add(n,e) {
                          this.count += n;
                          //判断this.count的值是否为偶数
                          if(this.count % 2 === 0) {
                              e.target.style.backgroundColor = 'red';
                          } else {
                              e.target.style.backgroundColor = '';
                          }
                      }
                  }
              })
          </script>
      ```

  - 事件修饰符

    - 在事件处理函数中调用**event.preventDefault()**或**event.stopPropagation()**是非常常见的需求。因此，vue提供了**事件修饰符**的概念，来辅助程序员更方便的**对事件的触发进行控制**。常用的5个时间修饰符如下：

    - | 事件修饰符   | 说明                                                        |
      | ------------ | ----------------------------------------------------------- |
      | **.prevent** | **阻止默认行为**（例如：阻止a链接的跳转、阻止表单的提交等） |
      | **.stop**    | **阻止事件冒泡**                                            |
      | .capture     | 以捕获模式触发当前的事件处理函数                            |
      | .once        | 绑定的事件只触发1次                                         |
      | .self        | 只有在event.target是当前元素自身触发事件处理函数            |
      | .passive     | 事件的默认行为为立即执行，无需等待事件回调执行完毕          |
      
    - 例如：
  
      ```html
      <!-- 加在事件后面 -->
      
      <div id="app">
              <a href="http://www.baidu.com" @click.prevent="show">跳转到百度首页</a>
      
              <hr>
              <div style="height: 150px; background-color: orange; padding-left: 100px; line-height: 150px;" @click="divHandler">
                  <button @click.stop="btnHandler">按钮</button>
              </div>
          </div>
      ```
  
  - `.exact`修饰符
  
    - `.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。
  
    - ```html
      <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
      <button @click.ctrl="onClick">A</button>
      
      <!-- 有且只有 Ctrl 被按下的时候才触发 -->
      <button @click.ctrl.exact="onCtrlClick">A</button>
      
      <!-- 没有任何系统修饰符被按下的时候才触发 -->
      <button @click.exact="onClick">A</button>
      ```
    
  - 按键修饰符
  
    - VUE中常见的按键别名
  
      - 回车`enter`
      - 删除`delete`捕获删除和退格键
      - 退出`esc`
      - 空格`space`
      - 换行`tab`特殊，必须配合`keydown`去使用
      - 上`up`
      - 下`down`
      - 左`left`
      - 右`right`
    
    - 系统修饰键（用法特殊）`ctrl` `alt` `shift` `meta`(`meta`就是`win`)
    
      - 配合`keyup`使用：按下修饰键的同时，再按下其他键，随后释放其他键（其他键任何一个都可以），事件才被触发
    
        或者可以指定其他键为单一键，如指定ctrl和y一起，即`@keyup.ctrl.y`
    
      - 配合`keydown`使用：正常触发事件
    
    - Vue.config.keyCodes.自定义键名 = 键码，可以去定义按键别名
    
      ```js
      <!DOCTYPE html>
      <html>
        <head>
          <meta charset="UTF-8" />
          <title>键盘事件</title>
          <!-- 引入Vue -->
          <script type="text/javascript" src="../js/vue.js"></script>
        </head>
        <body>
      
          <div id="root">
            <h2>欢迎打开{{name}}笔记</h2>
            <input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo"><br/>
            <input type="text" placeholder="按下tab提示输入" @keydown.tab="showInfo"><br/>
            <input type="text" placeholder="按下回车提示输入" @keydown.huiche="showInfo"><br/>
          </div>
      
          <script type="text/javascript">
            Vue.config.productionTip = false	// 阻止 vue 在启动时生成生产提示。
            Vue.config.keyCodes.huiche = 13		// 定义了一个别名按键
      
            new Vue({
              el: '#root',
              data: {
                name: 'cess'
              },
              methods: {
                showInfo(e) {
                  // console.log(e.key,e.keyCode)
                  console.log(e.target.value)
                }
              },
            })
          </script>
        </body>
      </html>
      ```
    
    - 在监听**键盘事件**时，外面经常需要**判断详细的按键**。此时，可以为**键盘相关的事件**添加**按键修饰符**，例如：
    
    - ```html
              <!-- 只有在'key'是'Esc'的时候调用'vm.clearInput()'' -->
              <input type="text" @keyup.esc="clearInput">
              <!-- 只有在'key'是'Enter'的时候调用'vm.submit()'' -->
              <input type="text" @keyup.enter="submit">
      ```



- 双向绑定指令

  - vue提供了**v-model双向数据绑定**指令，用来辅助开发者在**不操作DOM**的前提下，**快速获取表单的数据**

    ```html
     <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
        <div id="app">
            <p>用户的名字是: {{ username }}</p>
            <!-- 此处为双向，修改此处可以影响到所有username -->
            <input type="text" v-model="username">
    
            <hr>
            <!-- 此处为单向，修改此处无法影响到上面 -->
            <input type="text" :value="username">
            <hr>
            <!-- 不能渲染，只有表单元素可以用v-model -->
            <div v-model="username"></div>
            <hr>
            <select v-model="city">
                <option value="">请选择城市</option>
                <option value="1">北京</option>
                <option value="2">上海</option>
                <option value="3">广州</option>
            </select>
        </div>
    
        <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
        <script src="./vue.js"></script>
        <!-- 2.创建vue的实例对象 -->
        <script>
            // 创建Vue实例对象
            const vm = new Vue({
                //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                el:'#app',
                // data对象就是要渲染到页面上的数据
                data:{
                    username:'ls1',
                    city:''
                }
            })
        </script>
    ```

  - **注意：表单元素  input输入框（type="radio";type="checkbox";type="xxxx")   ,  textarea  ,   select可以使用**

    

  - 收集表单数据

    - 若`<input type="text"/>`（文本）,则`v-model`收集的是`value`值，用户输入的内容就是`value`值

    - 若`<input type="radio"/>`(单选），则`v-model`收集的是`value`值，且要给标签配置`value`属性
    
    - 若`<input type="checkbox"/>`（多选）
    
      - 没有配置`value`属性，那么收集的是`checked`属性（勾选or未勾选，是布尔值）

      - 配置了`value`属性
    
        - `v-model`的初始值是非数组，那么收集的就是`checked`（勾选or未勾选，是布尔值）
        - `v-model`的初始值是数组，那么收集的就是`value`组成的数组
    
        
    
  - v-model指令的修饰符

    - 为了**方便对用户输入的内容进行处理**，vue对v-model指令提供了3个修饰符，分别是：

      | 修饰符  | 作用                                                         | 示例                          |
      | ------- | ------------------------------------------------------------ | ----------------------------- |
      | .number | 自动将用户的输入值转为数值类型                               | <input v-model.number="age"/> |
      | .trim   | 自动过滤用户输入的首位空白字符                               | <input v-model.trim="msg"/>   |
      | .lazy   | 在"change"时 而非 "input" 时更新(在选中input时不会更新，到input失去焦点再更新数据源) | <input v-model.lazy="msg"/>   |

    - ```html
      <!-- 3.希望Vue能够控制下面这个div，帮我们把数据填充到div内部 -->
          <div id="app">
              <!-- 若没有加.number，则此处为字符串拼接，如45+5=455 -->
              <input type="text" v-model.number="n1"> + <input type="text" v-model.number="n2"> = <span>{{ n1 + n2 }}</span>
              <hr>
              <input type="text" v-model.trim="username">
              <button @click="showName">获取用户名</button>
              <hr>
              <input type="text" v-model.lazy="username">
          </div>
      
          <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
          <script src="./vue.js"></script>
          <!-- 2.创建vue的实例对象 -->
          <script>
              // 创建Vue实例对象
              const vm = new Vue({
                  //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                  el:'#app',
                  // data对象就是要渲染到页面上的数据
                  data:{
                      username:'ls1',
                      n1:1,
                      n2:2
                  },
                  methods: {
                      showName() {
                          console.log(`用户名是: "${this.username}"`);
                      }
                  }
              })
          </script>
      ```

      

- 条件渲染指令

  - 条件渲染指令用来辅助开发者按需控制DOM的显示与隐藏。条件渲染指令有如下两个，分别是：

    - v-if

    - v-show

    - 示例用法如下：

      ```html
          <div id="app">
              <p v-if="flag">这是被v-if控制的元素</p>
              <p v-show="flag">这是被v-show控制的元素</p>
          </div>
      
      <!-- 1.导入Vue的库文件，在window全局有了Vue这个构造函数 -->
          <script src="./vue.js"></script>
          <!-- 2.创建vue的实例对象 -->
          <script>
              // 创建Vue实例对象
              const vm = new Vue({
                  //el属性是固定的写法，表示当前vm实例要控制页面上的那个区域，接收的值是一个选择器
                  el:'#app',
                  // data对象就是要渲染到页面上的数据
                  data:{
                      //如果flag为true，则显示被控制的元素，如果为false则隐藏被控制的元素
                      flag:true
                  }
              })
          </script>
      ```

  - `v-show`的原理是：动态为元素添加或移除`display:none`样式，来实现元素的显示和隐藏

    - 如果要频繁的切换元素的显示状态，用v-show性能会更好

  - `v-if`的原理是：每次动态创建或移除元素，实现元素的显示和隐藏

    - 如果刚进入页面的时候，某些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示出来，此时v-if性能更好

  - 在实际开发中，绝大多数情况不用考虑性能问题，直接使用v-if就好了

  - v-else

    - v-if可以单独使用，或配合v-else指令一起使用：

      ```html
      <div v-if="Math.random() > 0.5">
          随机数大于0.5
      </div>
      <div v-else>
          随机数小于0.5
      </div>
      ```

    - 注意：v-else指令**必须配合**v-if指令一起使用，否则他将不会被识别

  - v-else-if

    - v-else-if指令充当v-if的"else-if"块，可以连续使用

      ```html
      <div v-if="type === 'A'">优秀</div>
      <div v-else-if="type === 'B'">良好</div>
      <div v-else-if="type === 'C'">一般</div>
      <div v-else>差</div>
      ```

    - 注意：v-else-if指令**必须配合**v-if指令一起使用，否则他将不会被识别

    
    
  - 动态参数

    - 也可以在指令参数中使用 JavaScript 表达式，方法是用方括号括起来：

      ```html
      <!-- 注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。 -->
      <a v-bind:[attributeName]="url"> ... </a>
      ```
    
    - 这里的 `attributeName` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果组件实例有一个 data property `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。
    
    - 同样地，可以使用动态参数为一个动态的事件名绑定处理函数：
    
      ```html
      <a v-on:[eventName]="doSomething"> ... </a>
      
      <-- 在这个示例中，当 eventName 的值为 "focus" 时，v-on:[eventName] 将等价于 v-on:focus -->
      ```
    
    - **对动态参数值约定**：动态参数预期会求出一个字符串，`null` 例外。
    
      这个特殊的 `null` 值可以用于显式地移除绑定。任何其它非字符串类型的值都将会触发一个警告。
    
    - **对动态参数表达式约定**：动态参数表达式有一些语法约束，因为某些字符，如**空格和引号，放在[ ]里是无效的**。例如：
    
      ```html
      <!-- 这会触发一个编译警告 -->
      <a v-bind:['foo' + bar]="value"> ... </a>
      ```
    
      
    
  - 绑定样式

    - class样式

      - 写法： `:class="xxx"`，xxx可以是字符串、数组、对象
      - `:style=[a,b]`,其中a、b是样式对象
      - `:style="{fontSize: xxx}"`，其中xxx是动态值

    - 注意：

      - 字符串写法适用于：类名不确定，要动态获取
      - 数组写法适用于：要绑定多个样式，个数不确定，名字也不确定
      - 对象写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

    - ```html
      <style>
        .basic {width: 300px;height: 50px;border: 1px solid black;}
        .happy {border: 3px solid red;background-color: rgba(255, 255, 0, 0.644);
          background: linear-gradient(30deg, yellow, pink, orange, yellow);}
        .sad {border: 4px dashed rgb(2, 197, 2);background-color: skyblue;}
        .normal {background-color: #bfa;}
        .atguigu1 {background-color: yellowgreen;}
        .atguigu2 {font-size: 20px;text-shadow: 2px 2px 10px red;}
        .atguigu3 {border-radius: 20px;}
      </style>
      
      <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div><br/><br/>
      
        <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
        <div class="basic" :class="classArr">{{name}}</div><br/><br/>
      
        <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj">{{name}}</div><br/><br/>
      
        <!-- 绑定style样式--对象写法 -->
        <div class="basic" :style="styleObj">{{name}}</div><br/><br/>
      
        <!-- 绑定style样式--数组写法 -->
        <div class="basic" :style="styleArr">{{name}}</div>
      </div>
      
      <script type="text/javascript">
        Vue.config.productionTip = false
      
        const vm = new Vue({
          el: '#root',
          data: {
            name: '尚硅谷',
            mood: 'normal',
            classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
            classObj: {
              atguigu1: false,
              atguigu2: false,
            },
            styleObj: {
              fontSize: '40px',
              color: 'red',
            },
            styleObj2: {
              backgroundColor: 'orange'
            },
            styleArr: [
              {
                fontSize: '40px',
                color: 'blue',
              },
              {
                backgroundColor: 'gray'
              }
            ]
          },
          methods: {
            changeMood() {
              const arr = ['happy', 'sad', 'normal']
              const index = Math.floor(Math.random() * 3)
              this.mood = arr[index]
            }
          },
        })
      </script>
      ```

      

    

- 列表渲染指令

  - vue提供了**v-for**列表渲染指令，用来辅助开发者**基于一个数组来循环渲染一个列表结构**。v-for指令需要使用**item in items**形式的特殊语法，其中：

    - items是**待循环的数组**

    - item是**被循环的每一项**

    - ```js
      data: { 
          list: [      //列表数据
              {id:1,name:'zs'},
              {id:2,name:'ls'}
          ]
      }
      
      //------分割线-----------
      <ul>
          <li v-for="item in list">姓名是: {{item.name}}</li>
      </ul>
      ```

  - v-for中 的索引

    - v-for指令还支持一个**可选的**第二个参数，即**当前项的索引**。语法格式为**(item,index) in  items**

    - 示例代码：

      ```js
      data: { 
          list: [      //列表数据
              {id:1,name:'zs'},
              {id:2,name:'ls'}
          ]
      }
      
      //------分割线-----------
      <ul>
          <li v-for="(item,index) in list">索引是： {{index}}，姓名是: {{item.name}}</li>
      </ul>
      ```

    - 注意：v-for指令中的**item项**和**index索引**都是形参，可以根据需要进行**重命名**。例如**(user,i) in userlist**

  - 如果使用了v-for，要绑定一个key属性

    - ```html
      <div id="app">
              <table class="table table-bordered table-hover table-striped">
                  <thead>
                      <th>索引</th>
                      <th>Id</th>
                      <th>姓名</th>
                  </thead>
                  <tbody>
                      <!-- 只要用到了v-for指令，那么一定要绑定一个：key属性 -->
                      <!-- 而且，尽量把id作为key的值 -->
                      <!-- 官方对key的值类型，是有要求的：字符串或数字类型 -->
                      <!-- key的值是不能重复的，否则会报错 -->
                      <tr v-for="(item,index) in list" :key="item.id" :title="item.name + index">
                          <td>{{ index }}</td>
                          <td>{{ item.id }}</td>
                          <td>{{ item.name }}</td>
                      </tr>
                  </tbody>
              </table>
          </div>
      ```

    - key的注意事项：

      - key的值只能是**字符串**或**数字**类型
      - key的值**必须具有唯一性**（即：key的值不能重复）
      - 建议把**数据项id属性的值**作为key的值（因为id属性的值具有唯一性）
      - 使用**index的值**当作key的值**没有任何意义**（因为index的值不具有唯一性）
      - 建议使用v-for指令时**一定要指定key的值**（既提升性能，又防止列表状态紊乱）
      
    - key的作用和原理
    
      - ![](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643033767087-2558e992-b48b-4b54-a9b8-86eb8534bd98.png)
    
        ![img](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643033764359-6a37a493-bb51-4b3b-8b14-822a3df68d6e.png)
    
      - key的内部原理
    
        - `虚拟DOM`中`key`的作用：**`key`是`虚拟DOM`中对象的标识**，当数据发生变化时，**`Vue`会根据<u>新数据</u>生成<u>新的</u>`虚拟DOM`，随后`Vue`进行新`虚拟DOM`与旧`虚拟DOM`的差异比较**，比较规则如下
        - 比较规则：
          - 旧`虚拟DOM`中找到了与新`虚拟DOM`相同的`key`
            - 若`虚拟DOM`中内容没变，则直接使用之前的`真实DOM`
            - 若`虚拟DOM`中内容变了，则生成新的`虚拟DOM`，随后替换掉页面中之前的`真实DOM`
          - 旧`虚拟DOM`中未找到与新`虚拟DOM`相同的`key`
            - 创建新的`真实DOM`，随后渲染到页面
        - 用`index`作为`key`可能会引发的问题
          - 若对数据进行逆序添加、逆序删除等**破坏顺序的操作**，会产生没有必要的`真实DOM`更新 --->界面效果没有问题，但是效率低
          - 若结构中还包含**输入类的`DOM`**：会产生错误`DOM`更新 ---> 界面有问题
        - 开发中如何选择`key`
          - 最好使用每条数据的**唯一标识作为`key`**，比如id、手机号、身份证号、学号等唯一值
          - 如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表，使用`index`作为`key`也是没有问题的
    
  - `v-for`也可以遍历一个对象
  
    - 如：
  
      ```js
      <ul id="v-for-object" class="demo">
        <li v-for="value in myObject">
          {{ value }}
        </li>
      </ul>
      
      
      Vue.createApp({
        data() {
          return {
            myObject: {
              title: 'How to do lists in Vue',
              author: 'Jane Doe',
              publishedAt: '2016-04-10'
            }
          }
        }
      }).mount('#v-for-object')
      
      // 结果是将对象的值（How to do那部分）渲染出来
      ```
    
    - 它可以接收三个参数
    
      - ```html
        <li v-for="(value, name, index) in myObject">
          {{ index }}. {{ name }}: {{ value }}
        </li>
        ```
    
      - 第一个参数是值，第二个参数是键名，第三个参数是索引值
    
    - 注意： `v-for`  在遍历对象时，会按 `Object.keys()` 的结果遍历，但是不能保证它在不同 JavaScript 引擎下的结果都一致。
    
      
    
  - 列表过滤
  
    - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643209841363-2dbb95a7-f003-4da1-b456-9a5232f3cf90.png)
  
    - 即如上图，再输入框中输入关键字，下方列表出现具有关键字的数据
  
    - ```html
      <title>列表过滤</title>
      <script type="text/javascript" src="../js/vue.js"></script>
      
      <div id="root">
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <ul>
          <li v-for="(p,index) of filPersons" :key="p.id">
            {{ p.name }}-{{ p.age }}-{{ p.sex }}
          </li>
        </ul>
      </div>
      
      <script type="text/javascript">
        Vue.config.productionTip = false
        // 用 watch 实现
        // #region 
        /* new Vue({
      			el: '#root',
      			data: {
      				keyWord: '',
      				persons: [
      					{ id: '001', name: '马冬梅', age: 19, sex: '女' },
      					{ id: '002', name: '周冬雨', age: 20, sex: '女' },
      					{ id: '003', name: '周杰伦', age: 21, sex: '男' },
      					{ id: '004', name: '温兆伦', age: 22, sex: '男' }
      				],
      				filPersons: []
      			},
      			watch: {
      				keyWord: {
      					immediate: true,
      					handler(val) {
      						this.filPersons = this.persons.filter((p) => {
      							return p.name.indexOf(val) !== -1
      						})
      					}
      				}
      			}
      		}) */
        //#endregion
      
        // 用 computed 实现
        new Vue({
          el: '#root',
          data: {
            keyWord: '',
            persons: [
              { id: '001', name: '马冬梅', age: 19, sex: '女' },
              { id: '002', name: '周冬雨', age: 20, sex: '女' },
              { id: '003', name: '周杰伦', age: 21, sex: '男' },
              { id: '004', name: '温兆伦', age: 22, sex: '男' }
            ]
          },
          computed: {
            filPersons() {
              return this.persons.filter((p) => {
                return p.name.indexOf(this.keyWord) !== -1
              })
            }
          }
        }) 
      </script>
      ```
  
  - 列表排序
  
    - ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643211061137-37aec6e3-9594-434d-b2d9-d31f752992e1.png)
  
    - 如上图，在输入框中输入关键词后，点击按钮，会按照年龄响应排序
  
    - ```html
      <title>列表排序</title>
      <script type="text/javascript" src="../js/vue.js"></script>
      
      <div id="root">
        <h2>人员列表</h2>
        <input type="text" placeholder="请输入名字" v-model="keyWord">
        <button @click="sortType = 2">年龄升序</button>
        <button @click="sortType = 1">年龄降序</button>
        <button @click="sortType = 0">原顺序</button>
        <ul>
          <li v-for="(p,index) of filPersons" :key="p.id">
            {{p.name}}-{{p.age}}-{{p.sex}}
            <input type="text">
          </li>
        </ul>
      </div>
      
      <script type="text/javascript">
        Vue.config.productionTip = false
        new Vue({
          el: '#root',
          data: {
            keyWord: '',
            sortType: 0, // 0原顺序 1降序 2升序
            persons: [
              { id: '001', name: '马冬梅', age: 30, sex: '女' },
              { id: '002', name: '周冬雨', age: 31, sex: '女' },
              { id: '003', name: '周杰伦', age: 18, sex: '男' },
              { id: '004', name: '温兆伦', age: 19, sex: '男' }
            ]
          },
          computed: {
            filPersons() {
              const arr = this.persons.filter((p) => {
                return p.name.indexOf(this.keyWord) !== -1
              })
              //判断一下是否需要排序
              if (this.sortType) {
                arr.sort((p1, p2) => {
                  return this.sortType === 1 ? p2.age - p1.age : p1.age - p2.age
                })
              }
              return arr
            }
          }
        })
      </script>
      ```
  
- v-cloak指令

  - 本质是一个特殊属性，`Vue`实例创建完毕并接管容器后，会删掉`v-cloak`属性

  - 使用`css`配合`v-cloak`可以解决网速慢时页面展示出{{ xxx }}的问题

  - ```html
    <title>v-cloak指令</title>
    
    <style>
      [v-cloak] {
        display:none;
      }
    </style>
    
    <div id="root">
      <h2 v-cloak>{{ name }}</h2>
    </div>
    
    // 够延迟5秒收到vue.js
    <script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>
    
    <script type="text/javascript">
      console.log(1)
      Vue.config.productionTip = false
      new Vue({
        el:'#root',
        data:{name:'cess'}
      })
    </script>
    ```

- v-once指令

  - `v-once`所在节点在初次动态渲染后，就视为静态内容了

  - 以后数据的改变不会引起`v-once`所在结构的更新，可以用于优化性能

  - ```html
    <title>v-once指令</title>
    <script type="text/javascript" src="../js/vue.js"></script>
    
    <div id="root">
      <h2 v-once>初始化的n值是: {{n}}</h2>
      <h2>当前的n值是: {{n}}</h2>
      <button @click="n++">点我n+1</button>
    </div>
    
    <script type="text/javascript">
      Vue.config.productionTip = false
      new Vue({ el: '#root', data: {n:1} })
    </script>
    ```

- v-pre指令

  - 跳过`v-pre`所在节点的编译过程

  - 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

  - ```html
    <title>v-pre指令</title>
    <script type="text/javascript" src="../js/vue.js"></script>
    
    <div id="root">
      <h2 v-pre>Vue其实很简单</h2>
      <h2 >当前的n值是:{{n}}</h2>
      <button @click="n++">点我n+1</button>
    </div>
    
    <script type="text/javascript">
      Vue.config.productionTip = false
      new Vue({ el:'#root', data:{n:1} })
    </script>
    ```

    



##### Vue数据监视

- 数据更新时的一个问题

  - this.persons[0] = {id:'001',name:'马老师',age:50,sex:'男'}更改`data`数据时，`Vue`不监听，模板不改变

  - ```html
    <title>更新时的一个问题</title>
    <script type="text/javascript" src="../js/vue.js"></script>
    
    <div id="root">
      <h2>人员列表</h2>
      <button @click="updateMei">更新马冬梅的信息</button>
      <ul>
        <li v-for="(p,index) of persons" :key="p.id">
          {{p.name}}-{{p.age}}-{{p.sex}}
        </li>
      </ul>
    </div>
    
    <script type="text/javascript">
      Vue.config.productionTip = false
    
      const vm = new Vue({
        el: '#root',
        data: {
          persons: [
            { id: '001', name: '马冬梅', age: 30, sex: '女' },
            { id: '002', name: '周冬雨', age: 31, sex: '女' },
            { id: '003', name: '周杰伦', age: 18, sex: '男' },
            { id: '004', name: '温兆伦', age: 19, sex: '男' }
          ]
        },
        methods: {
          updateMei() {
            // this.persons[0].name = '马老师'	//奏效
            // this.persons[0].age = 50				//奏效
            // this.persons[0].sex = '男'			//奏效
            // this.persons[0] = {id:'001',name:'马老师',age:50,sex:'男'} //不奏效
            this.persons.splice(0, 1, { id: '001', name: '马老师', age: 50, sex: '男' })
          }
        }
      })
    </script>
    ```

- 模拟一个数据监测

  - ```js
    let data = {
      name: '尚硅谷',
      address: '北京',
    }
    
    function Observer(obj) {
      // 汇总对象中所有的属性形成一个数组
      const keys = Object.keys(obj)
      // 遍历
      keys.forEach((k) => {
        Object.defineProperty(this, k, {
          get() {
            return obj[k]
          },
          set(val) {
            console.log(`${k}被改了，我要去解析模板，生成虚拟DOM.....我要开始忙了`)
            obj[k] = val
          }
        })
      })
    }
    
    // 创建一个监视的实例对象，用于监视data中属性的变化
    const obs = new Observer(data)
    console.log(obs)
    
    // 准备一个vm实例对象
    let vm = {}
    vm._data = data = obs
    ```

  - 原理：

    - `vue`会监视`data`中所有层次的数据
    - 如何监测**对象**中的数据
      - 通过`setter`实现监视，且要在`new Vue()`时旧传入要检测的数据
      - 对象创建后追加的属性，Vue不做响应式处理
      - 如需给后添加的属性左响应式，请使用如下API
        - **Vue.set(target,propetyName/index,value)**
        - **vm.$set(target,propetyName/index,value)**
    - 如何监测**数组**中的数据
      - 通过包裹数组更新元素的方法实现，本质就是做了两件事
        - 调用原生对应的方法对数组进行更新
        - 重新解析模板，进而更新页面
      - 在`Vue`修改数组中的某个元素一定要用如下方法
        - **`push()` `pop()` `unshift()` `splice()` `sort()` `reverse()`** 这几个方法被`Vue`重写了**`Vue.set()`或`vm.$set()`**
      - 特别注意：**`Vue.set()`和`vm.$set()`不能给`vm`或`vm的根数据对象`（data等）添加属性**



##### 过滤器

- **过滤器（Filters）**是vue为开发者提供的功能，常用于**文本的格式化**。过滤器可以用在两个地方：**插值表达式**和**v-bind属性绑定**

  - 过滤器应该被添加在JavaScript表达式的**尾部**，由"**管道符**"进行调用，示例代码：

    ```html
    <!-- 在双花括号中通过"管道符"调用capitalize过滤器，对message的值进行格式化 -->
    <p>{{ message | capitalize }}</p>
    
    <!-- 在v-bind中通过"管道符"调用formatId过滤器，对rawId的值进行格式化 -->
    <div v-bind:id='rawId | formatId'></div>
    ```

  - 过滤器的注意点：

    - 要定义到filters节点下，本质是一个函数
    - 在过滤器函数中，一定要有return值
    - 在过滤器的形参中，就可以获取到"管道符"前面待处理的那个值

- **私有过滤器**和**全局过滤器**

  - 在filters节点下定义的过滤器，称为"**私有过滤器**"，因为它**只能在当前vm示例所控制的el区域内使用**。如果希望在**多个vue实例之间共享过滤器**，则可以按照如下的格式定义全局过滤器：

    ```js
    //全局过滤器-独立于每个vm实例之外
    //Vue.filter（）方法接收两个参数
    //第一个参数，是全局过滤器的名字
    //第二个参数，是全局过滤器的"处理函数"
    Vue.filter('capitalize',(str) => {
        return str.charAt(0).toUpperCase() + str.slice(1) + '--'
    })
    ```

  - **如果**全局过滤器和私有过滤器的**名字一致**，此时按照**就近原则**，**调用私有过滤器**

- 连续调用多个过滤器

  - 过滤器可以**串联地**进行调用，例如：

    ```js
    // 把massage的值，交给filterA进行处理
    // 把filterA的返回值交给filterB进行处理
    // 把filterB的返回值作为最终的值渲染到页面上
    {{ message | filterA | filterB }}
    ```

- 过滤器传参

  - 过滤器的本质是JavaScript函数，因此可以接收参数，格式如下：

    ```js
    // arg1 和 arg2 是传递给 filterA 的参数
    <p>{{ message | filterA(arg1,arg2) }}</p>
    
    // 过滤器处理函数的形参列表中：
    //    第一个参数：永远都是"管道符"前面待处理的值
    //    从第二个参数开始，才是调用过滤器时传递过来的 arg1 和 arg2参数
    Vue.filter('filterA',(msg,arg1,arg2) => {
        //过滤器的逻辑代码
    })
    ```

- 过滤器的兼容性

  - 过滤器仅在vue2.x和1.x中支持，在**vue3.x的版本中剔除了过滤器相关的功能**
  - 在企业级项目开发中：
    - 如果使用的是2.x版本的vue，则依然可以使用过滤器相关的功能
    - 如果项目已经升级到了3.x版本的vue，建议使用**计算属性或方法**代替被剔除的过滤器功能

  