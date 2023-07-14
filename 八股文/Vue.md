# Vue

## 讲讲vue2的响应式原理？

- 在Vue初始化的时候，原始对象通过`Observer`为其添加getter和setter方法，将其转化为一个响应式的对象，
- 在第一次渲染页面的时候，每个组件用到的render函数都会交给`Watcher对象的一个实例`来执行（每个组件有一个watcher实例，该实例中含有该组件的render方法），watcher将自己设置为一个全局变量，然后读取数据，因为读取了数据，所以会触发响应式对象中的getter，随后getter会从全局变量的位置读取到当前正在读取的watcher实例并把watcher收集到数据的dep中（记录依赖，`dep.depend()`）
- 当数据发生改变时，就会触发数据的setter，从而就会触发数据的dep中的`(dep.notify())`进行派发更新，随后通知watcher。但是watcher不是立即执行的，因为数据变动有时候会有很多个，立即执行的话会重复执行很多render函数，执行效率会变低。所以watcher把自己交给调度器`Scheduler`
- 调度器会把watcher添加到队列中，然后将队列交给`nextTick`队列，nextTick里面的函数都是在微队列中，等同步代码执行完，再异步执行watcher以及其中的render函数，更新页面内容
- Dep
  - 当读取响应式对象的某个属性时，它会进行依赖收集：有人用到了我
  - 当改变某个属性时，它会派发更新：那些用我的人听好了，我变了
- Watcher
  - 每一个vue组件实例，都至少对应一个watcher，该watcher中记录了该组件的render函数。
  - watcher首先会把render函数运行一次以收集依赖，于是那些在render中用到的响应式数据就会记录这个watcher。
  - 当数据变化时，dep就会通知该watcher，而watcher将重新运行render函数，从而让界面重新渲染，同时重新记录当前的依赖。





## Vue2响应式的缺陷

- 无法监听到对象属性的动态添加和删除

  - 用this.$set解决响应式问题

    ```js
       fn3(){
       //this.$set('对象','添加的属性',新值) 
         this.$set(this.obj,'c',3)  //有响应
        }
    }
    ```

- 无法监听到数组下标和length长度的变化

  - 用重写的方法 push(), pop(), shift() ,unshift() ,splice() ,sort() ,reverse() 可以解决响应式问题

  - ##### 或者 this.$set() 也可以解决数组的响应问题

    ```js
    let arr=[1,2,3,4]
    fn3(){
        //老方法:
        //this.arr[0]=100  //无响应式
        //新方法  使用$set()
        //this.$set('数组','下标',值)  //有响应
          this.$set('this.arr','0',100) //arr=[100,2,3,4]
        }
    }
    ```





## Vue2和Vue3的区别

- 数据双向绑定原理不同
  - Vue2：
    1. **Object.defineProperty**只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。
    2. 在遇到一个对象的属性还是一个对象的情况下，需要递归监听，对性能影响比较大。
    3. 由于Vue会在初始化实例时对属性执行getter/setter转化，所有属性必须在data对象上存在才能让Vue将它转换为响应式。
    4. 对于已经创建的实例，Vue 不允许动态添加根级别的响应式属性。但是，可以使用 `Vue.set(object, propertyName, value)` 或`vm.$set`方法向嵌套对象添加响应式属性。([深入响应式原理](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Freactivity.html%23%E6%A3%80%E6%B5%8B%E5%8F%98%E5%8C%96%E7%9A%84%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9))
    5. **Object.defineProperty**，不具备监听数组的能力，需要重新定义数组的原型来达到响应式。（[变异数组方法](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Flist.html%23%E5%8F%98%E6%9B%B4%E6%96%B9%E6%B3%95)）
  - Vue3：
    1. 基于**Proxy**和**Reflect**，可以原生监听数组，可以监听对象属性的添加和删除或多层嵌套数据结构的响应。
    2. Proxy代理的是整个对象，而不是对象的某个特定属性，不需要我们通过遍历来逐个进行数据绑定。
    3. 不需要一次性遍历data的属性，可以显著提高性能。
- 数据和方法的定义不同
  - Vue2：
    `data() { return {}; }, methods:{ }`
  - Vue3：
    `数据和方法都定义在setup中，并统一进行return{}`
- vue2是选项式API，而vue3是组合式API
  - Vue2中选项API（options API）格式的代码的可读性较差，比如需要实现某一个功能代码涉及到`props、data、methods、computed、watch`等属性的时候，就会出现已下几个代码块：
    - 在 props 中接收参数
    - 在 data 中定义变量
    - 在 watch 中监听变化
    - 在 computed 中定义需要使用到的计算属性
    - 在 methods 中定义事件响应方法
  - Vue3的组合式API（composition API）就对这一缺点进行了优化，**使用组合式API我们能够将我们想要关联的代码都放到一起**，这样大大的增加了代码的可读性和可维护性。
- 获取props
  - vue2：console.log(‘props’,this.xxx)
  - vue3：setup(props,context){ console.log(‘props’,props) }
- 给父组件传值emit
  - vue2：this.$emit()
  - vue3：setup(props,context){context.emit()}







## v-if和v-show的区别

- 控制手段不同
  - 控制手段：`v-show`隐藏则是为该元素添加`css--display:none`，`dom`元素依旧还在。`v-if`显示隐藏是将`dom`元素整个添加或删除
- 编译过程不同
  - 编译过程：`v-if`切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；`v-show`只是简单的基于css切换
- 编译条件不同
  - 编译条件：`v-if`是真正的条件渲染，它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。只有渲染条件为假时，并不做操作，直到为真才渲染

- `v-show` 由`false`变为`true`的时候不会触发组件的生命周期
- `v-if`由`false`变为`true`的时候，触发组件的`beforeCreate`、`create`、`beforeMount`、`mounted`钩子，由`true`变为`false`的时候触发组件的`beforeDestory`、`destoryed`方法

- 性能消耗：`v-if`有更高的切换消耗；`v-show`有更高的初始渲染消耗；
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