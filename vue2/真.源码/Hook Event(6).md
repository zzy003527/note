# Hook Event(6)

## 一、目标

- 理解什么是 Hook Event ？明白其使用场景
- 深入理解 Hook Event 的实现原理



## 二、什么是Hook Event

- Hook Event 是 Vue 的自定义事件结合生命周期钩子实现的一种从组件外部为组件注入额外生命周期方法的功能。

 

### 2.1 使用场景

- 假设现在有这么一个第三方的业务组件，逻辑很简单，就在 mounted 生命周期中调用接口获取数据，然后将数据渲染到页面上。

  ```html
  <template>
    <div class="wrapper">
      <ul>
        <li v-for="item in arr" :key="JSON.stringify(item)">
          {{ item }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        arr: []
      }
    },
    async mounted() {
      // 调用接口获取组件渲染的数据
      const { data: { data } } = await this.$axios.get('/api/getList')
      this.arr.push(...data)
    }
  }
  </script>
  ```

  - 然后在使用的发现这个组件有些瑕疵，比如最简单的，接口等待时间可能比较长，我想在 mounted 生命周期开始执行的时候在控制台输出一个 `loading ...` 字符串，增强用户体验。
  - 这个需求该怎么实现呢？
  - 有两个办法：第一个比较麻烦，修改源码；而第二种方式则简单多了，就是我们今天介绍的 Hook Event，从组件外面为组件注入额外的生命周期方法。

  ```html
  <template>
    <div class="wrapper">
      <comp @hook:mounted="hookMounted" />
    </div>
  </template>
  
  <script>
  // 这就是上面的那个第三方业务组件
  import Comp from '@/components/Comp.vue'
  
  export default {
    components: {
      Comp
    },
    methods: {
      hookMounted() {
        console.log('loading ...')
      }
    }
  }
  </script>
  ```

  - 这时候你再刷新页面就会发现业务组件在请求数据的时候，会在控制台输出一个 `loading ...` 字符串。



### 2.2 作用

- Hook Event 有什么作用？
  - 通过 Hook Event 可以从组件外部为组件注入额外的生命周期方法。









## 三、实现原理

- Vue 的生命周期函数是通过一个叫 `callHook` 的方法来执行的

### 3.1 callHook

- ```js
  /**
   * callHook(vm, 'mounted')
   * 执行实例指定的生命周期钩子函数
   * 如果实例设置有对应的 Hook Event，比如：<comp @hook:mounted="method" />，执行完生命周期函数之后，触发该事件的执行
   * @param {*} vm 组件实例
   * @param {*} hook 生命周期钩子函数
   */
  export function callHook (vm: Component, hook: string) {
    // 在执行生命周期钩子函数期间禁止依赖收集
    // #7573 disable dep collection when invoking lifecycle hooks
    pushTarget()
    // 从实例配置对象中获取指定钩子函数，比如 mounted
    const handlers = vm.$options[hook]
    // mounted hook
    const info = `${hook} hook`
    if (handlers) {
      // 通过 invokeWithErrorHandler 执行生命周期钩子
      for (let i = 0, j = handlers.length; i < j; i++) {
        invokeWithErrorHandling(handlers[i], vm, null, vm, info)
      }
    }
    // Hook Event，如果设置了 Hook Event，比如 <comp @hook:mounted="method" />，则通过 $emit 触发该事件
    // vm._hasHookEvent 标识组件是否有 hook event，这是在 vm.$on 中处理组件自定义事件时设置的
    if (vm._hasHookEvent) {
      // vm.$emit('hook:mounted')
        // 执行 vm._events['hooks:mounted']数组当中的所有处理函数
      vm.$emit('hook:' + hook)
    }
    // 关闭依赖收集
    popTarget()
  }
  ```



#### 3.1.2 invokeWithErrorHandling

- ```js
  /**
   * 通用函数，执行指定函数 handler
   * 传递进来的函数会被用 try catch 包裹，进行异常捕获处理
   */
  export function invokeWithErrorHandling (
    handler: Function,
    context: any,
    args: null | any[],
    vm: any,
    info: string
  ) {
    let res
    try {
      // 执行传递进来的函数 handler，并将执行结果返回
      res = args ? handler.apply(context, args) : handler.call(context)
      if (res && !res._isVue && isPromise(res) && !res._handled) {
        res.catch(e => handleError(e, vm, info + ` (Promise/async)`))
        // issue #9511
        // avoid catch triggering multiple times when nested calls
        res._handled = true
      }
    } catch (e) {
      handleError(e, vm, info)
    }
    return res
  }
  ```





### 3.2 vm.$on

- ```js
  /**
   * 监听实例上的自定义事件，vm._event = { eventName: [fn1, ...], ... }
   * @param {*} event 单个的事件名称或者有多个事件名组成的数组
   * @param {*} fn 当 event 被触发时执行的回调函数
   * @returns 
   */
  Vue.prototype.$on = function (event: string | Array<string>, fn: Function): Component {
    const vm: Component = this
    if (Array.isArray(event)) {
      // event 是有多个事件名组成的数组，则遍历这些事件，依次递归调用 $on
      for (let i = 0, l = event.length; i < l; i++) {
        vm.$on(event[i], fn)
      }
    } else {
      // 将注册的事件和回调以键值对的形式存储到 vm._event 对象中 vm._event = { eventName: [fn1, ...] }
      (vm._events[event] || (vm._events[event] = [])).push(fn)
      // hookEvent，提供从外部为组件实例注入声明周期方法的机会
      // 比如从组件外部为组件的 mounted 方法注入额外的逻辑
      // 该能力是结合 callhook 方法实现的
      if (hookRE.test(event)) {
        vm._hasHookEvent = true
      }
    }
    return vm
  }
  ```





## 四、总结

### 4.1 什么是 Hook Event？

- Hook Event 是 Vue 的自定义事件结合生命周期钩子实现的一种从组件外部为组件注入额外生命周期方法的功能。



### 4.2 Hook Event 是如果实现的？

- ```html
  <comp @hook:lifecycleMethod="method" />
  ```

  - 处理组件自定义事件的时候（vm.$on) 如果发现组件有 `hook:xx` 格式的事件（xx 为 Vue 的生命周期函数），则将 `vm._hasHookEvent` 置为 `true`，表示该组件有 Hook Event
  - 在组件生命周期方法被触发的时候，内部会通过 `callHook` 方法来执行这些生命周期函数，在生命周期函数执行之后，如果发现 `vm._hasHookEvent` 为 true，则表示当前组件有 Hook Event，通过 `vm.$emit('hook:xx')` 触发 Hook Event 的执行

  这就是 Hook Event 的实现原理。

  