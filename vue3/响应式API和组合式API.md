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

    

- 响应式原理

  - 什么是响应式

    - 如：

      ```js
      let val1 = 2
      let val2 = 3
      let sum = val1 + val2
      
      console.log(sum) // 5
      
      val1 = 3
      
      console.log(sum) // 仍然是 5
      //如果我们更新第一个值，sum 不会被修改
      ```

    - 如果想要sum值同时被修改，那就需要做到：

      - **当一个值被读取时进行追踪**，例如 `val1 + val2` 会同时读取 `val1` 和 `val2`。
      - **当某个值改变时进行检测**，例如，当我们赋值 `val1 = 3`。
      - **重新运行代码来读取原始值**，例如，再次运行 `sum = val1 + val2` 来更新 `sum` 的值。

    - 这就是响应式

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

    - 调试computed（Vue3.2+）

      - `computed` 可接受一个带有 `onTrack` 和 `onTrigger` 选项的对象作为第二个参数：

        - `onTrack` 会在某个响应式 property 或 ref 作为依赖被追踪时调用。
        - `onTrigger` 会在侦听回调被某个依赖的修改触发时调用。

      - 所有回调都会收到一个 debugger 事件，其中包含了一些依赖相关的信息。推荐在这些回调内放置一个 `debugger` 语句以调试依赖。

        ```js
        const plusOne = computed(() => count.value + 1, {
          onTrack(e) {
            // 当 count.value 作为依赖被追踪时触发
            debugger
          },
          onTrigger(e) {
            // 当 count.value 被修改时触发
            debugger
          }
        })
        // 访问 plusOne，应该触发 onTrack
        console.log(plusOne.value)
        // 修改 count.value，应该触发 onTrigger
        count.value++
        ```

      - `onTrack` 和 `onTrigger` 仅在开发模式下生效

        

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
        
      - 停止侦听
        
        - 当 `watchEffect` 在组件的 setup()函数或生命周期钩子被调用时，侦听器会被链接到该组件的生命周期，并在组件卸载时自动停止。
        
        - 在一些情况下，也可以显式调用返回值以停止侦听：
        
          ```js
          const stop = watchEffect(() => {
            /* ... */
          })
          
          // later
          stop()
          ```
        
      - 清除副作用
        
        - 有时副作用函数会执行一些异步的副作用，这些响应需要在其失效时清除 (即完成之前状态已改变了) 。所以侦听副作用传入的函数可以接收一个 `onInvalidate` 函数作入参，**用来注册清理失效时的回调**。当以下情况发生时，这个失效回调会被触发：
        
          - 副作用即将重新执行时
        
          - 侦听器被停止 (如果在 `setup()` 或生命周期钩子函数中使用了 `watchEffect`，则在组件卸载时)
        
          - ```js
            watchEffect(onInvalidate => {
              const token = performAsyncOperation(id.value)
              onInvalidate(() => {
                // id has changed or watcher is stopped.
                // invalidate previously pending async operation
                token.cancel()
              })
            })
            ```
        
        - 在执行数据请求时，副作用函数往往是一个异步函数：
        
          ```js
          const data = ref(null)
          watchEffect(async onInvalidate => {
            onInvalidate(() => {
              /* ... */
            }) // 我们在Promise解析之前注册清除函数
            data.value = await fetchData(props.id)
          }
          ```
        
      - 副作用函数的刷新时机
        
        - Vue 的响应性系统会缓存副作用函数，并异步地刷新它们，这样可以避免同一个“tick” 中多个状态改变导致的不必要的重复调用。
        - 在核心的具体实现中，组件的 `update` 函数也是一个被侦听的副作用。当一个用户定义的副作用函数进入队列时，默认情况下，会在所有的组件 `update` **前**执行

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

  

- mixin

  - 功能：可以把多个组件**共用的配置**（即Vue实例下面的methods，data等）提取成一个混入对象

  - 使用方式

    - 定义混入

      ```js
      const mixin = {
          data() {....},
          methods: {....}
          ....
      }
      ```

    - 使用混入

      - 全局混入`app.mixin(xxx)`

        - 如：

          ```js
          const app = Vue.createApp({
            myOption: 'hello!'
          })
          
          // 为自定义的选项 'myOption' 注入一个处理器。
          app.mixin({
            created() {
              const myOption = this.$options.myOption
              if (myOption) {
                console.log(myOption)
              }
            }
          })
          
          app.mount('#mixins-global') // => "hello!"
          ```

      - 局部混入`mixins:['xxx']`

  - 例子：

    ```js
    // 定义一个 mixin 对象
    const myMixin = {
      created() {
        this.hello()
      },
      methods: {
        hello() {
          console.log('hello from mixin!')
        }
      }
    }
    
    // 定义一个使用此 mixin 对象的应用
    const app = Vue.createApp({
      mixins: [myMixin]
    })
    
    app.mount('#mixins-basic') // => "hello from mixin!"
    ```

  - 选项合并（当组件和 mixin 对象含有同名选项时，这些选项将以恰当的方式进行“合并”。）

    - 每个 mixin 可以拥有自己的 `data` 函数。每个 `data` 函数都会被调用，并将返回结果合并。在数据的 property 发生冲突时，会**以组件自身的数据为优先。**

      ```js
      const myMixin = {
        data() {
          return {
            message: 'hello',
            foo: 'abc'
          }
        }
      }
      
      const app = Vue.createApp({
        mixins: [myMixin],
        data() {
          return {
            message: 'goodbye',
            bar: 'def'
          }
        },
        created() {
          console.log(this.$data) // => { message: "goodbye", foo: "abc", bar: "def" }
        }
      })
      ```

    - 同名钩子函数将合并为一个数组，因此都将被调用。另外，mixin 对象的钩子将在组件自身钩子**之前**调用。

      ```js
      const myMixin = {
        created() {
          console.log('mixin 对象的钩子被调用')
        }
      }
      
      const app = Vue.createApp({
        mixins: [myMixin],
        created() {
          console.log('组件钩子被调用')
        }
      })
      
      // => "mixin 对象的钩子被调用"
      // => "组件钩子被调用"
      ```

    - 值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，**取组件对象的键值对**。

      ```js
      const myMixin = {
        methods: {
          foo() {
            console.log('foo')
          },
          conflicting() {
            console.log('from mixin')
          }
        }
      }
      
      const app = Vue.createApp({
        mixins: [myMixin],
        methods: {
          bar() {
            console.log('bar')
          },
          conflicting() {
            console.log('from self')
          }
        }
      })
      
      const vm = app.mount('#mixins-basic')
      
      vm.foo() // => "foo"
      vm.bar() // => "bar"
      vm.conflicting() // => "from self"
      ```

  - 自定义选项合并策略

    - 自定义选项在合并时，默认策略为简单地覆盖已有值。如果想让某个自定义选项以自定义逻辑进行合并，可以在 `app.config.optionMergeStrategies` 中添加一个函数：

      ```js
      const app = Vue.createApp({
        custom: 'hello!'
      })
      
      app.config.optionMergeStrategies.custom = (toVal, fromVal) => {
        console.log(fromVal, toVal)
        // => "goodbye!", undefined
        // => "hello", "goodbye!"
        return fromVal || toVal
      }
      
      app.mixin({
        custom: 'goodbye!',
        created() {
          console.log(this.$options.custom) // => "hello!"
        }
      })
      
      //在控制台中，先从 mixin 打印 toVal 和 fromVal，然后从 app 打印。如果存在，我们总是返回 fromVal，这就是为什么 this.$options.custom 设置为 hello!。
      ```

      - 合并策略接收在**父实例和子实例**上定义的该选项的值，**分别作为第一个和第二个参数**。

        

- 自定义指令

  - 什么是自定义指令

    - vue官方提供了v-text、v-for、v-model、v-if等常用的指令。除此之外vue还允许开发者自定义指令

  - 自定义指令的分类

    vue中的自定义指令分为两类，分别是：

    - **私有**自定义指令
    - **全局**自定义指令

  - 私有自定义指令

    - 在每个vue组件中，可以在directives节点下声明私有自定义指令

    - 如：

      ```js
      directives: {
          color: {
              // 为绑定到的HTML元素设置红色的文字
              bind(el) {
                  // 形参中的el是绑定了此指令的、原生的DOM对象
                  el.style.color = 'red'
              }
          }
      }
      ```

    - 当指令第一次被绑定到元素上的时候，会立即触发bind函数

    - 形参中的el表示当前指令所绑定到的那个DOM对象

    - 使用binding.value获取指令绑定的值

      - ```js
        <h1 v-color="color">App根组件</h1>
            <p v-color="'red'">测试</p>
        
        
        
         data() {
            return {
              color: 'blue'
            }
          },
        directives: {
            // 定义名为color的指令，指向一个配置对象
            color: {
              // 当指令第一次被绑定到元素上的时候，会立即触发bind函数
              // 形参中的el表示当前指令所绑定到的那个DOM对象
              bind(el,binding) {
                el.style.color = binding.value
              }
            }
          }
        ```

      - binding是bind函数的第二个参数，binding.value获取到的是v-color后面的值（binding只是一个名字，可以改变）

    - inserted函数

      - inserted(element,binding)

      - 指令所在元素被插入页面时调用

      - element是DOM元素，binding是要绑定的对象

    - update函数

      - bind函数**只调用一次**：当指令第一次绑定到元素时调用，**当DOM更新时bind函数不会被触发**

      - update函数会在**每次DOM更新**时被调用

      - 如：

        ```js
        directives: {
            color: {
                //当指令第一次被绑定到元素时被调用
                bind(el,binding) {
                    el.style.color = binding.value
                },
                //每次DOM更新时被调用
                update(el,binding) {
                    el.style.color = binding.value
                }
            }
        }
        ```

    - 函数简写

      - 如果bind和update函数中的逻辑完全相同，则对象格式的自定义指令可以简写成函数格式：

        ```js
        directives: {
            //在bind和update时，会触发相同的业务逻辑
            color(el,binding) {
                el.style.color = binding.value
            }
        }
        ```

    ​              或者：

    ```js
    app.directive('pin', (el, binding) => {
      el.style.position = 'fixed'
      const s = binding.arg || 'top'
      el.style[s] = binding.value + 'px'
    })
    ```

    

  - 全局自定义指令

    - 全局共享的自定义指令需要通过"Vue.directive()"进行声明

    - 示例代码：

      ```js
      const app = Vue.createApp({})
      //参数1：字符串，表示全局自定义指令的名字
      //参数2：对象，用来接收指令的参数值
      app.directive('color',function(el,binding) {
          el.style.color = binding.value
      })
      
      //或者
      Vue.directive('color',{
        bind(el,binding) {
          el.style.color = binding.value
        },
        //每次DOM更新时被调用
        update(el,binding) {
          el.style.color = binding.value
        }
      })
      ```

    - 全局自定义指令放到main.js中

  - 钩子函数

    - 一个指令定义对象可以提供如下几个钩子函数 (均为可选)：
      - `created`：在绑定元素的 attribute 或事件监听器被应用之前调用。在指令需要附加在普通的 `v-on` 事件监听器调用前的事件监听器中时，这很有用。
      - `beforeMount`：当指令第一次绑定到元素并且在挂载父组件之前调用。
      - `mounted`：在绑定元素的父组件被挂载后调用。
      - `beforeUpdate`：在更新包含组件的 VNode 之前调用。
      - `updated`：在包含组件的 VNode **及其子组件的 VNode** 更新后调用。
      - `beforeUnmount`：在卸载绑定元素的父组件之前调用
      - `unmounted`：当指令与元素解除绑定且父组件已卸载时，只调用一次。

  - 对象字面量

    - 如果指令需要多个值，可以传入一个 JavaScript 对象字面量。

    - 指令函数能够接受所有合法的 JavaScript 表达式。

    - ```js
      <div v-demo="{ color: 'white', text: 'hello!' }"></div>
      app.directive('demo', (el, binding) => {
        console.log(binding.value.color) // => "white"
        console.log(binding.value.text) // => "hello!"
      })
      ```

  - 注意：

    - 当在组件中使用时，自定义指令总是会被应用在组件的根节点上
    - 当被应用在一个多根节点的组件上时，指令会被忽略，并且会抛出一个警告。





- 渲染函数

  - DOM树

    - 例子：

      ```html
      <div>
        <h1>My title</h1>
        Some text content
        <!-- TODO: Add tagline -->
      </div>
      ```

      - 当浏览器读到这些代码时，它会建立一个 ”DOM 节点“ 树来保持追踪所有内容，如同你会画一张家谱树来追踪家庭成员的发展一样。
      - ![DOM Tree Visualization](https://v3.cn.vuejs.org/images/dom-tree.png)

  - 虚拟DOM树

    - Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM。

    - 如：

      ```js
      return h('h1', {}, this.blogTitle)
      ```

    - `h()` 到底会返回什么呢？

      - 其实不是一个*实际*的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，及其子节点的描述信息。
      - 我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为 **VNode**。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

  - h()参数

    - `h()` 函数是一个用于创建 VNode 的实用程序。

      也可称为 `createVNode()`，但由于频繁使用和简洁，它被称为 `h()` 。

    - 它接受三个参数：

      ```js
      // @returns {VNode}
      h(
        // {String | Object | Function} tag
        // 一个 HTML 标签名、一个组件、一个异步组件、或
        // 一个函数式组件。
        //
        // 必需的。
        'div',
      
        // {Object} props
        // 与 attribute、prop 和事件相对应的对象。
        // 这会在模板中用到。
        //
        // 可选的。
        {},
      
        // {String | Array | Object} children
        // 子 VNodes, 使用 `h()` 构建,
        // 或使用字符串获取 "文本 VNode" 或者
        // 有插槽的对象。
        //
        // 可选的。
        [
          'Some text comes first.',
          h('h1', 'A headline'),
          h(MyComponent, {
            someProp: 'foobar'
          })
        ]
      )
      ```

      - 如果没有 prop，那么通常可以将 children 作为第二个参数传入。如果会产生歧义，可以将 `null` 作为第二个参数传入，将 children 作为第三个参数传入。

    - 例子：

      ```js
      const { createApp, h } = Vue
      
      const app = createApp({})
      
      /** 递归地从子节点获取文本 */
      function getChildrenTextContent(children) {
        return children
          .map(node => {
            return typeof node.children === 'string'
              ? node.children
              : Array.isArray(node.children)
              ? getChildrenTextContent(node.children)
              : ''
          })
          .join('')
      }
      
      app.component('anchored-heading', {
        render() {
          // 从 children 的文本内容中创建短横线分隔 (kebab-case) id。
          const headingId = getChildrenTextContent(this.$slots.default())
            .toLowerCase()
            .replace(/\W+/g, '-') // 用短横线替换非单词字符
            .replace(/(^-|-$)/g, '') // 删除前后短横线
      
          return h('h' + this.level, [
            h(
              'a',
              {
                name: headingId,
                href: '#' + headingId
              },
              this.$slots.default()
            )
          ])
        },
        props: {
          level: {
            type: Number,
            required: true
          }
        }
      })
      ```

  - 约束

    - 组件树中的所有 VNode 必须是唯一的。因此，以下例子是错误的：

      ```js
      render() {
        const myParagraphVNode = h('p', 'hi')
        return h('div', [
          // 错误 - 重复的 Vnode!
          myParagraphVNode, myParagraphVNode
        ])
      }
      ```

    - 如果你真的需要重复很多次的元素/组件，你可以使用工厂函数来实现。例如，下面这渲染函数用完全合法的方式渲染了 20 个相同的段落：

      ```js
      render() {
        return h('div',
          Array.from({ length: 20 }).map(() => {
            return h('p', 'hi')
          })
        )
      }
      ```

  - 创建组件VNode

    - 要为某个组件创建一个 VNode，传递给 `h` 的第一个参数应该是组件本身。

      ```js
      render() {
        return h(ButtonCounter)
      }
      ```

    - 如果我们需要通过名称来解析一个组件，那么我们可以调用 `resolveComponent`

      `render` 函数通常只需要对全局注册的组件使用 `resolveComponent`。

      ```js
      // 此写法可以简化
      components: {
        ButtonCounter
      },
      render() {
        return h(resolveComponent('ButtonCounter'))
      }
      ```

    - 而对于局部注册的却可以跳过，我们可以直接使用它，而不是通过名称注册一个组件，然后再查找：

      ```js
      render() {
        return h(ButtonCounter)
      }
      ```

      

  - 使用JavaScript代替模板功能

    - `v-if`和`v-for`

      - 只要在原生的 JavaScript 中可以轻松完成的操作，Vue 的渲染函数就不会提供专有的替代方法。

      - 比如，在模板中使用的 `v-if` 和 `v-for`：

        ```html
        <ul v-if="items.length">
          <li v-for="item in items">{{ item.name }}</li>
        </ul>
        <p v-else>No items found.</p>
        ```

      - 这些都可以在渲染函数中用 JavaScript 的 `if`/`else` 和 `map()` 来重写：

        ```js
        props: ['items'],
        render() {
          if (this.items.length) {
            return h('ul', this.items.map((item) => {
              return h('li', item.name)
            }))
          } else {
            return h('p', 'No items found.')
          }
        }
        ```

    - `v-model`

      - `v-model` 指令扩展为 `modelValue` 和 `onUpdate:modelValue` 在模板编译过程中，我们必须自己提供这些 prop：

        ```js
        props: ['modelValue'],
        emits: ['update:modelValue'],
        render() {
          return h(SomeComponent, {
            modelValue: this.modelValue,
            'onUpdate:modelValue': value => this.$emit('update:modelValue', value)
          })
        }
        ```

    - `v-on`

      - 我们必须为事件处理程序提供一个正确的 prop 名称，例如，要处理 `click` 事件，prop 名称应该是 `onClick`。

        ```js
        render() {
          return h('div', {
            onClick: $event => console.log('clicked', $event.target)
          })
        }
        ```

    - 事件修饰符

      - 对于 `.passive` 、`.capture` 和 `.once` 事件修饰符，可以使用驼峰写法将他们拼接在事件名后面：

        ```js
        render() {
          return h('input', {
            onClickCapture: this.doThisInCapturingMode,
            onKeyupOnce: this.doThisOnce,
            onMouseoverOnceCapture: this.doThisOnceInCapturingMode
          })
        }
        ```

      - 对于所有其它的修饰符，私有前缀都不是必须的，因为你可以在事件处理函数中使用事件方法：

      - | 修饰符                                      | 处理函数中的等价操作                                         |
        | ------------------------------------------- | ------------------------------------------------------------ |
        | `.stop`                                     | `event.stopPropagation()`                                    |
        | `.prevent`                                  | `event.preventDefault()`                                     |
        | `.self`                                     | `if (event.target !== event.currentTarget) return`           |
        | 按键： `.enter`, `.13`                      | `if (event.keyCode !== 13) return` (对于别的按键修饰符来说，可将 13 改为另一个按键码 |
        | 修饰键： `.ctrl`, `.alt`, `.shift`, `.meta` | `if (!event.ctrlKey) return` (将 `ctrlKey` 分别修改为 `altKey`, `shiftKey`, 或 `metaKey`) |

      - 例子：

        ```js
        render() {
          return h('input', {
            onKeyUp: event => {
              // 如果触发事件的元素不是事件绑定的元素
              // 则返回
              if (event.target !== event.currentTarget) return
              // 如果向上键不是回车键，则终止
              // 没有同时按下按键 (13) 和 shift 键
              if (!event.shiftKey || event.keyCode !== 13) return
              // 停止事件传播
              event.stopPropagation()
              // 阻止该元素默认的 keyup 事件
              event.preventDefault()
              // ...
            }
          })
        }
        ```

    - 插槽

      - 可以通过 `this.$slots` 访问静态插槽的内容，每个插槽都是一个 VNode 数组：

        ```js
        render() {
          // `<div><slot></slot></div>`
          return h('div', {}, this.$slots.default())
        }
        
        //---------------------------------------------------------------------------
        props: ['message'],
        render() {
          // `<div><slot :text="message"></slot></div>`
          return h('div', {}, this.$slots.default({
            text: this.message
          }))
        }
        ```

      - 要使用渲染函数将插槽传递给子组件，请执行以下操作：

        ```js
        const { h, resolveComponent } = Vue
        
        render() {
          // `<div><child v-slot="props"><span>{{ props.text }}</span></child></div>`
          return h('div', [
            h(
              resolveComponent('child'),
              {},
              // 将 `slots` 以 { name: props => VNode | Array<VNode> } 的形式传递给子对象。
              {
                default: (props) => h('span', props.text)
              }
            )
          ])
        }
        ```

        - 插槽以函数的形式传递，允许子组件控制每个插槽内容的创建。

        - 任何响应式数据都应该在插槽函数内访问，以确保它被注册为子组件的依赖关系，而不是父组件。

        - 对 `resolveComponent` 的调用应该在插槽函数之外进行，否则它们会相对于错误的组件进行解析。

          ```js
          // `<MyButton><MyIcon :name="icon" />{{ text }}</MyButton>`
          render() {
            // 应该是在插槽函数外面调用 resolveComponent。
            const Button = resolveComponent('MyButton')
            const Icon = resolveComponent('MyIcon')
          
            return h(
              Button,
              null,
              {
                // 使用箭头函数保存 `this` 的值
                default: (props) => {
                  // 响应式 property 应该在插槽函数内部读取，
                  // 这样它们就会成为 children 渲染的依赖。
                  return [
                    h(Icon, { name: this.icon }),
                    this.text
                  ]
                }
              }
            )
          }
          ```

      - 如果一个组件从它的父组件中接收到插槽，它们可以直接传递给子组件。

        ```js
        render() {
          return h(Panel, null, this.$slots)
        }
        //this/$slots是从父组件接收到的插槽
        ```

      

    - `<component>`和`is`

      - 在底层实现里，模板使用 `resolveDynamicComponent` 来实现 `is` attribute。如果我们在 `render` 函数中需要 `is` 提供的所有灵活性，我们可以使用同样的函数：

        ```js
        const { h, resolveDynamicComponent } = Vue
        
        // ...
        
        // `<component :is="name"></component>`
        render() {
          const Component = resolveDynamicComponent(this.name)
          return h(Component)
        }
        ```

        - 就像 `is`, `resolveDynamicComponent` 支持传递一个组件名称、一个 HTML 元素名称或一个组件选项对象。

      - 如果我们只需要支持组件名称，那么可以使用 `resolveComponent` 来代替。

        如果 VNode 始终是一个 HTML 元素，那么我们可以直接把它的名字传递给 `h`：

        ```js
        // `<component :is="bold ? 'strong' : 'em'"></component>`
        render() {
          return h(this.bold ? 'strong' : 'em')
        }
        ```

        - 同样，如果传递给 `is` 的值是一个组件选项对象，那么不需要解析什么，可以直接作为 `h` 的第一个参数传递。
        - 与 `<template>` 标签一样，`<component>` 标签**仅在模板中作为语法占位符**需要，当迁移到 `render` 函数时，**应被丢弃**。

        

    - 自定义指令

      - 可以使用 `withDirectives`将自定义指令应用于 VNode：

        ```js
        const { h, resolveDirective, withDirectives } = Vue
        
        // ...
        
        // <div v-pin:top.animate="200"></div>
        render () {
          const pin = resolveDirective('pin')
        
          return withDirectives(h('div'), [
            [pin, 200, 'top', { animate: true }]
          ])
        }
        ```

      - `resolveDirective `是模板内部用来解析指令名称的同一个函数。只有当你还没有直接访问指令的定义对象时，才需要这样做。

    - 内置组件

      - 诸如 `<keep-alive>`、`<transition>`、`<transition-group>` 和 `<teleport>` 等内置组件默认并没有被全局注册。

      - 在模板中这些组件会被特殊处理，即在它们被用到的时候自动导入。当我们编写自己的 `render` 函数时，需要自行导入它们：

        ```js
        const { h, KeepAlive, Teleport, Transition, TransitionGroup } = Vue
        // ...
        render () {
          return h(Transition, { mode: 'out-in' }, /* ... */)
        }
        ```

        

  - 渲染函数的返回值

    - 返回一个字符串时会创建一个文本 VNode，而不被包裹任何元素：

      ```js
      render() {
        return 'Hello world!'
      }
      ```

    - 我们也可以返回一个子元素数组，而不把它们包裹在一个根结点里。这会创建一个片段 (fragment)：

      ```js
      // 相当于模板 `Hello<br>world!`
      render() {
        return [
          'Hello',
          h('br'),
          'world!'
        ]
      }
      ```

      

  - JSX

    - 例子：

      - 要编写这样的一个模板：

        ```html
        <anchored-heading :level="1"> <span>Hello</span> world! </anchored-heading>
        ```

    - 使用渲染函数：

      ```js
      h(
        'anchored-heading',
        {
          level: 1
        },
        {
          default: () => [h('span', 'Hello'), ' world!']
        }
      )
      ```

      - 可能会有点复杂

    - 使用JSX语法：

      ```js
      import AnchoredHeading from './AnchoredHeading.vue'
      
      const app = createApp({
        render() {
          return (
            <AnchoredHeading level={1}>
              <span>Hello</span> world!
            </AnchoredHeading>
          )
        }
      })
      
      app.mount('#demo')
      ```





- 插件

  - 插件是自包含的代码，通常向 Vue 添加全局级功能。它可以是公开 `install()` 方法的 `object`，也可以是 `function`

    插件的功能范围没有严格的限制——一般有下面几种：

    1. 添加全局方法或者 property。如：vue-custom-element
    2. 添加全局资源：指令/过渡等。如：vue-touch
    3. 通过全局 mixin 来添加一些组件选项。如vue-router
    4. 添加全局实例方法，通过把它们添加到 `config.globalProperties` 上实现。
    5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 vue-router

  - 我们将创建一个非常简化的插件版本，它显示 `i18n` 准备好的字符串。

    - 每当这个插件被添加到应用程序中时，如果它是一个对象，就会调用 `install` 方法。
    - 如果它是一个 `function`，则函数本身将被调用。
    - 在这两种情况下——它都会收到两个参数：由 Vue 的 `createApp` 生成的 `app` 对象和用户传入的选项。

  - 从设置插件对象开始。建议在单独的文件中创建它并将其导出，如下所示，以保持包含的逻辑和分离的逻辑。

    ```js
    // plugins/i18n.js
    export default {
      install: (app, options) => {
        // Plugin code goes here
      }
    }
    ```

  - 我们想要一个函数来翻译整个应用程序可用的键，因此我们将使用 `app.config.globalProperties` 暴露它。

    - 该函数将接收一个 `key` 字符串，我们将使用它在用户提供的选项中查找转换后的字符串。

      ```js
      // plugins/i18n.js
      export default {
        install: (app, options) => {
          app.config.globalProperties.$translate = key => {
            return key.split('.').reduce((o, i) => {
              if (o) return o[i]
            }, options)
          }
        }
      }
      ```

    - 用户使用插件时，将在 `options` 参数中传递一个包含翻译后的键的对象。

      ```js
      //options如：
      greetings: {
        hello: 'Bonjour!'
      }
      ```

  - 插件还允许我们使用 `inject` 为插件的用户提供功能或 attribute。

    ```js
    // plugins/i18n.js
    export default {
      install: (app, options) => {
        app.config.globalProperties.$translate = key => {
          return key.split('.').reduce((o, i) => {
            if (o) return o[i]
          }, options)
        }
    
        app.provide('i18n', options)
      }
    }
    ```

  - -----------------------------------------上面是定义插件，下面是使用插件----------------------------------------------------------

  - 在使用 `createApp()` 初始化 Vue 应用程序后，可以通过调用 `use()` 方法将插件添加到应用程序中。

  - `use()` 方法有两个参数。第一个是要安装的插件。

    它还会自动阻止你多次使用同一插件，因此**在同一插件上多次调用只会安装一次该插件**。

    第二个参数是可选的，并且取决于每个特定的插件。在演示 `i18nPlugin` 的情况下，它是带有转换后的字符串的对象。

  - 例子：

    ```js
    import { createApp } from 'vue'
    import Root from './App.vue'
    import i18nPlugin from './plugins/i18n'
    
    const app = createApp(Root)
    const i18nStrings = {
      greetings: {
        hi: 'Hallo!'
      }
    }
    
    app.use(i18nPlugin, i18nStrings)
    app.mount('#app')
    ```















#### TypeScript支持

- ts推荐配置

  - ```js
    // tsconfig.json
    {
      "compilerOptions": {
        "target": "esnext",
        "module": "esnext",
        // 这样就可以对 `this` 上的数据属性进行更严格的推断
        "strict": true,
        "jsx": "preserve",
        "moduleResolution": "node"
      }
    }
    ```

  - 请注意，必须包含 `strict: true` (或至少包含 `noImplicitThis: true`，它是 `strict` 标志的一部分) 才能在组件方法中利用 `this` 的类型检查，否则它总是被视为 `any` 类型。

- webpack配置

  - 如果使用自定义 Webpack 配置，需要配置 `ts-loader` 来解析 vue 文件里的 `<script lang="ts">` 代码块：

    ```js
    // webpack.config.js
    module.exports = {
      ...
      module: {
        rules: [
          {
            test: /\.tsx?$/,
            loader: 'ts-loader',
            options: {
              appendTsSuffixTo: [/\.vue$/],
            },
            exclude: /node_modules/,
          },
          {
            test: /\.vue$/,
            loader: 'vue-loader',
          }
          ...
    ```

    

- 开发工具

  - 项目创建——vuecli

    - 开始：

      ```bash
      # 1. Install Vue CLI, 如果尚未安装
      npm install --global @vue/cli@next
      
      # 2. 创建一个新项目, 选择 "Manually select features" 选项
      vue create my-project-name
      
      # 3. 如果已经有一个不存在TypeScript的 Vue CLI项目，请添加适当的 Vue CLI插件：
      vue add typescript
      ```

    - 请确保组件的 `script` 部分已将语言设置为 TypeScript：

      ```html
      <script lang="ts">
        ...
      </script>
      ```

    - 或者，如果你想将 TypeScript 与 [JSX `render` 函数](https://v3.cn.vuejs.org/guide/render-function.html#jsx)结合起来：

      ```html
      <script lang="tsx">
        ...
      </script>
      ```

    

- 定义Vue组件

  - 要让 TypeScript 正确推断 Vue 组件选项中的类型，需要使用 `defineComponent` 全局方法定义组件：

    ```ts
    import { defineComponent } from 'vue'
    
    const Component = defineComponent({
      // 已启用类型推断
    })
    ```

  - 如果使用的是单文件组件，则通常会被写成：

    ```vue
    <script lang="ts">
    import { defineComponent } from 'vue'
    export default defineComponent({
      // 已启用类型推断
    })
    </script>
    ```

    

- 与Options API（选项式API）一起使用

  - TypeScript 应该能够在不显式定义类型的情况下推断大多数类型。例如，对于拥有一个数字类型的 `count` property 的组件来说，如果你试图对其调用字符串独有的方法，会出现错误：

    ```js
    const Component = defineComponent({
      data() {
        return {
          count: 0
        }
      },
      mounted() {
        const result = this.count.split('') // => Property 'split' does not exist on type 'number'
      }
    })
    ```

  - 如果你有一个复杂的类型或接口，你可以使用 类型断言（type assertion ）对其进行指明：

    ```js
    interface Book {
      title: string
      author: string
      year: number
    }
    
    const Component = defineComponent({
      data() {
        return {
          book: {
            title: 'Vue 3 Guide',
            author: 'Vue Team',
            year: 2020
          } as Book
        }
      }
    })
    ```

  - 为globalProperties扩充类型

    - Vue 3 提供了一个 `globalProperties` 对象，用来添加可以被任意组件实例访问的全局 property。

    - 例如一个插件想要注入一个共享全局对象或函数。

      ```js
      // 用户定义
      import axios from 'axios'
      const app = Vue.createApp({})
      app.config.globalProperties.$http = axios
      // 验证数据的插件
      export default {
        install(app, options) {
          app.config.globalProperties.$validate = (data: object, rule: object) => {
            // 检查对象是否合规
          }
        }
      }
      ```

    - 为了告诉 TypeScript 这些新 property，我们可以使用模块扩充 (module augmentation)。

      在上述示例中，我们可以添加以下类型声明：

      ```ts
      import axios from 'axios'
      declare module '@vue/runtime-core' {
        export interface ComponentCustomProperties {
          $http: typeof axios
          $validate: (data: object, rule: object) => boolean
        }
      }
      ```

      我们可以把这些类型声明放在同一个文件里，或一个项目级别的 `*.d.ts` 文件 (例如在 TypeScript 会自动加载的 `src/typings` 文件夹中)。对于库/插件作者来说，这个文件应该被定义在 `package.json` 的 `types` property 里。

    - 注意：

      - 确认声明文件是一个 TypeScript 模块
      - 为了利用好模块扩充，你需要确认文件中至少有一个顶级的 `import` 或 `export`，哪怕只是一个 `export {}`。
      - 在 TypeScript 中，任何包含一个顶级 `import` 或 `export` 的文件都被视为一个“模块”。如果类型声明在模块之外，该声明会覆盖而不是扩充原本的类型。

  - 注解返回类型

    - 由于 Vue 声明文件的循环特性，**TypeScript 可能难以推断 computed 的类型**。因此，你可能**需要注解计算属性的返回类型**。

    - ```js
      import { defineComponent } from 'vue'
      
      const Component = defineComponent({
        data() {
          return {
            message: 'Hello!'
          }
        },
        computed: {
          // 需要注解
          greeting(): string {
            return this.message + '!'
          },
      
          // 在使用 setter 进行计算时，需要对 getter 进行注解
          greetingUppercased: {
            get(): string {
              return this.greeting.toUpperCase()
            },
            set(newValue: string) {
              this.message = newValue.toUpperCase()
            }
          }
        }
      })
      ```

  - 注解props

    - Vue 对定义了 `type` 的 prop 执行运行时验证。要将这些类型提供给 TypeScript，我们需要使用 `PropType` 指明构造函数：

      ```js
      import { defineComponent, PropType } from 'vue'
      
      interface Book {
        title: string
        author: string
        year: number
      }
      
      const Component = defineComponent({
        props: {
          name: String,
          id: [Number, String],
          success: { type: String },
          callback: {
            type: Function as PropType<() => void>
          },
          book: {
            type: Object as PropType<Book>,
            required: true
          },
          metadata: {
            type: null // metadata 的类型是 any
          }
        }
      })
      ```

    - 注意：

      - 由于 TypeScript 中的设计限制，当它涉及到为了对函数表达式进行类型推理，你必须注意对象和数组的 `validator` 和 `default` 值：

        ```js
        import { defineComponent, PropType } from 'vue'
        
        interface Book {
          title: string
          year?: number
        }
        
        const Component = defineComponent({
          props: {
            bookA: {
              type: Object as PropType<Book>,
              // 请务必使用箭头函数
              default: () => ({
                title: 'Arrow Function Expression'
              }),
              validator: (book: Book) => !!book.title
            },
            bookB: {
              type: Object as PropType<Book>,
              // 或者提供一个明确的 this 参数
              default(this: void) {
                return {
                  title: 'Function Expression'
                }
              },
              validator(this: void, book: Book) {
                return !!book.title
              }
            }
          }
        })
        ```

  - 注解emit

    - 我们可以为触发的事件注解一个有效载荷。另外，所有未声明的触发事件在调用时都会抛出一个类型错误。

    - ```js
      const Component = defineComponent({
        emits: {
          addBook(payload: { bookName: string }) {
            // perform runtime 验证
            return payload.bookName.length > 0
          }
        },
        methods: {
          onSubmit() {
            this.$emit('addBook', {
              bookName: 123 // 类型错误！
            })
            this.$emit('non-declared-event') // 类型错误！
          }
        }
      })
      ```





- 与组合式API一起使用

  - 在 `setup()` 函数中，不需要将类型传递给 `props` 参数，因为它将从 `props` 组件选项推断类型。

    ```typescript
    import { defineComponent } from 'vue'
    
    const Component = defineComponent({
      props: {
        message: {
          type: String,
          required: true
        }
      },
    
      setup(props) {
        const result = props.message.split('') // 正确, 'message' 被声明为字符串
        const filtered = props.message.filter(p => p.value) // 将引发错误: Property 'filter' does not exist on type 'string'
      }
    })
    ```

  - 类型声明refs

    - Refs 根据初始值推断类型：

      ```typescript
      import { defineComponent, ref } from 'vue'
      
      const Component = defineComponent({
        setup() {
          const year = ref(2020)
      
          const result = year.value.split('') // => Property 'split' does not exist on type 'number'
        }
      })
      ```

    - 有时我们可能需要为 ref 的内部值指定复杂类型。我们可以在调用 ref 重写默认推理时简单地传递一个泛型参数：

      ```typescript
      const year = ref<string | number>('2020') // year's type: Ref<string | number>
      
      year.value = 2020 // ok!
      ```

    - 注意：**如果泛型的类型未知，建议将 `ref` 转换为 `Ref<T>`**

  - 为模板引用定义类型

    - 有时可能需要为一个子组件标注一个模板引用，以调用其公共方法。

    - 例子：

      ```typescript
      //这是子组件
      import { defineComponent, ref } from 'vue'
      const MyModal = defineComponent({
        setup() {
          const isContentShown = ref(false)
          const open = () => (isContentShown.value = true)
          return {
            isContentShown,
            open
          }
        }
      })
      ```

      ```typescript
      //这是父组件
      import { defineComponent, ref } from 'vue'
      const app = defineComponent({
        components: {
          MyModal
        },
        template: `
          <button @click="openModal">Open from parent</button>
          <my-modal ref="modal" />
        `,
        setup() {
          const modal = ref()
          const openModal = () => {
            modal.value.open()
          }
          return { modal, openModal }
        }
      })
      ```

      - 它可以工作，但是没有关于 `MyModal` 及其可用方法的类型信息。为了解决这个问题，你应该在创建引用时使用 `InstanceType`：

        ```typescript
        setup() {
          const modal = ref<InstanceType<typeof MyModal>>()
          const openModal = () => {
            modal.value?.open()
          }
          return { modal, openModal }
        }
        ```

        

  - 类型声明reactive

    - 当声明类型 `reactive` property，我们可以使用接口：

      ```typescript
      import { defineComponent, reactive } from 'vue'
      
      interface Book {
        title: string
        year?: number
      }
      
      export default defineComponent({
        name: 'HelloWorld',
        setup() {
          const book = reactive<Book>({ title: 'Vue 3 Guide' })
          // or
          const book: Book = reactive({ title: 'Vue 3 Guide' })
          // or
          const book = reactive({ title: 'Vue 3 Guide' }) as Book
        }
      })
      ```

      

  - 类型声明computed

    - 计算值将根据返回值自动推断类型

    - ```typescript
      import { defineComponent, ref, computed } from 'vue'
      
      export default defineComponent({
        name: 'CounterButton',
        setup() {
          let count = ref(0)
      
          // 只读
          const doubleCount = computed(() => count.value * 2)
      
          const result = doubleCount.value.split('') // => Property 'split' does not exist on type 'number'
        }
      })
      ```

      

  - 为事件处理器添加类型

    - 在处理原生 DOM 事件的时候，正确地为处理函数的参数添加类型或许会是有用的。让我们看这个例子：

      ```vue
      <template>
        <input type="text" @change="handleChange" />
      </template>
      <script lang="ts">
      import { defineComponent } from 'vue'
      export default defineComponent({
        setup() {
          // `evt` 将会是 `any` 类型
          const handleChange = evt => {
            console.log(evt.target.value) // 此处 TS 将抛出异常
          }
          return { handleChange }
        }
      })
      </script>
      ```

      - 如上例子，在没有为 `evt` 参数正确地声明类型的情况下，当我们尝试获取 `<input>` 元素的值时，TypeScript 将抛出异常。

    - 解决方案是将事件的目标转换为正确的类型：

      ```ts
      const handleChange = (evt: Event) => {
        console.log((evt.target as HTMLInputElement).value)
      }
      ```

      

- 
