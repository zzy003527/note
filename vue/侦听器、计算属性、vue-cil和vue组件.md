## 侦听器、计算属性、vue-cil和vue组件

#### Watch侦听器

- 什么是watch侦听器

  - **watch侦听器**允许开发者监视数据的变化，从而**针对数据的变化做特定的操作**

  - 语法格式：

    ```js
    const vm = new Vue({
        el:'#app',
        data: { username:'' },
        //所有的侦听器，都应该被定义到watch节点下
        watch: {
             //监听username值的变化
             //侦听器本质上是一个函数，要监视哪个数据的变化，就把数据名作为方法名即可
             //newVal是"变化后的新值"，oldVal是"变化之前的旧值",注意要新值在前，旧值在后
            username(newVal,oldVal) {
                console.log(newVal,oldVal)
            }
        }
    })
    ```

- 使用watch检测用户名是否可用

  - 监听username值的变化，并使用axios发起Ajax请求，检测当前输入的用户名是否可用：

    ```js
    watch: {
        //监听username值的变化
        async username(newVal) {
            if(newVal === '') {
                return
            }
            //使用axios发起请求，判断用户名是否可用
            const { data:res } = await axios.get('https://www.escook.cn/api/finduser/' + newVal)
            console.log(res)
        }
    }
    ```

- 侦听器的格式

  - 方法格式的侦听器

    - 缺点1：无法在刚进入页面的时候，自动触发
    - 缺点2：如果侦听的是一个对象，如果对象中的属性发生了变化，不会触发侦听器

  - 对象格式的侦听器

    - 好处1：可以通过**immediate**选项，让侦听器自动触发

      - immediate选项的默认值是false

      - immediate的作用是：控制侦听器是否自己触发一次

      - ```js
         watch: {
                        //定义对象格式的侦听器
                        username: {
                            //侦听器的处理函数
                            handler(newVal,oldVal) {
                                console.log(newVal,oldVal);
                            },
                            //immediate的默认值是false
                            //immediate的作用是：控制侦听器是否自动触发一次
                            immediate:true
                        }  
                    }
        ```

    - 好处2：可以通过deep选项，让侦听器深度监听对象中每个属性的变化

      - deep选项的默认值为false

      - deep的作用是：让侦听器深度监听对象中每个属性的变化

      - ```js
         watch: {
                        info: {
                            handler(newVal) {
                                console.log(newVal);
                            },
                            //开启深度监听，只要对象中任何一个属性变化，都会触发侦听器
                            deep:true
                        }
                    }
        ```

  - 监听对象单个属性的变化

    - 如果要侦听的是**对象的子属性**,则必须包裹一层单引号

    - ```js
      'info.username'(newVal) {
               console.log(newVal);
             }
      ```







#### 计算属性

- 什么是计算属性

  - 计算属性指的是**通过一系列运算**之后，最终得到一个**属性值**

  - **这个动态计算出来的属性值**可以被模板结构或methods方法使用。示例代码：

    ```js
            <!-- 专门用户呈现颜色的div盒子 -->
            <div class="box" :style="{ backgroundColor: rgb }">
                {{ rgb }}
            </div>
            <button @click="show">按钮</button>
    
    
    var vm = new Vue({
        el:'#app',
        data: {
            r:0,g:0,b:0
        },
        //所有计算属性都要放到computed中
        computed: {
            rgb() {
                return `rgb(${this.r},${this.g},${this.b})`
            }
        },
        method: {
            show() {
                console.log(this.rgb)
            }
        }
    })
    ```

  - 特点：

    - 在定义的时候，要定义为"方法"
    - 在使用计算属性的时候，当普通的属性使用即可

  - 好处：

    - 实现了代码的复用
    - 只要计算属性中依赖的数据与变化了，则计算属性会重新求值







#### axios

- axios是一个专注于网络请求的库

- axios的基础语法

  - 基本语法如下：

    ```js
    axios({
        //请求方式
        method:'请求的类型',
        //请求的地址
        url:'请求的URL地址',
        //URL中的查询参数(get用)
        params: {},
        //请求体参数(post用)
        data: {}
    }).then(result) => {
        //.then用来指定请求成功之后的回调函数
        //形参中的result是请求成功之后的结果
    }
    ```

  - 调用axios得到的返回值是Promise对象

- axios的基本使用

  - 发起GET请求

    ```js
    axios({
        //请求方式
        method:'GET',
        //请求的地址
        url:'http://www.liulongbin.top:3006/api/getbooks',
        //URL中的查询参数(get用)
        params: {
            id: 1
        }
    }).then(result) => {
        console.log(result)
    }
    ```

  - 发起POST请求

    ```js
    <button id="btnPost">发起POST请求</button>
    
    document.querySelector('#btnPost').addEvenetListener('click',async function() {
    //如果调用某个方法的返回值是Promise实例，则前面可以添加await
    //await只能用在被async"修饰"的方法中
        const {data} = await axios({
        //请求方式
        method:'POST',
        //请求的地址
        url:'http://www.liulongbin.top:3006/api/post',
        //请求体参数(post用)
        data: {
            name:'zs',
            age:20
        }
      })
        //data是服务器返回的真实数据 
        console.log(data)
    })
    ```

  - 使用async、await和结构赋值发生GET请求

    ```js
    <button id="btnGet">发起GET请求</button>
    
    document.querySelector('#btnGet').addEvenetListener('click',async function() {
    //如果调用某个方法的返回值是Promise实例，则前面可以添加await
    //await只能用在被async"修饰"的方法中
        //解构赋值时，使用：进行重命名
        const {data:res} = await axios({
        //请求方式
        method:'GET',
        //请求的地址
        url:'http://www.liulongbin.top:3006/api/getbooks'
      })
        console.log(res.data)
    })
    ```

  - 步骤：

    - 调用axios之后，使用async、await进行简化
    - 使用解构赋值，从axios封装的大对象中，把data属性解构出来
    - 把解构出来的data属性，使用 冒号 进行重命名，一般都重命名为{ data：res }

- 使用axios.get和axios.post发送请求

  - axios.get

    - 语法格式：

      ```js
      axios.get('URL地址',{
          //GET参数
          params:{}
      })
      ```

    - 

    - d

  - axios.post

    - 语法格式：

      ```js
      axios.post('URL地址',{post请求数据})
      ```







#### vue-cli

- 单页面应用程序

  - 什么是单页面应用程序
    - **单页面应用程序**，简称SPA，指的是**一个Web网站只有唯一的一个HTML页面**，所有的功能与交互都在这唯一的一个页面内完成。

- 什么是vue-cli

  - **vue-cli是Vue.js开发的标准工具**。它简化了程序员基于webpack创建工程化的Vue项目的过程
  - 中文官网: https://cli.vuejs.org/zh/

- 安装

  - vue-cli是npm上的一个**全局包，使用npm install命令**，即可安装：

    ```js
    npm install -g @vue/cli
    ```

- 使用

  - 在要创建项目的文件夹的cmd，运行：

    ```js
    vue create 项目的名称
    ```

  - 之后选择 Manually select features，回车

  - 选择Babel，CSS Pre-processors，回车（目前先不用Linter）

  - 选择vue的版本，回车

  - 选择Less（css预编译），回车

  - 选择In dedicated config files（将Babel、ESlint等插件的配置项，放到自己独立的配置文件中），回车

  - 选择y/N是否保存当前配置

  - 若选y，则需要再命名一个预设的名字,回车

- vue项目中src目录的构成

  - assets文件夹：存放项目中用到的静态资源文件，例如：css样式表、图片资源
  - components文件夹：程序员封装的、可复用的组件，都要放到components目录下
  - main.js ： 是项目的入口文件，整个项目的运行，要先执行main.js
  - App.vue ： 是项目的根组件（写什么，项目中就看到什么）（把原来的删了，然后自己写，外部要加template标签）

- vue项目的运行流程

  - 在工程化的项目中，vue要做的事情：通过**main.js**把**App.vue**渲染到**index.html**的指定区域中
  - 其中：
    - **App.vue**用来编写待渲染的**模板结构**
    - **index.html**中需要预留一个**el区域**
    - **main.js**把App.vue渲染到了index.html所预留的区域中
  - Vue实例的$mount()方法，作用和el属性完全一样









#### vue组件

- 什么是组件化开发

  - **组件化开发**指的是：根据**封装**的思想，**把页面上可重用的UI结构封装为组件**，从而方便项目的开发和维护

- vue中的组件化开发

  - vue是一个**支持组件化开发**的前端框架
  - vue中规定：**组件的后缀名**是**.vue**

- vue组件的三个组成部分

  - **template** -> 组件的**模板结构**

  - **script** -> 组件的**JavaScript行为**

  - **style** -> 组件的**样式**

  - 例子：

    ```html
    <template>
        <div>
            <div class="test-box">
                <h3>这是用户自定义的Test.vue --- {{ username }} </h3>
            </div>
        </div>
    </template>
    
    
    <script>
    export default {
        //注意：.vue组件中的data不能像之前一样，不能指向对象
        //组件中的data必须是一个函数
        // data: {
        //     username:'zs'
        // }
        data() {
            //这个return出去的{}中可以定义数据
            return {
                username:'zs'
            }
        },
        methods: {
            changeName() {
                //在组件中，this就表示当前组件的实例对象
                //此处的this是组件的实例
                this.username = 'ls'
            }
        }
    }
    </script>
    
    
    <style>
    .test-box {
        background-color: pink;
    }
    </style>
    ```

  - **在组件中，this就表示当前组件的实例对象**

  - **在template中，只能有一个根元素，所以要用一个大div把自己的结构包裹进去**

  - **若要启用less，则在style加一个属性 <u>lang="less"</u>**

- 组件之间的**父子关系**

  - 组件在被封装好了之后，**彼此之间是相互独立的**，不存在父子关系
  - 在**使用组件**的时候，**根据彼此的嵌套关系**，形成了**父子关系，兄弟关系**

- 使用组件的**三个步骤**

  - 步骤1：使用import语法**导入需要的组件**

  - 步骤2：使用**components**节点注册组件

  - 步骤3：**以标签形式**使用刚才注册的组件

  - ```html
    <template>
        <div class="app-container">
            <h1>App根组件</h1>
            <hr>
    
            <div class="box">
                <!-- 渲染Left组件和Right组件 -->
                <!-- 以标签形式使用注册好的组件 -->
                <Left></Left>
                <Right></Right>
            </div>
    
        </div>
    </template>
    
    <script>
    //导入需要使用的.vue组件
    import Left from '@/components/Left.vue'
    import Right from '@/components/Right.vue'
    
    export default {
        //使用components属性注册组件
        components: {
            'Left' : Left,
            'Right': Right
        }
    }
    </script>
    
    <style lang='less'>
    .app-container {
        padding: 1px 20px 20px;
        background-color: #efefef;
    }
    </style>
    
    ```

  - 通过components注册的是**私有子组件**

    - 例如：在**组件A**的components节点下，注册了**组件F**。则组件F只能用在组件A中，不能被用在**组件C**中

- 注册**全局组件**

  - 在vue项目的**main.js**入口文件中，通过**Vue.component()**方法，可以注册全局组件。

  - 示例代码：

    ```js
    //导入需要全局注册的组件
    import Count from '@/components/Count.vue'
    
    //参数1：字符串格式，表示组件的“注册名称”
    //参数2：需要被全局注册的那个组件
    Vue.component('MyCount',Count)
    ```

- 组件的**props**

  - props是组件的**自定义属性**，在**封装通用组件**的时候，合理地使用props可以极大的**提高组件的复用性**

  - 语法格式：

    ```js
    //在绑定自定义属性的时候，前面加上v-bind，可以让字符串变成数字
    <MyCount :init="6"></MyCount>
    
    export default {
        //组件的自定义属性
        props: ['自定义属性A','自定义属性B','其他自定义属性...']
        
        //组件的私有数据
        data() {
            return { }
        }
    }
    ```

  - props是只读的

    - vue规定：组件中封装的自定义属性是**只读的**，程序员**不能直接修改**props的值，否则会直接报错

    - 要想修改props的值，可以**把props的值转存到data中**，因为data中的数据都是可读可写的

      ```js
        props: ['init'],
        data() {
          return {
            count: this.init      //把this.init的值转存到count中
          }
        },
      ```

  - props的**default**默认值

    - 在声明自定义属性时，可以通过**default**来**定义属性的默认值。**

    - 示例代码：

      ```js
      export default {
          props: {
              init: {
                  //用default属性定义属性的默认值
                  default: 0
              }
          }
      }
      ```

  - props的**type**值类型

    - 在声明自定义属性的时候，可以通过type来定义属性的值类型

    - 示例代码：

      ```js
      export default {
          props: {
              init: {
                  //用default属性定义属性的默认值
                  default: 0
                  //用type属性定义属性的值类型
                  //如果传递过来的值不符合此类型，则会在终端报错
                  type:Number
              }
          }
      }
      ```

  - props的**required**必填项

    - 在拥有此项后，如果使用count的时候没有对拥有此项的自定义属性传值的话，就会报错

    - 示例代码：

      ```js
      export default {
          props: {
              init: {
                  //用default属性定义属性的默认值
                  default: 0
                  //用type属性定义属性的值类型
                  //如果传递过来的值不符合此类型，则会在终端报错
                  type:Number
                  required: true
              }
          }
      }
      ```

- 组件之间的样式冲突

  - 默认情况下，**写在.vue组件中的样式会全局生效**，因此很容易造成**多个组件之间的样式冲突问题**

  - 导致组件之间样式冲突的根本原因是：

    - 单页面应用程序中，所有组件的DOM结构，都是基于**唯一的index.html页面**进行呈现的
    - 每个组件中的样式，都会**影响整个index.html页面**中的DOM元素

  - **解决方法**

    - **给组件的style标签加上scoped属性**

    - 但是：在加上之后，在父组件中的样式无法对子组件生效。因此，在对子组件的元素添加样式时，在选择器前面加上         **：：v-deep**

    - 例子：

      ```html
      <style scoped>
      .kl{
          float: left;
          background-color: orange;
          width: 50%;
          height: 300px;
      }
      h3 {
          color: red;
      }
      ::v-deep h5 {
          color: pink;
      }
      </style>
      ```

- 

- 

- 