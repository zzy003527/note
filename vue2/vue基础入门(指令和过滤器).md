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

  - 按键修饰符

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

    - d

  - v-else-if

    - v-else-if指令充当v-if的"else-if"块，可以连续使用

      ```html
      <div v-if="type === 'A'">优秀</div>
      <div v-else-if="type === 'B'">良好</div>
      <div v-else-if="type === 'C'">一般</div>
      <div v-else>差</div>
      ```

    - 注意：v-else-if指令**必须配合**v-if指令一起使用，否则他将不会被识别

      

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

  