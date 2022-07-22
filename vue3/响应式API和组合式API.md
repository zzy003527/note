## 响应式API和组合式API

#### 组合式API

- 基础

  - 什么是组合式API

    - 组合式 API 是一系列 API 的集合，使我们可以使用函数而不是声明选项的方式书写 Vue 组件。它是一个概括性的术语，涵盖了以下方面的 API：
      - 响应性 API：例如`ref()`和`reactive()`，使我们可以直接创建响应式状态、计算属性和侦听器。
      - 生命周期钩子：例如`onMounted()`和`onUnmounted()`，使我们可以在组件各个生命周期阶段添加逻辑。
      - 依赖注入：例如`provide()`和`inject()`，使我们可以在使用响应性 API 时，利用 Vue 的依赖注入系统。

  - `setup`组件选项

    - 为了开始使用组合式 API，我们首先需要一个可以实际使用它的地方。在 Vue 组件中，我们将此位置称为 `setup`。

      新的 `setup` 选项在组件被创建**之前**执行，一旦 `props` 被解析完成，它就将被作为组合式 API 的入口。

    - 注意：**在 `setup` 中应该避免使用 `this`，**因为它不会找到组件实例。`setup` 的调用发生在 `data` 、`computed`  或 `methods` 被解析之前，所以它们无法在 `setup` 中被获取。

    - `setup` 选项是一个接收 `props` 和 `context` 的函数。此外，我们将 `setup` 返回的所有内容都暴露给组件的其余部分 (计算属性、方法、生命周期钩子等等) 以及组件的模板。

    - 把 `setup` 添加到组件中：

      ```js
      // src/components/UserRepositories.vue
      
      export default {
        components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
        props: {
          user: {
            type: String,
            required: true
          }
        },
        setup(props) {
          console.log(props) // { user: '' }
      
          return {} // 这里返回的任何内容都可以用于组件的其余部分
        }
        // 组件的“其余部分”
      }
      ```

    - 

  - 带`ref`的响应式变量

    - 在 Vue 3.0 中，可以通过一个新的 `ref` 函数**使任何响应式变量在任何地方起作用**，如下所示：

      ```js
      import { ref } from 'vue'
      
      const counter = ref(0)
      ```

    - `ref` 接收参数并将其包裹在一个带有 `value` 的对象中返回，然后可以使用该 value 访问或更改响应式变量的值：

      ```js
      import { ref } from 'vue'
      
      const counter = ref(0)
      
      console.log(counter) // { value: 0 }
      console.log(counter.value) // 0
      
      counter.value++
      console.log(counter.value) // 1
      ```

  - 在`setup`内注册声明周期钩子

    - 组合式 API 上的生命周期钩子与选项式 API 的名称相同，但前缀为 `on`：即 `mounted` 看起来会像 `onMounted`。

    - 这些函数接受一个回调，当钩子被组件调用时，该回调将被执行。

    - 将其添加到 `setup` 函数中：

      ```js
      // src/components/UserRepositories.vue `setup` function
      import { fetchUserRepositories } from '@/api/repositories'
      import { ref, onMounted } from 'vue'
      
      // 在我们的组件中
      setup (props) {
        const repositories = ref([])
        const getUserRepositories = async () => {
          repositories.value = await fetchUserRepositories(props.user)
        }
      
        onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`
      
        return {
          repositories,
          getUserRepositories
        }
      }
      ```

    - 现在我们需要对 user prop 的变化做出反应。为此，我们将使用独立的 watch 函数。

  - `watch`响应式更改

    - 就像我们在组件中使用 `watch` 选项并在 `user` 上设置侦听器一样，我们也可以使用从 Vue 导入的 `watch` 函数执行相同的操作。它接受 3 个参数：

      - 一个想要侦听的**响应式引用**或 getter 函数
      - 一个回调
      - 可选的配置选项

    - 例子：

      - ```js
        import { ref, watch } from 'vue'
        
        const counter = ref(0)
        watch(counter, (newValue, oldValue) => {
          console.log('The new counter value is: ' + counter.value)
        })
        ```

      - 每当 `counter` 被修改时，例如 `counter.value=5`，侦听将触发并执行回调 (第二个参数)，在本例中，它将把 `'The new counter value is:5'` 记录到控制台中。

      - 等效的选项式API

        ```js
        export default {
          data() {
            return {
              counter: 0
            }
          },
          watch: {
            counter(newValue, oldValue) {
              console.log('The new counter value is: ' + this.counter)
            }
          }
        }
        ```

  - 独立的`computed`属性

    - 与 `ref` 和 `watch` 类似，也可以使用从 Vue 导入的 `computed` 函数在 Vue 组件外部创建计算属性

    - 例子：

      - ```js
        import { ref, computed } from 'vue'
        
        const counter = ref(0)
        const twiceTheCounter = computed(() => counter.value * 2)
        
        counter.value++
        console.log(counter.value) // 1
        console.log(twiceTheCounter.value) // 2
        ```

      - 这里给 `computed` 函数传递了第一个参数，它是一个类似 getter 的回调函数，输出的是一个*只读*的**响应式引用**。为了访问新创建的计算变量的 **value**，我们需要像 `ref` 一样使用 `.value` 。

        

- Setup

  - 理解：Vue3中一个新的配置项，值为一个函数

  - 组件中所用到的：数据、方法等，均要配置在setup中

  - `setup`函数的两种返回值：

    1. 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用

    2. 若返回一个渲染函数：则可以自定义渲染内容

    3. 如：

       ```vue
       <template>
           <h1>我是App根组件</h1>
           <h2>姓名: {{name}}</h2>
           <h2>年龄: {{age}}</h2>
           <button @click="sayHello">说话</button>
       </template>
       
       <script>
       export default {
           name: 'App',
           setup() {
               // 数据
               let name = '张三'
               let age = 18
       
               // 方法
               function sayHello() {
                   alert(`我叫${name},我${age}岁了,你好啊！`)
               }
       
               // 返回一个对象（常用）
               return {
                   name,
                   age,
                   sayHello
               }
       
               // 返回一个函数（渲染函数）
               // return () => h('h1', '哈哈啊哈')
           }
       }
       </script>
       
       <style>
       </style>
       
       ```

  - 注意点：

    1. 尽量不要与Vue2混用
       - vue2的配置（data、methods、computed等）中**可以访问到**setup中的属性、方法
       - 但是在setup中**不能访问到**vue2的配置（data、methods、computed等）
       - 如果有重名，setup优先

    2. setup不能是一个async函数 ，因为返回值不再是return的对象，而是promise，模板看不到return对象中的属性(后期也可以返回一个Promise实例，但需要Suspense和异步组件的配合)

  - 使用 `setup` 函数时，它将接收两个参数：

    1. `props`
    2. `context`

  - Props

    - `setup` 函数中的第一个参数是 `props`。正如在一个标准组件中所期望的那样，`setup` 函数中的 `props` 是响应式的，当传入新的 prop 时，它将被更新。

    - ```js
      // MyBook.vue
      
      export default {
        props: {
          title: String
        },
        setup(props) {
          console.log(props.title)
        }
      }
      ```

    - 注意：因为 `props` 是响应式的，所以**不能使用 ES6 解构**，它会消除 prop 的响应性

    - 如果需要解构 prop，可以在 `setup` 函数中使用 `toRefs` 函数来完成此操作：

      ```js
      // MyBook.vue
      
      import { toRefs } from 'vue'
      
      setup(props) {
        const { title } = toRefs(props)
      
        console.log(title.value)
      }
      ```

    - 如果 `title` 是可选的 prop，则传入的 `props` 中可能没有 `title` 。在这种情况下，`toRefs` 将不会为 `title` 创建一个 ref 。需要使用 `toRef` 替代它：

      ```js
      // MyBook.vue
      import { toRef } from 'vue'
      setup(props) {
        const title = toRef(props, 'title')
        console.log(title.value)
      }
      ```

  - Context

    - 传递给 `setup` 函数的第二个参数是 `context`。`context` 是一个普通 JavaScript 对象，暴露了其它可能在 `setup` 中有用的值：

      ```js
      // MyBook.vue
      
      export default {
        setup(props, context) {
          // Attribute (非响应式对象，等同于 $attrs)
          console.log(context.attrs)
      
          // 插槽 (非响应式对象，等同于 $slots)
          console.log(context.slots)
      
          // 触发事件 (方法，等同于 $emit)
          console.log(context.emit)
      
          // 暴露公共 property (函数)
          console.log(context.expose)
        }
      }
      ```

    - `context` 是一个普通的 JavaScript 对象，也就是说，它不是响应式的，这意味着你可以安全地对 `context` 使用 ES6 解构。

      ```js
      // MyBook.vue
      export default {
        setup(props, { attrs, slots, emit, expose }) {
          ...
        }
      }
      ```

    - `attrs` 和 `slots` 是有状态的对象，它们总是会随组件本身的更新而更新。这意味着你应该避免对它们进行解构，并始终以 `attrs.x` 或 `slots.x` 的方式引用 property。

    - 与 `props` 不同，`attrs` 和 `slots` 的 property 是**非响应式**的。如果打算根据 `attrs` 或 `slots` 的更改应用副作用，那么应该在 `onBeforeUpdate` 生命周期钩子中执行此操作。

  - setup能访问到的参数

    - 执行 `setup` 时，你只能访问以下值：
      - `props`
      - `attrs`
      - `slots`
      - `emit`
    - 换句话说，**无法访问**以下组件选项：
      - `data`
      - `computed`
      - `methods`
      - `refs` (模板 ref)

  - setup使用this

    - **在 `setup()` 内部，`this` 不是该活跃实例的引用**，因为 `setup()` 是在解析其它组件选项之前被调用的，所以 `setup()` 内部的 `this` 的行为与其它选项中的 `this` 完全不同。这使得 `setup()` 在和其它选项式 API 一起使用时可能会导致混淆。

  - setup的两个注意点

    - setup执行的时机

      - 在beforeCreate之前执行一次，this是undefined

    - setup的参数

      - props：值为对象，包含：组件外部传递过来的，且组件内部声明接收了的属性

      - context：上下文对象

        - attrs：值为对象，包含：组件外部传递过来的，但**没有在props配置中声明的属性**，相当于`this.$attrs`

        - slots：收到的插槽内容，相当于`this.$slots`

          - 父组件写了传入插槽的结构，但是子组件没有声明solt插槽，那么父组件传过来的结构就成为一个虚拟DOM保存在context.solts中
          - vue3中父组件传具名插槽要用`v-solt：`指令

        - emit：分发自定义事件的函数，相当于`this.$emit`

          ```js
          setup(props,context) {
              function test() {
                  context.emit('hello','aaa')     //可以在context.emit触发自定义事件
              }
          }
          ```

      - d

    - d

  

- 生命周期钩子

  - Vue3生命周期钩子和Vue2的不同：

    - Vue2的`beforeDestroy`改为`beforeUnmount`
    - Vue2的`destroyed`改为`unmounted`	

  - 可以通过在生命周期钩子前面加上 “on” 来访问组件的生命周期钩子

  - 下表包含如何在 `setup () `内部调用生命周期钩子：

  - | 选项式 API        | `setup`对应的钩子   |
    | ----------------- | ------------------- |
    | `beforeCreate`    | Not needed*         |
    | `created`         | Not needed*         |
    | `beforeMount`     | `onBeforeMount`     |
    | `mounted`         | `onMounted`         |
    | `beforeUpdate`    | `onBeforeUpdate`    |
    | `updated`         | `onUpdated`         |
    | `beforeUnmount`   | `onBeforeUnmount`   |
    | `unmounted`       | `onUnmounted`       |
    | `errorCaptured`   | `onErrorCaptured`   |
    | `renderTracked`   | `onRenderTracked`   |
    | `renderTriggered` | `onRenderTriggered` |
    | `activated`       | `onActivated`       |
    | `deactivated`     | `onDeactivated`     |

  - 因为 `setup` 是围绕 `beforeCreate` 和 `created` 生命周期钩子运行的，所以不需要显式地定义它们。换句话说，在这些钩子中编写的任何代码都应该**直接**在 `setup` 函数中编写。

  - 这些函数接受一个回调函数，当钩子被组件调用时将会被执行:

    ```js
    // MyBook.vue
    
    export default {
      setup() {
        // mounted
        onMounted(() => {
          console.log('Component is mounted!')
        })
      }
    }
    ```

    

- ref函数（处理基本类型）

  - 因为直接在setup中定义的变量是非响应式的（就是修改数据后无法立即呈现在页面上），所以需要通过ref函数来将数据修饰为响应式的

  - 作用：定义一个响应式的数据

  - 语法：`const 变量名 = ref(变量的初始值)`

    - 创建一个包含响应式数据的**引用对象（reference对象，简称ref对象）**
    - JS中操作数据：`变量名.value`
    - 模板中读取数据：不需要.value，直接：`<div>{{变量名}}</div>`

  - 备注：

    - 接收的数据可以是基本类型，也可以是对象类型
    - 基本类型的数据：响应式依然是靠`Object.defineProperty()`的get和set完成的
    - 对象类型的数据：内部使用了Vue3的一个芯焊丝——`reactive`函数

  - 例子：

    ```vue
    <template>
        <h1>我是App根组件</h1>
        <h2>姓名: {{name}}</h2>
        <h2>年龄: {{age}}</h2>
        <button @click="changeInfo">修改人的信息</button>
    </template>
    
    <script>
    export default {
        name: 'App',
        setup() {
            // 数据
            let name = '张三'
            let age = 18
    
            // 方法
            function changeInfo() {
                name = '李四'
                age = 45
            }
    
            // 返回一个对象（常用）
            return {
                name,
                age,
                changeInfo
            }
        }
    }
    </script>
    
    <style>
    </style>
    
    
    // 如此处，想要点击按钮修改数据，但是在点击时候页面没有改变，但是实际上数据是改变了的，因为此时的name和age都不是响应式的，所以无法立即呈现
    // 所以需要用到ref函数将name和age变为响应式
    ```

    应改为这样：

    ```vue
    <script>
    import { ref } from 'vue'
    export default {
        name: 'App',
        setup() {
            // 数据
            let name = ref('张三')
            let age = ref(18)
    
            // 方法
            function changeInfo() {
                name.value = '李四'
                age.value = 45
                console.log('123123');
            }
    
            // 返回一个对象（常用）
            return {
                name,
                age,
                changeInfo
            }
        }
    }
    </script>
    
    ```

    - 注意：通过ref修饰后的name和age分别成为了一个对象，想要修改name和age的值需要使用name.value和age.value

  - 如果ref函数修饰的是对象，则用`对象名.value.属性名`获取属性

    ```vue
    <script>
    import { ref } from 'vue'
    export default {
        name: 'App',
        setup() {
            // 数据
            let name = ref('张三')
            let age = ref(18)
            let job = ref({
                type: '前端工程师',
                salary: '30k'
            })
    
            // 方法
            function changeInfo() {
                name.value = '李四'
                age.value = 45
                job.value.type = '后端工程师'
                job.value.salary = '35k'
                console.log(job.value)
            }
    
            // 返回一个对象（常用）
            return {
                name,
                age,
                changeInfo,
                job
            }
        }
    }
    </script>
    ```

    

- reactive函数

  - 作用：定义了一个对象类型的响应式数据（基本类型不用reactive，基本类型用`ref`函数）

  - 语法：`const 代理对象 = reactive(被代理对象)`接收一个对象（或数组），返回一个代理器对象（proxy对象）

  - reactive定义的响应式数据是"深层次的"

  - 内部基于ES6的PRoxy实现，通过代理对象操作源对象内部数据都是响应式的

  - 例子：

    ```js
    let job = reactive({
                type: '前端工程师',
                salary: '30k'
            })
    
            // 方法
            function changeInfo() {
                name.value = '李四'
                age.value = 45
                job.type = '后端工程师'
                job.salary = '35k'
                console.log(job.value)
            }
    
    ```

    - 如果使用了reactive修饰了一个变量，那么这个变量的属性修改就不用`.value`了，只需要直接`变量名.属性名`就可以了

  - 如果不想要写ref的value，可以直接用reactive包含一个对象

    ```vue
    <script>
    import { reactive } from 'vue'
    export default {
        name: 'App',
        setup() {
            // 数据
            let person = {
                name: '张三',
                age: 18,
                job: {
                    type: '前端工程师',
                    salary: '30k'
                },
                hobby: ['抽烟', '喝酒', '烫头']
            }
            let p = reactive(person)
    
            // 方法
            function changeInfo() {
                p.name = '李四'
                p.age = 45
                p.job.type = '后端工程师'
                p.job.salary = '35k'
            }
    
            // 返回一个对象（常用）
            return {
                p,
                changeInfo
            }
        }
    }
    </script>
    ```

    - 这样就可以省略value，直接用p调用属性了

  

- reactive与ref的对比

  - 从定义数据角度对比：

    - ref用来定义：**基本数据类型**
    - reactive用来定义：**对象（或数组）类型数据**
    - 备注：ref也可以用来定义**对象（或数组）类型数据**，它内部会自动通过`reactive`转为**代理对象**

  - 从原理角度对比：

    - ref通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）
    - reactive通过使用**Proxy**来实现响应式（数据劫持），并通过**Reflect**操作**源对象**内部的数据

  - 从使用角度对比：

    - ref定义的数据：操作数据**需要**`.value`，读取数据时模板中直接读取，**不需要**`.value`	
    - reactive定义的数据：操作数据与读取数据：**均不需要**`.value`

    

- Vue3中的响应式原理

  - Vue2的响应式

    - 实现原理：

      - 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）

      - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）

        ```js
        Object.defineProperty(data,'count',{
            get() {}
            set() {}
        })
        ```

    - 存在问题：

      - 新增属性、删除属性，界面不会更新
      - 直接通过下标修改数组，界面不会自动更新

  - Vue3的响应式

    - 实现原理：

      - 通过Proxy（代理）：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等

      - 通过Reflect（反射）：对被代理对象的属性进行操作

      - MDN文档中描述的Proxy与Reflect：

        - Proxy:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
        - Reflect:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      - 模拟 Vue3中实现响应式

        ```js
        const p = new Proxy(person,{
            // 有人读取p的属性时调用
            get(target,propName) {
                console.log(`有人读取了p身上的${propName}属性`)
                return Reflect.get(target,propName)
            },
            // 有人修改p的属性或给p增加某个属性时调用
            set(target.propName,value) {
                console.log(`有人修改了p身上的${propName}属性`)
                Reflect.set(target,propName,value)
            },
            // 有人删除p的属性时调用
            deleteProperty(target,propName) {
                console.log(`有人删除了p身上的${propName}属性`)
                return Reflect.deleteProperty(target,propName )
            }
        })
        ```

        

- 计算属性与监视

  - computed函数

    - 与Vue2中computed配置功能一致

    - 写法

      ```js
      import {computed} from 'vue'
      
      setup() {
          //数据
          let person = reactive({
              firstName: '张',
              lastName: '三'
          })
          ...
          //计算属性（简写）
          let fullName = computed(() => {
              return person.firstName + '-' + person.lastName
          })
          //计算属性（完整）
          let fullName = computed({
              get(value) {
                  return person.firstName + '-' + person.lastName
              },
              set() {
                  const nameArr = value.split('-')
                  person.firstName = nameArr[0]
                  person.lastName = nameArr[1]
              }
          })
      }
      ```

    - 

  - watch函数

    - 与vue2中watch配置功能一致

    - 两个小坑

      - 监视reactive定义的响应式数据时：**oldValue无法正确获取、强制开启了深度监视（deep配置失效）**

      - 监视reactive定义的响应式数据**中某个属性**时：deep配置**有效**

      - ```js
        //数据
        let person = reactive({
            name:'张三',
            age:18,
            job:{
                j1:{
                    salary:'20k'
                }
            }
        })
        
        //情况一：监视ref定义的响应式数据
        watch(sum,(newValue,oldValue)=> {
            console.log('sum变化了',newValue,oldValue)
        },{immediate:true})
        
        //情况二：监视多个ref定义的响应式数据
        watch([sum,msg],(newValue,oldValue)=> {
            console.log('sum或msg变化了',newValue,oldValue)
        })
        
        //情况三：监视reactive定义的响应式数据
         //若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue
         //若watch监视的是reactive定义的响应式数据，则强制开启了深度监视
        watch(person,(newValue,oldValue)=> {
            console.log('person变化了',newValue,oldValue)
        },{immediate:true,deep:false})   //此处的deep配置失效
        
        //情况四：监视reactive定义的响应式数据中的某个属性
        watch(()=>person.job,(newValue,oldValue)=> {
            console.log('person中的job变化了',newValue,oldValue)
        },{immediate:true,deep:true})
        //此处要配置deep，是因为监视reactive中的某个属性依旧是一个对象
        
        //情况五：监视reactive所定义的一个响应式数据中的某些属性
        watch([()=>person.name,()=>person.age],(newValue,oldValue)=> {
            console.log('person的name或age变化了',newValue,oldValue)
        })
        ```

      - 注意：

        - 如果监视的是reactive定义的响应式数据**对象**，则watch的第一个参数直接填写参数名就行
        - 如果监视的是ref定义的响应式数据**对象**，则watch的第一个参数需要填写`参数名.value`。或者直接写参数名并配置deep：true

    - watchEffect函数

      - watch的套路是：既要指明监视的属性，也要指明监视的回调

      - watchEffect的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性

      - watchEffect有点像computed：

        - 但computed注重的是计算出来的值（回调函数的返回值），所以必须要写返回值
        - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值

      - ```js
        //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调
        watchEffect(()=> {
            const x1 = sum.value
            const x2 = person.age
            console.log('watchEffect配置的回调执行了')
        })
        ```

  - 

  

- toRef

  - 作用：创建一个**ref对象**，其value值指向另一个对象中的某个属性
  - 语法：`const name = toRef(person,'name')`
  - 应用：要将响应式对象中的某个属性单独提供给外部使用时
  - 扩展：`toRefs`与`toRef`功能一致，但可以批量创建多个ref对象，语法：`toRefs(person)`

- shallowReactive与shallowRef

  - shallowReactive：**只处理对象最外层属性的响应式**（浅响应式）
  - shallowRef：只处理基本数据类型的响应式，不进行对象的响应式处理**（就是不修改对象的属性）**
  - 什么时候用？
    - 如果有一个对象数据，结构比较深，但变化时**只是外层属性变化**——shallowReactive
    - 如果有一个对象数据，后续功能**不会修改该对象中的属性**，而是用新的对象来替换——shallowRef

- readonly与shallowReadonly

  - readonly：让一个响应式数据变为只读的（深只读）

  - shallowReadonly：让一个响应式数据变为只读的（浅只读）

  - 应用场景：不希望数据被修改时

  - ```js
    //使用
    let person = reactive({
        name:'张三',
        age:18,
        job:{
            j1:{
                salary:20
            }
        }
    })
    
    person = readonly(person)
    //或
    person = shallowReadonly(person)
    ```

- toRaw与markRaw

  - toRaw：

    - 作用：将一个**由`reactive`**生成的**响应式对象**转为**普通对象**
    - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新

  - markRaw：

    - 作用：标记一个对象，使其用于不会再成为响应式对象
    - 应用场景：
      1. 有些值不应被设置为响应式的，例如复杂的第三方类库等
      2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能

  - 使用：

    ```js
    let person = reactive({
        name:'张三',
        age:18,
        job:{
            j1:{
                salary:20
            }
        }
    })
    
    const p = toRaw(person)        //获取到的p就是原始的对象
    
    let car = {name:'奔驰',price:40}
    person.car = markRaw(car)               //给person新添加car属性，但是markRaw包装后car不可改变（不会成为响应式对象）
    ```

- customRef

  - 作用：创建一个自定义的ref，并对其依赖项和更新触发进行显式控制

  - 实现防抖效果：

    ```vue
    <template>
      <input type="text" v-model="keyWord">
      <h3>{{keyWord}}</h3>
    </template>
    
    <script>
        import {ref,customRef} from 'vue'
        export default {
            name: 'App',
            setup() {
                //自定义ref
                function myRef(value,delay) {
                    let timer
                    return customRef((track,trigger)=> {
                        return {
                            get() {
                                console.log('有人从myRef这个容器中读取数据了')
                                track()  //通知Vue追踪value（return出去的值）的变化（提前告诉get，value是有用的，后面trigger会用到）
                                return value
                            },
                            set(newValue) {
                                console.log('有人吧myRef这个容器中的数据改变了')
                                clearTimeout(timer)
                                timer = setTimeout(()=> {
                                    value = newValue
                                    trigger()   //通知Vue去重新解析模板（为了让修改的数据立刻呈现在页面）
                                },delay)
                                
                            }
                        }
                    })
                }
                
                //let keyWord = ref('hello')       使用Vue提供的ref
                let keyWord = myRef('hello',500)     //使用程序员自定义的ref
                return {
                    keyWord
                }
            }
        }
    </script>
    ```

  - 

  

- provide与inject

  - 作用：实现**祖孙组件间**通信

  - 套路：父组件有一个`provide`选项来提供数据，子组件有一个`inject`选项来开始使用这些数据

  - 参数

    - `provide` 函数允许你通过两个参数定义 property：
      1. name (`<String>` 类型)
      2. value
    - `inject` 函数有两个参数：
      1. 要 inject 的 property 的 name
      2. 默认值 (**可选**)

  - 具体写法：

    - 祖组件中：

      ```js
          import { provide } from 'vue'
      
      setup() {
          ...
          let car = reactive({name:'奔驰',price:'40万'})
          provide('car',car)
          return {...toRefs(car)}
          ...
      }
      ```

    - 孙组件中:

      ```js
          import { inject } from 'vue'
      
      setup(props,context) {
          ...
          const car = inject('car')
          return {car}
          ...
      }
      ```

  - 注意：

    - 当使用响应式 provide / inject 值时，**建议尽可能将对响应式值的所有修改限制在<u>定义 provide 的组件内部</u>**。

    - 有时我们需要在注入数据的组件内部更新 inject 的数据。在这种情况下，我们**建议 provide 一个方法来负责改变响应式值**。

    - 如果要确保通过 `provide` 传递的数据不会被 inject 的组件更改，我们建议对提供者的 property 使用 `readonly`。

      ```js
      provide('location', readonly(location))
      ```

  

- 模板引用

  - 在使用组合式 API 时，响应式引用和模板引用的概念是统一的。为了获得对模板内元素或组件实例的引用，我们可以像往常一样声明 ref 并从 setup()返回：

    ```vue
    <template> 
      <div ref="root">This is a root element</div>
    </template>
    
    <script>
      import { ref, onMounted } from 'vue'
    
      export default {
        setup() {
          const root = ref(null)
    
          onMounted(() => {
            // DOM 元素将在初始渲染后分配给 ref
            console.log(root.value) // <div>This is a root element</div>
          })
    
          return {
            root
          }
        }
      }
    </script>
    ```

    - 模板引用只会在初始渲染之后获得赋值。
    - 作为模板使用的 ref 的行为与任何其他 ref 一样：它们是响应式的，可以传递到 (或从中返回) 复合函数中。

  - JSX

    - render函数

      ```js
      createElement(
        'anchored-heading', {
          props: {
            level: 1
          }
        }, [
          createElement('span', 'Hello'),
          ' world!'
        ]
      )
      
      //对应的模板是
      <anchored-heading :level="1">
        <span>Hello</span> world!
      </anchored-heading>
      
      //这样子写很麻烦
      ```

    - 所以有一个 Babel 插件，用于在 Vue 中使用 JSX 语法，它可以让我们回到更接近于模板的语法上。

      ```js
      import AnchoredHeading from './AnchoredHeading.vue'
      
      new Vue({
        el: '#demo',
        render: function (h) {
          return (
            <AnchoredHeading level={1}>
              <span>Hello</span> world!
            </AnchoredHeading>
          )
        }
      })
      ```

      - 将 `h` 作为 `createElement` 的别名是 Vue 生态系统中的一个通用惯例，实际上也是 JSX 所要求的。

    - 模板引用在JSX中的用法

      ```js
      export default {
        setup() {
          const root = ref(null)
      
          return () =>
            h('div', {
              ref: root
            })
      
          // with JSX
          return () => <div ref={root} />
        }
      }
      ```

  - 侦听模板引用

    - 侦听模板引用的变更可以替代前面例子中演示使用的生命周期钩子。

    - 但与生命周期钩子的一个关键区别是，`watch()` 和 `watchEffect()` 在 DOM 挂载或更新*之前*运行副作用，所以当侦听器运行时，模板引用还未被更新。

      ```vue
      <template>
        <div ref="root">This is a root element</div>
      </template>
      
      <script>
        import { ref, watchEffect } from 'vue'
      
        export default {
          setup() {
            const root = ref(null)
      
            watchEffect(() => {
              // 这个副作用在 DOM 更新之前运行，因此，模板引用还没有持有对元素的引用。
              console.log(root.value) // => null
            })
      
            return {
              root
            }
          }
        }
      </script>
      ```

    - 因此，使用模板引用的侦听器应该用 `flush: 'post'` 选项来定义，这将在 DOM 更新*后*运行副作用，确保模板引用与 DOM 保持同步，并引用正确的元素。

      ```vue
      <template>
        <div ref="root">This is a root element</div>
      </template>
      
      <script>
        import { ref, watchEffect } from 'vue'
      
        export default {
          setup() {
            const root = ref(null)
      
            watchEffect(() => {
              console.log(root.value) // => <div>This is a root element</div>
            }, 
            {
              flush: 'post'
            })
      
            return {
              root
            }
          }
        }
      </script>
      ```

    - 

    - d

  - 

  - 

  - 

  - 

  - 

  - d

  

- 响应式数据的判断

  - isRef：检查一个值是否为一个ref对象
  - isReactive：检查一个对象是否是由`reactive`创建的响应式代理
  - isReadonly：检查一个对象是否是由`readonly`创建的只读dialing
  - isProxy：检查一个对象是否是由`reactive`或者`readonly`方法创建的代理

- 组合式API（Composition API）的优势

  - 使用传统OptionsAPI（选型式API）中，新增或者修改一个需求，就需要分别在data、methods、computed里修改
  - 用了组合式API就可以更优雅地组织我们的代码、函数。让相关的代码更有序地组织在一起

  

- Fragment

  - 在Vue2中：组件必须由一个根标签
  - 在Vue3中：组件可以没有根标签，内部会将多个标签包含在一个Fragment虚拟元素中
  - 好处：减少标签层级，减少内存占用

- Teleport

  - 什么是Teleport

    - `Teleport`是一种能够将我们的`<strong style="color:#DD5145>组件html结构</strong>"`移动到指定位置的技术

      ```js
      <teleport to="移动位置">
          <div v-if="isShow" class="mask">
              <div class="dialog">
                  <h3>我是一个弹窗</h3>
                  <button @click="isShow = false">关闭弹窗</button>
              </div>
          </div>
      </teleport>
      ```

    - 比如这个teleport在Child组件中，但是设置了`to="body"`，所以这个teleport包含的内容是**在body标签的子级的**
    - 注意：虽然teleport的位置可能不在Child组件下（在body下），但是teleport依旧是Child的逻辑子组件（即依旧可以从Child中获取到props等数据）

  - 在同一目标下使用teleport

    - 一个常见的用例场景是一个可重用的 `<Modal>` 组件，它可能同时有多个实例处于活动状态。对于这种情况，多个 `<teleport>` 组件可以将其内容挂载到同一个目标元素。

    - ```html
      <teleport to="#modals">
        <div>A</div>
      </teleport>
      <teleport to="#modals">
        <div>B</div>
      </teleport>
      
      <!-- result-->
      <div id="modals">
        <div>A</div>
        <div>B</div>
      </div>
      ```

    - 顺序将是一个简单的追加——**稍后挂载将位于目标元素中较早的挂载之后。**

    

- Suspense

  - 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

  - 使用步骤：

    - 异步引入组件

      ```js
      import {defineAsyncComponent} from 'vue'
      const Child = defineAsyncComponent(()=> import('./components/Child.vue'))
      ```

    - 使用`Suspense`包裹组件，并配置好`default`与`fallback`

      ```vue
      <template>
          <div class="app">
              <h3>我是App组件</h3>
              <Suspense>
                  <template v-slot:default>
                      <Child/>
                  </template>
                  <template v-slot:fallback>
                      <h3>加载中......</h3>
                  </template>
              </Suspense>
          </div>
      </template>
      ```

- 全局API的转移

  - Vue2中有很多全局API和配置

    - 如：

      ```js
      //注册全局组件
      Vue.conponent()
      //注册全局指令
      Vue.directive()
      ```

  - Vue3对这些API做出了调整

    - 将全局的API，即：`Vue.xxx`调整到应用实例（`app`）上

    - | 2.x全局API（`Vue`）     | 3.x实例API（`app`）         |
      | ----------------------- | --------------------------- |
      | Vue.config.xxxx         | app.config.xxxx             |
      | Vue.config.produtionTip | 移除                        |
      | Vue.component           | app.component               |
      | Vue.directive           | app.directive               |
      | Vue.mixin               | app.mixin                   |
      | Vue.use                 | app.use                     |
      | Vue.prototype           | app.config.globalProperties |

  - 其他改变

    - data选项应始终被声明为一个函数

    - 过渡类名的更改：

      - Vue2的写法：

        ```js
        .v-enter
        .v-leave-to {
            opacity: 0;
        }
        .v-leave,
        .v-enter-to {
            opacity: 1;
        }
        ```

      - Vue3的写法：

        ```js
        .v-enter-from,
        .v-leave-to {
            opacity: 0;
        }
        .v-leave-from,
        .v-enter-to {
            opacity: 1;
        }
        ```

    - **移除**keyCode作为v-on的修饰符，同时也不再支持`config.keyCodes`

    - **移除**`v-on.native`修饰符

      - 父组件中绑定事件

        ```vue
        <my-component
            v-on:close="handleComponentEvent"
            v-on:click="handleNativeClickEvent"
        />              
        ```

      - 子组件中声明自定义事件

        ```vue
        <script>
            export defalut {
                emits: ['close']
            }
        </script>
        ```

    - **移除**过滤器（filter）

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

































































